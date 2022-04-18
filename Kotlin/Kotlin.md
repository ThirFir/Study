## 목차 ##
1. [변수 선언](#변수-선언)
2. [코틀린에서의 NULL](#코틀린에서의-null)
3. [배열과 리스트](#배열과-리스트)
4. [반복문](#반복문)
5. [조건문 When](#조건문-when)
6. [함수](#함수)
7. [클래스](#클래스)
8. [데이터 클래스](#데이터-클래스)

# **변수 선언** #
### **기본 형태** ###
* 변경 가능한 변수
    * var 변수이름 : 타입 = 값
* 변경 불가능한 상수
    * val 변수이름 : 타입 = 값

타입이 명확한 경우 타입은 생략 가능하며, 선언과 동시에 초기화를 해주어야 한다.
```
val v1 : Int = 1
var v2 = 1

v1 = 2   // Compile Error.
v2 = 2
```
<br/>원시 타입(Int, Boolean, Double ... )이 아닌 경우에는 **lateinit var** 키워드를 이용하여 초기화를 늦출 수 있다.
```
lateinit var str : String

str = "Hello"
```
<br/>

# **코틀린에서의 NULL** #


코틀린에서는 변수가 null일 수 있는지 아닌지가 중요하고, 자바에서 흔히 발생하는 **NullPointerException**을 컴파일 시간에 잡아주므로 안전성이 높다. <br/>? 키워드를 사용하여 null의 가능성을 판단하며 사용법은 다음과 같다.

```
var v1 : Int = 1     // v는 절대 null이 될 수 없음.
v1 = null            // Compile Error.
var v2 : Int? = 1    // 변수 v2는 null일 수도 있고 아닐 수도 있음.
v2 = null            // OK.
```   

null일 가능성이 있는 어떤 클래스의 메소드를 사용할 때는 변수 뒤에 **?** 를 붙여줘야 한다.

```
var name_not_null : String = "Lee"
var name_can_null : String? = null

var name1 : String = name_not_null.uppercase()   // OK.
var name2 : String = name_can_null.uppercase()   // Compile Error.
var name3 : String = name_can_null?.uppercase()   // Compile Error.
var name4 : String? = name_can_null?.uppercase()  // OK.  name_can_null이 null 이면 null반환.
```

어떤 변수가 해당 위치에서 반드시 null이 아님을 확신할 수 있으면 **!!** 키워드를 사용할 수도 있다. 하지만 이는 **런타임 에러**를 발생시킬 수 있다.

```
var name1 : String? = "Lee"
var name2 : String = name1!!.uppercase()
```
<br/>

# **배열과 리스트** #
1. **Array**
    * 값의 변경이 용이하나 크기가 고정된다.
    * 연속적인 메모리 공간에 저장된다.
2. **List**
    * 값의 변경(+ 삽입, 삭제)이 불가능하다.
    * 불연속적인 메모리 공간에 저장된다.
3. **arrayList**
    * 값의 변경이 가능한 List.
4. **MutableList**
    * arrayList와 기능은 동일하나 형태만 다르다.

선언 시에는 val 또는 var 키워드를 사용할 수 있다. 두 방식 모두(List형이 아닐 경우) 내부 값의 변경은 가능하나, val 키워드를 사용 시에는 해당 변수가 가리키는 곳(참조 값)을 변경할 수는 없다.
```
val array : Array<Int> = arrayOf(1,2,3)
val list : List<Int> = listOf(1,2,3)

array[0] = 5        // OK.
list[0] = 5         // 변경 불가능. Compile Error.

val arrayList = arrayListOf<Int>()

arrayList.add(10)
arrayList.add(20)
arrayList.remove(0)
```
변수 타입에 Any를 사용할 경우 여러 타입의 값을 저장할 수 있다.
```
val array : Array<Any> = arrayOf(1, "HI", 3.14)
val array2 = arrayOf(2, "Hello", 9.81)      // 생략도 가능
```
<br/>

# **반복문** #
### **기본 형태** ###
* for(변수이름 in range)
    * 변수의 타입은 제시하지 않아도 된다.
* while(조건문)
    * 다른 언어와 사용법 동일

-기본적인 사용법
```
for(v in 1..10)         // 10 포함 X
    println(v)          // 1 2 3 ... 8 9
for(v in 1 until 10)    // 10도 포함
    println(v)          // 1 2 3 ... 9 10
for(v in 1..10 step 2)  
    println(v)          // 1 3 5 7 9
for(v in 10 downTo 1)
    println(v)          // 10 9 8 7 ... 1
```

for 문을 이용하여 배열 또는 리스트에 모든 인덱스를 방문할 수 있다.
```
val arrayList = arrayListOf(1,2,3,4,5,6,7,8,9,10)

for(v in arrayList)
    println(v)      // 1 2 3 ... 9 10
```

배열/리스트의 **withIndex()** 메소드를 이용하여 반복문에서 인덱스와 해당하는 값을 모두 이용할 수 있다.
```
val arrayList = arrayListOf(1,2,3,4,5,6,7,8,9,10)

for((index, v) in values.withIndex())
    println("arrayList[${index}] = ${v}")   // arrayList[0] = 1 arrayList[1] = 2 ...
```
<br/>

# **조건문 When** #
다른 언어의 switch문과 동일한 동작을 하는 키워드로, 형태가 간단하게 바뀌었다.
### **기본 형태** ###
```
* when(어떤 변수){
    케이스1 -> {동작}
    케이스2 -> {동작}
    ...
    else -> {동작}
}
```
```
var value = getValue();

when(value){
    0 -> {println(0)}
    1,2,3 -> {println(1~3)}
    in 4..10 -> {println(4~10)}
    else -> {println(11~)}
}
```
다음의 형태로도 사용 가능하다. 이때 else는 반드시 포함되어야 한다.
```
var a = when(value){
    1 -> 10
    2 -> 20
    else -> 30      // else가 반드시 포함되어야 함.
}
```
<br/>

# **함수** #
### **기본 형태** ###
* fun 함수명(매개변수1 : 타입, 매개변수2 : 타입, ...) : 반환타입 { }

반환 값이 없을 경우 반환타입을 Unit으로 명시하거나, 생략할 수 있다.
```
fun getMax(a : Int, b : Int) : Int {
    if(a > b) return a
    else return b
}

...
var k = getMax(1, 2)
```
<br/>

# **클래스** #
### **기본 형태** ###
접근 제한자를 명시하지 않을 경우 기본적으로 public 접근 제한자를 갖는다.
```
public class Person{            // public은 생략 가능
    var name : String = ""
    var age = 0                 // 선언과 동시에 초기화 필요  
}
...
val p = Person()
```

### **기본 생성자(주 생성자)** ###
기본 생성자(주 생성자)는 클래스명 옆에 **constructor** 키워드를 붙여 이용한다. 이때 constructor키워드는 생략 가능하다.
```
class Person constructor (name : String, age : Int) {
    var name = name
    var age = age
}
```
```
class Person (name : String, age : Int) {
    var name = name
    var age = age
}
```
내부에서의 변수 선언을 다음과 같이 한줄로 줄이는 것도 가능하다.
```
class Person (var name : String, var age : Int) { ... }
```
### **초기화 블록** ###
기본 생성자를 사용할 경우, 기본 생성자에는 블록이 없으므로 어떠한 코드를 포함할 수가 없다. 이때 **init**이라는 키워드를 이용하여 초기화 블록을 설정하여 내부에 코드를 작성할 수 있다.
```
class Person (var name : String, var age : Int) { 
    init {
        println("Initializing block")
        ...
    }
}
```

### **보조 생성자(생성자 오버로딩)** ###
기본 생성자(주 생성자) 이외에 추가적인 생성자가 필요할 경우, constructor 키워드를 사용하여 추가적인 생성자를 사용할 수 있다.
이때 아래와 같은 방식으로 반드시 **주 생성자**를 **상속**받아야 한다.
```
class Person (var name : String, var age : Int) { 
    constructor (name : String) : this(name, 0) {
        this.name = name
    }
}
```
<br/>

constructor 키워드를 클래스명 옆에 사용하지 않고 클래스 내부에서 사용할 수 있다. 다만, 이 경우에는 주 생성자가 없는 것이다.
```
class Person{
    var name : String = ""
    var age : Int = 0
    constructor(name : String, age : Int){
        this.name = name
        this.age = age
    }
    constructor(name){
        this.name = name
    }
}
```

### **호출 순서** ###
보조 생성자를 호출하면, 우선 상속받은 주 생성자를 먼저 호출하여 클래스를 초기화한다. 이후 보조 생성자를 실행한다.
```
class Person (var name : String, var age : Int) { 
    init {
        println("Initializing block. name : $name, age : $age")
    }
    constructor(name : String) : this(name, 0) {
        println("Secondary constructor. name : $name, age : $age")
    }
}
...
val p = Person("Lee")
```
#### **결과** ####
```
Initializing block. name : Lee, age : 0
Secondary constructor. name : Lee, age : 0
```

### **기본값(default) 설정** ###
변수에 기본값을 설정할 수 있다.
```
class Person (var name : String = "default", var age : Int = "0") { ... }

val p0 = Person()                           // name = "default", age = 0
val p1 = Person("Kim")                      // name = "Kim", age = 0
val p2 = Person("Park", 30)                 // name = "Park", age = 30
val p3 = Person(age = 30, name = "Park")    // 매개변수의 순서를 바꾸더라도 변수명을 앞에 추가한다면 가능
```
<br/>

# **데이터 클래스** #
다음과 같이 클래스를 선언함으로써 별도의 메소드 생성없이 유용한 기본 메소드들을 이용할 수 있다.
```
data class Person(var name : String, var age : Int)
```
### **메소드** ###
1. getter : Type
2. setter
3. equals(other) : Boolean
4. hashCode() : Int
5. toString() : String
6. copy() : Class
<br/>기타 등등

<br/>  

# **싱글톤** #
**싱글톤(Singleton) 패턴이란 ?**
- 객체의 인스턴스가 오직 1개만 생성되는 패턴

**싱글톤 패턴의 장점**
- 메모리의 낭비를 방지
- 다른 클래스들 간 데이터 공유가 용이

**문제점**
- 멀티스레드 환경에서 안전하지 않음
- 자식클래스를 가질 수 없음
- 내부 상태를 변경하기 어려움

<br/>

### **코틀린에서 싱글톤** ###
- 주 생성자, 부 생성자를 정의할 수 없음
- 초기화하기 위해 init블록을 사용할 수 있음
```
object Singleton {
    var a : Int = 0
    init{ ... }
    fun func() { ... }
}

...
Singleton.func()
```

## **companion object** ##
클래스 내부에 선언하며, companion object 블럭 내부에 속한 멤버들은 외부에서 [클래스 이름.멤버]의 형태로 호출할 수 있다. 또한 블럭 내부에서 클래스의 private 멤버에 접근할 수 있어 [팩토리 메소드](#팩토리-메소드)에 유용하다.
```
class CompanionClass private constructor(var msg : String) {
    companion object {
        fun showMessage() {
            val cClass = CompanionClass("Hello")
            println("${cClass.msg}")
        }
    }
}

...
>>> CompanionClass.showMessage()
```
```
Hello
```

<br/>

# **상속** #
코틀린에서 class는 기본적으로 final 이다. 따라서 상속 가능한 클래스로 만들기 위해서는 **open** 키워드를 붙여주어야 한다. 메소드를 재정의할 경우, 필수적으로 **override** 키워드를 붙여주어야 한다.
### **기본 형태** ###
```
open class Super { 
    open fun func() { ... }
 }
class Child : Super() { 
    override fun func() { ... }     
 }
```

### **abstract class** ###
추상 클래스를 사용할 때는 3가지 경우가 있다.
```
abstract class Super {
    fun func1() { ... }         // 재정의할 수 없는 메소드
    open fun func2() { ... }    // 재정의할 수 있는 메소드
    abstract fun func3()        // 반드시 재정의해야 하는 메소드
}
class Child : Super() {
    override fun func3() { ... }
}
```
### **interface** ###
```
interface Super {
    fun func() 
}
class Child : Super {

}
```

### **여러 클래스를 상속받는 경우** ###
자바에서 extends와 implements를 구분하여 사용하였는데, 코틀린은 이를 더 간략화하여 사용한다. 인터페이스를 상속받을 경우 클래스명만, 일반 클래스를 상속받을 경우 클래스명()을 작성한다. 여기서 ()는 부모 클래스의 생성자를 호출하는 것으로, 인자가 필요할 경우 인자를 넣어주면 된다.
```
interface SuperInterface { ... }
class SuperClass(var a : Int) { ... }
class Child : SuperInterface, SuperClass(1) { ... }
```
<br/>

# **확장 함수** #
**확장 함수란?**
- 어떤 클래스의 인스턴스가 호출할 수 있는 함수를 클래스 밖에 정의하는 것.
- fun 클래스명.함수명( ... ){ ... }


<br/>

# **const** #
### **val과 const val** ###
- val : 런타임에 결정됨
- const val : 컴파일 시간에 결정됨, 지역 변수로 사용 불가능

```
fun add(a : Int, b : Int) : Int{
    return a + b
}

...
val n = add(1, 2)           // OK
const val m = add(1, 2)     // Error
```

<br/>

# **가변 인자(vararg)** #
**가변 인자(vararg)**
- 개수가 정해지지 않은 인자

#### **사용법** ####
```
fun sum(vararg num : Int) : Int {
    var result = 0
    for(n in num)
        result += n
    return result
}
```

<br/>

# **팩토리 메소드** #
**팩토리 메소드 패턴이란**
- ㅓ