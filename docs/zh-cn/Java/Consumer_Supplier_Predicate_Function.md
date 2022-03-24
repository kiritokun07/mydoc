## Java8之Consumer、Supplier、Predicate和Function攻略
> https://zhuanlan.zhihu.com/p/76340256

#### Consumer：有请求参数没有返回参数的lambda
```
/**
 * consumer接口测试
 */
@Test
public void test_Consumer() {
    //① 使用consumer接口实现方法
    Consumer<String> consumer = new Consumer<String>() {
        @Override
        public void accept(String s) {
            System.out.println(s);
        }
    };
    Stream<String> stream = Stream.of("aaa", "bbb", "ddd", "ccc", "fff");
    stream.forEach(consumer);
    System.out.println("********************");
    //② 使用lambda表达式，forEach方法需要的就是一个Consumer接口
    stream = Stream.of("aaa", "bbb", "ddd", "ccc", "fff");
    Consumer<String> consumer1 = (s) -> System.out.println(s);//lambda表达式返回的就是一个Consumer接口
    stream.forEach(consumer1);
    //更直接的方式
    //stream.forEach((s) -> System.out.println(s));
    System.out.println("********************");
    //③ 使用方法引用，方法引用也是一个consumer
    stream = Stream.of("aaa", "bbb", "ddd", "ccc", "fff");
    Consumer consumer2 = System.out::println;
    stream.forEach(consumer);
    //更直接的方式
    //stream.forEach(System.out::println);
}
```

#### Supplier：有返回参数没有请求参数的lambda
```
/**
 * Supplier接口测试，supplier相当一个容器或者变量，可以存储值
 */
@Test
public void test_Supplier() {
    //① 使用Supplier接口实现方法,只有一个get方法，无参数，返回一个值
    Supplier<Integer> supplier = new Supplier<Integer>() {
        @Override
        public Integer get() {
            //返回一个随机值
            return new Random().nextInt();
        }
    };
    System.out.println(supplier.get());
    System.out.println("********************");
    //② 使用lambda表达式，
    supplier = () -> new Random().nextInt();
    System.out.println(supplier.get());
    System.out.println("********************");
    //③ 使用方法引用
    Supplier<Double> supplier2 = Math::random;
    System.out.println(supplier2.get());
}

/**
 * Supplier接口测试2，使用需要Supplier的接口方法
 */
@Test
public void test_Supplier2() {
    Stream<Integer> stream = Stream.of(1, 2, 3, 4, 5);
    //返回一个optional对象
    Optional<Integer> first = stream.filter(i -> i > 4).findFirst();
    //optional对象有需要Supplier接口的方法
    //orElse，如果first中存在数，就返回这个数，如果不存在，就放回传入的数
    System.out.println(first.orElse(1));
    System.out.println(first.orElse(7));
    System.out.println("********************");
    Supplier<Integer> supplier = new Supplier<Integer>() {
        @Override
        public Integer get() {
            //返回一个随机值
            return new Random().nextInt();
        }
    };
    //orElseGet，如果first中存在数，就返回这个数，如果不存在，就返回supplier返回的值
    System.out.println(first.orElseGet(supplier));
}
```

#### Predicate：有请求参数，返回参数是bool类型
```
/**
 * Predicate谓词测试，谓词其实就是一个判断的作用类似bool的作用
 */
@Test
public void test_Predicate() {
    //① 使用Predicate接口实现方法,只有一个test方法，传入一个参数，返回一个bool值
    Predicate<Integer> predicate = new Predicate<Integer>() {
        @Override
        public boolean test(Integer i) {
            if (i > 5) {
                return true;
            }
            return false;
        }
    };
    System.out.println(predicate.test(6));
    //② 使用lambda表达式
    predicate = (t) -> t > 5;
    System.out.println(predicate.test(1));
    System.out.println("********************");
}

/**
 * Predicate谓词测试，Predicate作为接口使用
 */
@Test
public void test_Predicate2() {
    //① 将Predicate作为filter接口，Predicate起到一个判断的作用
    Predicate<Integer> predicate = new Predicate<Integer>() {
        @Override
        public boolean test(Integer integer) {
            if (integer > 5) {
                return true;
            }
            return false;
        }
    };
    Stream<Integer> stream = Stream.of(1, 23, 3, 4, 5, 56, 6, 6);
    List<Integer> list = stream.filter(predicate).collect(Collectors.toList());
    list.forEach(System.out::println);
}
```

