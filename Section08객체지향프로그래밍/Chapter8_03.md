# Chapter8_03 생성자(Constructors)

## 생성자(Constructors)
**객체가 생성될 때 실행되는 함수로 객체가 가지고 있는 속성을 설정해줄 수 있다.**

```cpp
#include <iostream>
using namespace std;

class Fraction
{
    // 멤버를 숨김
private:
    int m_numerator;
    int m_denominator;

    // print() 메서드는 공개
public:
    void print()
    {
        cout << m_numerator << " / " << m_denominator << endl;
    }
};

int main(void)
{
    Fraction frac;
    frac.print(); // frac의 멤버들이 초기화가 되지 않았음

    // output (매번 다름)
    // 15146048 / 0
    return 0;
}
```
객체를 생성후 멤버의 값을 출력해 보면 쓰레기값이 들어가 있다.
> 값을 넣어주지 않았기 때문

생성한 `frac`에 값을 넣어주려면 멤버를 `public`으로 변경하여 값을 초기화할 수 있다.

### 생성자 사용
**생성자는 객체가 인스턴스화 되었을 때 실행되는 함수이다.**
> **생성자를 이용하여 객체 생성시 값을 초기화할 수 있음**<br>

**[생성자 선언 방법]**<br>
`클래스 이름(){ ... }`
```cpp
// Fraction class
public:
    Fraction() // 생성자
    {
        m_numerator = 1;
        m_denominator = 1;
    }
// ...

int main(void)
{
    Fraction frac; // 생성자가 실행됨
    frac.print();

    // output 1 / 1

    return 0;
}
```
Fraction frac; 코드가 실행되면 바로 생성자를 호출한다.

### 매개변수가 있는 생성자
**생성자도 함수이기 때문에 매개변수를 가질 수 있다.**
```cpp
// Fraction class ...
public:
    Fraction(const int& num_in, const int& den_in) // 매개변수가 있는 생성자
    {
        m_numerator = num_in;
        m_denominator = den_in;
    }
//...
int main(void)
{
    Fraction frac(1, 5);
    frac.print();

    // output 1 / 5

    return 0;
}
```
`Fraction(const int& num_in, const int& den_in)` 코드는 인자로 전달받은 값으로 멤버 변수를 초기화하고 있다.

### default 생성자
**생성자가 하나도 없을 경우 아무 일을 하지 않는 `default` 생성자를 갖게 된다.**
```cpp
class Fraction
{
public:
    int m_numerator;
    int m_denominator;
}
```
위 Fraction 클래스에는 `Fraction(){}`를 `default`로 가지고 있다.<br>
생성자를 만들었을 경우 기본 생성자가 생성되지 않기 때문에 기본 생성자를 사용할 수 없다.*(`Fraction frac` 불가능)*

> **기본 생성자를 사용할 때 ()를 생략함**<br>
> 
> **생성자가 존재하는데 기본 생성자를 사용하고 싶은 경우 기본 생성자를 만들면 됨**

### +
생성자를 가지고 있고 멤버도 `public`으로 공개되어 있고 유니폼 이니셜라이저를 사용하여 값을 초기화 하는 경우 타입 변환을 허용하지 않는다.

### 클래스 내부에 다른 클래스의 인스턴스가 멤버 변수로 있는 경우 생성자의 호출 순서
```cpp
#include <iostream>
using namespace std;
class Second
{
public:
    Second() // Second의 생성자
    {
        cout << "class second constructor()" << endl;
    }
};

class First
{
    // Second의 인스턴스를 가지고 있음
    Second sec;

public:
    First() // First의 생성자
    {
        cout << "class First constructor()" << endl;
    }

};

int main(void)
{
    First first; // First 인스턴스 생성
    
    // output
    // class second constructor()
    // class First constructor()
    return 0;
}
```
클래스 내부에 다른 클래스의 인스턴스가 멤버 변수로 있는 경우 멤버로 있는 인스턴스의 생성자를 먼저 호출하고 본인의 생성자를 호출한다.
> **First 클래스에서 멤버 변수인 `sec`가 생성,초기화가 되어있지 않으면 `sec`를 사용할 수 없기 때문에 `sec`를 먼저 생성하게 됨**