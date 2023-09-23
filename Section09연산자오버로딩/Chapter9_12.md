# Chapter9_12 이니셜라이저 리스트(std::initializer_list)

## std::initializer_list
**initializer_list를 사용하면 사용자 정의 자료형에서 생성자나 또는 대입 연산자를 만들 때 편하게 값을 초기화 할 수 있다.**

### 일반 자료형에 대한 이니셜라이저 리스트
```cpp
int main()
{
	int my_arr1[5] = { 1,2,3,4,5 };
	int* my_arr2 = new int[5] {1, 2, 3, 4, 5};

	auto il = { 10, 20, 30 };

	return 0;
}
```
- 정적 배열인 my_arr1과 동적 배열인 my_arr2 모두 중괄호를 이용해 리스트 멤버를 초기화 할 수 있다.
- `auto il = { 10, 20, 30 };`
  - il의 자료형을 확인해보면 initializer_list로 컴파일러가 설정해주는 것을 확인할 수 있다.

### 사용자 정의 자료형에 대한 이니셜라이저 리스트
```cpp
#include <iostream>
#include <cassert>
#include <initializer_list>
using namespace std;

class IntArray
{
private:
	unsigned m_length = 0;
	int* m_data = nullptr;

public:
	IntArray(unsigned length) : m_length(length)
	{
		m_data = new int[length];
	}

	IntArray(const std::initializer_list<int>& list) : IntArray(list.size())
	{
		int count = 0;
		for (auto& element : list)
		{
			m_data[count] = element;
			count++;
		}

		/*for (unsigned count = 0; count < list.size(); count++)
			m_data[count] = list[count];*/ // initializer_list는 []를 지원하지 않는다.
	}

	~IntArray()
	{
		delete[] this->m_data;
	}

	friend ostream& operator <<(std::ostream& out, IntArray& arr)
	{
		for (unsigned i = 0; i < arr.m_length; i++)
			out << arr.m_data[i] << " ";
		out << endl;
		return out;
	}
};

int main()
{
	IntArray int_array = { 1, 2, 3, 4, 5 };

	return 0;
}
```
- `IntArray(const std::initializer_list<int>& list) : IntArray(list.size())`
  - 먼저 `IntArray(list.size())`를 통해 메모리 공간을 할당한다.
  - 매개변수로 `std::initializer_list`타입의 참조 리스트를 받는다.
    - 생성자에서 `= {...}` 형식이나 `{...}` 형식으로 리스트를 초기화 할 수 있게 됨
  - `foreach`문을 이용하여 매개변수로 받은 `list`의 요소들을 초기화할 객체의 `m_data`로 옮긴다.
    - std::initializer_list는 []를 지원하지 않기 때문에 foreach문을 사용한다.