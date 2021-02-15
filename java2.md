# 0 데이터 타입의 분류(이것이 자바다)

* 기본타입(primitive type): 직접 값을 스택에 저장
* 참조 타입(referencer type): 힙 영역의 객체를 주소를 통한 참조.(스택에는 변수와 참조하는 객체의 주소값을 ,힙에는 실제 값(객체))
  * 객체를 참조하는 배열은 변수만 stack에 있고 힙에는 배열의 각 값의 주소를 가진 배열객체와 실제 값을 가진 객체로 이루어짐. 

## null

* null의 의미는 참조 타입 변수가 힙 영역의 객체를 참조하고 있지 않다는 뜻. 이럴때 무엇인가 하려고 한다면 npe
* null을 대입한다면 참조를 잃은 해당 변수는 JVM의 GC를 구동시켜 heap 메모리에서 객체를 제거한다.

## new 연산자

* 객체 생성시 사용.

  ```java
  String a="tmon";
          String b="tmon";
          String c=new String();
          c="tmon";
          String d = new String("tmon");
  
          System.out.println(a==b);//true
          System.out.println(a==c);//true
          System.out.println(a==d);//false
          System.out.println(c==d);//false
          System.out.println(a.equals(b));//아래까지 다 트루
          System.out.println(a.equals(c));
          System.out.println(c.equals(d));
  ```

## 배열

```java
String[] stringA={"a","b","c"};
String stringB[]={"a","b","c"};
//위와 같이 두가지 방법으로 선언할 수 있다.

 String[] stringC;
//stringC={"b"};//에러, 배열변수를 이미 선언한 뒤 이런방식으로 초기화 할 수 없다.
stringC=new String[]{"b"};//이런식으로 New를 사용해야 한다.

String[] stringD=new String[5];//이렇게 선언시 아래와 같이 가능, 단 길이 지정 해야함.
stringD[3]="a";
```

## 객체 지향 프로그래밍

* 자바는 속성과 동작을 각각 필드와 메소드로 부른다.
* 캡슐화: 객체와 필드를 하나로 묶고 외부에서는 알지 못하게 감추는 것.외부의 사용자가 값을 함부로 변경하지 못하도록 하기 위함. 노출의 여부는 접근 제한자를 활용
* 상속: 부모의 필드와 메소드를 자식도 사용할 수 있다.
* 다형성: 인터페이스.
* 인스턴스: 클래스로부터 만들어진 객체를 해당 클래스의 인스턴스라 한다.
* OOP의 단계: 클래스 생성->객체생성->객체 이용

## Class

* 한 소스파일 안에 여러개의 클래스를 생성하는 경우 파일 이름과 같은 클래스에만 public을 붙일 수 있다.(한개의 파일에 한개의 클래스를 설정하는 것이 좋다)

  ```java
  public class Student{
    
    int fieldName;//필드, 변수와 달리 생성자와 메소드 전체에서 사용되며 객체가 소멸하지 않는 한 객체와 함께 존재한다. 생성자나 메소드 내부에 선언될 수 없다.(그런 게 있다면 그것은 지역변수다)
    Student(){//생성자, 객체 초기화 담당, 리턴타입이 없음. 클래스로부터 객체가 생성될 때 호출
      ...
    }
    void method()//메소드
    
  }
  
  //메인소스
  Student s1=new Student();//뒤의 student()는 생성자를 의미하며 new 연산자로 인해 객체 생성 및 객체의 주소를 리턴한다.
  ```

* 외부에서 해당 클래스의 필드를 사용하기 위해서는 해당 클래스로 부터 객체를 생성해야 한다

  ```java
  public class Car{
    int speed;
  }
  
  public class Person{
    Car car=new Car();
    car.speed=10;
  }
  ```

### 생성자

* 명시적으로 생성자가 선언된 경우 기본생성자를 사용할 수 없다.

* This.필드=매개변수

* 생성자 오버로딩: 매개 변수의 타입과 개수, 순서를 통해 생성자를 다양하게 하는 것.

  ```java
  Car(String model){
    this.model=model;
    this.color="은색";
    this.maxspeed=250;
  }
  
  Car(String model, String color){
    this.model=model;
    this.color=color;
    this.maxspeed=250;
  }
  Car(String model, String color,int maxspeed){
    this.model=model;
    this.color=color;
    this.maxspeed=maxspeed;
  }
  //이런식으로 각 생성자 내용이 비슷할 경우 아래와 같이 this()를 이용하여 간단하게 할 수 있다
  
  Car(String model){
    this(model,"은색",250);
  }
  
  Car(String model,String color){
    this(model,color,250);
  }
  
  Car(String model, String color,int maxspeed){
    this.model=model;
    this.color=color;
    this.maxspeed=maxspeed;
  }
  
  ```

## 메소드

* 외부 클래스의 메소드를 호출하기 위해서는 해당 외부 클래스 객체를 생성하고 객체 변수명.메소드 방식으로 활용해야 한다.

* 메소드 오버로딩

  * 조건: 매개 변수의 타입, 개수, 순서 중 하나가 달라야 한다.(이름은 같고, 리턴타입은 상관 없음.)

    ```java
    //다음과 같은 경우는 메소드 오버로딩이 아니다. 컴파일 에러 발생한다
    int divide(int x,int y);
    double divide(int a,int b);
    ```

    

### 필드 초기화

* 생성자를 통한 초기화(공통적인 내용이 필요할 때)
* 초기 필드 선언 시 초기화(디양한 값드로 초기화 되어야 할 때)

### 접근 제한자

* public: 공개 멤버를 만든다

* protected: 같은 패키지 또는 자식 클래스에서 사용할 수 있는 멤버를 만든다.

* private: 외부에 노출되지 않는 멤버를 만든다

* default: 위 세가지 외. 같은 패키지에 소속된 클래스에서만 사용할 수 있는 멤버를 만든다.

* 클래스의 접근 제한.(public, default)

  * 클래스 선언 시 고려사항: 같은 패키지 내에서만 사용할 것인지, 다른 패키지에서도 사용할 수 있게 할 것인지 결정.

    ```java
    class a{...}//default
    public class a{...}//public
    ```

* 생성자의 접근 제한

  * 객체를 생성하기 위해서는 new연산자를 통해 생성자를 호출해야 한다. 접근 제한에 따라 호출 가능 여부가 결정.
  * 생성자도 위와 같이 4개의 접근제한자를 가진다.
  * 생성자를 선언하지 않으면 클래스의 접근 제한자와 같은 접근 제한자를 가진 생성자를 가진다.
  * public: public과 같은데 만약 클래스가 default이고 생성자를 public으로 선언한다 해도 같은 패키지에서만 생성자를 호출할 수 있다
  * protected: default 접근 제한과 같이 같은 패키지에 속하는 클래스에서 생성자를 호출할 수 있지만 다른 패키지에 속한 클래스가 해당 클래스의 자식 클래스라면 호출할 수 있다(결국 protected와 같다)
  * default: 생성자 선언지 아무 접근 제한자도 설정하지 않았다면 default.
  * private: 오로지 클래스 내부에서만 생성자를 호출할 수있고 객체를 만들 수 있다.

