# 이것이 자바다 13~

# 13 제네릭

## 13.1 왜 제네릭을 사용해야 하는가

* 잘못된 타입이 사용될 수 있는 문제를 컴파일 과정에서 제거할 수 있게 되었다.(강한 타입 체크)

* 클래스, 인터페이스, 메소드를 정의할 때 타입을 파라미터로 사용할 수 있도록 한다.

  * public class A<T>{...}
  * public interface A<T>{...}

* 타입 변환을 제거한다

  ```java
  //비 제네릭
  List list=new ArrayList();
  list.add("hello");
  String str=(String)list.get(0);//타입 변환 해야하고 이는 성능 저하를 일으킨다
  
  //제네릭
  List<String> list=new ArrayList<String>();
  list.add("hello");
  String str=list.get(0);//타입 변환을 하지 않는다.
  ```

## 13.2 제네릭 타입

* 다양한 객체를 저장하고 싶어 Object 클래스를 활용했을 때 타입 변환이 일어난다.

  ```java
  public class Box{
    private Object object;
    public void set(Object object){this.object=object;}
    public Object get(){return object;}
  }
  
  Box box=new Box();
  box.set("hello");//자동 타입 변환
  String str=(String)box.get();//강제 타입 변환
  ```

* 제네릭은 실제 클래스가 사용될 때 구체적인 타입을 지정함으로써 타입 변환을 최소화 시킨다.

```java
//타입 변환이 일어나지 않는다->성능 저하를 줄임
public class Box<T>{
  private T t;
  public void set(T t){this.t=t;}
  public T get(return t;)
}

Box<String> box=new Box<String>();//이렇게 선언하면 위의 T가 모두 String으로 변하는 느낌.
box.set("hello");
String str=box.get();

```

## 13.3 멀티 타입 파라미터

* 콤마로 구분

  ```java
  public class Product<T,M>{
    private T kind;
    private M model;
    
    public T getKind(){return this.kind;}
    public M getKind(){return this.model;}
    
    public void setKind(T kind){this.kind=kind;}
    public void setModel(M model){this.model=model;}
  }
  
  //main
  Product<Tv,String> pT=new Product<Tv,String>();//뒷 부분 <>이렇게 표시할 수 있음.
  pT.setKind(new Tv());
  pT.setModel("스마트Tv");
  Tv tv=pT.getKind();
  String tvModel=pT.getModel();
  ```

## 13.4 제네릭 메소드

* 매개 타입과 리턴 타입으로 타입 파라미터를 갖는 메소드, 리턴 타입 앞에 <>기호 추가 및 리턴과 매개를 타입 파라미터로.

* public<T> Box<T> boxing(T t){...}

* 제네릭 메소드 호출방법

  1. 타입 파라미터의 구체적인 타입을 명시적으로 지정

  2. 컴파일러가 매개값의 타입을 보고 구체적인 타입을 추정하도록.

     ```java
     Box<Integer> box=<Integer>boxing(100);//1
     Box<Integer> box=boxing(100);//2
     ```

* 메소드의 파라미터로 제네릭 타입이 오는 경우

  ```java
  //utils 클래스
  public static <K,V> boolean compare(Pair<K,V> p1,Pair<K,V> p2){
          boolean keyCompare=p1.getKey().equals(p2.getKey());
          boolean valueCompare=p1.getValue().equals(p2.getValue());
          return keyCompare&&valueCompare;
      }
  //public class Pair<K,V>{...}
  
  //메인 
  Pair<Integer,String> p1=new Pair<>(1,"사과");
  Pair<Integer,String> p2=new Pair<>(1,"사과");
  boolean result=Utils.compare(p1,p2);
  ```

## 13.5 제한된 타입 파라미터

* 예를 들어 숫자 연산을 하는 제네릭 메소드는 매개값으로 Number 또는 하위 클래스 타입의 인스턴스만 가져야 한다.

* 상위 타입은 클래스 뿐만 아니라 인터페이스도 가능하다.(인터페이스라도 implements를 쓰지 않는다)

  ```java
  //Utils 클래스
  public <T extends Number> int compare(T t1,T t2){
    ...
    return Double.compare(v1,v2);  
  }
  
  //메인
  int result=Utils.compare(10,20);//number로 제한했기 때문에 Utils.compare("a","b"); 안된다
  ```

