## 자바 가비지컬렉션(Garbage Collection)

### Garbage : 앞으로 사용되지 않는 객체의 메모리
자바의 메모리 관리 기법으로 애플리케이션이 동적으로 할당했던 메모리 영역 중 더 이상 사용하지 않는 영역을 정리하는 기능  
GC는 Heap 메모리에서 활동하며, 개발자가 직접 관여하지 않아도 더 이상 사용하지 않는 점유된 메모리를 제거해주는 역할

### Stop The World  
GC를 수행하기 위해 JVM이 멈추는 현상을 의미  
GC가 작동하는 동안 GC관련 Thread를 제외한 모든 Thread는 멈춤  
일반적으로 '튜닝'이라는 것은 이 시간을 최소화하는 것을 의미함

### GC가 왜 필요한가?  
프로그램이 동적으로 할당했던 메모리 영역(Heap 영역) 중 필요 없게 된 영역(어떤 변수도 가리키지 않음)을 알아서 해제  
할당 받은 메모리 영역을 제대로 해제하지 않아 Memory Leak이 발생한다.  
  - 메모리 누수를 막음
  - 해제된 메모리 접근 막음
  - 이중해제를 막음
  - GC작업은 순수 오버헤드
  - 개발자는 언제 GC가 메모리를 해제하는지 모름


GC가 가능 했던건 __'약한 세대 가설(weak generational hypothesis)'__   
두 가지 가정을 기반으로 메모리 구조를 크게 2개로 물리적 공간을 나눔(Young Generation, Old Generation)  
대부분 객체는 금방 접근 불가능(Unreachable)한 상태가 된다.  
-> 대부분의 객체는 중괄호 안에서 생성 이 객체들은 괄호가 끝나는 시점에서 더 이상 사용되지 않음  
오래된 객체에서 젊은 객체로의 참조는 아주 적게 발생  
-> 일반적으로 순차적인 로직에 의해 객체를 생성하여 활용  
이 과정에서 앞에 생성된 객체는 그 다음의 로직에서 사용된 이후 대부분 사용되지 않게 됨  
이런 특성으로 인해 더 이상 참조되지 않는 객체에 대해 GC를 할 수 있게 됨  

### GC의 종류
- Serial GC
- Parallel GC
- CMS GC
- G1 GC
- Z GC
  
### GC Algorithm
1. Reference Cpunting Algorithm  
각 객체마다 Reference Count를 관리하며, 이 카운트가 0이 되면 GC를 수행  
카운트가 0이 되면 바로 메모리에서 제거된다는 장점이 있음  
순환 참조 구조에서 Reference Count가 0이 되지 않는 문제가 발생하여 Memory Leak이 발생할 수 있음  
1. Mark-and-Sweep Algorithm
Rset(Root Set)에서 Reference를 추적하여 참조 상황을 파악  
Mark 단계에서 Garbage가 아닌 객체를 마킹하는 작업을 수행  
그 이후 Sweep 단계에서 마킹되지 않은 객체를 지우는 작업 수행  
위 단계를 마친 마킹 정보를 초기화  
GC가 동작하고 있을 경우, Mark 작업과 애플리케이션 Thread의 충돌을 방지하기 위해 Heap 사용이 제한됨 그리고 Compaction 작업이 없어 비어 보이는 공간이 충분하지 않을 경우 Out Of Memory가 발생할 수 있음  
순환 참조 구조 객체도 지울 수 있음  
단점  
- 의도적으로 GC를 실행시켜야 한다.
- 애플리케이션 실행과 GC실행이 병행된다  

### JVM MEMORY 구조  
![jvmm_emory](jvm_memory.png)  
모든 쓰레드가 공유하는 area : Method Area, Heap  
각 쓰레드마다 고유하게 생성하며 쓰레드 종료시 소멸되는 area : Stack, PC Registers, Native Method Stack

### JVM GC의 Root Space  
![jvmm_emory](Root_Space.png)  


### Heap 영역(Object 타입의 데이터들)
![jvmm_emory](Heap_Area.png)