* 필드와 메소드의 접근 제한

  * 클래스 내부, 패키지 내부, 다른 패키지에서도 사용 가능 등의 여부를 결정해야 한다
  * public: 필드와 메소드가 public접근 제한을 가질 경우 클래스도 public 접근 제한을 가져야 한다
  * 나머지 위와 동일.

### getter와 setter 메소드

* 자바는 무결성을 위해 메소드를 총한 데이터 변경 방법을 선호한다.

* 클래스 선언시 가능하면 필드를 private으로 선언해서 외부로부터 보호하고, getter,setter를 이용해서 필드값을 변경/사용하는 것이 좋다.

  ```java
  private int num;
      
  public int getNum() {
    return num;
  }
  
  public void setNum(int num) {
    this.num = num;
  }
  ```

### annotation

* 메타데이터, 즉 컴파일 과정과 실행 과정에서 코드를 어떻게 컴파일하고 처리할 것인지 알려주는 정보

* 어노테이션이 용도

  * 컴파일러에게 코드 문법 에러를 체크하도록 정보 제공

     ex)@Override: 정확히 오버라이드가 되지 않았다면 컴파일러는 에러를 발생시킴

  * SW개발 툴이 빌드나 배치 시 코드를 자동으로 생성할 수 있도록 정보를 제공

  * 실행 시(런타임 시)특정 기능을 실행하도록 정보를 제공

* 어노테이션 타입 정의와 적용

  ```java
  public @interface annotationName{
    String elementName1();//이런식으로 엘리먼트를 가질 수 있음, 단 메소드 처럼 ()를 붙여야 함
    int elementName2() default 5;
  }
  //사용은 @annotationName(elementName1="값",elementName2=3)//디폴트값이 있는 경우 생략 가능
  ```

  ```java
  public @interface aName{
    String value();//기본 엘리먼트
    
  }
  
  //기본 엘리먼트를 가진 어노테이션을 사용할 경우 @aName("값") 이런식으로 사용 가능
  //다른 엘리먼트가 있다면 @aName(value="값", elementName3=33)//이런식으로
  ```

* 어노테이션 적용 대상

  * TYPE: 클래스, 인터페이스, 열거타입

  * ANNOTATION_TYPE: 어노테이션

  * FILED,CONSTRUCTOR,METHOD...등등.

  * @Target 활용해서 적용될 대상 지정, ElementType배열 활용(적용될 대상을 여러개 지정하기 위해)

    ```java
    @Target(ElementType.TYPE,ElementType.FIELD)
    public @interface aName(){
      
    }
    ```

* 어노테이션 유지 정책

  * 어느 범위까지 유지할 것인지

  * @Retention 어노테이션과 RetentionPolicy를 활용

  * SOURCE: 소스상에서만 유지, 바이트 고드 파일에는 정보가 남지 않는다

  * CLASS: 바이트 코드 파일까지 어노테이션 정보를 유지, 하지만 리플렉션을 이용해서 어노테이션 정보를 얻을 수는 없다

  * RUNTIME: 바이트 코드 파일까지 유지 및 리플렉션을 이용해 런타입 시에 어노테이션 정보를 얻을 수있다.

    * 리플렉션: 런타임 시 클래스의 메타 정보를 얻는 기능. ex) 클래스가 가진 필드, 생성자, 메소드등의 정보

    ```java
    @Target(ElementType.TYPE,ElementType.FIELD)
    @Retention(RetentionPolicy.RUNTIME)
    public @interface aName(){
      
    }
    ```

* 런타임 시 어노테이션 정보 사용하기

  * 어노테이션 자체는 아무런 동작을 하지 않는 표식이지만 리플렉션을 이용하면 어노테이션의 적용 여부 및 엘리먼트 값을 읽고 처리할 수 있다.
  * 클래스에 적용된 어노테이션 정보를 얻으려면 java.lang.Class를 사용하면 된다
  * 필드, 생성자, 메소드에 적용된 어노테이션 정보를 얻으려면 getFileds(),getConstructors(), getDeclaredMethods() 메소드를 통해 java.lang.reflect 패키지의 filed,constructor,method 타입의 배열을 얻어야 한다.
  * 그 다음 class, field,contructor,method가 가지고 있는 아래와 같은 메소드를 호출해서 적용된 어노테이션 정보를 얻을 수 있다.
    * isAnnotaionPresent(어노테이션 클래스.class): 지정한 어노테이셔이 적용되었는지 여부
    * getAnnotation(어노테이션 클래스.class): 지정한 어노테이션이 적용되어 있으면 어노테이션을 리턴, 아니면 null리턴.
    * getAnnotaions(): 적용된 모든 어노테이션 리턴
    * getDeclaredAnnotations(): 직접 적용된 모든 어노테이션을 리턴.

  ```java
  //어노테이션 클래스
  @Target(ElementType.METHOD)
  @Retention(RetentionPolicy.RUNTIME)
  public @interface aName {
      String value() default "-";
      int number() default 15;
  }
  
  //어노테이션 적용 클래스
  public class service {
      @aName
      public void method1(){
          System.out.println("실행 내용 1");
      }
      @aName("*")
      public void method2(){
          System.out.println("실행 내용 2");
      }
      @aName(value="#",number = 20)
      public void method3(){
          System.out.println("실행 내용 3");
      }
  }
  
  
  //리플렉션 테스트
  public class annotationTestApp {
      public static void main(String[] args) {
  
          //service 클래스로 부터 메소드 정보를 얻음(리플렉션)
          Method[] declaredMethods = service.class.getDeclaredMethods();
          for (Method method : declaredMethods) {
              if(method.isAnnotationPresent(aName.class)){
                  System.out.println(method.getName());
                  aName aannotation = method.getAnnotation(aName.class);
                  for(int i=0;i<aannotation.number();i++){
                      System.out.print(aannotation.value());
                  }
                  System.out.println();
  
                  try {
                      method.invoke(new service());//객체를 생성해서 생성된 객체의 메소드를 호출.
                      //이거 없으면 method실행이 안됨.(여기선 프린트문 출력 안됨)
                  } catch (Exception e) {
                      e.printStackTrace();
                  }
              }
          }
  
      }
  }
  ```

  

## 1.4 산술연산

### 1.4.6 큰 숫자

* 기본 정수 타입이나 부동 소수점 타입의 정밀도로 충분하지 않을 때 java.math패키지의 BigInteger, BigDecimal 클래스 이용(해당 클래스의 객체는 임의 길이로 연속된 수를 나타냄.)

  Ex) BigInteger n= new BigInteger("912392193219319239129");

  Ex) BigInteger n=BigInteger.valueOf(8129214912481249L);

* 객체에 연산자를 사용할 수 없으므로 큰 숫자를 다룰 때는 반드시 메서드를 호출해야 한다.

  ```java
  BigInteger r=BigInteger.valueOf(5).multiply(n.add(k));//r=5*(n+k)
  ```

## 1.5 문자열

### 1.5.1 문자열 연결

* 문자열 연결 시 + 연산자 이용.(문자열이 아닌값과 +연산 이용시 문자열이 된다)

* 여러 문자열을 구분자로 구분해서 결합할 때는 join 메서드를 사용한다

  ```java
  String names=String.join(",","kim","park");
  ```

