# Gradle

## Project

- 等效于Maven的pom.xml

- project实际对应一个build.gradle文件,实际上project就是一个build.gradle的实例

- 一个模块只有一个build.gradle

- project.name就是项目名称

  - 执行./gradlew build,第一个步骤,gradle会给这个项目创建一个settings实例,对应settings.gradle文件

    - 一个项目无论有多少个模块,他都只会有一个settings配置文件

    - build.gradle会在每个模块下都有一个

    - gradle会针对settings的实例,按照配置层次进行扫描,解决多模块的依赖关系,settings实例中,最重要的属性就是include
      ```groovy
      //按层次扫描,gradle扫描时会先扫描first-order,在取扫描first-order-controller
      rootProject.name='first-gradle'
      include 'first-order'
      include 'first-order:first-order-controller'
      ```

    - 初始化,针对扫描出来的build.gradle进行初始化,广度加载,创建project实例

  - project的组成

    - 属性

      - 内置属性

        - 内部属性的使用
          ```groovy
          println project.name
          //在使用内部属性时,project可以省略掉
          println name
          println project.property('name')
          
          //./gradlew 为这个project指定默认执行的task
          defaultTasks("my-task")
          ```

          

      - 扩展属性
        ```groovy
        //自定义扩展属性
        ext.prop1 = "扩展属性1"
        ext{
            prop1 = "扩展属性1"
        }
        //关于扩展属性,我们可以在project,task,subproject去读写
        println ext.prop1
        println prop1
        //子模块读取
        println parent.ext.prop1
        println prop1
        
        ```

      - 属性的作用于
        - 在自身project对象中去找
        - 在自身ext中去找
        - 通过被添加到project中插件带来的属性extensions
        - 通过插件添加到project中的约定属性convertion
        - 在task中定义的属性
        - 从ext父类继承的属性
      
      - project中常用属性
        - group定义项目的组
      
      - projiect 中的方法
      
        - 寻找方法的顺序和寻找属性的顺序是相同的
      
        - buildscript{}为gradle提供构建脚本,写在配置文件的最前面;
      
          - gradle如使用docker构建等需要的插件时在buildscript中编写
          - buildscript和app的依赖是分开的
      
        - ```groovy
          buildscript{
              //负责给当前项目构建脚本提供依赖
              repositories{
                  //执行顺序从上到下,先去本地仓库寻找,没有找到再去阿里云仓库,最后到maven中央仓库寻找
                  mavenLocal()
                  maven{
                      url "https://maven.aliyun.com/repository/public"
                  }
                  mavenCentral()
              }
              dependcies{
                  //classpath 指定依赖的路径
                  classpath "com.bmuschko:gradle-docker-plugin:3.3.4"
              }
          }
          //负责给当前项目应用提供依赖
          dependcies{
              implementation "com.google.guava:guava:23.0"
              mygroup "junit:junit:4.12"
          }
          ```
      
        - ```groovy
          //定义插件的方法
          apply plugin:'java'
          plugins{
              id 'java-library'
          }
          
          //用于配置,作用是定义分组,在repositories中使用对应的分组引入依赖,不同的分组在不同的环境中使用
          configurations{
              //由plugin "java"插件默认提供了implementation, testRuntime等分组
              myGroup
              //分组可以继承,自定义分组myImplementationGroup就拥有了implementation的特性
              myImplementationGroup.extendsFrom(implementation)
          }
          //负责给当前项目应用提供依赖
          dependcies{
              implementation "com.google.guava:guava:23.0"
              //我们自定义的分组,需要注意我们需要写自定义分组的实现,才能对分组进行处理
              mygroup "junit:junit:4.12"
          }
          
          //对configurations进行扩展
          task xx {
              //一下代码可以打印自定义分组引入的依赖的所有真实路径
              println configuration.myImplementation.asPath
              def libPath = projectDir.absolutePath + "/src/main/lib"
              //将依赖包拷贝到指定目录
              copy {
                  from configuration.myImplementation.myImplementation.files.first()
                  into libPath
              }
          }
          ```
      
        - ```groovy
          //仓库
          repositories{
              //配置有密码的仓库
              maven(){
                  credentials {
                      username=""
                      password =""
                  }
                  url = ""
              }
              repositories {
                  flatDir(dir:"../lib",name:"libs dir")
                  flatDir {
                      dirs '',''
                      name = 'dirs'
                  }
              }
          }
          ```
      
        - ```groovy
          //gradle可以通过dependencies命令查看包的依赖关系
          dependencies {
              //complie已经过时了,推荐使用implementation
          	implementation 'com.goole.guava:11.0.2'
              //排除依赖
              implementation('org.slf4j-simple:1.6.4') {
                  exclude 'org.sl4j-api'
              }
              //依赖其他模块
              implementation project(':projectA')
              //依赖文件树下的所有文件
              implementation fileTree(dir:'libs',include:['*.jar'])
              
              implementation ('com.goole.guava:guava:11.0.2'){
                  //在版本冲突情况下有限使用该版本
                  isForce = true
                  //禁用依赖传递
                  transitive = false
              }
          }
          ```
      
        - ```groovy
          allprojects{
              
          }
          ```
      
        - 
      
        - 
      
        - 常用分组
      
          - api
          - implementation
            - implementation继承自api,api有的特性,implementation也都有
            - api具有传递性 例: a实现b, b实现c, api对于所有人都是透明的, c可以使用a中任何方法
            - 而implementation则是封闭的,c无法调用a中的方法
            - compileOnly仅在编译时使用, 如lombok
            - runtimeOnly仅在运行时使用, 一些包在编译时不使用,运行时使用
            - testComplieOnly, testImplementation, testRuntimeOnly测试环境使用