## 13.6 와일드 카드 타입

* 제네릭 타입을 매개값이나 리턴 값으로 사용할 때 3가지 형태로 와일드 카드를 사용할 수 있다

  1. 제네릭타입<?>: 제한없음, 모든 클래스나 인터페이스 타입이 올 수 있다
  2. 제네릭타입<? extends 상위타입>: ?에는 젤 높아봐야 상위타입까지 올 수 있다(<상위타입).(상위 클래스 제한)
  3. 제네릭타입<? super 하위타입>: ?에는 젤 낮아봐야 하위타입까지 올 수 있다(>하위타입).(하위 클래스 제한)

* cf) 타입 파라미터로 배열 생성 방법

  ```java
  private T[] students;
  students=(T[])(new Object[30]);//new T[n] 형태 안된다.
  ```

## 13.7 제네릭 타입의 상속과 구현

* 제네렉 타입도 상속을 가질 수 있고 자식 제네릭 타입은 더 많은 타입 파라이터를 가질 수 있다.

  ```java
  public class ChildProduct<T,M,C> extends Product<T,M>{..}
  ```

* 제네릭 인터페이스를 구현한 클래스도 제네릭 타입이 된다.

  ```java
  public interface Storage<T>{
    public void add(T item,int index);
    public T get(int index);
  }
  
  public class StorageImpl<T> implements Storage<T>{
    ...
  }
  
  ```

# 14 람다식

## 14.1 람다식이란

* 자바 8이 함수적 프로그래밍을 위해 지원하는 기능.
* 함수적 프로그래밍은 병렬 처리와 이벤트 지향 프로그래밍에 적합하다.
* 익명 함수를 생성하기 위한 식으로 객체 지향 언어보다는 함수 지향 언어에 가깝다.
* 런타임 시 익명 구현 객체를 생성한다.(코드블럭이 익명 구현 객체로 바뀌는 것.)

## 14.2 람다식 기본 문법

* (타입 매개변수, ...)->{실행문;}

  * 매개변수는 오른쪽 중괄호를 실행하기 위해 필요한 값을 제공하는 역할(매개변수 이름은 개발자 자유)

  * -> 기호는 매개변수를 이용해서 중괄호를 실행하겠다는 의미

    ```java
    (int a)->{System.out.println(a);}
    ```

  * 매개 변수 타입은 런타임 시에 대입되는 값에 따라 자동으로 인식되어 생략 가능. 또한 매개 변수가 하나만 있다면 ()생략 가능, 없다면 빈 괄호 반드시 사용해야 함.

    ```java
    a->{System.out.println(a);}
    ```

  * 중괄호 실행 후 결과값을 리턴해야 한다면 return문 작성

    ```java
    (x,y)->{return x+y;}
    ```

  * 만약 리턴문만 있다면 다음과 같이 작성

    ```java
    (x,y)->x+y;
    ```

## 14.3 타겟 타입과 함수적 인터페이스

* 람다식은 인터페이스 변수에 대입된다. 즉, 람다식은 인터페이스의 익명 구현 객체를 생성한다.
* 인터페이스는 구현 클래스가 필요한데 람다식은 익명 구현 클래스를 생성하고 객체화한다.
* 인터페이스에 따라 람다식의 작성법이 달라지므로 람다식이 대입된 인터페이스를 타겟 타입이라 한다.

### 14.3.1 함수적 인터페이스(@FunctionalInterface)

* 함수적 인터페이스: 하나의 추상 메소드가 선언된 인터페이스, 람다식의 타겟 타입이 될 수 있음.(람다식은 하나의 메소드를 정의하기 때문에 여러개 추상 메소드가 선언된 인터페이스는 구현 객체를 생성할 수 없다.)
* @FuntionalInterface: 함수적 인터페이스를 작성할 때 두개 이상의 추상 메소드가 작성되지 않도록 컴파일러가 체킹할 수 있도록 붙이는 어노테이션.

### 14.3.2 매개 변수와 리턴값이 없는 람다식

```java
@FunctionalInterface
public interface A{
  public void method();
}

//main
A fi=()->{...};
fi.method();//{...}를 실행시킨다.
```