* 혹은 StringBuilder를 이용

  ```java
  StringBuilder builder=new StringBuilder();
  while(문자열이 있으면){
    builder.append(다음 문자열);
  }
  String result=builder.toString();
  ```

### 1.5.2 부분 문자열

* 문자열 분리는 substring이 구분자 분리는 split이 cf)정규표현식

### 1.5.3 문자열 비교

* Equals 메소드 사용(==연산자는 동일 객체시 비교 가능)

### 1.5.4 숫자와 문자열 사이의 변환

* 정수->문자열: Integer.toString

  ```java
  int n=42;
  String str=Integer.toString(n);
  ```

* 정수를 담고있는 문자열-> 정수: Integer.parseInt메소드 활용

  ```java
  String str="101010";
  int n=Integer.parseInt(str);
  ```

 ### 1.5.6 코드 포인트와 코드 유닛

* 코드 포인트: 유효한 유니코드(21비트) 값 cf) 스트림.



## 1.6 입력과 출력

### 1.6.1 입력 읽어 오기

```java
Scanner in=new Scanner(System.in);

String name=in.nextLine();//한줄 읽기
Sting firstName=in.next(); //구분된 단어 읽기
int age=in.nextInt(); //정수 읽기
```

* 비밀번호는 콘솔읽기 활용

  ```java
  Console terminal=System.console();
  String username=terminal.readLine("user name: ");
  char[] passwd=terminal.readPassword("pwd: ");
  ```

### 1.6.2 포맷 적용 출력

* 숫자의 개수를 제한한 출력

  ``` Java
  System.out.printf("%8.2f",1000.0/3.0);//필드 폭 총 8자리, 정밀도는 2자리,,,333.33
  ```

* 매겨변수 지정

  ```java
  System.out.printf("hello %s",name);
  ```

## 1.7 제어 흐름

### 1.7.2 루프

```java
for(int i=0,j=n-1;i<j;i++,j--)//이런거 가능
for(;;)//무한 루프
```

## 1.8 배열과 배열 리스트

### 1.8.1 배열 다루기

```java
String[] names=new String[100];
```

* 배열 길이: array.length

### 1.8.2 배열 생성

* new 연산자로 배열을 선언하면 배열을 기본 값으로 채운다

  * 숫자는 0, 불린은 false, 객체 배열은 null 참조

* 원하는 값을 알경우

  ```java
  int[] a={2,3,4};
  ```

### 1.8.3 배열 리스트

* 자바는 배열을 생성하면 길이를 변경할 수 없기 때문에 java.util 패키지의 ArrayList 클래스를 활용하여 배열을 관리할 수 있다.(생성한 만큼,)

* 배열과 배열리스트 문법은 다르다!! 

  ```java
  ArrayList<String> friends;//제너릭클래스
  friends=new ArrayList<>();//또는 new ArrayList<String>();
  
  ArrayList<String> friends=new ArrayList<>(List.of("Peter","Paul"));
  
  
  friends.remove(1);
  friends.add(0,"Paul");
  friends.set(1,"Mary");
  String first=friends.get(0);//String 객체 반환
  ```

### 1.8.4 기본 타입의 래퍼 클래스

*  제너릭 클래스는 기본 타입을 타입 매개변수로 활용할 수 없으므로 래퍼(wrapper) 클래스를 활용한다(오토박싱,언박싱)

  ```java
  ArrayList<int> //X
  ```

### 1.8.5 향상된 for 루프

```java
int sum=0;
for(int n:numbers){
  sum+=n;
}//n=numbers[0],n=nubers[1],,,,
```

### 1.8.6 배열과 배열 리스트 복사

```java
//같은 배열 참조
int[] numbers=primes;
numbers[5]=42;//prime[5]도 42가 된다.

//공유하지 않는 배열 사본
int[] copiedPrimes=Array.copyOf(primes,primes.length);

//배열 리스트
ArrayList<String> people =friends;
people.set(0,"Mary");//friend.get(0)도 메리다

//진짜 복사
ArrayList<String> copiedFriends=new ArrayList<>(friends);
  
//배열을 배열 리스트에 복사
String[] names="asd";
ArrayList<String> friends=new ArrayList<>(List.of(names));

//배열 리스트를 배열에 복사
String[] names=friends.toArray(new String[0]);

//기본 타입에 대한 복사는 까다로워서 스트림활용해야 한다.


```

### 1.8.8 명령줄 인수

* Public static void main(String[] args)가 있을때 다음과 같이 자바를 실행시킨다면(Greeting이라는 클래스)

  Java Greeting -g cruel world,

  args[0]=-g, args[1]=cruel, args[2]=world 이다.

### 1.8.9 다차원 배열

```java
int [][] square=new int[4][4];
int [][] triangle=new int[n][]; //이것도 가능 

//향상된 for loop으로 초기화
for(int[] row:triangle){
  for(int element :row){
    ///
  }
}

//출력
System.out.println(Arrays.deepToString(triangle));
```



## 1.9 기능적 분해

### 1.9.1 정적 메서드 선언 및 호출

* 동일한 클래스 내에 여러 메서드를 나누는 경우 static 활용

### 1.9.2 배열 매개변수와 반환 값

* 메서드는 배열을 반환할 수 있다.

  ```java
  public static int[] firstLast(int[] values){
    if(values.length()==0)return new int[0];
    else return new int[]{values[0],values[values.length-1]};
  }
  ```

### 1.9.3 가변 인수

* 호출하는 쪽에서 원하는 개수만큼 파라미터를 넣을 수 있게 하는 메소드(...)

```java
public static double average(double...values)//더블 타입의 배열이 된다. 구현부도 배열처럼 사용하면 된다.
public static double max(double first,double...rest)//이런식으로 같이 쓸 수 있지만 메서드의 마지막 매개변수여야 한다.
```

# 2. 객체지향 프로그래밍

## 2.2 클래스 구현

### 2.2.1 인스턴스 변수

* 인스턴스 멤버: 객체(인스턴스)생성 후 사용할 수 있는 필드와 메소드

* 자바에서는 인스턴스 변수로 객체의 상태를 나타낸다. 인스턴스 변수는 클래스 안에 선언한다

  ```java
  public class Employee{
    private String name;//private으로 선언 시 같은 클래스에 속한 메서드만 변수에 접근 가능
    private double salary;
  }
  ```

* 자바에서 static으로 선언하지 않은 메서드는 모두 인스턴스 메서드다.

* this.변수라고 표기함으로써 단순한 변수가 아닌 인스턴스 변수임을 명시한다.

### 2.2.6 값을 사용한 호출

```java
//다음과 같이 하면 fred를 e 매개변수에 복사(객체를 공유하기 때문에 변경이 이루어짐)
public class EvilManager{

  public void giveRandomRaise(Employee e){
    double percentage=0.1;
    e.raiseSalary(percentage);
  }
}

boss.giveRandomRaise(fred);

//하지만 이런식으로 기본타입에 대해서는 sales가 x로 복사되지만 sales는 변하지 않는다
  public void giveRandomRaise(double x){
    double amount=x*10;
		x+=amount
  }

public void giveRandomRaise(double x){
    double amount=x*10;
		x+=amount
  }
boss.giveRandomRaise(sales);
//이것도 sales는 변경되지 않는다.

```