#### Function<请求类型,返回类型>：请求参数和返回参数都在泛型上
```
/**
 * Function测试，function的作用是转换，将一个值转为另外一个值
 */
@Test
public void test_Function() {
    //① 使用map方法，泛型的第一个参数是转换前的类型，第二个是转化后的类型
    Function<String, Integer> function = new Function<String, Integer>() {
        @Override
        public Integer apply(String s) {
            return s.length();
            //获取每个字符串的长度，并且返回
        }
    };
    Stream<String> stream = Stream.of("aaa", "bbbbb", "ccccccv");
    Stream<Integer> stream1 = stream.map(function);
    stream1.forEach(System.out::println);
}
```

#### Runnable：无请求参数无返回参数


## 妙用“Function”消灭if...else
> https://juejin.cn/post/7011435192803917831

#### 抛异常接口
```
@FunctionalInterface
public interface ThrowExceptionFunction {

    /**
     * 抛出异常信息
     *
     * @param message 异常信息
     * @return void
     **/
    void throwMessage(String message);

}
```

#### 空值与否分支处理
```
/**
 * 空值与非空值分支处理
 */
public interface PresentOrElseHandler<T> {

    /**
     * 值不为空时执行消费操作
     * 值为空时执行其他的操作
     *
     * @param action      值不为空时，执行的消费操作
     * @param emptyAction 值为空时，执行的操作
     * @return void
     **/
    void presentOrElseHandle(Consumer<? super T> action, Runnable emptyAction);

}
```

#### 分支处理操作
```
/**
 * 分支处理接口
 **/
@FunctionalInterface
public interface BranchHandle {

    /**
     * 分支操作
     *
     * @param trueHandle  为true时要进行的操作
     * @param falseHandle 为false时要进行的操作
     * @return void
     **/
    void trueOrFalseHandle(Runnable trueHandle, Runnable falseHandle);

}
```

#### 工具类
```
public class VUtils {

    /**
     * 如果参数为true抛出异常
     *
     * @param b
     * @return com.example.demo.func.ThrowExceptionFunction
     **/
    public static ThrowExceptionFunction isTure(boolean b) {

        return (errorMessage) -> {
            if (b) {
                throw new RuntimeException(errorMessage);
            }
        };
    }

    /**
     * 参数为true或false时，分别进行不同的操作
     *
     * @param b
     * @return com.example.demo.func.BranchHandle
     **/
    public static BranchHandle isTureOrFalse(boolean b) {

        return (trueHandle, falseHandle) -> {
            if (b) {
                trueHandle.run();
            } else {
                falseHandle.run();
            }
        };
    }

    /**
     * 参数为true或false时，分别进行不同的操作
     *
     * @param str
     * @return com.example.demo.func.BranchHandle
     **/
    public static PresentOrElseHandler<?> isBlankOrNoBlank(String str) {

        return (consumer, runnable) -> {
            if (str == null || str.length() == 0) {
                runnable.run();
            } else {
                consumer.accept(str);
            }
        };
    }

}
```

#### 测试方法
```
@Test
public void isTrue1() {
    VUtils.isTure(true).throwMessage("抛异常了");
}

@Test
public void isTrueOrFalse() {
    VUtils.isTureOrFalse(false)
            .trueOrFalseHandle(() -> System.out.println("true 了"), () -> System.out.println("false 了"));
}

@Test
public void isBlankOrNotBlank() {
    VUtils.isBlankOrNoBlank("")
            .presentOrElseHandle(System.out::println, () -> System.out.println("空字符串"));
}
```