### 14.3.3 매개 변수가 있는 람다식

```java
@FunctionalInterface
public interface B{
  public void method(int x);
}

//main
B fi=(x)->{...};
fi.method(5);
```

### 14.3.4 리턴값이 있는 람다식

```java
@FunctionalInterface
public interface C{
  public int method(int x,int y);
}

//main
//x+y;대신 메소드 호출도 가능 ex) return sum(x+y);
C fi=(x,y)->{...return x+y;};//만약 {...}안에 return문밖에 없다면 C fi=(x,y)->x+y;
int result=fi.method(3,5);

```

## 14.4 클래스 멤버와 로컬변수 사용

* 람다식의 실행 블록에는 클래스의 멤버(필드와 메소드) 및 로컬 변수를 사용할 수있다.

### 14.4.1 클래스의 멤버 사용

* 클래스의 멤버인 필드와 메소드를 제약없이 사용할 수 있다

* 하지만 this 사용시 주의 점이 있다.

  * 일반적으로 익명 객체 내부에서 this 사용 시 익명 객체의 참조이지만 람다식에서는 this는 람다식을 실행한 객체의 참조이다.

    ```java
    public class UsingThis{
     public int outterField=10;
    
        class Inner{
            int innerField=20;
    
            void functionA(){
            MyFunctionalInterface fi=()->{
                System.out.println(outterField);
                System.out.println(UsingThis.this.outterField);//바깥 객체의 참조를 얻으려면 class.this 사용
                System.out.println(innerField);
                System.out.println(this.innerField);//inner객체 참조.
            };
            fi.functionA();
            }
    
        }
    }
    
    //main
    UsintThis usingThis=new UsingThis();
    UsingThis.Inner inner=usingThis.new Inner();
    inner.functionA();
    ```

### 14.4.2 로컬 변수 사용

* 람다식에서 바깥 클래스의 필드나 메소드는 제한 없이 사용할 수 있으나 메소드의 매개변수 또는 로컬 변수를 사용하면 이 두변수는 final 특성을 가져야 한다.(람다식 내부에서 읽을 수는 있지만 람다식 내부 또는 외부에서 변경할 수 없다.)
  * 즉 람다식 내부에서 쓰이는 변수는 final의 특성을 갖으니 외부 내부에서 추가 변경할 수 없다.

## 14.5 표준 API의 함수적 인터페이스

* 자바 8부터 빈번하게 사용되는 함수적 인터페이스는 표준 API 패키지로 제공한다.
* 해당 패키지에서 제공하는 함수적 인터페이스의 목적은 메소드 또는 생성자의 **매개 타입으로 사용되어 람다식을 대입**할 수 있도록 하기 위함이다.
* 크게 5가지 종류가 있다: Consumer, Supplier, Function, Operator, Predicate

### 14.5.1 Consumer 함수적 인터페이스

* 매개 값은 있고 리턴값은 없음.

* accept(): 리턴값은 없고 매개값을 소비하는 메소드

  ```java
  //인터페이스 명: Consumer<T>   추상 메소드: void accept(T t)
  Consumer<String> consumer=t->{t를 소비하는 실행문;};//t의 타입은 String
  //인터페이스 명:BiConsumer<T,U> 추상 메소드: void accept(T t,U u)
  BiConsumer<String, String> biconsumer=(t,u)->{t와 u를 소비하는 실행문;};//t,u의 타입은 String
  //인터페이스 명:DoubleConsumer 추상 메소드: void accept(double value)
  DoubleConsumer dconsumer=d->{d를 소비하는 실행문;};
  
  //실제 실행할 때에는 
  comsumer.accept("hello");
  bicomsumer.accept("hello","world");
  dcomsumer.accept(8.0);
  ```

### 14.5.2 Supplier 함수적 인터페이스

* 매개 변수가 없고 리턴값이 있는 getXXX()메소드.(공급=실행 후 호출한 곳으로 데이터를 리턴.)

  ```java
  Supplier<String> supplier=()->{...; return "문자열";};
  IntSupplier intsupplier=()->{...;return int값;}
  
  //실행 할 때
  int num=intsupplier.getAsInt();
  ```

### 14.5.3 Function 함수적 인터페이스