## Task

- 任务,他是gradle中最小执行单位,类似于method或function,如,编译,打包,生成javadoc,一个project中有多个tasks

```groovy
task say{
    println "Hello world"
}
task sayDoLast{
    doFirst{
        println "int"
    }
    doLast{
        println "doLast"
    }
}
```

# Groovy

- 方法和属性
  ```groovy
  def value = "123";
  ```

- 方法

  - 基本写法
    ```groovy
    def myMethod(String a,String b){
    	println("$a+$b");
    }
    ```

    

  - 参数括号可以省略
    ```groovy
    //在调用方法时,参数的括号可以省略
    myMethod "a","b"
    myMethod("a","b");
    ```

  - return语句可以不写

    - 如果不写return,groovy会把方法中最后一句代码作为返回值

    - ```groovy
      //此方法会比较a和b的大小并返回a和b中较大的那个
      def myMethod2(int a,int b){
          if(a>b){
              a
          }else{
              b
          }
      }
      ```

  - 支持参数动态类型推导
    ```groovy
    //groovy的入参可以省略声明
    
    def myMethod3(a,b){
        a + b
    }
    ```

    

- 字符串

  - 单引号
    - 单引号没有运算能力,没法解析\$符
  - 双引号
    - 可以使用\$
  - 三引号
    - 支持多行
    - 不支持运算\$

- JavaBean

  - Groovy会给类中每一个没有可见性修饰符的属性,生成getter/setter方法

  - ```groovy
    class Person{
    	def name = "tom"
        private int age = 16
        
        public def getCountry(){
            "China"
        }
    }
    def person = new Person()
    println person.name
    ```

  - 只要我们写了getter/setter方法,即使不定义属性,我们也可以操作这个属性
  - 对于没有定义的属性,我们只写get方法,那么你就不能改变它的值

- 集合

  - 数组

    - 定义list

      ```groovy
      def list = ["123", "hello", "ok"]
      ```

    - 可以使用负数作为数组下标
      ```groovy
      println list[0] //获取第一个值
      println list[-1] //获取导数第一个值
      println list[0..2] //获取第一到第三个值
      println list[-1..0] //获取倒数第一和正数第一的值
      ```

  - Map

    - 定义map
      ```groovy
      //定义
      def map = [:]
      println map.size()
      
      //定义
      def map2 = [
          name:'tom',
          age:'test'
      ]
      //判断是否存在
      map.containsKey('name')
      
      //获取
      println map.name
      println map['name']
      println map.get('name', 'default value')
      map.each{k,v-> println "${k}:${v}"}
      for(e in map){
          println "${e.key}:${e.value}"
      }
      ```

  - 闭包

    - Closure ->groovy.lang.Closure

    - 闭包就是一段代码块,他可以在任何地方去执行

    - 定义
      ```groovy
      //语义
      //没有参数的闭包
      {println "hello world"}
      //有一个参数的闭包
      {[x->] println x}
      {it -> println it}//简化
      //多个参数的闭包
      {String x,int y -> println "${x},${y}"}
      {x,y-> println "${x},${y}"}//简化
      
      //调用闭包
      //闭包可以作为一个参数
      def example = {it -> println "hello gradle,hello ${it}"}
      //自身调用
      //调用方法1
      example('tom')
      //调用方法2
      example.call('tom')
      
      //闭包对象可以作为参数传递给其他方法
      //定义一个方法,接受值为闭包
      def myMethod(closure){
          def str = "tom"
          closure(str)
      }
      //调用其他闭包参数
      myMethod(example)
      myMethod({it -> println it})
      myMethod {it -> println it}//简化
      ```

    - 进阶
      ```groovy
      //定义一个方法
      def mapEach(mapc){
          def map = [name:"tom",age:"16"]
          map.each{
              //it就是map的循环遍历结果
              mapc(it.key,it.value)
          }
      }
      ```

