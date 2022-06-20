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