* 매개값과 리턴값이 있는 applyXXX()메소드

* applyXXX() 메소드들은 매개값을 리턴값으로 매핑(타입 변환)하는 역할을 한다.

  ```java
  //인터페이스명: Function<T,R>  추상 메소드: R apply(T t) //T를 R로 매핑
  Function<Student,String> function=t->{return t.getName();}
  //apply(T t)의 리턴값이 R이므로 람다식{}내부의 return 타입은 R.
  ```

  ```java
  //인터페이스명: ToIntFunction<T> 추상 메소드 int applyAsInt(T t) //t를 int로 매핑
  ToIntFunction<Student> function=t->{return t.getScore();}
  ```

  ```java
  //실행할 때
  public static void printString(Function<Student,String> function){
    for(Student student:list){
    System.out.println(function.apply(student));//여기서 람다식 실행
    }
  }
  //main
  printString(t->t.getName());//printString 호출, 이때 람다식이 실행되는건 아님.
  ```

### 14.5.4 Operator 함수적 인터페이스

* Function과 동일하게 매개 변수와 리턴값이 있는 applyXXX()메소드를 가지고 있음, 단 리턴값으로 매핑하는 역할보다 매개값을 이용해서 연산을 수행한 후 동일한 타입으로 리턴값을 제공하는 역할.

  ```java
  //인터페이스명: IntBinaryOperator  //추상 메서드: int applyAsInt(int,int)
  IntBinaryOperator operator=(a,b)->{...; return int값;}
  
  //실제 사용
  public static int maxOrMin(IntBinaryOperator operator){
    int result=score[0];
    for(int score:scores){
      result=operator.applyAsInt(result,score);
    }
    return result;
  }
  
  //main
  int max=maxOrMin((a,b)->{
    if(a>b)return a;
    else return b;
     }
  );
  ```

### 14.5.5 Predicate 함수적 인터페이스

* 매개 변수와 boolean 리턴값이 있는 testXXX()메소드를 가지고 있다. 매개값을 조사해서 boolean을 리턴

  ```java
  Predicate<Student> predicate=t->{return t.getSex().equals("남자");}
  
  //실제 사용
  public static double avg(Predicate<Student> predicate){
    int count=0,sum=0;
    for(Student student:list){
      if(predicate.test(student)){
        ...
      }
    }
  }
  
  //main
  double maleAvg=avg(t->t.getSex().equals("남자"));
  ```

### 14.5.6 andThen()과 compose() 디폴트 메소드

* Consumer, Function, Operator는 andThen(), compose() 디폴트 메소드를 가지고 있다.

* 두 디폴트 메소드는 함수적 인터페이스를 연결하고 첫번째 처리 결과를 두번째 매개값으로 제공해서 최종 결과값을 얻을 때 사용한다.

* andThen(): 앞의 처리 후 뒤에로 넘긴 후 뒤에거 처리.

* Compose():  뒤에꺼 처리 후 앞에에게 넘긴 후 앞에 처리.

* Comsumer의 순차적 연결

  * consumer는 리턴이 없기 때문에 여기서 andThen()은 함수적 인터페이스의 호출 순서만 정한다.

    ```java
    Comsumer<Member> comsumerA=(m)->{
      System.out.println(m.getName());
    };
    
    Comsumer<Member> comsumerB=(m)->{
      System.out.println(m.getId());
    };
    
    Consumer<Member> cousumerAB=consumerA.andThen(consumerB);
    consymerAB.accept(new Member("홍길동","hong",null));
    ```

* Function의 순차적 연결

  ```java
  Function<Member,Address> fuctionA;
  Function<Address,String> fuctionB;
  Function<Member,String> fuctionAB;
  String city;
  functionA=(m)->m.getAddress();
  functionB=(a)->a.getCity();
  functionAB=functionA.andThen(functionB);
  
  city=functionAB.apply(
    new Member("홍길동",'hong',new Address("한국","서울"))
  );
  ```

### 14.5.7 and(), or(), negate() 디폴트 메소드와 isEqual() 정적 메소드

