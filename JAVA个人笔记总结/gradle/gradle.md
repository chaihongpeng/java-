# Gradle

## Project

- 等效于Maven的pom.xml

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
  - 