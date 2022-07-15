# Channel 通道

- 与IO不同点,是双向通道,即可以用来输入数据也可以用来输出数据
- 常用的channel
  - FileChannel:文件传输通道
  - DatagramChannel:UDP网络传输通道
  - SocketChannel:TCP传输通道
    - 类似Socket
  - ServerSocketChannel:服务器端专用TCP传输通道
    - 类似java中的ServerSocket
      ```
      //非阻塞nio
      ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
              //创建服务器
              ServerSocketChannel ssc = ServerSocketChannel.open();
              //绑定端口
              ssc.bind(new InetSocketAddress(8080));
              //设置非阻塞模型,非阻塞模式accept()不会阻塞代码,没有连接返回空值
              ssc.configureBlocking(false);
              //建立与客户端连接
              while (true) {
                  //建立客户端通讯
                  SocketChannel sc = ssc.accept();
                  //将socketChannel设置为非阻塞,read为空时返回0
                  sc.configureBlocking(false);
                  sc.read(byteBuffer);
                  byteBuffer.flip();
                  byte[] array = byteBuffer.array();
                  System.out.println(new String(array));
                  byteBuffer.clear();
              }
      ```
    
      

# Buffer 缓冲区

- 内存缓冲区,用于暂存channel读入读出的数据
- 常用的buffer
  - ByteBuffer
    - MappedByteBuffer
    - DirectByteBuffer
    - HeepByteBuffer

## ByteBuffer

- 结构
  - capacity
    - buffer的真实长度
  - position
    - 当前的操作指针位置
  - limit
    - 限制操作位置
    - 写入操作,limit总是等于capacity缓冲的真实长度
    - 读取操作,limit等于当前buffer中有效数据的长度
- 操作API
  - 分配缓存空间
    - `ByteBuffer.alloate(int);`分配一个JVM堆内存,会增大GC的压力
    - `ByteBuffer.allocateDirect(int);`分配一个系统直接内存,不会增大GC的压力;零拷贝减少一次拷贝的次数;系统内存,分配效率低
  - `buffer.put;`写入数据
  - `buffer.flip();`切换读模式,将limit移动到原来postion的位置,并将postion移动到零
  - `buffer.get();`获取当前位置的元素,并获取下一个位置的元素
  - `buffer.compact();`切换读模式,但是保留之前未读取的元素;具体做法将之前未读取元素向前移动,postion移动到未读取元素的末尾
  - `buffer.clear();`将limit移动到末尾,将position移动到零位置
- 消息边界问题
  - 消息大小比ByteBuffer要长,只能扩容ByteBuffer
  - 消息小于ByteBuffer;就会出现半包
  - 一个ByteBuffer中能存下多个消息.就会造成粘包


# Selector 选择器

- Channel的register(<selector>,<ops>,<att>);
  - selector被关联的选择器
  - attr附件,事件触发时,可以通过key.attachment();方法拿到附件对象,也可以通过key.attach()方法更换att附件

- Selector结构:
  - Collection\<selectionKey\>用于存储selectionKey集合
    - 首次创建Selector时,selectionKey集合是空的,当channel执行register()就会生成一个selectorKey到selector中
  - selectedKeys集合,当有事件被触发时就会把selector中对应触发了时间的selectorKey添加进入;通过`selector.selectedKeys()`方法获取
    - selectedKeys中的内容不会主动被清除,对于处理过的事件要主动从容器中删除掉

- 用于监听Channel管道上发生的事件,并分配线程处理事件

  ```java
  //打开选择器
  Selector selector = Selector.open();
  //打开网络传输通道
  FileInputStream fileInputStream = new FileInputStream("./hello/test");
  ServerSocketChannel ssc = ServerSocketChannel.open();
  
  //绑定选择器,监视accept()事件
  SelectionKey register = ssc.register(selector, 0, null);
  register.interestOps(SelectionKey.OP_ACCEPT);
  
  //切换为非阻塞
  ssc.configureBlocking(false);
  
  //绑定端口
  ssc.bind(new InetSocketAddress(8080));
  
  while (true) {
      //无事件就阻塞
      selector.select();
  
      Iterator<SelectionKey> iterator = selector.selectedKeys().iterator();
      while (iterator.hasNext()) {
          //获取事件
          SelectionKey key = iterator.next();
          //处理key时要将其从selectedKey中移除
          iterator.remove();
          //处理接受事件
          if (key.isAcceptable()) {
              ServerSocketChannel channel = (ServerSocketChannel) key.channel();
              //切换为非阻塞模式
              channel.configureBlocking(false);
              //绑定读取事件
              SocketChannel sc = channel.accept();
              SelectionKey scKey = sc.register(selector, 0, null);
              scKey.interestOps(SelectionKey.OP_READ);
          }
          //处理读取事件
          else if (key.isReadable()) {
              try {
                  SocketChannel socketChannel = (SocketChannel) key.channel();
                  ByteBuffer byteBuffer = ByteBuffer.allocate(1024);
                  int read = socketChannel.read(byteBuffer);
                  //断开连接,数据返回-1
                  if (read == -1) {
                      key.cancel();
                  }
                  byteBuffer.flip();
              } catch (IOException e) {
                  //防止客户端异常导致服务器关闭
                  e.printStackTrace();
                  key.cancel();
              }
          }
  
  
          //忽略事件
          key.cancel();
      }
  }
  ```

  

# 对应关系

- 一个Thread线程对应一个Selector管理连接
- 一个Selector对应多个Channel
- 一个Channel对应一个Buffer
- 程序切换到哪个Channel上是由时间决定的,Event是一个重要的概念
- Selector会根据不同的时间在各个通道上切换
- Buffer是一个内存块,底层是由一个数组
- 数据的读取通过Buffer 
- BIO要么是输入流要么是输出流,不是双向的,但是NIO的Buffer是可以读也可以写,但是需要flip方法切换

# jdk7文件类

- Path:

  - Paths工具类

- File:

  - Files工具类

  - ```java
    //获取一个路径
    Path path = Paths.get("./hello/d1");
    //创建一个文件夹,但不支持多级创建
    Files.createDirectory(path);
    //支持多级创建目录
    Files.createDirectories(path);
    //拷贝文件
    Path source = Paths.get("./from");
    Paht target = Paths.get("./to");
    //不支持覆盖,文件存在会报异常
    Files.copy(source,target);
    //支持覆盖
    Files.copy(source,target,StandardCopyOption.REPLACE_EXISTING);
    //移动文件,StandardCopyOption.ATOMIC_MOVE保证文件移动的原子性
    Files.move(source,target,StandardCopyOption.ATOMIC_MOVE);
    //删除文件或目录,但是不支持递归删除目录
    Files.delete(path);
    ```

  - 文件目录树遍历
    ```
    Files.walkFileTree(Paths.get("G:/Test"), new SimpleFileVisitor<>() {
                @Override
                public FileVisitResult visitFile(Path file, BasicFileAttributes attrs) throws IOException {
                    System.out.println(file.getFileName());
                    return super.visitFile(file, attrs);
                }
            });
    ```

    

# 服务器的方案

- 多线程版
  - 缺点无法控制线程的数量
  - 线程多时,线程本身占用内存高
  - 线程上线文切花成本高
- 线程池版
  - 阻塞模式下,线程仅能处理一个socket连接

