# Chapter8_04 생성자 멤버 초기화 목록(Member Initializer List)

## Member Initializer List
**생성자를 호출할 때 기본으로 초기화 값을 정해줄 수 있다.**

[사용 방법]<br>
생성자 괄호 뒤에 콜론(:)을 표기하고 괄호나 중괄호를 이용해 초기화<br>
ex) `Something() : m_i(1) // Something클래스의 멤버 m_i를 1로 초기화한다`

```cpp
#include <iostream>
using namespace std;

class Something
{
private:
	int m_i;
	double m_d;
	char m_c;
	int m_arr[5];

public:
	Something() : m_i(1), m_d(3.14), m_c('a'), m_arr{ 1, 2, 3, 4, 5 }
	{

	}

	void print()
	{
		cout << m_i << " " << m_d << " " << m_c << endl;
		for (auto& e : m_arr)
			cout << e << " ";
		cout << endl;
	}
};

int main(void)
{
	Something some;
	some.print();

    // output
    // 1 3.14 a
    // 1 2 3 4 5

	return 0;
}
```
`Something() : m_i(1), m_d(3.14), m_c('a'), m_arr{ 1, 2, 3, 4, 5 }`를 통해서 멤버의 값을 초기화 해준다.
- `m_1` = 1, `m_d` = 3.14, `m_c` = a, `m_arr` = { 1, 2, 3, 4, 5 }
- `중괄호 { }` 를 이용하여 초기화 할 수 있는데, 이 경우 형변환이 일어나지 않는다. (엄격함)

### 클래스 멤버로 다른 클래스가 있을 경우
```cpp
class B 
{
private:
	int m_b;

public :
	B(const int& m_b_in) : m_b(m_b_in) { }
};

class Something
{
private:
	// ...

	B m_b;

public:
	Something() 
		: m_i{ 1 }
		, m_d{ 3.14 }
		, m_c{ 'a' }
		, m_arr{ 1, 2, 3, 4, 5 }
		, m_b(m_i - 1) // 1 - 1 = 0
	{ }

	// ...
};
```
`Something` 클래스의 생성자에서 `m_b`를 초기화하기 위해 `B` 클래스의 생성자 `B(const int& m_b_in) : m_b(m_b_in) { }`하고  `0`을 전달하게 됨.

### 초기화 우선순위
클래스의 멤버를 선언과 동시에 초기화하고 생성자를 통해 초기화할 경우 우선운위를 확인해보자.
```cpp
#include <iostream>
using namespace std;

class B
{
private:
	int m_b;
public:
	B(const int& m_b_in) : m_b(m_b_in) { }
};

class Something
{
private: // 멤버 변수를 선언과 동시에 초기화
	int m_i = 100;
	double m_d = 100.0;
	char m_c = 'F';
	int m_arr[5] = { 100, 200, 300, 400, 500 };
	B m_b{ 1024 };

public: // 생성자를 통한 초기화
	Something()
		: m_i{ 1 }
		, m_d{ 3.14 }
		, m_c{ 'a' }
		, m_arr{ 1, 2, 3, 4, 5 }
		, m_b(m_i - 1)
	{ }

	void print()
	{
		cout << m_i << " " << m_d << " " << m_c << endl;
		for (auto& e : m_arr)
			cout << e << " ";
		cout << endl;
	}
};

int main(void)
{
	Something some;
	some.print();

    // output
    // 1 3.14 a
    // 1 2 3 4 5

	return 0;
}
```
출력 결과가 생성자를 통한 초기화 값인 것을 통해 멤버를 선언과 동시에 초기화한 것이 우선순위가 높은 것을 확인할 수 있다.
- ***정적 멤버일 경우에는 다름***

### 생성자 내부에서 다시 초기화 하는 경우
```cpp
class Something
{
private:
	int m_i = 100;
	double m_d = 100.0;
	char m_c = 'F';
	int m_arr[5] = { 100, 200, 300, 400, 500 };
	B m_b{ 1024 };

public:
	Something()
		: m_i{ 1 }
		, m_d{ 3.14 }
		, m_c{ 'a' }
		, m_arr{ 1, 2, 3, 4, 5 }
		, m_b(m_i - 1)
	{ 
		m_i =  2;
		m_d = 6.28;
		m_c = 'A';
	}
    // void print()
}

int main(void)
{
	Something some;
	some.print();

    // output
    // 2 6.28 A
    // 1 2 3 4 5
	return 0;
}
```
`Member Initializer List`를 먼저 초기화 하고 생성자 안에서 정의한 값이 대입된다.
- **`Something() : m_i{ 1 }, m_d{ 3.14 }, m_c{ 'a' }, m_arr{ 1, 2, 3, 4, 5 }, m_b(m_i - 1)`가 초기화 된 이후<br> `mi_i = 2;`,  `m_d = 6.28;`, `m_c = 'A';`가 실행됨**