什么是响应式?

# Interface

- Runnable
  - void run();

- Consumer
  - void accept(T t);





# 结构

- 观察者Observer
  - Consumer是一个特殊的观察真
- 被观察者
  - 触发事件并决定什么时候发送的角色
  - Flowable是支持背压的一种观察者
- 订阅Subscribe
  - 主观应该是观察者订阅被观察者
  - 为了体现链式调用,经常是被观察者订阅观察者
  - subscribe就是观察者和被观察者建立关系的一个操作

# 操作符

- 创建操作符
  - create();创建一个回调接口
- 转换操作符

# Reactive API

- ```java
  Flux.just(1, 2, 3, 4)
          //订阅时调用
          .doOnSubscribe(value -> {
              System.out.println("doOnSubscribe");
          })
          //流中有新元素进入时
          .doOnNext(e -> {
              System.out.println("doOnNext" + e);
  
          })
          //处理完所有方法后执行
          .doOnComplete(() -> {
              System.out.println("doOnComplete");
          })
          //开始订阅
          .subscribe()
  ;
  ```



- ```java
  //编程时生成
  Flux.generate(
          //初始化状态
          () -> 1,
          //生成流
          (state, sink) -> {
              //管道输入下一个状态
              sink.next(state);
              //管道关闭
              if (state == 10) {
                  sink.complete();
              }
              //返回下一次状态
              return state + 1;
          });
  ```



- ```java
  Flux
          .just("test", "admin", "user", "manager")
          .doOnSubscribe(subscription -> System.out.println("start subscribe"))
          .doOnNext(System.out::println)
      	// then()方法,用于连接两个管道,保证两个管道间的执行顺序性
          .thenMany(Flux.just("new", "ok"))
          .doOnNext(System.out::println)
          .doOnSubscribe(subscription -> System.out.println("last subscribe"))
          .subscribe()
  ;
  ```

- ```java
  //Flux和Mono之间的互转
  Mono<List<String>> mono = Flux.just("test", "a").collectList();
  Flux<String> flux = Mono.just("test").flux();
  ```

- ```java
  //zip,所有的管道都获取到值或进行统一处理
  Mono<String> user = Mono.just("user");
  Mono<String> record = Mono.just("record");
  Mono<String> order = Mono.just("order");
  Mono.zip(user, record, order).doOnNext(t -> {
      t.getT3();
  });
  ```

- ```java
  //异常处理的两种方法
  Mono<String> test = Mono.just("test");
  test.map(v -> {
      if (v.equals("test")) {
          throw new RuntimeException();
      }
      return v;
  });
  test.flatMap(v -> {
      if (v.equals("test")) {
          return Mono.error(new RuntimeException());
      }
      return Mono.just(v);
  });
  ```

- ```java
  //将两个流组合到一起 
  Mono.just("test")
                  .zipWith(Mono.just("test2"))
                  .subscribe(value -> {
                      String t1 = value.getT1();
                      String t2 = value.getT2();
                  });
  ```

# WebFlux

# JDK9 Reactive Stream

- Flow
  - Publisher 发布者
    - subscribe()与订阅者产生订阅关系
  - Subscriber订阅者
    - onSubscribe()签署订阅关系时调用
    - onNext()接受到数据时调用
    - onError()发生错误时调用
    - onComplete()数据处理完成后调用
  - Subscription订阅关系
    - request()告诉发布者,这边还需要多少资源,发布者就可以消耗这些资源进行发送
  - Processor中间角色,同时实现了发布者和订阅者接口,用于多个发布者和订阅者间的数据传输