* predicate는 and(),or(),negate() 디폴트 메소드를 가지고 있다.(&&,||,!)

  ```java
  IntPredicate predicateA=a->a%2==0;
  IntPredicate predicateB=(a)->a%3==0;
  IntPredicate predicateAB;
  boolean result;
  
  predicateAB=predicateA.and(predicateB);
  result=predicateAB.test(9);
  predicateAB=predicateA.or(predicateB);
  result=predicateAB.test(9);
  predicateAB=predicateA.negate();
  result=predicateAB.test(9);
  ```

* isEqual(): 정적 메소드

  ```java
  //targetObject, sourceObject 동등비교.
  Predicate<Object> predicate=Predicate.isEqual(targetObject);
  boolean result=predicate.test(sourceObject);
  ```

### 14.5.8 minBy(),maxBy() 정적 메소드

* BinaryOperator<T> 함수적 인터페이스는 minBy(),maxBy() 정적 메소드를 제공한다.

* 매개값으로 제공되는 Comparator를 이용해서 최대 T와 최소 T를 얻는 BinaryOperator<T>를 리턴.

  ```java
  BinaryOperator<Fruit> binaryOperator;
  Fruit fruit;
  
  binaryOperator=BinaryOperator.minBy((f1,f2)->Integer.compare(f1.price,f2.price));
  fruit=binaryOperator.apply(new Fruit("딸기",6000),new Fruit("수박",10000));
  System.out.println(fruit.name);//딸기.
  ```

## 14.6 메소드 참조

* 메소드를 참조해서 매개 변수의 정보 및 리턴타입을 알아내어 람다식에서 불필요한 매개변수를 제거하는 것이 목적.

  ```java
  (left,right)->Math.max(left,right);//메소드 참조 안하면
  Math::max;//메소드 참조하면
  ```

* 메소드 참조도 람다식과 마찬가지고 인터페이스의 익명 구현 객체로 생성된다.(타겟 타입인 인터페이스의 추상 메소드가 어떤 매개 변수를 가지고, 리턴 타입이 무엇인가에 따라 달라짐.)

### 14.6.1 정적 메소드와 인스턴스 메소드 참조

* 정적 메소드 참조: 클래스::메소드
* 인스턴스 메소드 참조: (객체 생성 후) 참조 변수::메소드

### 14.6.2 매개 변수의 메소드 참조

* 람다식에서 제공되는 매개변수의 메소드를 호출하거나 매개변수를 매개값으로 사용하는 경우도 있다.

* a의 클래스 이름:: 메소드 이름.

  ```java
  (a,b)->{a.MethodSomething(b);}//메소드 참조 안하면
  클래스:: MethodSomething //메소드 참조 하면, 정적 메소드 참조와 비슷해 보이지만 a의 인스턴스 메소드가 참조되므로 전혀 다른 코드임.
  ```

  ```java
  ToIntBiFunction<String,String> function;
  function=(a,b)->a.compareToIgnoreCase(b);//메소드 참조 하면 String::compareToIgnoreCase;
  System.out.println(function.applyAsInt("Java8","JAVA8"));
  ```

### 14.6.3 생성자 참조

* 단순히 객체를 생성하고 리턴하도록 구성된 람다식은 생성자 참조로 대치할 수 있다.

  ```java
  (a,b)=>{return new 클래스(a,b);}//생성자 참조 안하면
  클래스::new//생성자 참조 하면, 생성자 오버로딩되어 여러개 있을 경우 컴파일러는 함수적 인터페이스의 추상메소드와 동일한 매개 변수 타입과 개수를 가지고 있는 생성자를 찾아 실행한다.없으면 컴파일 에러.
  ```

  ```java
  Function<String, Member> function1=Member::new;//생성자 참조
  Member member1=function1.apply("angel");
  
  BiFunction<String, String, Member> function2=Member::new;//생성자 참조
  Member member2=function2.apply("천사","angel");
  //두개의 Member::new는 다른 생성자를 호출한다.
  
  
  ```


# 15. 컬렉션 프레임워크

## 15.1 컬렉션 프레임워크 소개

