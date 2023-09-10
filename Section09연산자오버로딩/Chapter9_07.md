# Chapter9_07 괄호 연산자 오버로딩 & 함수 객체

## 괄호 연산자 오버로딩
**괄호 연산자 `()`**
- 객체를 함수인것 처럼 호출할 수 있음
- 첨자 연산자와 오버로딩 방법이 동일함
> ***객체를 함수처럼 호출하는 것을 `functor`라고 한다.***
### 괄호 연산자 `()` 오버로딩 방법
```cpp
#include <iostream>
using namespace std;

class Accumulator
{
private:
    int m_counter = 0;

public:
    int operator()(int i)
    {
        return (m_counter += i);
    }
};

int main(void)
{
    Accumulator acc;

    // functor
    cout << acc(10) << endl;
    cout << acc(20) << endl;

    return 0;
}
```
- `int operator()(int i)`
  - 괄호 연산자의 매개변수로 받은 i를 `acc`가 가진 `m_counter`와 더하고 그 값을 반환한다.
> ***'객체 + `()`'로 생성자처럼 함수를 호출함***