## 2.3 객체 생성

### 2.3.1 생성자 구현

* 생성자는 이름이 클래스와 같고 반환 타입이 없다(반환자가 있으면 메서드다)

* 생성자는 new 연산자를 사용한 시점에 실행된다.

  ```java
  public Employee(String name,double salary){
     this.name=name;
     this.salary=salary;
  }
  
  Employee james=new Employee("james",500);
  ```

### 2.3.2 오버로딩

* 파라미터에 따라 생성자를 다양하게 호출할 수 있다(생성자가 여러개 일 수 있다.)

### 2.3.6 최종 인스턴스 변수

* 최종으로 선언한 인스턴스 변수는 생성자 실행이 끝난기 전에 초기화 해야 한다.(초기화 후 수정  불가,setXXX메소드 없음, 단 변경 가능한 객체를 가리키는 참조에 사용하면 참조 자체가 변하지 않는다는 사실을 알려줄 뿐 객체의 내용을 변경하는 것은 가능)

  ```java
  //클래스 내
  private final ArrayList<Person> friends=new ArrayList<>();//이 배열 리스트에 요소 추가하는 것은 괜찮다.
  ```

### 2.3.7 인수 없는 생성자

* 클래스에 생성자가 없으면 자동으로 아무 작업도 하지 않은(기본 값으로 설정된) 인수 없는 생성자를 받는다.(따라서 모든 클래스에는 생성자가 적어도 하나는 있다.)

## 2.4 정적 변수와 정적 메서드

* 정적 멤버는 클래스에 '고정'된 멤버로 객체를 생성하지 않고 사용할 수 있는 필드(정적필드)와 메소드(정적메소드). 객체가 아닌 클래스에 소속된 멤버이기 때문에 클래스 멤버라고도 불린다.
* 클래스 로더가 클래스(바이트 코드)를 로딩해서 메모리 영역에 적재할 때 클래스별로 관리. 즉 클래스 로딩되면 바로 사용가능
* 객체마다 가지고 있어야 할 데이터라면 인스턴스 필드로, 공용적인 데이터라면 정적 필드로 선언하는 것이 좋다.
* 인스턴스 필드를 이용해서 실행해야 하는 메소드라면 인스턴스 메소드를, 그렇지 않다면 정적 메소드를 활용한다.
* 클래스.필드 / 클래스.메소드()로 사용

### 2.4.1 정적변수

* **클래스 안에 변수를 static으로 선언하면 해당 변수는 클래스당 하나만 존재한다.**

```java
public class Employee{
  private static int lastId=0;//정적 변수
}
```

* 보통 정적 필드는 필드 선언과 동시에 초기값을 추는 것이 보통, 만약 어떤 계산을 한 초기값이 필요하다면 객체 생성 필요없이 사용해야 하므로 생성자(객체 생성시 실행)를 통해서  초기화하는 것이 아닌 정적 블록을 이용

* 정적 블록은 여러개 선언되도 상관없다.(선언된 순서대로 실행)

* 정적 블록 내 인스턴스 필드나 인스턴스 메소드,this를 사용할 수 없다.(객체 없이도 실행되기 때문) 사용하고 싶다면 정적 블록 내에 객체를 생성해서 참조 변수로 접근해야 한다.(main도 static이다)

  ```java
  public class Tv{
    static String info;
    int num;
    static{
      info="television"+"good";
      Tv tv=new Tv();
      tv.num=10;
    }
  }
  
  //main
  Tv.info//television good
  ```

  

### 2.4.2 정적 상수와 정적 final 변수

* 아래에서 static이 없으면 인스턴스 변수

```java
public class Math{
  public static final double PI=3.141592;
  private static final Random generator=new Random();//정적 final 변수
}

```

### 2.4.4 정적 메서드(잘 모르겠다..)

* 객체에 작동하지 않는 메서드.(즉 인스턴스 변수에 접근할 수 없다)
* 단, 자신이 속한 클래스의 정적 변수에 접근할 수 있다.

### 2.4.5 팩토리 메서드(잘 모르겠다..)

* 클래스의 새 인스턴스를 반환하는 정적 메서드
* 매개변수가 없는 생성자를 두 개씩 둘 수 없으므로 생성자대신 팩토리 메서드를 사용
* 또한 서브클래스의 객체를 반환할 수 있음.

### * final

* 초기값이 저장되면 최종적인 값이 되어서 프로그램 실행 도중 변경할 수 없는 것,

  * 초기값을 줄 수 있는 방법 1: 필드 선언시
  * 2: 생성자 이용

  ```java
  public class finalTest {
      final String a="abc";
      final int num;
      public finalTest(){
          num=1;
      }
  }
  //이후 외부에서 값을 변경하려고 하면 에러 발생.
  ```

* final도 변하지 않는 값이니 상수라고 말할 수 있을까?: 아니다. 상수는 여러 가지 값으로 초기화 될 수 없고 객체마다 저장할 필요가 없다. 하지만 final 필드는 객체마다 저장되고 생성자를 통한 초기화가 가능하다.

  * 따라서 상수는 static이면서 final 이어야 한다.

  * 상수 초기화 방법 1: 선언 시

  * 2: 정적 블록 이용

    ```java
    static final double PI=3.14;
    static final String NAME;
    static{
      NAME="a";
    }
    ```

  * 사용은 class.상수변수

## * 싱글톤

```java
public class singleton{
  private static singleton s=new singleton();//클래스 내부에서는 new연산자로 생성자 호출 가능
  private class(){}//외부에서 접근하지 못하도록 하기 위해 private이용.
  
  static singleton getInstance(){
    return s;
  }
}

//main
singleton s1=singleton.getInstance();//new로 하면 안된다. 
singleton s2=singleton.getInstance();
System.out.println(s1==s2);//true
```

## 2.5 패키지

* 패키지를 임포트 할 때 해당 패키지의 하위 패키지 까지 임포트 되는 것이 아니다. 따라서 하위 패키지의 클래스를 이용하고 싶다면 그것에 대한 임포트도 추가적으로 작성해 줘야 한다.

* 패키지 이름 전체를 작성해야 할 때가 있는데 서로 다른 패키지에 동일한 클래스 이름이 존재하고, 두 패키지가 모두 임포트 되었을 때이다.

  ```java
  package명~~~.tier t=new package명~~~.tier();
  package2명~~~.tier t2=new package2명~~~.tier();
  ```

  

### 2.5.4 패키지 접근

* 소스 파일 하나에 여러 클래스를 포함할 수 있지만 기껏해야 한개만 public으로 선언할 수 있다(파일 이름과 일치하는)

### 2.5.6 정적 임포트

* 정적 메서드와 정적 변수를 임포트 하는 것.

  ```java
  import static java.lang.Math.*;
  //이렇게 임포트할 경우 Math클래스의 정적 메서드와 정적 번수를 클래스 이름 접두어 없이 사용할 수 있다.
  ```

## 3.1 인터페이스

### 3.1.1 인터페이스 선언

* 추상 메서드: 기본 구현을 작성하지 않고 선언만 한 메서드
* 인터페이스의 모든 메서드는 public이 된다
* 인터페이스를 구현 하는 클래스는 인터페이스의 메서드를 반드시 public으로 선언해야 한다.

