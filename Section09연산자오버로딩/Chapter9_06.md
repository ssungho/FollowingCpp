# Chapter9_06 첨자 연산자 오버로딩

## 첨자 연산자 오버로딩

**첨자 연산자 `[]`**
- `[index]`: index로 원하는 위치의 값에 바로 접근하기 위해 사용한다.

### 첨자 연산자를 오버로딩하지 않은 경우
**사용자 정의 리스트 `IntList`** 
```cpp
#include <iostream>
using namespace std;

class IntList
{
private:
    int m_list[10] = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };

public:
    void setItem(int index, int value)
    {
        m_list[index] = value;
    }
    int getItem(int index)
    {
        return m_list[index];
    }
    int* getList()
    {
        return m_list;
    }
};

int main(void)
{
    IntList my_list;

    my_list.setItem(3, 1);
    cout << my_list.getItem(3) << endl;

    my_list.getList()[3] = 10;
    cout << my_list.getList()[3] << endl;

    return 0;
}
```

`IntList`의 멤버인 `m_list`에 접근하기 위해서 각 위치에 `setter/getter`함수를 만들어 사용해야 한다.
- `void setItem(int index, int value)`
  - 배열의 `index`위치에 `value`를 넣어준다.

- `int getItem(int index)`
  - 배열의 `index`위치에 값을 가져온다.

- `int* getList()`
  - `IntList`의 멤버인 `m_list`의 포인터를 반환한다.
    - ***포인터를 반환하기 때문에 `[index]`로 접근하게 되면 값을 쓰거나 지울 수 있게 된다.***
    - `my_list.getList()[3] = 10;` 
      - *`my_list.m_list[3] = 10;`로 값을 쓴 것*
      - *`my_list.getList()[3]`는 값을 읽은 것*


### `[]` 연산자 오버로딩 (멤버 함수)
```cpp
class IntList
{
private:
    int m_list[10] = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };

public:
    // 값을 읽고 쓸 수 있게 Reference로 반환
    int& operator[](const int index)
    {
        return m_list[index];
    }

    const int& operator[](const int index) const // 값을 읽기만 하는 버전
    {
        return m_list[index];
    }
};
```
- 반환 타입은 `int&`
  - 멤버 배열이 `int` 타입 배열이기 때문(`float` 타입이라면 `float`를 반환하면 됨)
  - ***`&`참조 형식으로 반환하는 이유는 L-value로 접근한 위치에 값을 대입하기 위해서 시용***
    - ***참조 형식이 아니라면 값을 대입할 수 없음 (R-value)***

- `const` 키워드를 사용하여 값을 읽기만 할 수 있다.
  - 값을 변경할 수 없기 때문

### assert를 이용한 에러 경고
```cpp
#include <iostream>
#include <cassert>
using namespace std;

class IntList
{
private:
    int m_list[10] = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };

public:

    int& operator[](const int index)
    {
        assert(index >= 0);
        assert(index < 10);

        return m_list[index];
    }

    const int& operator[](const int index) const
    {
        assert(index >= 0);
        assert(index < 10);

        return m_list[index];
    }
};
```
- `assert`를 이용하여 `IntList`안에 정의된 `m_list`의 크기를 넘어서는 `index`접근을 방지할 수 있다.
  - `assert`안의 조건문이 틀렸을 경우 런타임 에러가 발생함
  - `cout << my_list[12] << endl;` -> 런타임 에러 발생
    - `my_list.size() < 12`

### 동적 할당으로 생성한 경우 주위할 점
```cpp
IntList* list = new IntList;

(*list)[3] = 10; // dereferencing
```
- 동적으로 할당한 `IntList`인스턴스의 인덱스에 접근하기 위해서 `(*list)`처럼 `*(역참조 연산자)`를 이용해서 접근해야 한다. 
- `*(역참조 연산자)`를 사용하지 않고 접근할 경우 `list`안에 있는 `m_list`에 접근하는 것이 아니라, ***`list` 포인터가 가리키는 메모리 주소에서 입력받은 인덱스에 접근한다는 뜻이 된다.***
  - `list[3]` -> list 포인터가 가리키는 주소의 3번째 인덱스 값에 접근하는 것