# Chapter8_11 정적 멤버 함수

## 정적 멤버 함수를 이용하여 private 정적 멤버를 불러오는 방법
**정적 멤버 변수가 `private`로 선언되었을 경우 외부에서 인스턴스 생성 없이 값을 불러오기 위해서 getter계열의 멤버 함수를 `static`으로 만들어 주면 된다.**
```cpp
#include <iostream>
using namespace std;

class Something
{
private:
	static int s_value;

public:
	static int getValue()
	{
		return s_value;
	}
};

int Something::s_value = 1024;

int main()
{
	cout << Something::getValue() << endl;

	Something s1;
	cout << s1.getValue() << endl;

	return 0;
}
```
`Something::getValue()`를 이용하여 정적 멤버 변수인 `s_value`의 값을 불러올 수 있음

## 정적 멤버 함수의 특징
**정적 멤버 함수는 `this`포인터를 사용할 수 없다.**
```cpp
class Something
{
	// 모든 Something 인스턴스에서 접근이 가능하다.
private:
	static int s_value;
	int m_value;

public:
	static int getValue()
	{
		return s_value;
        // return this->s_value; 불가능
        // return this->m_value; 불가능
	}

	int temp()
	{
		return this->s_value + this->m_value;
	}
};
```
정적 멤버 함수에서는 `this`로 접근할 수 있는 모든 것이 안된다.
`s_value`같은 경우 정적으로 메모리에 올라가 있는 상태기 때문에 바로 접근할 수 있었지만, `this`를 사용해서 접근할 수는 없다.<br>
`m_value`는 일반 멤버 변수이기 때문에 정적 멤버 함수인 `getValue()`에서 접근할 수 없는 것

## 일반적인 함수 포인터와 정적 멤버 함수 포인터의 차이
### 일반적인 멤버 함수 포인터
**멤버 함수 포인터의 경우 인스턴스에 종속적으로 설계되어 있다.**
```cpp
// temp()에 대한 함수 포인터 fptr
int (Something::*fptr1)() = &Something::temp;

// s2인스턴스에서 fptr1 사용
cout << (s2.*fptr1)() << endl;
```
`fptr1`은 `Something`클래스의 `temp()`를 가리키고 있으며 이 함수 포인터를 사용하기 위해서는 이 함수가 어떤 객체의 정보를 가지고 사용될 것인지를 알아야 하기 때문에 ***클래스의 인스턴스가 필요하다.***<br>
- `s2`의 정보를 가지고 `fptr1`을 실행시킨다.
- fptr1에서 s2의 `this포인터`를 사용하는 형태

### 정적 멤버 함수 포인터
**정적 멤버 함수 포인터는 인스턴스에 종속적이지 않는다.**
```cpp
// static member function getValue를 담는 함수 포인터
int (* fptr2)() = &Something::getValue;

// 인스턴스 없이 출력
cout << fptr2() << endl;
```
`fptr2`는 정적 멤버 함수인 `getValue`를 담고있기 때문에 포인터를 선언할 때 `Something`에 속해있는 포인터라고 선언할 필요가 없다.
포인터 사용시 인스턴스가 없어도 사용 가능하다.
- 인스턴스 내부의 `this포인터`를 사용하는 형식이 아닌 정적인 데이터를 가지고 실행하기 때문

> ***정적 멤버 함수에서는 this 포인터를 사용할 수 없음***