### 3.1.2 인터페이스 구현

* 인터페이스 타입 변수는 인터페이스를 구현한 어떤 클래스의 객체라도 참조할 수 있다 또한 해당 객체를 해당 인터페이스를 기대하는 메서드에도 전달할 수 있다.

  ```java
  public interface IntSequence(){
    boolean hasNext();//자동으로 public이여서 굳이 선언 안함,
    int next();
  }
  
  public class DigitSequence implements IntSequence{
    private int number;
    
    public DigitSequence(int n){
      number=n;
    }
    
    public boolean hasNext(){
      return number!=0;
    }
    
    public int next(){
      ...
    }
  }
  
  IntSequence digits=new DigitSequence(123);//가능
  double avg=average(digits,100);
  
  ```

### 3.1.4 캐스트와 instanceof 연산자

* 객체를 캐스트 할 때 실제 클래스나 그 슈퍼타입으로만 캐스트 할 수 있다.
* 따라서 확실하지 않을 때는 instanceof로 먼저 검사하고 실시하자.(객체 instanceof Type) 

### 3.1.5 인터페이스 확장

* extend를 통해 인터페이스에 없는 추가 메소드를 요구 및 제공할 수 있다. 단 그 구현 클래스는 인터페이스의 기존 메소드 + 해당 인터페이스가 추가한 메소드까지 모두 구현해야 한다.

  ```java
  public interface Closeable(){
    void close();
  }
  
  public interface Channel extends Closeable(){
    boolean isOpen();
  }
  
  //channel을 구현하는 클래스는 close,isOpen모두 구현해야 한다.
  ```

### 3.1.6 여러 인터페이스 구현

* 클래스는 여러개의 interface를 구현할 수 있다. ex) implements aaaa,bbb,cccc{}

### 3.1.7 상수

* 인터페이스에 정의한 변수는 자동으로 public static final이다.

### 3.2.1 정적 메서드

* Java 8 이전에는 인터페이스의 모든 메서드가 추상 메서드여야 했는데 9부터는 실제 구현이 있는 메서드(정적 메소드, 기본 메소드, 비공개 메서드)를 인터페이스에 추가할 수 있다.

  ```java
  public interface IntSequence{
    ...
     static IntSequence digitsOf(int n){
      return new DigitSequence(n);
    }
  }
  ```

### 3.2.2 기본 메소드

```java
public interface IntSequence{
  default boolean hasNext(){
    return true;
  }
}
//이 인터페이스를 구현하는 클래스는 hasNext 메소드를 오버라이드하거나 기본 구현을 상속하거나.
```

### 3.2.3 기본 메소드의 충돌 해결

* 한 클래스가 같은 이름의 기본 메소드를 가진 인터페이스를 모두 구현한다고 할 때 충돌 해결하는 법

  ```java
  //가정: 각 인터페이스는 getId라는 기본 메소드를 가지고 있음.
  public class Employee implements Person, Identified{
    public int getId(){
      return Identified.super.getId();//이런식으로 하나에 위임해야 한다.
    }
  }
  ```

### 3.2.4 비공개 메서드(자바 9부터)

### 3.3.1 Comparable 인터페이스

* 사전식 정렬에 주로 사용되는 인터페이스

  ```java
  public interface Comparable<T>{
    int compareTo(T other);
  }
  
  public class Employee implements Comparable<Employee>{
    ...
    public int compareTo(Employee other){
      return Double.compare(salary,other.salary);
    }
  }
  ```

### 3.3.2 Comparator 인터페이스

* 길이 기준 정렬에 사용되는 인터페이스

  ```java
  public interface Comparator<T>{
    int compare(T first, T second);
  }
  
  class LengthComparator implements Comparator<String>{
    public int compare(String first,String second){
      return first.length()-second.length();
    }
    
  }
   
  //사용하려면 클래스의 인스턴스를 만들어야 한다
  Comparator<String> comp=new LengthComparator();
  if(comp.compare(words[i],words[j])>0){}
  ```

### 3.3.3 Runnable 인터페이스

* 별도의 스레드로 돌리고 싶을 때(멀티 쓰레드)

* Runnable 인터페이스에는 run()메소드 한개만 있다.

  ```java
  class HelloTask implements Runnable{
    public void run(){
      ...
    }
  }
  //이 태스크를 새 스레드에서 실행하려면 Runnable로 스레드를 생성하고 시작해야 한다, 다음과 같이
  Runnable task=new HelloTask();
  Thread thread=new Thread(task);
  thread.start();
  //이렇게 하면 run 메소드는 별도의 스레드에서 실행되므로 현재 스레드는 다른 작업을 계속 할 수 있다.
  
  ```

## 3.4 람다 표현식

* 나중에 한 번 이상 실행할 수 있게 전달하는 코드블록, 객체로 함수를 표현

### 3.4.1 람다 표현식 문법

```java
first.length()-second.length();
이거를
(String first,String second)->first.length()-second.length()//변수의 명세까지 갖춘 것.매개변수 타입은 생략 가능하다.
(String first,String secode)->{...}//여러 일을 할 때 이런식으로도 가능
Runnable task=()->{..}//매개변수가 없는 경우 빈괄호를 붙여야 한다.
EventHandler<ActionEvent> listener=event->System.out.println();//메서드에 매개변수 한개만 있고 이 매개변수의 타입을 추론할 수 있다면 괄호도 생략 가능.=(ActionEvent event)


```

### 3.4.2 함수형 인터페이스

* 단일 추상 메서드(추상 메서드가 한개)를 가진 인터페이스를 함수형 인터페이스라 한다.

## 3.5 메서드 참조와 생성자 참조

### 3.5.1 메서드 참조

```java
Array.sort(strings,(x,y)->x.compatrToIgnoreCase(y));
= Array.sort(strings,String::compareToIgnoreCase);

//리스트에서 널값 제거를 람다식으로
list.removeIf(Objects::isNull);

//for each같이 리스트 출력을 람다식으로
list.forEach(System.out::println);
```

### 3.5.2 생성자 참조(음...)

* 메서드 이름이 New라는 점만 제외하면 메서드 참조와 같다.

## 3.6 람다 표현식 처리

### 3.6.1 지연 실행 구현

* 람다 사용의 핵심 목적은 지연 실행.(코드를 나중에 실행)
  * 나중 실행 이유(별도의 스레드에서 실행, 여러번 실행, 어떤 일이 일어날때 실행, 필요할 때 실행 등)

```java
//주어진 파라미터에 따른 반복 수행을 하고 싶을 때

//다음과 같은 함수형 인터페이스를 골라야 한다.
public interface IntConsumer{
  void accept(int value);
}

public static void repeat(int n,IntConsumer action){
  for(int i=0;i<n;++i)action.accept(i);
}

repeat(10,i->System.out.println("..")));

//여러가지 표준 함수형 인터페이스가 있는데 맞지 않는 경우 다음과 같이 새 인터페이스를 정의해서 사용
@FunctionalInterface
public interface PixelFuntion{
  Color apply(int x,int y);
}
```

### 이후 내용 있는데 넘어감..

# 4.상속과 리플렉션

