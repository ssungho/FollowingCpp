# Chapter9_09 복사 생성자, 복사 초기화, 반환값 최적화

## 복사 생성자
**복사 생성자, 복사 초기화**
- 생성할 객체와 같은 클래스의 객체를 전달받아 그 정보를 바탕으로 객체를 생성, 초기화할 때 호출되는 생성자다.
> ***같은 타입의 객체를 복사하여 새로운 객체를 만들때 복사 생성자가 호출됨***

### 복사 생성자가 호출되는 경우
```cpp
#include <iostream>
#include <cassert>
using namespace std;

class Fraction
{
private:
    int m_numerator;
    int m_denominator;

public:
    Fraction(int num = 0, int den = 1)
        : m_numerator(num), m_denominator(den)
    {
        assert(den != 0);
    }

    // private로 변경시 외부에서 복사 불가
    Fraction(const Fraction &Fraction) // copy constructor
        : m_numerator(Fraction.m_numerator), m_denominator(Fraction.m_denominator)
    {
        cout << "Copy Constructor called!" << endl;
    }

    friend std::ostream &operator<<(std::ostream &out, const Fraction &f)
    {
        out << f.m_numerator << " / " << f.m_denominator << endl;
        return out;
    }
};
```

***복사 생성자***
```cpp
Fraction(const Fraction &Fraction)
    : m_numerator(Fraction.m_numerator), m_denominator(Fraction.m_denominator)
{
    cout << "Copy Constructor called!" << endl;
}
```
- 매개변수를 같은 클래스의 객체의 참조 형식으로 받는다.
- 멤버 변수들은 참조한 객체의 멤버 변수로 초기화한다.

***호출되는 경우***
```cpp
int main()
{
    Fraction frac(3, 5);

    Fraction fr_copy(frac); // copy ctor
    Fraction fr_copy_init = frac; // copy init

    cout << frac << fr_copy << fr_copy_init << endl;

}
```
- `Fraction fr_copy(frac);`
  - 이미 생성된 `frac`을 이용하여 복사 생성자가 호출된다.
- `Fraction fr_copy_init = frac;`
  - 생성과 동시에 초기화 할 경우에도 복사 생성자가 호출된다.

### 복사 생성자가 호출되지 않는 경우
```cpp
Fraction no_copy(Fraction(3, 10));
```
- `Fraction(3, 10)`으로 생성된 익명 객체를 `no_copy`를 생성할 때 넣어주는 방식이 아닌 `no_copy(3, 10)`으로 작동한다.
- 익명 객체는 쓰이고 나면 알아서 사라지는 `R-value`로 작동하기 때문에 컴파일러 입장에서는 복사 생성이 불필요한 과정으로 판단하고 복사 생성자를 호출하지 않은 것이다.
> ***컴파일러가 코드를 알아서 최적화하는 경우가 있음***<br>
> ***복사 생성을 외부에서 못하게 막고 싶다면 `private`로 정의하면 됨***

## 반환값 최적화
```cpp
Fraction doSomething()
{
    Fraction temp(1, 2);

    return temp;
}

int main()
{
    Fraction result = doSomething();
    cout << result << endl;

    return 0;
}
```
- `Fraction result = doSomething();`
  - `Fraction doSomething()`으로 생성된 `temp(1, 2)`로 `result`를 초기화하고 있다.

### 디버그 모드와 릴리즈 모드에서 작동 차이
위 코드를 **디버그 모드**에서 실행시킨다면 `doSomething()`으로 생성한 객체를 이용하여 `result`가 초기화 될 때 복사 생성자가 호출된다.<br>
하지만 **릴리즈 모드**에서 실행시킨다면 컴파일러가 코드를 최적화 하는 수준이 높아져 복사 과정을 생략하게 된다.
- temp(1, 2) 자체가 result와 같은 주소로 생성됨

***요약***
> ***컴파일러는 디버그 모드에서는 최적화를 최소화하여 프로그램의 동작을 정확히 추적하고 디버깅할 수 있게 하며,*** <br>
> ***릴리즈 모드에서는 다양한 최적화 기법을 이용하여 코드의 실행 속도를 높이고 메모리 사용량을 줄인다.***