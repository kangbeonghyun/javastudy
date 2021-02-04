# 1주차_JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가. 

## 목표: 자바 소스 파일(.java)를 JVM으로 실행하는 과정 이해하기

## 학습할 것

* JVM이란 무엇인가
* JVM 구성 요소
* 컴파일 하는 방법
* 실행하는 방법
* 바이트코드란 무엇인가
* JIT컴파일러란 무엇이며 어떻게 동작하는지
* JDK와 JRE의 차이
* 가비지 컬렉션

---

### Overview

컴파일러(javac)에 의해 .java 가 .class(바이트코드)로 변환된다.

.class 파일은 classloader(JRE에 있음)에 의해 JVM에 적재된다.

JVM은 .class파일을 로드한뒤 Execution engine을 통해 해석한 뒤 바이트 코드를 JIT 컴파일 방식으로 실행한다.

---

* JVM(Java virtual machine)이란 무엇인가
  * OS마다 따로 코드를 작성할 필요 없이 **플랫폼에 독립적**일 수 있도록 만들어 주는 가상머신
  * 메모리 관리, Garbage Collection등을 수행.
  * 스택 기반

* JVM 구성 요소(*글쓰는 개발자 _jbee/ https://asfirstalways.tistory.com/158*)

![jvm구조](/Users/beonghyunkang/Desktop/1주차/jvm구조.png)

1. Class Loader(*글쓰는 개발자 _jbee/ https://asfirstalways.tistory.com/158*)

   * **Runtime**시 동적으로 .Class를 로드하고 링크를 통해 배치하는 작업을 수행하는 모듈

2. Execution Engine

   * 클래스를 실행시키는 역할. 이때 바이트코드를 바이너리 코드로 변경(해석)하는데 2가지 방식 존재
     1. 인터프리터: 실행 엔진이 자바 바이트 코드를 명령어 단위로(한 줄씩)읽어서 실행
     2. JIT(Just In Time): 인터프리터 방식으로 실행하다 적정 시점에 바이트 코드 전체를 컴파일 하여 네이티브 코드로 변경하고, 이후에는 다시 인터프리팅 하지 않고 네이티브 코드로 직접 실행하는 방식.(아래에서 자세히)

3. Runtime Area: OS에서 할당받은 메모리 공간

   ![runtimearea](/Users/beonghyunkang/Desktop/1주차/runtimearea.png)

   * PC 레지스터: Thread가 시작될 때 생성되며 Thread가 어떤 부분을 어떤 명령으로 실행할지 기록(현재 수행중인 JVM 명령 주소를 가짐)

   * JVM 스택 영역: 메서드 실행 시 임시로 할당되고 메소드를 빠져 나가면 사라지는 데이터를 저장하기 위한 영역, 메소드 안의 지역 변수, 매개 변수 등을 임시로 저장(스택방식)

   * Native method stack: 바이너리 코드를 실행시키는 영역. 이 영역 덕분에 자바가 아닌 다른 언어도 실행시킬 수 있음.

   * Method Area: 클래스가 로드될때 초기화 되는 대상을 저장하기 위한 메모리 공간.(사실상 바이트 코드의 대부분이 올라감), 필드정도, 메서드 정보, 타입 정보 등 거의 모두 올라감.

     - Runtime Constant Pool: 상수 자료형을 저장하여 참조하고 중복을 막는 역할

   * Heap: 객체를 저장하는 가상 메모리 공간. 공간이 부족해지면 GC수행.

     ![JVM_HEAP](/Users/beonghyunkang/Desktop/1주차/JVM_HEAP.png)

     - Permanent Generation: 생성된 객체들의 Meta정보의 주소값이 저장된 공간.  

     - New/Young 영역: Eden(객체들이 최초로 생성되는 공간), Survivor 0,1(Eden에서 참조되는 객체들이 저장되는 공간)

     - Old 영역:

       