* 배열의 문제점을 해결하고 알려져 있는 자료구조를 바탕으로 객체들을 효율적으로 추가, 삭제, 검색할 수 있도록 java.util 패키지에 컬랙션과 관련된 인터페이스와 클래스들을 포함시켜 놓은 것.
* 컬렉션 프레임워크의 주요 인터페이스와 해당 인터페이스로 사용 가능한 컬렉션 클래스(구현 클래스)
  * List: ArrayList, Vector, LinkedList
  * Set: HashSet, TreeSet
  * Map: HashMap, HashTable, TreeMap, Properties.

## 15.2 List 컬렉션

```java
List<String> list=...;
list.add("홍길동");
list.add(1,"홍길동");
String str=list.get(1);
list.remove(0);
```

### 15.2.1 ArrayList

* 리스트 인터페이스의 구현클래스로, ArrayList에 객체를 추가하면 객체가 인덱스로 관리됨.

* 일반 배열과 다르게 저장 용량이 필요시 자동적으로 늘어남.

  ```java
  List<String> list=new ArrayList<String>();//디폴트 10개, 초기값 늘리고 싶다면 (30);이런식으로
  ```

* 특정 인덱스의 객체 삭제 시 뒤에꺼 하나씩 당겨진다

* 특정 인덱스에 객체 삽입 시 뒤로 1칸씩 밀린다.

* 고정된 객체들로 구성된 List를 생성할 때

  ```java
  List<String> list=Arrays.asList("홍길동","강병현","김자바");
  ```

## 15.2.2 Vector

* ArrayList와 다른 점은 Vector는 동기화된 메소드로 구성되어 있기 때문에 멀티 스레드가 동시에 이 메소드들을 실행할 수 없고, 하나의 스레드가 실행을 완료 해야 다른 스레드를 실행할 수 있다.(Thread safe)

  ```java
  List<Board> list=new Vector<Board>();
  list.add(new board("a","b","c"));
  ```

### 15.2.3 LinkedList

* ```java
  List<String> list2=new LinkedList<String>();
  ```

## 15.3 Set 컬렉션

* 인덱스로 객체를 가져오는 메소드는 없으므로 반복자(Iterator 인터페이스를 구현한 객체)를 이용한다

  ```java
  Set<String> set=...;
  Iterator<String> iterator=set.iterator();
  while(iterator.hasNext()){
    String str=iterator.next();
  }
  
  //또는 반복자 사용하지 않고 향상된 for문 이용할 수도 있다
  Set<String> set=...;
  for(String str:set){
    ...
  }
  ```

### 15.3.1 HashSet

* set인터페이스의 구현 클래스.

  ```java
  Set<String> set=new HashSet<String>();
  ```

* HashSet은 객체를 저장하기 전에 먼저 객체의 hashCode()메소드를 호출해서 해시코드를 얻어낸다. 이미 저장되어 있는 객체들의 해시코드와 비교해서 같으면 equls()메소드로 두 객체를 비교해서 true가 나오면 그때 동일한 객체로 판단하고 중복저장을 하지 않는다.

  ```java
  //이건 객체를 키로 할 경우
  Set<Member> set= new HashSet<>();
  set.add(new Member("홍길동",30));
  set.add(new Member("홍길동",30));
  System.out.println(set.size());//2
  //따라서 의도한 대로 하려면 equals(), hashCode() 오버라이딩 해서 중복 저장 안되게 해야한다.
  @Override
  public boolean equals(Object obj){
    if(obj instanceof Member){
      Member member=(Member)obj;
      return member.name.equals(name)&&(member.age==age);
    }else return false;
  }
  
  @Override
  public int hashCode(){
    return new.hashCode()+age;
  }
  
  ```

## 15.4 Map 컬렉션

* ```java
  Map<String,Integer> map=...;
  map.put("홍길동",30);
  int score=map.get("홍길동");
  map.remove("홍길동");
  ```

* 저장된 전체 객체를 대상으로 하나씩 얻고 싶을 경우

  1. KeySet()이용: 모든 키를 Set 컬렉션으로 얻은 다음 반복자를 통해서 키를 얻고 get으로 조회

     ```java
     Map<K,V> map=~;
     Set<K> keySet=map.keySet();
     Iterator<K> keyIterator=keySet.iterator();
     whike(keyIterator.hasNext()){
       K key=keyIterator.next();
       V value=map.get(key);
     }
     ```

  2. entrySet()메소드: Map.Entry를 Set 컬렉션으로 얻은 다음 반복자를 통해서 Map.Entry를 하나씩 얻고, getKey, getValue로 값들 조회

     ```java
     Set<Map.Entry<K,V>> entrySet=map.entrySet();
     Iterator<Map,Entry<K,V>> entryIterator=entrySet.iterator();
     while(entryIterator.hasNext()){
       Map.entry<K,V> entry=entryIterator.next();
       K key=entry.getKey();
       V value=entry.getValue();
     }
     ```

