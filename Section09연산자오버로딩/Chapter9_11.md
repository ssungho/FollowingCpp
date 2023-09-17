# Chapter9_11 대입 연산자 오버로딩, 깊은 복사, 얕은 복사

## 대입 연산자 오버로딩
> **대입 연산자를 오버로딩하기 위해서는 얕은 복사와 깊은 복사에 대한 이해가 필요하다.**

**MyString Class**
```cpp
#include <iostream>
#include <cassert>
using namespace std;

class MyString
{
public:
	char* m_data = nullptr;
	int m_length = 0;

public:
	MyString(const char* source = "")
	{
		// 문자열이 비었는지 확인
		assert(source);

		// 글자수 확인
		m_length = std::strlen(source) + 1;
		m_data = new char[m_length];

		for (int i = 0; i < m_length; i++)
			m_data[i] = source[i];

		m_data[m_length - 1] = '\0';
	}

	~MyString()
	{
		delete[] m_data;
	}

	char* getString() { return m_data; }
	int getLength() { return m_length; }
};
```
- `MyString` 클래스는 문자열을 구현한 클래스로 문자열을 저장할 `char*`타입 변수와, 문자열의 길이를 저장할 `int`타입 변수를 멤버로 가지고있다. 

- 생성자는 `char*`타입의 문자열을 인자로 받고 `m_data`와 `m_length`를 설정한다.

### 얕은 복사(Shallow copy)
**데이터가 가리키는 메모리 주소의 값을 복사한다.**

```cpp
int main()
{
	MyString hello("Hello");

	cout << (int*)hello.m_data << endl;
	cout << hello.getString() << endl;

	{
		MyString copy = hello;
		cout << (int*)copy.m_data << endl;
		cout << copy.getString() << endl;
	}

	cout << hello.getString() << endl;

	return 0;
}
```
- MyString hello("Hello");
  - MyString타입의 hello생성하며 m_data를 "Hello"로 설정한다.
- cout << (int*)hello.m_data << endl;
  - hello의 m_data가 가리키는 주소를 출력하기 위해 (int*)로 형변환하여 출력한다.
- cout << hello.getString() << endl;
  - getString()을 이용하여 m_data를 출력한다.

이후 스코프를 만들어 `hello`의 정보를 이용하여 새로운 MyString 객체 `copy`를 생성과 동시에 초기화 한다.
- `copy`를 생성할 때 `hello` 객체를 복사하면서 기본 복사 생성자가 호출된다.
- 기본 복사 생성자는 `hello` 객체가 가진 `char* m_data`가 가리키는 메모리 주소를 동일하게 갖게 된다.
  - 메모리 주소를 복사하는 셈
> ***이렇게 별개의 주소를 갖게 되는 것이 아닌 같은 메모리 주소를 가리키게 되는 것을 얕은 복사라고 한다.***

이렇게 얕은 복사를 이용해 객체를 복사하여 생성한 `copy`의 `m_data`주소를 출력해보면 이전에 출력한 `hello`의 `m_data`주소는 같다. (getString()을 통한 출력된 문자열도 같음)
  
하지만 스코프를 벗어나 copy 객체가 사라진다면 MyString의 소멸자가 실행되고 `copy`가 가진 `m_data`를 가리키던 포인터는 사라지게 되며 같은 메모리 주소를 참조하고 있던 원본 객체인 `hello`에게도 영향을 미치게 된다.
- cout << hello.getString() << endl;
  - 이상한 문자열이 출력됨
    - 가리키던 주소가 사라졌기 때문

### 깊은 복사(Deep copy)
**새로운 메모리 공간을 할당하여 값을 복사한다.**

