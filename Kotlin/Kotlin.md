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

반환 값이 없을 경우 반환타입은 Unit으로 명시하거나, 생략할 수 있다.
```
fun getMax(a : Int, b : Int) : Int {
    if(a > b) return a
    else return b
}

...
var k = getMax(1, 2)