새로운 객체는 eden영역에 생성 -> eden이 다 사용되면 GC 발생 -> 살아남은 객체(Reachable)는 S0으로 옮겨지고 그 외에 객체(Unreachable)는 메모리에서 해제 -> 이 과정이 계속해서 발생해서 S0이 다 사용되면 -> S0에 있던 객체는 S1으로 이동 age 값이 증가 -> S1이 다 사용되면 S1에 있던 객체는 S0으로 이동 age 값이 증가 -> age 값이 특정 수준 이상이 되면 Old Generation으로 영역으로 옮겨진다(Promotion) -> 해당 과정 계속해서 반복

### Memory Leak  
애플리케이션에서 사용되지 않지만 의도치 않게 참조가 있는 객체는 가비지 컬렉션 대상이 아니며 잠재적인 메모리 누수가 발생할 수 있다.  
> Silly mistakes silently grow up to be a monster.
 
__어리석은 실수는 조용히 자라나 괴물이 된다.__  
- Autoboxing  
![autoboxing](메모리_예제코드.png)  
autoboxing으로 인해 sum = sum + l이 모든 반복에서 새 객체를 생성하여 1000개의 객체가 생성된다.  
-> 가능한 한 primitive data를 사용한다. 위 예제에선 Long 대신 long을 사용  
- Chache 사용  
![Cache](Cache.png)  
객체의 레퍼런스를 캐시에 넣어 놓고 캐시를 비우는 것을 잊으면 객체가 애플리케이션에서 더 이상 필요하지 않더라도 map이 강한 참조를 보유하고 있어 GC를 할 수 없어 leak 위험이 있다.  
-> 캐시의 키에 대한 레퍼런스가 캐시 밖에서 필요 없어지면 해당 엔트리를 캐시에서 자동으로 비워주는 WeakHashMap을 쓰는 방법도 있다.  
- close()  
```java
try
{
  Connection con = DriverManager.getConnection();
    ...
    con.close();
}

catch(exception ex)
{
}
```  
이 예제 코드는 예외가 발생하는 경우에는 close()가 되지않아서 메모리의 불필요한 사용이 된다.  
-> 닫는 작업에 항상 신경쓰자  

- 참조 관련 다른 예시
```java
import java.util.HashMap;
import java.util.Map;
public class CustomKey {
       public CustomKey(String name)
       {
              this.name=name;
       }
       private String name;
       public static void main(String[] args) {
              Map<CustomKey,String> map = new HashMap<CustomKey,String>();
              map.put(new CustomKey("Shamik"), "Shamik Mitra");
              String val = map.get(new CustomKey("Shamik"));
              System.out.println("Missing equals and hascode so value is not accessible from Map " + val);
       }
}
```

위 예시에서 val는 map에 참조가 있지만 애플리케이션이 액세스를 할 순 없다. GC에 대상이 되지않지만 쓸모없는 객체가 되었다.

- 메모리 직접 관리  
```java
public class Stack {

    private Object[] elements;

    private int size = 0;

    private static final int DEFAULT_INITIAL_CAPACITY = 16;

    public Stack() {
        this.elements = new Object[DEFAULT_INITIAL_CAPACITY];
    }

    public void push(Object e) {
        this.ensureCapacity();
        this.elements[size++] = e;
    }

    public Object pop() {
        if (size == 0) {
            throw new EmptyStackException();
        }

        return this.elements[--size]; // 주목!!
    }

    private void ensureCapacity() {
        if (this.elements.length == size) {
            this.elements = Arrays.copyOf(elements, 2 * size + 1);
        }
    }
}
```

stack의 구현체는 size보다 큰 부분에 있는 값들은 필요없이 메모리를 차지하고 있다.  
```java
   public Object pop() {
        if (size == 0) {
            throw new EmptyStackException();
        }

        Object value = this.elements[--size];
        this.elements[size] = null;
        return value;
    }
```

pop메서드 부분을 객체를 꺼내준 부분을 null로 설정해서 GC가 발생할 때 레퍼런스가 정리되게 할 수 있다.  
-> 메모리를 직접 관리 할 때 프로그래머만이 필요한 부분과 필요없는 부분을 알기 때문에 직접 해당 레퍼런스를 null로 만들어서 명시해야한다.

### 출처 
[예제코드](https://dzone.com/articles/memory-leak-andjava-code)  
[참조영상](https://youtu.be/FMUpVA0Vvjw)