복사 생성자를 구현하여 메모리의 주소를 가리키는 포인터를 복사하는 것이 아닌 메모리를 새로 할당하고 내용을 복사한다.
```cpp
class MyString
{
public:
	char* m_data = nullptr;
	int m_length = 0;

public:
	MyString(const char* source = "")
	{
		// 문자열이 비었는지 확인
		assert(source);

		// 글자수 확인
		m_length = std::strlen(source) + 1;
		m_data = new char[m_length];

		for (int i = 0; i < m_length; i++)
			m_data[i] = source[i];

		m_data[m_length - 1] = '\0';
	}

	MyString(const MyString& source)
	{
		cout << "Copy constructor " << endl;

		m_length = source.m_length;

		if (source.m_data != nullptr)
		{
			m_data = new char[m_length];

			for (int i = 0; i < m_length; i++)
				m_data[i] = source.m_data[i];
		}
		else
			m_data = nullptr;
	}

	MyString& operator = (const MyString& source)
	{
		cout << "Assignment operator " << endl;

		if (this == &source)
			return *this;

		delete[] m_data;
		m_length = source.m_length;

		if (source.m_data != nullptr)
		{
			m_data = new char[m_length];

			for (int i = 0; i < m_length; i++)
				m_data[i] = source.m_data[i];
		}

		return *this;
	}

	~MyString()
	{
		delete[] m_data;
	}

	char* getString() { return m_data; }
	int getLength() { return m_length; }
};
```
- MyString(const MyString& source)
  - 복사 생성자로 새로운 메모리를 할당받아 source의 m_data 값을 복사한다.
```cpp
MyString hello("Hello");
MyString str1 = hello; // copy constructor 실행
```
`hello.m_data`의 주소와 `str1.m_date`의 주소는 다르다.
- `str1`이 깊은 복사를 통해 생성되었기 때문


### 대입 연산자 = 오버로딩
대입 연산자는 객체를 다른 객체에 대입을 할 경우에 호출된다.


**깊은 복사와의 차이**
- 대입 연산자는 이미 생성한 객체를 대입할 때 호출된다.
- 깊은 복사는 객체를 생성과 동시에 초기화 할 경우 호출된다.

```cpp
int main()
{
	MyString hello("Hello"); // hello 생성

	MyString str1 = hello; // str1(hello); , 복사 생성자 호출

	MyString str2; // str2 생성

	str2 = hello; // str2에 hello를 대입 , 대입 연산자 호출

	return 0;
}
```

> *`MyString str1 = hello;`에서는 `str1`을 생성하면서 이미 존재하는 `hello`라는 객체를 복사하기 때문에 복사 생성자가 호출되고,*<br>
>  *`str2 = hello;`에서는 `str2`라는 객체는 이미 생성했고 `hello`라는 객체를 `str2`에 대입하는 것이기 때문에 대입 연산자가 호출된다.*

```cpp
MyString& operator = (const MyString& source)
{
	cout << "Assignment operator " << endl;

	if (this == &source)
		return *this;

	delete[] m_data;
	m_length = source.m_length;

	if (source.m_data != nullptr)
	{
		m_data = new char[m_length];

		for (int i = 0; i < m_length; i++)
			m_data[i] = source.m_data[i];
	}

	return *this;
}
```
- 대입 연산은 이항연산이지만 멤버 함수로 오버로딩 하기 때문에 하나의 매개변수만 받는다.
  - 왼쪽 피연산자가 함수를 호출하는 객체가 되고 오른쪽 피연산자가 인자가 됨
- 자기 자신을 대입하는 경우 자기 자신을 반환한다.
- 자신이 가지고 있던 데이터를 지워주고 새로운 공간을 할당받아 source가 가진 m_data로 값을 채운다.
  - 깊은 복사와 같은 방식으로 값을 채워준다.

### 깊은 복사 확인
깊은 복사를 위한 복사 생성자를 구현했다면 아래 코드를 다시 실행했을 때 이상한 값이 출력되지 않는다.
```cpp
int main()
{
	MyString hello("Hello");

	cout << (int*)hello.m_data << endl;
	cout << hello.getString() << endl;

	{
		MyString copy = hello;
		cout << (int*)copy.m_data << endl;
		cout << copy.getString() << endl;
	}

	cout << hello.getString() << endl;

	return 0;
}
```

**출력**
```
00000266D0840460
Hello
Copy constructor
00000266D08404B0
Hello
Hello
```
> ***`hello`와 `copy`의 `m_data`는 다른 주소를 가리키고 있기 때문에 `copy`가 소멸하더라도 `hello`의 `m_data`는 변화가 없다.***