### 15.4.1 HashMap

* 동일 키가 될 조건은 hashCode()의 리턴값이 같고, equals()메소드가 true를 리턴해야 한다.

* 키와 값의 타입은 기본 타입을 사용할 수 없고 클래스 및 인터페이스 타입만 가능하다.

  ```java
  Map<String,Integer> map=new HashMap<String,Integer>();
  ```

### 15.4.2 HashTable

* HashMap과의 차이점은 HashTable은 동기화된 메소드로 구성되어 있기 때문에 멀티 스레드가 동시에 이 메소드들을 실행할 수 없고, 하나의 스레드가 실행을 완료해야만 다른 스레드를 실행할 수 있다(Thread Safe)

  ```java
  Map<String, Integer> map=new HashTable<String, Integer>();
  ```

### 15.4.3 Properties

* HashTable의 하위 클래스.

* 키와 값을 String 타입으로 제한한 컬렉션.

* 애플리케이션의 옵션정보, DB연결정도, 다국어 정보가 저장된 프로퍼티 파일(~.properties)읽을 때 주로 사용.

  ```java
  Properties properties=new Properties();
  properties.load(new FileReader("프로퍼티 파일 경로"));
  //cf. class.getResourc(파일): 파일의 상대 경로를 URL 객체로 리턴
  //getPath(): 파일의 절대경로 리턴.
  String path=클래스.class.getResource("프로퍼티 파일 명").getPath();
  Properties properties=new Properties();
  properties.load(new FileReader(path));
  String driver=properties.getProperty("driver");//해당 키의 값 읽기.
  ```

## 15.5 검색 기능을 강화시킨 컬렉션

* TreeSet, TreeMap: 이진 트리 이용해서 계층적 구조(tree 구조)를 가지면서 객체를 저장한다.

### 15.5.1 이진 트리 구조

* 이진 트리가 범위 검색을 쉽게 할 수 있는 이유는 값들이 정렬되어 있어 그룹핑이 쉽기 때문이다.

### 15.5.2 TreeSet

* 이진 트리를 기반으로 한 Set 컬렉션.

* 하나의 노드는 노드값인 **Value**와 왼쪽 오른쪽 자식 노드를 참조하기 위한 두개의 변수로 구성.

  ```java
  TreeSet<String> treeSet=new TreeSet<String>();
  //Set 인터페이스 타입 변수에 대입해도 되긴 하지만 이렇게 한 이유는 TreeSet 검색 메소드 사용 위해.
  ```

* 정렬과 관련된 메소드.

  * descendingIterator(): 내림차순으로 정렬된 Iterator 객체를 리턴
  * descendingSet(): 내림차순으로 정렬된 NavigableSet 객체를 리턴.
    * NavigableSet은 TreeSet의 메소드 뿐만 아니라 descendingSet() 메소드도 제공

  ```java
  NavigableSet<E> descendingSet=treeSet.descendingSet();
  NavigableSet<E> ascendingSet=descendingSet.descendingSet();//두번 호출하면 오름차순.
  ```

* 범위 검색 메소드: headSet, tailSet, subSet.

### 15.5.3 TreeMap

* TreeSet과 차이점은 키와 값이 저장된 Map.Entry를 저장한다는 점.(한 노드에 좌우 자식 참조 변수, **Map.Entry**)

* TreeMap에 객체를 저장하면 자동 정렬된다.(tree니까..)

  ```java
  TreeMap<String, Integer> treeMap=new TreeMap<String,Integer>();
  //map 인터페이스 타입 변수에 대입해도 되긴 하지만 TreeMap 검색 메소드 사용 위해.
  ```

