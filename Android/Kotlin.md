## 목차 ##
- [2. 코틀린 기초](#2-코틀린-기초)
    - [2.1 함수와 변수](#21-함수와-변수)
    - [2.2 클래스와 프로퍼티](#22-클래스와-프로퍼티)
    - [2.3 enum과 when](#23-enum과-when)
    - [2.4 이터레이션](#24-이터레이션)
    - [2.5 예외 처리](#25-예외-처리)
- [3. 함수 정의와 호출](#3-함수-정의와-호출)
    - [3.1 코틀린의 컬렉션](#31-코틀린의-컬렉션)
    - [3.2 확장 함수와 확장 프로퍼티](#32-확장-함수와-확장-프로퍼티)
    - [3.3](#3-)
<br/>

# **2. 코틀린 기초** # 

## **2.1 함수와 변수** ##

### **2.1.1 함수** ###

```
fun max(a : Int, b : Int) : Int {
    return if (a > b) a else b
}
```
- 함수의 선언에 fun 키워드를 사용한다.
- 파라미터(매개변수) 선언부에서 파라미터 이름 : 파라미터 타입의 형식으로 작성한다.
- 파라미터 선언 다음에 반환 타입을 명시한다.
- 코틀린에서 if는 식(expression)이다. 이 특성을 이용하여 3항 연산자를 대체할 수 있다.
- 이처럼 본문이 중괄호로 둘러싸인 함수를 **블록이 본문인 함수**라고 부른다.

<br/>

**문(statement)과 식(expression)**
> 식은 값을 만들어내며 다른 식의 하위 요소로 계산에 참여할 수 있다.<br/>
> 반면 문은 자신을 둘러싸고 있는 가장 안쪽 블록의 최상위 요소로 존재하며 아무런 값을 만들어내지 않는다. <br/>
> 자바에서는 모든 제어구조가 문인 반면 코틀린에서는 루프를 제외한 대부분의 제어 구조가 식이다.

이러한 코틀린의 특성을 이용하여 위의 함수를 더 간결하게 표현할 수 있다.
```
fun max(a : Int, b : Int) = if (a > b) a else b
```
- 이러한 함수의 구조에서는 사용자가 반환 타입을 명시하지 않아도 컴파일러가 본문 식을 분석하여 식의 결과 타입을 함수 반환 타입으로 설정한다. 이러한 기능을 **타입 추론**이라 부른다.
- 이처럼 본문이 등호와 식으로 이루어진 함수를 **식이 본문인 함수**라고 부른다.

### **2.1.2 변수** ###

```
var a : Int = 10

val b = 20
val answer = "Hello"
```
- var : 변경 가능한 참조. 변수
- val : 변경 불가능한 참조. 상수
- 타입을 지정하지 않더라도 컴파일러가 식을 분석하여 변수 타입을 결정한다.
- val의 경우, val이 가리키는 참조가 변하는 것이 아니라면, 내부의 값은 변경될 수 있다.

## **2.2 클래스와 프로퍼티** ##

### **2.2.1 프로퍼티** ###
```
/* 자바에서의 클래스 */
public class Person{
    private final string name;
    public Person(String name) {
        this.name = name;
    }
    public String getName() {
        return name;
    }
}
```
자바에서는 데이터를 필드<small>(field)</small>에 저장하며 멤버 필드의 가시성은 private이다. 클래스는 접근자 메서드를 제공하고, getter와 setter가 이에 속한다.
이들 필드와 접근자를 묶어 프로퍼티<small>(property)</small>라 부른다. 코틀린은 이 프로퍼티를 언어 기본 기능으로 제공하며, 자바의 필드와 접근자 메서드를 완전히 대신한다.

```
/* 코틀린 */
class Person {
    val name : String           // 읽기 전용 프로퍼티
                                // 코틀린은 비공개 필드와 getter를 만들어냄
    var isMarried : Boolean     // 쓸 수 있는 프로퍼티
                                // 코틀린은 비공개 필드, getter, setter를 만들어냄
}
```
Person에는 비공개 필드가 들어있고, 생성자가 그 필드를 초기화하며, 게터를 통해 그 비공개 필드에 접근한다.
```
val person = Person("Bob", true)    // new 키워드를 사용하지 않고 생성자를 호출
println(person.name)    // 자바에서와 달리 getName()을 호출하지 않고, name을 직접
                        // 호출해도 코틀린이 자동으로 게터를 호출해준다.
person.isMarried = false // 자바의 setMarried(false)와 동일
println(person.isMarried)
```
> Bob  
> false

### **2.2.2 커스텀 접근자** ###

```
// 직사각형 클래스
class Rectangle(val height : Int, val width : Int) {
    val isSquare : Boolean
        get() { return height == width }
}
```
어떤 직사각형이 정사각형인지 별도의 필드에 저장하지 않고도, 프로퍼티 게터 get()을 이용해 사각형의 너비와 높이가 같은지 검사하면 정사각형 여부를 그때그때 알 수 있다.
프로퍼티에 접근할 때마다 게터가 프로퍼티 값을 매번 다시 계산한다.
이때 get() = height == width 로 작성할 수도 있다.

## **2.3 enum과 when** ##
### **2.3.1 num 클래스** ###
```
enum class Color {
    RED, ORANGE, YELLOW, GREEN, BLUE, INDIGO, VIOLET
}
```
자바와 마찬가지로, enum 클래스 안에 프로퍼티나 메서드를 정의할 수 있다.
```
enum class Color (
    val r : Int, val g : Int, val b : Int   // 상수의 프로퍼티 정의
 ) {
    RED(255,0,0), ORANGE(255, 165, 0),  /* 각 상수를 생성할 때 그에 대한
                                           프로퍼티 값을 지정함 */
    ...
    VIOLET(238, 130, 238);      // 반드시 세미콜론을 붙여야 함
    
    fun rgb() = (r * 256 + g) * 256 + b     // 메서드 정의
}
println(Color.BLUE.rgb())
```
> 255

### **2.3.2 when** ###
```
when(어떤 변수){
    케이스1 -> {동작}
    케이스2 -> {동작}
    ...
    else -> {동작}
}
```
if와 마찬가지로 when도 값을 만들어내는 **식**이다. 따라서 식이 본문인 함수에 when을 바로 사용할 수 있다.
```
fun getResult(a : Int) =
    when (a) {
        1 -> "one"
        2 -> "two"
        ...
        else -> "null"
    }
```
### **2.3.3 when과 임의의 객체를 함께 사용** ###
```
fun mix(c1 : Color, c2 : Color) =
    when (setOf(c1, c2)) {
        setOf(RED, YELLOW) -> ORANGE
        setOf(YELLOW, BLUE) -> GREEN
        setOf(BLUE, VIOLET) -> INDIGO
        else -> throw Exception("Dirty color")
    }
```
### **2.3.4 인자 없는 when 사용** ###
위 함수는 호출될 때마다 여러 Set 인스턴스를 생성하므로 비효율적이다. 이때 다음과 같이 인자가 없는 when 식을 사용하면 불필요한 객체 생성을 막을 수 있다. 다만 가독성에 저하가 있다.
```
fun mixOptimized(c1 : Color, c2 : Color) =
    when {
        (c1 == RED && c2 == YELLOW) ||
        (c1 == YELLOW && c2 == RED) -> ORANGE
        ...
        else -> throw Exception("Dirty color")
    }
```
### **2.3.5 스마트 캐스트** ###
코틀린에서는 is를 사용해 변수의 타입을 검사할 수 있다. 이는 자바의 instanceof와 비슷하다. 하지만 자바에서 어떤 변수의 타입을 instanceof로 확인한 이후 그 타입에 속한 멤버에 접근하기 위해서는 명시적으로 변수타입을 캐스팅해야 한다.  
코틀린에서는 is로 타입을 검사한 후에는 이러한 명시적 캐스팅없이 컴파일러가 알아서 캐스팅을 수행해주는데, 이를 **스마트 캐스트**라 칭한다. (단, 검사한 다음에 그 값이 바뀔 수 없는 경우에만 작동함)
```
if(e is Person)
    return e.name
```
코틀린에서 값을 캐스팅할 때는 as 키워드를 사용한다.
```
val a = 1
val b = a as Float
```
### **if와 when의 분기에서 블록 사용** ###
if나 when 모두 분기에 블록을 사용할 수 있고, 그런 경우에 블록 마지막 문장이 블록 전체의 결과가 된다.
fun function() =
    if(...){
        println("1")
        1
    }
    else if(...){
        println("2")
        2
    }
    else -1
    
## **2.4 이터레이션 : while, for** ##
### **2.4.1 while 루프** ###
자바와 동일한 문법을 사용한다.  

### **2.4.2 수에 대한 이터레이션** ###
코틀린에서는 for문에 범위를 이용한다. 코틀린에서의 범위는 닫힌 구간(양끝을 포함)이다.
```
for(i in 1..10)
    println("$i")   // >>> 1 2 3 ... 10
for(i in 10 downTo 1 step 2)
    println("$i")   // >>> 10 8 6 4 2

```

### **2.4.3 컬렉션에서 이터레이션** ###

.withIndex()를 사용하여 인덱스를 같이 얻을 수 있다.
```
var list = arrayListOf("10","11","1001")
for((index, element) in list.withIndex())
    println("$index : $element")
```
> 0 : 10  
> 1 : 11  
> 2 : 1001

### **2.4.4 연산자 in** ###
in 연산자로 어떤 값이 범위에 속하는지 검사할 수 있다. !in도 가능하다.
```
if(a in 1..10)  // a가 1 ~ 10에 있는 값인지
```
또한, Comparable 인터페이스를 구현한 클래스에도 in 연산자를 사용할 수 있다.

## **2.5 예외 처리** ##
다른 클래스처럼 new를 붙이지 않는다.
```
if( ... ) 
    throw IllegalArgumentException(" ... ")
```

또한 코틀린의 throw는 식이므로 다른 식에 포함될 수 있다.
```
val percent = 
    if (n in 0..100) number
    else throw IllegalArgumentException(" ... ")    // 이때는 변수가 초기화되지 않는다.
```

### **2.5.1 try, catch, finally** ###
코틀린은 체크 예외<small>*(RuntimeException을 상속하지 않는 Exception클래스)*</small>와 언체크 예외<small>*(RuntimeException을 상속하는 Exception클래스)*</small>를 구분하지 않는다. 그래서 함수가 던지는 예외를 지정하지 않고 발생한 예외를 잡아도 되고 잡지 않아도 된다.
```
fun readNumber(read : BufferedReader) : Int? {  // 함수가 던질 수 있는 예외를 명시할 필요가 없음
    try {
        val line = read.readLine()
        return Integer.parseInt(line)
    }
    catch(e : NumberFormatException) {
        return null
    }
    finally{
        reader.close()
    }
}
```

### **2.5.2 try를 식으로 사용** ###
역시 블록의 마지막 식이 값의 전체 값이 된다.
```
val n = try {
    ...
    1
} catch (e : NumberFormatException) {
    -1
}
```
<br/>

# **3. 함수 정의와 호출** #

## **3.1 코틀린의 컬렉션** ##
코틀린은 자체 컬렉션을 제공하지 않는다. 표준 자바 컬렉션을 활용함으로써 자바 코드와 상호작용하기가 더 쉽기 때문이다.
하지만 코틀린에서는 자바에서보다 더 많은 기능을 쓸 수 있다.

## **3.2 확장 함수와 확장 프로퍼티** ##
**확장 함수**란 어떤 클래스의 멤버 메서드인 것처럼 호출할 수 있지만 그 클래스의 밖에 선언된 함수를 말한다.
```
package strings
fun String.lastChar() : Char = this.get(this.length - 1)    // 문자열의 마지막 문자를 반환, this 생략 가능

println("Kotlin".lastChar())
```
> n

이처럼 원하는 메서드를 클래스에 새로이 추가할 수 있다.  
여기서 클래스 이름(String)을 **수신 객체 타입**, 확장 함수가 호출되는 대상이 되는 값(this)을 **수신 객체**라고 부른다.  

확장 함수 내부에서는 수신 객체의 메서드나 프로퍼티를 바로 사용할 수 있다. 단, private이나 protected 멤버를 사용할 수는 없다. 따라서 확장 함수가 캡슐화를 깨지는 않는다고 볼 수 있다.

### **3.2.1 임포트** ###
확장 함수를 정의하였더라도 모든 소스코드에서 그 함수를 사용할 수 있는 것은 아니다. 확장 함수를 사용하기 위해서는 그 함수를 임포트해주어야 한다.
```
import strings.lastChar
또는
import strings.*
```
as 키워드를 사용하여 다른 이름으로 부를 수도 있다.
```
import strings.lastChar as last
val c = "Kotlin".last()
```

### **3.2.2 확장 함수의 오버라이드** ###
확장 함수는 정적<small>*(컴파일 시점)*</small> 메서드이므로 오버라이드할 수 없다.
```
open class View {
    open fun click() = println("View")
}
class Button : View {
    override fun click() = println("Button")
}
...
val view : View = Button()
view.click()
```
> Button

### **3.2.3 확장 프로퍼티** ###
확장 함수와 비슷하다. 확장 프로퍼티는 상태를 저장할 적절한 방법이 없기에<small>*(기존 클래스의 인스턴스 객체에 필드를 추가할 수 없음)*</small> 실제로는 아무 상태도 가질 수 없다. 하지만 프로퍼티 문법(get())으로 더 짧게 코드를 작성할 수 있어 편한 경우가 있다.
```
val String.lastChar : Char
    get() = get(length - 1)
```
뒷받침하는 필드가 없으므로 게터를 반드시 정의해야 한다.
var를 사용한다면 set()도 정의해야 한다.

## **3.3 컬렉션 처리** ##

### **3.3.1 가변 인자 함수** ###
vararg 키워드를 이용하여 임의 개수의 인자를 전달받는다.
```
fun listOf<T>(vararg values : T) : List<T> { ... }
```
또한 코틀린에서 가변 인자 함수를 호출할 때 배열을 그냥 넘기는 자바와 달리, 배열을 풀어서 각 원소들을 넘겨주어야 하는데, 이때 사용하는 것이 <strong>스프레드 연산자(*)</strong>이다.
```
fun sum(vararg ns : Int) : Int { ... }
...
val numbers = intArrayOf(1, 2, 3, 4)
val s = sum(*numbers)
```

### **3.3.2 중위 호출, 구조 분해 선언** ###
인자가 하나뿐인 일반 메서드나 확장 함수에 **중위 호출**을 사용할 수 있다. 구조는 다음과 같다.
```
1.to("one")     // 일반적인 방식
1 to "one"      // 중위 호출
```
중위 호출이 가능한 메서드를 선언할 때는 infix 변경자를 함수 선언 앞에 붙여준다.
```
infix fun Any.to(other : Any) = Pair(this, other)
```
여기서 to 함수는 Pair의 인스턴스를 반환한다. 이를 이용하여 다음과 같이 Pair의 두 변수를 즉시 초기화할 수 있다.
```
val (number, name) = 1 to "one"
```
이런 기능을 **구조 분해 선언**이라 부른다.

참고로 to함수는 실제로는 제네릭 함수이다.

## **3.4 문자열과 정규식 다루기** ##
