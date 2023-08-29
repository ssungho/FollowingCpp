# Chapter8_05 위임 생성자(Delegating Constructors)
**생성자가 생성자를 불러오는 것을 위임 생성자라고 한다.**

## 일반 생성자 사용
### 학생의 id와 name을 입력받는 생성자
```cpp
#include <iostream>
#include <string>
using namespace std;

class Student
{
private:
	int m_id;
	string m_name;

public:
	Student(const int& id_in, const string& name_in) : m_id(id_in), m_name(name_in) // id와 name을 입력받음
	{ }

	void print()
	{
		cout << m_id << " " << m_name << endl;
	}
};

int main()
{
	Student st1(0, "Jack Jack");

	return 0;
}
```
`Student`를 인스턴스화할 때 생성자를 통해 `id`와 `name`을 입력받으면 깔끔하게 작성할 수 있다.


그런데 `id`는 기본값을 아무거나 주면 되고 `name`만 입력받는 생성자를 만들고 싶다면 **생성자의 매개변수에  기본값을 설정해 주면 될 것이다.**

```cpp
// ... 
class Student
{
private:
	int m_id;
	string m_name;

public:
	Student(const string& name_in) : m_id(0), m_name(name_in)
	{ }

	Student(const int& id_in = 0, const string& name_in = "default") : m_id(id_in), m_name(name_in)
	{ }

	void print()
	{
		cout << m_id << " " << m_name << endl;
	}
};

int main()
{
	Student st1(0, "Jack Jack"); // id와 name 모두 입력받는 생성자
	Student st2("Dash"); // name만 입력받는 생성자

	return 0;
}
```

매개변수의 기본값은 오른쪽 매개변수부터 작성되기 때문에 `name`의 기본값을 지정해 줘야 `id`의 기본값도 설정해 줄 수 있음


> **위 방법은 객체지향 관점에서 봤을 때, 매우 좋지 못한 방법이다.** <br>
> **어떠한 기능을 하는 코드는 한 곳에만 나와야 가장 좋음** <br>
> - ***수정과 디버깅(유지보수)에 유리함***

## 위임 생성자 사용
**생성자가 생성자를 호출함하여 값을 초기화한다.**

###  c++11 이후의 방법
**다른 생성자를 호출할 때 매개변수를 입력하여 값을 초기화한다.**
```cpp
class Student
{
private:
	int m_id;
	string m_name;

public:
    // 생성자가 다른 생성자를 호출 
	Student(const string& name_in) : Student(0, name_in)
	{ }

	Student(const int& id_id, const string& name_in) : m_id(id_in), m_name(name_in)
	{ }
};
```
> `Student(const string& name_in) : Student(0, name_in)`를 실행하면 `Student(const int& id_id, const string& name_in) : m_id(id_in), m_name(name_in)`이 생성자로 (0, name_in)을 넘기는 것

### 초기화 함수를 따로 생성하는 방법
**init(Initialize)함수를 만들어서 값을 초기화한다.**
```cpp
class Student
{
private:
	int m_id;
	string m_name;

public:
	Student(const string& name_in) : Student(0, name_in)
	{
		init(0, name_in); // 초기화 함수를 이용하여 값을 설정
	}

	Student(const int& id_in, const string& name_in)
	{
		init(id_in, name_in); // 초기화 함수를 이용하여 값을 설정
	}

    // 초기화 함수
	void init(const int& id_in, const string& name_in)
	{
		m_id = id_in;
		m_name = name_in;
	}
};
```
초기화 함수를 이용하여 다른 생성자에서 값을 초기화하여 사용한다.
> **최근 문법보다 초기화 함수를 사용하는 경우를 권장하는 경우가 있음**<br>
> ~~*개인적으로 아래 방법을 선호함*~~