* 정렬과 관련된 메소드

  * decendingKeySet(): 내림차순으로 정렬된 키의 NavigableSet객체 리턴.
  * decendingMap(): 내림차순으로 정렬된 NavigableMap 객체를 리턴, 이하 설명은 TreeSet과 같은 패턴.

  ```java
  NavigableMap<K,V> descendingMap=treeMap.descendingMap();
  NavigableMap<K,V> ascendingMap=descendingMap.descendingMap();
  ```

* 범위 검색 메소드: headMap, tailMap,subMap

### 15.5.4 Comparable과 Comparator

* Integer, String 은 모두 Comparable 인터페이스를 구현하고 있다.

* 사용자 정의 클래스도 Comparable를 구현한다면 자동 정렬이 가능하다.(compareTo메소드를 오버라이드 해야 한다.)

  * compareTo 리턴 타입은 int

  ```java
  public class Person implements Comparable<Person>{
    //필드 생성자...
    @Override
    public int comparableTo(Person o){
      if(age<o.age)return -1;
      else if(age==o.age)return 0;
      else return 1;
    }
  }
  ```

* Comparable 비구현 객체를 정렬하는 방법: TreeSet 또는 TreeMap의 생성자의 매개값으로 정렬자(Comparator)를 제공하면 된다.

  * 정렬자: Comparator 인터페이스를 구현한 객체.(compareTo메소드가 정의되어 있음.)

  ```java
  TreeSet<E> treeSet=new TreeSet<E>(new AscendingComparator());
  ```

  ```java
  //Fruit class
  public class fruit{
    //필드와 생성자., comparable구현은 없다
  }
  
  //DescendingComparator class
  public class DescendingComparator implements Comparator<Fruit>{
    @Override
    public int compareTo(Fruit o1,Fruit o2){
      if(o1.price<o2.price)return 1;
      else if(o1.price==o2.price)return 0;
      else return -1;
    }
  }
  
  //main
  TreeSet<Fruit> treeset=new TreeSet<Friut>(new DescendingComparator());
  ```

## 15.6 LIFO와 FIFO 컬렉션

* 스택이용한 예: JVM 스택 메모기
* 큐 예: 스레드풀(ExecutorService)의 작업 큐.

### 15.6.1 Stack

```java
Stack<E> stack=new Stack<E>();
```

### 15.6.2 Queue

* 링크드리스트는 queue인터페이스를 구현한 대표적인 클래스.
  * 링크드리스트는 list 인터페이스를 구현했기 때문에 List 컬렉션이기도 함.

```java
Queue<E> queue=new LinkedList<E>();
```

## 15.7 동기화된 컬렉션

* Vector, HashTable은 ThreadSafe하지만 ArrayList, HashSet, HashMap은 안전하지 않다.

* 그런 메소드들은 멀티 쓰레드 환경에서 안전하지 않은데, 멀티 쓰레드 환경으로 전달할 필요도 있을 것.

* 컬렉션 프레임워크는 비동기화된 메소드를 동기화된 메소드로 래핑하는 Collections의 synchronizedXXX() 메소드를 제공하고 있다.

  ```java
  List<T> list=Collections.synchronizedList(new ArrayList<T>());//리스트를 동기화된 리스트로
  Set<E> set=Collections.synchronizedSet(new HashSet<E>());//맵을 동기화된 맵으로
  Map<K,V> map=Collections.synchronizedMap(new HashMap<K,V>());//set을 동기화된 set으로
  ```

## 15.8 병렬 처리를 위한 컬렉션

* 동기화된 컬렉션은 멀티 스레드 환경에서 하나의 thread가 안전하게 처리하도록(다른 스레드 잠금) 도와주지만 빠르지 못함 

* 따라서 병렬처리 가능하기 하는 컬렉션인 java.util.concurrent 패키지의 ConcurrentHashMap,

  ConcurrentLinkedqueue이다. 

* concurrentHashMap은 부분잠금 활용(처리하는 요소가 포함된 부분만 잠금, 나머지는 다른 스레드가 변경할 수 있도록)

```java
Map<K,V> map=new ConcurrentHashMap<K,V>();
```

* comcurrentLinkedQueue는 락-프리 알고리즘을 구현한 컬렉션.

  ```java
  Queue<E> queue=new ConcurrentLinkedQueue<E>();
  ```

  
















