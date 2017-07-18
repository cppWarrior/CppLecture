# C++열혈강의 Chapter1,2


## 어떤 내용이 있나요?

Chapter 1, 2는 C언어를 배웠다는 가정 하에 C++에 추가된 요소들을 설명하는 챕터들입니다.

크게 3가지를 설명하는데

첫 번째, 함수 오버로딩(Function Overloading)
두 번째, 이름공간 (Namespace)
세 번째, 참조자 ‘&’

입니다!

그 외에도 inline함수, Bool자료형, new&delete가 있지만 나올 때 잠시 설명하면 되는 수준이거나 뒤에 더 자세히 다루는 내용이기에 생략했습니다.


### 함수 오버로딩

C언어를 배웠을 때를 생각해보면...
동일한 이름의 함수를 정의할 때 컴파일 오류가 발생합니다.

```cpp
int MyFunc(int num){
	num++;
	return num;
}

int MyFunc(int a, int b){
	Return a+b;
}

//컴파일 에러!!!
```

하지만 C++에서는 매개변수의 갯수가 다르면 가능하며, 이를 '함수 오버로딩'이라고 합니다.

예외사항으로 

```cpp
int MyFunc(int n){ … }
void MyFunc(int n){ … } 
```
같은 경우에는 컴파일 에러가 납니다.

왜냐하면 함수 MyFunc()을 사용했을 때, 반환형이 int인지 void인지 알 수 없기 때문입니다.



### 이름공간 (namespace)

C++을 처음 배우면서 'using namespace std'를 자주 보았을 것입니다.
namespace는 큰 프로젝트를 나눠서 작업을 할 때 변수나 함수가 겹치는 것을 방지하기 위해 만들어졌습니다.

예를들어 A 프로그래머와 B 프로그래머가 동일한 함수 void MyFunc()를 만들었다면, 컴파일 시 에러가 날 수 밖에 없습니다. 

하지만 namespace를 활용하면

```cpp
namespace programmerA{
	void MyFunc() {…}
} 

namespace ProgrammerB{
	Void MyFunc() {…}
}
```

로 이름으로 인한 컴파일 에러가 나지 않습니다,

서로 다른 namespace에 있는 MyFunc()를 사용하려면

:: 을 사용하여

```cpp
programmerA::MyFunc();
programmerB::MyFunc(); ```
와 같은 형태로 사용하시면 됩니다.


심지어 namespace 안에 namespace를 넣을 수 도 있습니다.


```cpp
namespace programmerC{
	namespace SubOne{
	int num = 3;}
	namespace SubTwo{
	int num = 4;}
}
```
namespace 안에 있는 namespace를 사용하려면 ::을 한번 더 사용해주시면 됩니다.

```cpp
std::cout<< programmerC::SubOne::num;
std::cout<< programmerC::SubTwo::num;
 ```
위 예는 결과적으로 3,4를 출력하게 됩니다.

namespace를 사용하면 ::을 자주 쓰게되는데 이게 귀찮아질 수 있습니다.
그래서 using을 사용해서

```cpp
using programmerA::Myfunc; // ex2-1에 사용된 namespace
MyFunc();
```
이런 식으로 이름공간 지정 없이 함수를 호출 할 수 있습니다.


우리가 흔히 썼던 cout, cin 도 사실
```cpp
using std::cout;
using std::cin; ```
을 선언해서 사용해야 합니다.

하지만 using을 계속 쓰면 불편하니 
```cpp using namespace std;```
로 한 번에 사용합니다. 

책에 있는 설명 그대로 따오면 
using namespace는
‘이름공간 std에 선언된 모든 것에 대해 이름공간 지정의 생략’을 명령하는 것 입니다.


###참조자

참조자는 쉽게 말하면 변수의 별명이라고 생각하시면 됩니다.
```cpp
int num1 = 5;
int &num2 = num1;
```
참조자를 선언하는 수에 제한이 없으며, 참조자를 대상으로 참조자를 선언할 수 있습니다.

```cpp
//위 예제
int &num3 = num2;
```

참조자는 변수에 대해서만 선언되고, 선언하는 순간 다른 변수를 참조해야만 합니다.
```cpp
int &num4; ( X )
```

쓸모없어 보이는 참조자는 함수에서 잘 활용됩니다.
 
포인터를 처음배웠던 기억을 더듬어보면
두 변수의 값을 바꾸려고 할 때,

```cpp
void swap(int a, int b){
int temp;
temp = a;
a = b;
b = temp; } ```
로 정의되어 있는 함수를 사용하면
원하는대로 실행되지 않죠… 그 이유는 함수에 변수의 참조된 값만이 전달되기 때문입니다.

하지만 포인터를 사용하여 주소값을 전달하면 원하는 결과를 얻을 수 있습니다.
```cpp
void swap(int *a, int *b){
int temp;
temp = *a;
*a = *b;
*b = *temp; }```

여기서 방금 알게된 참조자를 사용하면
```cpp
void swap(int &a, int &b){
int temp;
temp = a;
a = b;
b = temp;}```
로 간단하게 사용할 수 있습니다.

이것이 가능한 이유는 앞에서 참조자가 변수의 별명이라고 했는데,
사실 참조자는 참조된 변수가 할당된 메모리 공간을 공유고 있습니다.
참조자는 변수명 뿐만아니라 변수의 주소까지 참조하고 있었던 것입니다...

따라서 포인터로 변수의 주소값을 전달할 수고없이 간단하게 참조자를 사용하면 됩니다. 
