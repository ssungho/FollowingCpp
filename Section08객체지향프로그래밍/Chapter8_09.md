# Chapter8_09 클래스와 const

## const 인스턴스
**const 키워드를 추가해 생성한 인스턴스는 그 내용을 변경할 수 없고 const 멤버 함수만 사용할 수 있다.**
```cpp
#include<iostream>
using namespace std;

class Something
{
public:
    int m_value = 0;

    void setValue(int value)
    {
        m_value = value;    
    }
    int getValue()
    {
        return m_value;
    }
};

int main(void)
{
    const Something something;
    // something.setValue(3); -> 컴파일 오류
    // something.getValue(); -> 컴파일 오류

    return 0;
}
```
`const`로  something 인스턴스를 생성했기 때문에 멤버의 값을 바꾸는 `something.setValue(3)`는 컴파일 에러가 나타날 것이라고 생각할 수 있다. <br>
그런데 값을 불러오는 `getValue()`의 경우에도 컴파일 오류가 나타나는데 이것은 컴파일러가 판단할 때 멤버의 값을 바꿨는지 안바꿨는지를 확인하는 것이 아니라 **멤버 함수에 `const`가 붙었는지 안 붙었는지를 확인하기 때문이다.**

### const 멤버 함수
**멤버 함수를 const로 만드는 방법은 함수 이름 위에 `const`키워드를 붙여주면 된다.**
```cpp 
int getValue() const // 함수 뒤에 const를 붙여줌
{
    return m_value;
}
```
**함수를 `const`로 선언했다는 것은 함수에서 멤버를 바꾸는 일이 일어나지 않는다는 것을 명시한 것**과 같기 때문에 `setValue`에 `const`를 붙인다 한들 내부에서 멤버 변수를 변경하는 코드가 있어 컴파일 에러가 난다. 
> ***복잡한 함수를 작성하게 될 때 const를 넣을 수 있다면 넣어주는 것이 실수를 줄이고, 디버깅에 유리하다.***

## 복사 생성자와 인스턴스 매개변수
**함수를 실행할 때 매개변수로 값을 넘겨주면 본체가 아닌 복사가 일어나는데 const 키워드와 &를 이용하여 효율적으로 값을 넘겨줄 수 있다.**

### 복사 생성자
**함수의 파라미터로 인스턴스를 넘겨주면 클래스 내부의 복사 생성자 (Copy Constructor)가 실행된다.**
```cpp
#include <iostream>
using namespace std;

class Something
{
public:
    int m_value = 0;

    Something() // 생성자
    {
        cout << "Constructor" << endl;
    }

    void setValue(int value) { m_value = value; } // Setter
    int getValue() const { return m_value; } // Getter

};

// 주소 출력 함수
void print(Something st_in)
{
    cout << &st << endl;

    cout << st.m_value << endl;
}

int main(void)
{
    Something something;

    cout << &something << endl;

    print(something);

    return 0;
}
```
위 코드를 실행시켜 보면 직접 출력한 `something`의 주소와 `print(something)` 함수로 출력한 주소가 다른 것을 확인할 수 있다.

- print에서 매개변수로 받은 something은 something을 복사한 다른 객체임

하지만 복사된 객체의 생성자는 실행되지 않은 것처럼 보이지만 사실 클래스 내부에 아래와 같은 `Copy Constructor`가 숨어있고 이것이 실행되는 것이다.

```cpp
 // 복사 생성자
Something(const Something &st_in)
{
    m_value = st_in.m_value;

    cout << "Copy Constructor" << endl;
}
```

> ***복사 생성자(Copy Constructor)는 객체의 주소값을 참조(&)하기 때문에 복사가 일어나지 않고 `const`를 사용하기 때문에 원본에 변형이 일어나지 않는 것***

### const 와 &를 사용

`print(Something st_in)` 함수를 원본은 변형시키지 않고 복사가 일어나지 않게 바꾸려면 const 키워드와 참조(&)를 이용하면 된다.
```cpp
void print(cont Something& st_in)
{
    cout << &st << endl;

    cout << st.getValue() << endl;
}
```
`st_in`와 `something`의 주소가 같은 것을 확인할 수 있다.
> ***이렇게 하면 불필요한 복사가 일어나지 않음***

### const의 여부를 통한 오버로딩
**const를 이용해서 오버로딩이 가능하다.**
```cpp
#include <iostream>
#include <string>
using namespace std;

class Something
{
public:
    string m_value = "default";

    const string &getValue() const
    {
        cout << "const version" << endl;
        return m_value;
    }
    string &getValue()
    {
        cout << "non-const version" << endl;
        return m_value;
    }
};

int main(void)
{
    Something something;
    something.getValue();

    const Something something2;
    something2.getValue();

    return 0;
}
```
출력결과 <br>
`non-const version` <br>
`const version`

**정리**
> ***클래스의 인스턴스를 const로 선언한 것은 클래스의 멤버를 const로 정의한 것과 같고, const로 선언된 멤버 함수만 사용할 수 있다.*** <br>
> ***const로 만들 수 있는 것은 const를 붙이는게 논리적 오류를, 실수를 줄이는데 도움이 된다.***
