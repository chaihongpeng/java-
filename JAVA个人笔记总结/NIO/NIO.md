# Channel 通道

- 与IO不同点,是双向通道,即可以用来输入数据也可以用来输出数据
- 常用的channel
  - FileChannel:文件传输通道
  - DatagramChannel:UDP网络传输通道
  - SocketChannel:TCP传输通道
  - ServerSocketChannel:服务器端专用TCP传输通道

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

# Selector 选择器

- 用于监听Channel管道上发生的事件,并分配线程处理事件



# 服务器的方案

- 多线程版
  - 缺点无法控制线程的数量
  - 线程多时,线程本身占用内存高
  - 线程上线文切花成本高
- 线程池版
  - 阻塞模式下,线程仅能处理一个socket连接