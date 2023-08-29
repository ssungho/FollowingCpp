# Chapter8_06 소멸자(Destructor)

## 소멸자(Destructor)
소멸자는 변수가 영역을 벗어나서 사라질 때 호출되는 함수를 의미한다.
> ***1. 소멸자는 instance가 메모리에서 해제될 때 내부에서 자동으로 호출된다.***<br> 
> ***2. 동적 할당으로 만들어진 경우에는 영역을 벗어나도 자동으로 메모리가 해제되지 않기 때문에 `delete`으로 메모리를 해제할 때에만 소멸자가 호출된다.***<br> 
> ***3. 소멸자를 프로그래머가 직접 호출하는 것은 대부분의 경우 권장하지 않는다.***

### 정의 방법
- ```~{클래스 명}```
- 매개변수와 반환값이 없다.

```cpp
#include <iostream>
using namespace std;

class Simple 
{
private:
	int m_id;

public:
	Simple(const int& id_in) :m_id(id_in)
	{
		cout << "Constructor" << m_id << endl;
	}
	~Simple()
	{
		cout << "Destructor" << m_id << endl;
	}
};

int main(void) 
{
	Simple s1(0);
	Simple s2(1);

    return 0;
}
```
<br>

**실행결과**
```
Constructor0
Constructor1
Destructor1
Destructor0
```

생성자가 다 호출 되고 main함수가 끝날 때 소멸자가 실행된다.
- `return 0;`에 들어와야 `s1`과 `s2`의 소멸자가 실행됨
- 나중에 선언된 `s2`의 소멸자가 먼저 실행됨

### 동적 할당을 한 경우
```cpp
int main(void) 
{
	Simple *s1 = new Simple(0);
	Simple s2(1);

	delete s1;

	return 0;
}
```
**실행결과**
```
Constructor0
Constructor1
Destructor0
Destructor1
```
동적으로 할당된 `s1`은 `delete`로 직접 메모리를 해제했기 때문에 이때 소멸자가 실행되고 `s2`는 `return 0;` 코드가 실행되면서 소멸자가 실행된다.

### 동적 할당시 소멸자 사용 
```cpp
#include <iostream>
using namespace std;

// 사용자 정의 배열
class IntArray
{
private:
	int* m_arr = nullptr;
	int m_length = 0;

public:
	IntArray(const int length_in)
	{
		m_length = length_in;
		m_arr = new int[m_length];

        cout << "Constructor" << endl;
	}
	int size() { return m_length; }
};

int main(void) 
{
	while (true)
	{
		IntArray my_int_arr(10000); // 메모리 누수가 발생함
	}

	return 0;
}
```
위 코드를 실행하면 `while`문 안에서 동적으로 배열이 계속 생성되고 해제되지 않기  때문에 메모리 누수가 일어난다.
- 메모리를 직접 해제를 해줘야함


**이럴 때 소멸자를 이용해서 깔끔하게 처리할 수 있다.**
```cpp
~IntArray()
{
    // nullptr이 아닐 경우에만 소멸 
	if(m_arr != nullptr)
	    delete[] m_arr;

    cout << "Destructor" << endl; // 소멸자 확인용
}
```
이제 코드를 실행하면 while문을 돌면서 생성과 소멸을 반복하게 된다.<br>
> *`if(m_arr != nullptr)`구문은 nullptr을 소멸시킬 경우 오류가 생길 수 있기 때문에 작성한 안전장치인 셈*


### 정리
> ***클래스 설계를 할 때 `new`를 통해 메모리를 할당하고 있다면 꼭 `소멸자`나 상응하는 부분에서 `delete`를 통해 메모리를 해제해줘야 한다.***<br>
> ***위처럼 동적 할당 배열을 만들어 쓸수도 있지만 `new()`와 `delete()`의 속도가 느리기 때문에 `std::vector`를 사용할 수 도 있다.***