* 필드: 인스턴스 변수와 정적 변수를 총칭
* 멤버: 필드. 메서드, 클래스 내부에 있는 중첩 클래스와 인터페이스를 총칭

```java
public class Manager extends Employee{
   추가 필드
    추가 메소드 또는 슈퍼클래스 오버라이드 메서드
}
```

* 부모의 모든 필드와 메서드를 상속받는 것이 아니라 private 접근제한을 갖는 필드와 메소드는 상속에서 제외된다, 또한 다른 패키지에 존재한다면 default [접근 제한자](#접근-제한자)를 갖는 것도 상속 대상에서 제외된다.

* 상속을 사용하는 이유: 부모 클래스의 수정으로 자식 클래스의 수정 효과를 가져온다.

* 다중 상속을 허용하지 않는다.

* 자식 객체를 생성하면 사실은 내부적으로 부모객체가 먼저 생성됨. 하지만 부모의 생성자를 호출한 적이 없는데?-> 자식의 생성자 맨 첫줄에서 호출됨.

  ```java
  public 자식클래스생성자(){
    super();//명시적으로 선언하지 않았다면 다음을 기본 생성자를 생성함. 만약 부모에 기본 생성자가 없고 매개가 있는 생성자가 있다면 명시적으로 super(매개값)을 호출해야 한다.
  }
  ```

  

### 4.1.3 메서드 오버라이딩

* 자식 클래스에서 다시 수정해야 하는 경우 동일한 메소드를 재 정의 하는 것.(수정되면 호출 시 부모의 메소드는 숨겨지고 오버라이딩된 자식 메소드가 호출된다.)

* 오버라이딩 규칙

  * 부모의 메소드와 동일한 시그니처(리턴 타입, 메소드 이름, 매개 변수 리스트)를 가져야 한다
  * 접근 제한을 더 강하게 오버라이딩 할 수 없다.(부모가 public인데 자식을 default, private으로 할 수 없다, 반대는 가능)
  * 새로운 예외를 throws할 수 없다.

* 서브클래스 메서드는 슈퍼 클래스의 비공개 인스턴스 변수에 직접 접근할 수 없다.

* 슈퍼클래스 메서드를 호출할 때는 super키워드를 사용한다.

* super는 객체 참조가 아닌 지시자이다.

* 오버라이딩할 때 파라미터는 같아야 하지만 리턴 타입은 서브타입으로 바꿀 수 있다.

* super()

  * 부모의 메서드를 호출하고 싶을 경우

    ```java
    class Parent{
      void method1(){}
    }
    
    class Child extends Parent{
      void method1(){}//overriding
      void method3(){
        method1()//오버라이딩 된 메소드 호출
        super.method1();//부모 메소드 호출
      }
    }
    ```

    

## 4.1.4 서브클래스 생성

* 서브클래스 생성자는 슈퍼 클래스의 비공개 인스턴스 변수에 접근할 수 없으므로 이런 인스턴스 변수는 슈퍼클래스 생성자로 초기화 해야한다.

  ```java
  public Manager(String name,double salary){
    super(name,salary);//슈퍼클래스의 생성자 명시적 호출
    bonus=0;
  }
  ```

### 4.1.5 슈퍼클래스 할당

* 서브클래스의 객체를 슈퍼클래스 타입 변수에 할당할 수 있다.

  ```java
  Manager boss=new Manager(..);
  Employee empl=boss;
  double salary=empl.getSalary(); //getSalary()는 manager의 메소드
  //이것을 동적 메서드 조회라 하며 이로인해 또다른 Employee 서브클래스의 인스턴스든 상관없이 모든 직원 객체로 작동하는 코드를 작성할 수 있다. 단 슈퍼클래스에 있는 메서드만 호출 가능.(서브에서 새로 추가한 것에 대한 것은 불가능)
  ```

### 4.1.7 최종 메서드와 최종 클래스(final)

* 클래스에 final 키워드가 붙는 경우 해당 클래스는 최종적인 클래스 이므로 자식 클래스를 만들 수 없다(상속할 수 없다)
* final 이 붙은 메서드는 어느 서브클래스도 해당 메서드를 오버라이드 할 수 없다.

### *다형성

* 같은 타입이지만 실행 결과가 다양한 객체를 이용할 수 있는 성질, 코드 측면에서 하나의 타입에 여러 객체를 대입함으로써 다양한 기능을 이용할 수 있도록 함.
* 자바는 다형성을 위해 부모 클래스로 타입 변환을 허용함.



### 4.1.8 추상 메서드와 추상 클래스

* 구현이 없는 메서드=추상메서드

* 추상 메서드가 포함된 클래스= 추상 클래스

* 클래스에서 추상 메서드를 선언해 서브클래스가 해당 메서드를 구현하도록 강제할 수 있음.

* 추상 클래스는 인터페이스와 달리 인스턴스 변수와 생성자를 포함할 수 있다.

* 추상 클래스의 인스턴스는 생성할 수 없다. 하지만 서브클래스의 객체 참조를 저장한다면 가능

  ```java
  public class Student extends Person{
    private int id;
    
    public Student(String name,int id){
      super(name);
      this.id=id;
    }
    
    public int getId(){
      return id;
    }
  }
  Person p=new Student("Fred",1729)
  ```

### 4.1.9 보호접근(protected)

* 메서드를 서브클래스 전용으로 제한하거나 서브클래스 메서드에서 슈퍼클래스의 인스턴스 변수에 접근하고 싶을 때

  ```java
  public class Employee{
    protected double salary;
    ..
  }
  
  //Employee와 같은 패키지에 속한 모든 클래스가 이 필드에 접근할 수 있음
  ```



## 4.2 Object: 보편적 슈퍼클래스

### 4.2.1 toString 메서드

* 객체의 문자열 표현을 반환하는 메서드

### 4.2.4 객체 복제

* 얕은 복사: 같은 객체를 참조하도록 복사(객체를 가리키는 주소 복사)
* 깊은 복사: 참조하는 객체까지 별도로 생성하여 복사.
* Object.clone=얕은 복제, 원본 객체에 있는 모든 인스턴스 변수를 복제된 객체로 복사

## 4.3 열거

* 한정된 값만을 갖는 데이터 타입. ex) 요일, 계절 등.
* 열거 타입 선언 방법

```java
public enum Week{Monday,Tuesday...};

Week weekvariable;
weekvariable=Week.Monday;//열거 상수는 항상 열거 타입과 함께 쓰여야 한다(.)
```

* Week.class는 메소드 영역에, weekvariable 변수는 스택영역에,  실제 값 객체는  heap에 저장
* 이때 위와 같은 경우 힙에 있는 같은 객체를 참조하게 된다.(weekvariable==Week.monday)

### 4.3.1 열거의 매서드

* 열거 타입의 각 인스턴스는 유일하므로 equals메소드로 비교하는 것이 아닌 ==을 이용하면 된다,
* 메서드 구현 필요 없이 toString, valueOf, values등을 자동으로 제공한다.
* ordinal은 인스턴스 위치를 돌려줌

### 4.3.2 생성자, 메서드, 필드

* 열거의 생성자는 언제나 비공개다.

## 4.4 실행 시간 타입 정보와 리소스

## 4.5 리플렉션

# 5 예외 단정 로깅

