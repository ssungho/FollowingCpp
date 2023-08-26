# 캡슐화, 접근 지정자, 접근 함수

## 캡술화(Encapsulation)
**클래스 안에 서로 연관있는 속성과 기능들을 하나의 캡슐(capsule)로 만들어 데이터를 외부로부터 보호하는 것**

### 구조체로 정의한 Date
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

struct Date
{
	// 멤버 변수라는 것을 나타내는 m_
	int m_month;
	int m_day;
	int m_year;
};

int main(void) 
{
	// struct로 정의한 Date today 초기화 방법
	Date today {8, 27, 2023};

	today.m_month = 8;
	today.m_day = 27;
	today.m_year = 2023;

	return 0;
}
```
구조체로 정의한 Date타입인 today는 Uniform initialization, 대입등으로 값을 초기화 할 수 있다.
> **외부에서 struct 멤버에 접근 가능**

## 클래스로 정의한 Date
```cpp
// 클래스로 변경
class Date
{
	// 멤버 변수라는 것을 나타내는 m_
	int m_month;
	int m_day;
	int m_year;
};
```
구조체를 클래스로 변경시 외부에서 멤버 변수들을 접근할 수 없게 된다.
> 접근 지정자가 기본값이 `private`으로 지정되어 있기 떄문

## 접근 지정자(Access specifiers)
**접근 지정자는 클래스 내부의 멤버를 접근할 수 있는 범위를 지정해 주는 것**
```cpp
// 클래스로 변경
class Date
{
public: // access specifiers

	// 멤버 변수라는 것을 나타내는 m_
	int m_month;
	int m_day;
	int m_year;
};
```
접근 지정자를 `public`으로 선언후 외부에서 클래스의 멤버에 접근 가능하다.
> **`public`는 외부에서 접근 가능**<br>
> **`private`는 불가능**<br>
> **`protected`는 상속된 클래스에서만 접근 가능함**

 ## 접근 함수(access function)
접근 지정자가 `private`으로 지정되어 있을 경우 멤버 변수에 접근하기 위해서 `접근 함수(Access function)`이 필요하다.

```cpp
class Date
{
	int m_month;
	int m_day;
	int m_year;

public:
	void setDate(const int& month_input, const int& day_input, const int& year_input)
	{
		m_month = month_input;
		m_day = day_input;
		m_year = year_input;
	}
};

int main(void)
{
	Date today;

	// setDate()는 public이기 때문에 접근 가능
	today.setDate(8, 27, 2023);

	return 0;
}
```
`public` 멤버 메서드인 `setDate()`를 이용하여 `today`의 멤버들을 설정할 수 있다.<br>
같은 클래스 내에서의 접근은 `private`로 지정되어 있어도 접근이 가능하다.
- `setDate()`에서 `m_month`, `m_day`, `m_year`에 접근이 가능한 이유임

### getter / setter
```cpp
// ...
public:
	// setter
	void setMonth(const int& month_input){m_month = month_input;}
	void setDay(const int& day_input){m_day = day_input;}
	void setYear(const int& year_input){m_year = year_input;}

	// getter
	int getMonth() { return m_month; } // const int& getMonth() {...} 이런식으로 표현 가능
	int getDay() { return m_day; }
	int getYear() { return m_year; }
// ...
```
각각의 멤버에 접근하는 `getter/setter`
- getter에서 값을 변경하지 않게 하기 위해 `const int& getMonth()` 이런식으로 표현할 수 있음

### 다른 인스턴스(같은 클래스)의 멤버 접근
```cpp
// class Date...

    // original의 값을 복사하는 함수
    void copyFrom(const Date& original)
    {
        m_month = original.m_month;
        m_day = original.m_day;
        m_year = original.m_year;
    }
// ...

int main(void)
{
	Date today;
	today.setDate(8, 27, 2023);

	Date copy;
	copy.copyFrom(today); // copy = 8, 27, 2023
	
    cout << &today << " " << &copy; // 주소는 다름! 
    return 0;
}
```
값을 복사하는 방법은 다양한 방법이 있다. (`memcpy()` 등등)
> **다른 인스턴스의 멤버 변수에 접근할 수 있는 이유는 같은 클래스의 멤버이기 때문**
> ** -> today와 copy는 같은 Date 클래스이기 때문에 가능!**