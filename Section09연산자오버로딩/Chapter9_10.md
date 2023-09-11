# 변환 생성자(Converting constructor), explicit, delete

## 변환 생성자
**매개변수가 하나뿐인 생성자** 
변환 생성자는 매개변수로 전달받는 인자의 자료형을 명시적으로 알 수 있는 상황에서 하나의 매개변수로 객체를 생성하는 생성자다.
>`doSomethig(Some some)`라느 함수가 있을 때, `Some`의 생성자가 `int` 자료형 하나를 가지고 `Some` 객체를 생성할 수 있다면, `doSomething(1)`코드가 실행되면 변환 생성자가 실행된다.

### 변환 생성자 예시
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

    Fraction(const Fraction& Fraction)
        : m_numerator(Fraction.m_numerator), m_denominator(Fraction.m_denominator)
    {
        cout << "Copy Constructor called!" << endl;
    }

    friend std::ostream& operator<<(std::ostream& out, const Fraction& f)
    {
        out << f.m_numerator << " / " << f.m_denominator << endl;
        return out;
    }
};

void doSomething(Fraction frac)
{
    cout << frac << endl;
}

int main() 
{
    Fraction frac(7);

    doSomething(frac);
    doSomething(Fraction(7));
    doSomething(7); // 위와 같은 결과

    return 0;
}
```
- **`Fraction(int num = 0, int den = 1)`**
  - Fraction의 생성자는 `num`과 `den` 두 개의 매개변수를 갖지만, 둘 다 기본값이 존재하기 때문에 하나의 인자를 가지고 객체를 생성할 수 있다.
    - *변환 생성 가능*
- **`doSomething(7);`**
  - `doSomething`은 `Fraction` 객체를 인자로 받는데 `7`이라는 `int` 타입의 값을 받았지만 변환 생성자를 통해 `7 / 1`이라는 결과가 출력된다. 
    - 변환 생성자를 통해 생성된 객체는 익명 객체처럼 함수를 벗어나면 사라짐

### explicit
**`explicit` 키워드를 사용하면 암시적 형변환을 막아 변형 생성자의 호출을 막을 수 있다. **
```cpp
explicit Fraction(char) = delete;
```
> ***explicit 키워드로 암시적 형변환을 불가능하게 만들어 변환 생성자가 실행되지 않음(에러 발생)***

### delete
**`delete` 키워드를 사용하여 특정 매개변수를 받는 버전을 제한할 수 있고, 자동 형변환등을 막을 수 있다.**
```cpp
Fraction(char) = delete;
```
- `char` 타입을 매개변수로 받는 `Fraction`의 생성자를 더이상 사용할 수 없음.
  - `Fraction('c');` 
    - 런타임 에러가 발생한다.