# 6 제네릭 프로그래밍

* 제네릭을 통해 타입 변환을 피할 수 있다(=타입 캐스트는 성능 저하를 일으킨다)

  ```java
  package com.company;
  
  public class Box<T> {
  
      private T t;
      public T get(){
          return t;
      }
      public void set(T t){
          this.t=t;
      }
  }
  
  ```

  ```java
  package com.company;
  
  public class Main {
  
      public static void main(String[] args) {
  
          Box<String> box1 = new Box<>();
          box1.set("hello");
          System.out.println("box1.get() = " + box1.get());
          Box<Integer> box2=new Box<>();
          box2.set(123);
          System.out.println("box2.get() = " + box2.get());
          
  
      }
  }
  ```

* 제네릭이 아니었다면 형변환 해줘야 했을 것.

## 6.1 제네릭 클래스

* 타입 매개변수가 한개 이상 있는 클래스.

  ```	java
  public class Entry<K,V>{
    ...
  }
  ```

* 제네릭 클래스의 객체 생성시 생성자의 타입 매개변수를 생략할 수 있다.

  ```java
  Entry<String, Integer> entry=new Entry<>("abc",42);
  ```

## 6.2 제네릭 메서드

* 타입 매개변수를 받는 메서드.

* 메서드 선언 시 타입 매개변수를 제어자(public,static과 같은)와 반환 타입 사이에 두어야 한다

  ```java
  public static <T> void swap(T[] array,int i,int j)
  ```

## 6.3 타입 경계

* 제네릭 클래스나 제네릭 메소드가 받는 타입 매개변수의 타입을 제한해야 할 때 타입 경계 지정.

## 6.4 타입 가변성과 와일드카드

* Manager가 Employee의 서브클래스면 Manager[]도 Employee[]의 서브타입이다
* 하지만 ArrayList<Manager>는 ArrayList<Employee>의 서브타입이 아니다. 따라서 오른쪽과 같이 변환할 수 없다.

### 6.4.1 서브타입 와일드 카드

```java
public static void printNames(ArrayList<? extends Employee> staff){
  ...
}
//? extends Employee는 미지의 Employee 서브타입을 가리킨다. 따라서 ArrayList<Employee>나 ArrayList<Manager>와 같은 서브타입의 배열 리스트로 printNames 메소드를 호출할 수 있다.단 이걸 이용해서 쓸 수는 없다.(읽기용)
```

### 6.4.2 슈퍼타입 와일드카드

* 제네릭 함수형 인터페이스를 메서드 매개변수로 받을 때는 super 와일드카드를 사용해야 한다.

  ```java
  public static void printAll(Employee[] staff, Predicate<? super Employee> filter){
    for(Employee e: staff){
      if(filter.test(e)){
        ...
      }
    }
  }
  ```

  



# 8 스트림

## 8.1 반복에서 스트림 연산으로 전환

```java
//컬렉션 이용
int count=0;
for(String w: words){
  if(w.length()>12)count++;
}

//스트림 이용
long count=words.stream()//stream()을 parallelStream()으로 바꾸어 주면 필터링과 카운팅을 병렬로 수행.
  .filter(w->w.length()>12)//스트림을 변환(중간 처리)
  .count();//종료연산(최종 처리(집계))
```

* 스트림과 iterator의 차이
  * 둘다 비슷한 반복자이나 람다식을 쓸수있는점
  * 내부 반복자 활용으로 병렬처리가 쉽다는 점(for문은 index, while문은 iterator를 즉, 외부반복자를 이용.)
  * 중간 처리와 최종 처리 작업을 수행하는 점.
* 스트림과 컬렉션의 차이
  * 스트림은 요소를 저장하지 않는다.
  * 스트림 연산은 원본을 변경하지 않는다.(새 스트림을 돌려준다)
  * lazy 방식으로 동작한다. 요청한 만큼 요청할 때 실행.(최종 처리를 시작할 대 중간처리를 시작.)
* 스트림 워크플로우
  1. 스트림 생성
  2. 초기 스트림을 다른 스트림으로 변환하는 중간 연산 지정
  3. 종료 연산을 적용해 결과 산출.(종료 연산은 지연 연산이 실행되게 함.)
* 스트림을 왜 써야 하는가
  * 내부 반복자로 인해 멀티 코어 CPU를 최대한 활용할 수 있다. 즉 일을 맡기고 알아서 가장 효율적인 방법으로 처리해서 리턴해줌.

## 8.2 스트림 생성

* Collection 인터페이스의 stream 메서드를 사용하면 어떤 컬렉션이든 스트림으로 변환할수 있다

* 배열은 정적 메소드 Stream.of를 사용해야 한다.

  ```java
  Stream<String> words=Stream.of(,..);
  ```

* 요소가 없는 스트림을 만들려면 정적 메서드 Stream.empty를 사용한다

  ```java
  Stream<String> silence=Stream.empty();
  ```

* 상수 값의 스트림

  ```java
  Stream<String> echos=Stream.generate(()->"a");
  ```

* 수열 스트림

  ```java
  Stream<BigInteger> integers=Stream.iterate(BigInteger.ZEOR,n->n.add(BigInteger.ONE));
  ```

* 유한 스트림

  ```java
  Stream<BigInteger> integers=Stream.iterate(BigInteger.ZERO,n->n.compareTo(limit)<0,n->n.add(BigInteger.ONE));
  ```

## 8.3 filter,map,flatMap

```java
//map은 변환
Stream<String> lowercaseWords=words.stream().map(String::toLowerCase);
//map 람다식
Stream<String> firstLetters=words.stream().map(s->s.substring(0,1));
```

## 8.4 서브스트림 추출과 스트림 결합

* Stream.limit(n): n개 이후 새 스트림 반환

  ```java
  Stream<Double> randoms=Stream.generate(Math::random).limit(100);
  ```

* Stream.skip(n): 처음 n개 요소를 버림.

* Stream.takeWhile(predicate): 프레디케이트가 참인 동안 스트림에서 모든 요소를 가져온 후 중단(dropWhile은 조건이 참인동안 버림)

  ```java
  Stream<String> initalDigits=codePoints(str).takeWhile(s->"01234".contains(s));
  ```

* Concat: 두 스트림 연결

  ```java
  Stream<String> combined=Stream.concat(codePoints("Hello"),codePoints("world"));
  //["H","E","l"...."l","d"]를 돌려줌.
  ```

## 8.5 기타 스트림 변환

## 8.6 단순 리덕션(종료연산)

* 스트림을 프로그램에서 사용할 수 있는 넌스트림 값으로 리듀스 한다.

* Optional<T>: 결과를 감싸고 있거나 결과가 없음을 나타냄.

* Max, min: 최대 최소, 단 Optional<T> 값을 반환.

* findAny: 어떤 일치 결과가 나오든 상관없을 때

  ```java
  Optional<String> startsWithq=words.parallel().filter(s->s.startWith("a")).findAny();
  ```

* anyMatch: 일치하는 요소가 있는 지 알 고 싶을 때.(프레디케이트 인수를 받으므로 filter를 사용할 필요가 없음.)

## 8.7 옵션 타입

### 8.7.1 옵션 값을 사용하는 방법

