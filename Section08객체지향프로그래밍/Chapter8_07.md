# Chapter8_07 this 포인터와 연쇄 호출(Chaining Member Functions)

## this 포인터
**클래스 안에서 `this`키워드를 사용하면 자기 자신의 주소를 가리킬 수 있다.**

### 주소 출력
```cpp
#include <iostream>
using namespace std;

class Simple
{
private:
	int m_id;

public:
	Simple(int id)
	{
		setId(id);

		cout << this << endl;
	}

	void setId(int id) { m_id = id; }
	int getId() { return m_id; }
};

int main(void)
{
	Simple s1(1), s2(2);

	s1.setId(2);
	s2.setId(4);

	cout << &s1 << endl;
    cout << &s2 << endl;

	return 0;
}
```
-> 각각의 생성자가 출력한 값과 주소를 직접 출력한 값이 같다.<br>

`setId()`라는 함수는 `Simple` 클래스의 인스턴스가 모두 공유를 해서 사용하는데 `Simple` 클래스의 내부에서는 `this->setId()`로 실행된다. 
- 각 인스턴스들은 자신을 가리키는 포인터를 가지고 있고 포인터를 보려면 `this`키워드를 사용하면 된다.
- 보이지 않는 포인터가 내부적으로 들어가 있는 것

### 연쇄 호출(Chaining Member Functions)
`this` 포인터를 이용하여 연쇄적으로 함수를 호출할 수 있다.
```cpp
#include <iostream>
using namespace std;

class Calc 
{
private:
	int m_value;

public:
	Calc(int init_value) : m_value(init_value)
	{}

	void add(int value) { m_value += value; }
	void sub(int value) { m_value -= value; }
	void mult(int value) { m_value *= value; }

	void print() { cout << m_value << endl; }
};

int main(void)
{
	Calc calc(10);  // value 10으로 지정
	calc.add(10);   // value = 10 + 10 = 20
	calc.sub(1);    // value = 20 - 1 = 19
	calc.mult(2);   // value = 19 * 2 = 38

	calc.print();   // 38

	return 0;
}
```
위 코드에서 `Calc` 클래스의 함수들을 `this` 키워드를 이용해서 한 줄로 실행시키게 만들 수 있다.

*자신을 디레퍼런싱한 값을 반환하게 수정*
```cpp
Calc& add(int value) { m_value += value; return  *this; }
Calc& sub(int value) { m_value -= value; return *this; }
Calc& mult(int value) { m_value *= value; return *this; }
```

**결과** <br>
`calc.add(10).sub(1).mult(2).print();`로 표현 가능

> **실용적인 측면은 모르겠으나, `this` 포인터를 이런식으로도 사용할 수 있다.**