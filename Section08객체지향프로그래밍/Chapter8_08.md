# 클래스 코드와 헤더 파일

## 헤더파일을 따로 만들어서 클래스 코드를 정리하는 방법
클래스를 만드는 경우 cpp파일에 직접 만드는 경우보다 처음부터 헤더 파일을 만들어서 클래스의 선언과 정의를 분리하는 경우가 많다. (헤더파일에 전부 정의할 수도 있음)
### 분리할 클래스
아래 코드의 Calc 클래스를 헤더파일로 분리해보자.
```cpp
#include <iostream>
using namespace std;

class Calc
{
private:
	int m_value;

public:
	Calc(int init_value) 
		: m_value(init_value)
	{}

	Calc& add(int value) 
	{
		m_value += value;
		return *this; 
	}
	Calc& sub(int value) 
	{
		m_value -= value;
		return *this;
	}
	Calc& mult(int value) 
	{
		m_value *= value; 
		return *this; 
	}

	void print()
	{
		cout << m_value << endl;
	}
};

int main(void)
{
	Calc calc(10);
	calc.add(10).sub(1).mult(2).print();

	return 0;
}
```
### 1. 확장자 명이 .h인 헤더파일을 생성
분리할 클래스인 Calc의 이름으로 헤더파일을 작성한다.<br>
`Calc.h`
> **보통 클래스 이름과 헤더파일의 이름을 동일하게 사용함**

### 2. Calc 클래스를 Calc.h로 옮기고 cpp파일에서  Calc.h 헤더파일을 include
본 클래스에서 함수 선언만 남기고 함수 전체를 Calc.h로 옮긴다.
```cpp
// Calc.h
#pragma once

#include <iostream> // print()에 cout, endl이 포함되어 있기 때문에 사용

class Calc
{
private:
	int m_value;

public:
	Calc(int init_value) 
		: m_value(init_value)
	{}

	Calc& add(int value) 
	{
		m_value += value;
		return *this; 
	}
	Calc& sub(int value) 
	{
		m_value -= value;
		return *this;
	}
	Calc& mult(int value) 
	{
		m_value *= value; 
		return *this; 
	}

	void print()
	{
        using namespace std;
		cout << m_value << endl;
	}
};
```
헤더파일을 사용할 때 주위할 점은 헤더파일 내에서 `using namespace`를 사용하면 `include`하는 것들이 전부 영향을 받게 되기 때문에 사용하는 것을 권장하지 않는다. 

다 옮겼다면 cpp 파일에서 이 헤더파일(Calc.h)을 include 해준다.
```cpp
// cpp파일
#include <iostream>
#include "Calc.h" // 사용자 정의 헤더파일이기 때문에 "" 사용
using namespace std;

int main(void)
{
	Calc calc(10);
	calc.add(10).sub(1).mult(2).print();

	return 0;
}
```

## Calc.h 헤더파일을 새로운 cpp 파일로 다시 분리
메인 cpp 파일에서 Calc 클래스를 헤더파일(Calc.h)로 분리했지만 Calc.h에 선언만 남기고 내용을 다른 cpp 파일로 분리할 수 있다.

### Calc.cpp 생성후 Calc.h 헤더파일에서 선언만 남기고 옮김
*Calc.cpp*
```cpp
#include "Calc.h"
#pragma once

Calc::Calc(int init_value)
	: m_value(init_value)
{}

Calc& Calc::add(int value)
{
	m_value += value;
	return *this;
}

Calc& Calc::sub(int value)
{
	m_value -= value;
	return *this;
}

Calc& Calc::mult(int value)
{
	m_value *= value;
	return *this;
}

void Calc::print()
{
	std::cout << m_value << std::endl;
}
```
Calc.cpp도 Calc.h를 include한다. <br>
옮긴 함수의 출처(어느 클래스 소속인지)를 알리는 용도로 `클래스명::`을 붙여서 사용한다.
- ex) `Calc& Calc::add(int value){...}`
- `add`함수는 `Calc` 클래스에 소속되어 있다는 뜻!


*Calc.cpp로 분리하고 선언만 남은 헤더파일의 모습*
```cpp
// Calc.h
#pragma once
#include <iostream>

class Calc
{
private:
	int m_value;

public:
	Calc(int init_value);

	Calc& add(int value);
	Calc& sub(int value);
	Calc& mult(int value);

	void print();
};
```

*메인 cpp 파일*
```cpp
// 기존의 메인 cpp 파일
#include <iostream>
#include "Calc.h"
using namespace std;

int main(void)
{
	Calc calc(10);
	calc.add(10).sub(1).mult(2).print();

	return 0;
}
```
> **우리는 메인 cpp파일을 봤을 때 Calc.h를 포함하고 있다는 것을 확인하고 Calc.h를 확인하여 이 클래스가 어떤 기능을 가지고 있는지 Calc의 멤버들을 보고 대략적으로 판단할 수 있고, 좀더 자세한 정보를 필요로 한다면 Calc.cpp로 가서 자세한 내용을 확인하면 된다.**<br>
> ~~*처음부터 헤더파일을 만들어서 클래스를 정의하는 방식을 권장*~~