* 값이 없을 때는 대체 값을 생산하고, 값이 있을 때는 해당 값을 소비하는 메서드를 사용하는 것이 Optional을 잘 활용하는 것.(값이 있으면 그냥 그 값이고 null이 아닌 값이면 optional로 래핑한 것이 결과, null이면 비어있는 결과)

  ```java
  optionValue.ifPresent(v->results.add(v));
  =optionValue.ifPresent(reuslts::add);
  optionValue.ifPresentOrElse(v->v,()->)//if-else느낌
  Optional<Boolean> added=optionValue.map(results::add);//반환 받으려면
  ```

### 8.7.2 옵션 값을 사용하지 않는 방법

```java
Optional<T> value=...;
value.get().somemethod();//있으면 얻어오고 없으면 nosuchelementexception을 던진다.
```

* isPresent 메서드를 활용할 수도 있다.

### 8.7.3 옵션 값 생성

* Optional.ofNullable(obj)
* * obj가 null이 아니면 Optional.of(obj)를 반환
  * null이면 Optional.empty()를 반환

### 8.7.5 옵션 값을 스트림으로 변환

* stream 메서드는 Optional<T>를 요소 0개 도는 1개를 가진 Stream<T>로 변환.

  ```java
  //잘못된 ID는 건너뛰면서 사용자로 구성된 스트림을 얻으려면
  Stream<Users> users=ids.map(Users::lookup)
    .flatMap(Optional::stream);
  ```

## 8.8 결과 모으기

* 스트림을 리스트로 모으려면

  ```java
  List<String> result=stream.collect(Collectors.toList());
  ```

## 8.9 맵으로 모으기

# 10 병행 프로그래밍

## 10.1 병행 태스크

### 10.1.1 태스크 실행

* Runnable 인터페이스: 동시에 실행하려는 태스크를 나타냄.

  ```java
  public interface Runnable{
    void run();//스레드 안에서 실행됨.
  }
  ```

* 자바 병행성 라이브러리에 있는 실행자 서비스(Excutor service)는 태스크를 스케줄링하고 해당 태스크를 수행할 스레드를 선택해 실행한다.

* Executor클래스는 실행자 서비스를 만드는 팩토리 메서드가 있어 각 프로그램(태스크를 가진)에게 최적화된 실행자 서비스를 돌려준다. 이때 각 태스크는 가능하면 idle thread에서 실행되지만 모든 스레드가 실행중이면 새 스레드를 할당한다.

* 태스크를 제출하면 퓨처(미래 어느 시점에 결과가 나오는 계산을 표현하는 객체)를 얻는다.

### 10.1.2 퓨처

* Runnable은 태스크를 수행하지만 값을 내지 않는다. 따라서 결과를 계산하는 태스크가 있다면 Callable<V> 인터페이스를 활용.

  ```java
  public interface Callable<V>{
    V call() throws Exception;//V타입 값을 반환함.
  }
  ```

* 퓨처 인터페이스 메서드

  * Get(): 결과가 나오거나 타임아웃에 이를 때까지 블록한다. Get 메서드를 호출한 스레드는 메서드가 정상적으로 반환되거나 예외를 던질 때까지 일을 진행하지 않는다.
  * Cancle: 태스크 취소.
  * invokeAll: 각 태스크를 한꺼번에 제출하는 것.
  * invokeAny: 제출한 태스크중 하나가 정상적으로 완료되면 즉시 반환,

## 10.2 비동기 계산

* 기다리는 것이 아닌 대기 없는 계산.

### 10.2.1 완료 가능한 퓨처

* CompletableFuture: future인터페이스를 구현한 클래스로 결과를 얻게함,(콜백을 등록해야 한다.)

  ```java
  CompletableFuture<String> f=...;
  f.thenAccept((String s)-> 결과 s를 처리);
  ```

* 태스크를 비동기로 수행하고 CompletableFuture를 얻으려면 실행자 서비스에 직접 제출하는 것이 아닌 정적 메서드 CompletableFuture.supplyAsync를 호출해야 한다.

  ```java
  CompletableFuture<String> f=CompletableFuture.supplyAsync(()->{String result;결과계산;return result;},executor);//첫번째 인수는 Callable<T>이 아닌 Supplier<T>이다.
  ```

* CompletableFuture는 수동으로 완료 값을 설정할 수 있다(promise한 객체이다).

  ```java
  CompletableFuture cf=CompletableFuture.completedFuture("done");// 입력받은 값으로 이미 완료된 CompletableFuture를 생성하는 방법
  ```

### 10.2.2 완료 가능한 퓨처 합성

* 복잡한 완료 가능한 퓨처의 합성을 프로세싱 파이프라인으로 합성하는 메커니즘을 제공해 문제를 해결한다.
* allOf: 지정한 퓨처들이 모두 완료된 후 void 결과로 완료한다.
* Join(); 모든 CompletableFuture의 실행완료를 기다릴 수 있음.

# 11 애너테이션

## 11.1 애너테이션 사용

### 11.1.1 애너테이션 요소

* 애너테이션은 요소라고 하는 키/값 쌍을 포함할 수 있다. Ex) @Test(timeout=10000)

* 애너테이션 요소: 기본 타입, String,Class 객체. enum인스턴스, 애너테이션.

* 요소 이름이 value고 이 요소만 지정할 때는 value=를 생략할 수 있다.

* 요소 값이 배열이면 해당 해열의 구성요소를 중괄호로 감싼다(배열 요소가 하나이면 중괄호 생략 가능)

  @BugReport(reportedBy={"Harry","Fred})

### 11.1.2 다중 애너테이션과 반복 애너테이션

* 아이템 하나에 여러 애너테이션, 같은 애너테이션을 반복해서 쓸 수 있다.

### 11.1.3 선언에 애너테이션 붙이기

* 클래스나 인터페이스 뿐만 아니라 메서드, 생성자, 인스턴스 변수 등에서 애너테이션을 붙일 수 있다.
* 크게 선언과 타입 사용 카테고리로 나눈다.

### 11.1.4 타입 사용에 애너테이션 붙이기

```java
public User getUser(@NonNull String userId)//매개변수가 널이 아님을 보장
List<@NonNull String> //타입 사용 애너테이션
```

## 11.2 애너테이션 정의

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Test{
  ...
}
//target과 retention은 메타애너테이션으로 각각 Test(내가 정의한)애너테이션을 적용할 수 있는 위치와 접근할 수 있는 위치를 나타낸다.

```

* @Retention 메타애너테이션: 애너테이션에 접근할 수 있는 위치를 지정

* * RetentionPolicy.SOURCE: 애너테이션이 소스 처리기에는 보이지만 ,클래스 파일에는 포함되지 않는다

  * RetentionPolicy.CLASS: 애너테이션이 클래스 파일에 포함되지만, 가상머신은 해당 애너테이션을 로드하지 않는다.(디폴트)

    RetentionPolicy.RUNTIME: 애너테이션을 실행 시간에 이용할 수 있고, 리플렉션 API로 접근할 수 있다.

* 요소의 기본 값 지정

  ```java
  public @interface Test{
    long timeout() default 0L;
    //배열의 경우 String[] reportedBy() default{};
    ...
  }
  ```

### 11.3.1 컴파일용 애너테이션











