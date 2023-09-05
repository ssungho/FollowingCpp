# Chapter8_12 친구 함수와 클래스 friend

## friend
**`friend` 키워드를 이용하여 함수나 클래스를 등록하면, 그 함수나 클래스에서 자신의 `private` 멤버들을 접근할 수 있다.** 

### friend 함수 등록 방법
**`void doSomething(A& a)`함수에서 A클래스의 private 멤버에 접근하고 싶다면, <br>
`friend + 함수의 프로토타입`을 작성한다.**
- ex) `friend void Get(SomeClass& sc)`
```cpp
class A
{
private:
    int m_value = 1;

    // friend 함수 등록
    friend void doSomething(A& a);
};

void doSomething(A& a)
{
    cout << a.m_value << endl; // A 클래스의 m_value에 접근 가능
}
```
A 클래스에서 doSomething 함수를 friend로 지정했기 때문에 A 클래스 외부의 함수인 doSomething 함수에서 A 클래스의 private 멤버인 `m_value`에 접근할 수 있음

### 전방선언
**여러 클래스에서 같은 함수를 `friend`로 등록할 때 매개변수로 들어가는 클래스에 대한  전방선언이 필요한 경우가 있다.**
```cpp
class A
{
private:
    int m_value = 1;

    friend void doSomething(A &a, B &b); // B의 존재를 알 수 없음
};

class B
{
private:
    int m_value = 2;

    friend void doSomething(A &a, B &b);
};

void doSomething(A &a, B &b);
{
    cout << a.m_value << " " << b.m_value << endl;
}
```
doSomething에서 A 클래스의 멤버와 B 클래스의 멤버를 접근한다고 했을 때 A 클래스와 B 클래스 모두 doSomething을 friend로 등록하고 전방선언까지 해줘야 사용할 수 있다.<br>
> ***왜냐하면 A 클래스에서 friend로 등록하는 시점에 B라는 클래스가 존재하는지 알 수 없기 때문***

```cpp
// 전방선언 forward declaration
class B;

class A
{
private:
    int m_value = 1;

    friend void doSomething(A &a, B &b); // 이제 B클래스의 존재를 알게 됨
};

class B
{
private:
    int m_value = 2;

    friend void doSomething(A &a, B &b);
};

void doSomething(A &a, B &b);
{
    cout << a.m_value << " " << b.m_value << endl;
}
```

### friend 클래스 등록 방법
**멤버를 공개하고 싶은 클래스에 friend를 추가하여 클래스에 정의한다.**
- ex) `friend class SomeClass;`
```cpp
class A
{
private:
    int m_value = 1;

    friend class B; // 클래스 B를 friend로 등록
};

class B
{
private:
    int m_value = 2;

    void doSomething(A &a)
    {
        cout << a.m_value << endl; // A 클래스의 private 멤버에 접근 가능
    }
};
```
> ***A 클래스에서 B 클래스를 friend로 등록했다는 것은 B 클래스에서 A 클래스의 private 멤버에 대해 접근을 허락한 것***

### 멤버 함수를 friend로 등록하는 방법
**`friend + 클래스::함수의 프로토타입`으로 다른 클래스의 함수도 friend로 등록할 수 있다.**
```cpp
#include <iostream>
using namespace std;

// B에서 A클래스의 인스턴스에 접근하기 때문에 전방선언으로 알림
class A;

class B
{
private:
    int m_value = 2;

public:
    void doSomething(A &a);
};

class A
{
private:
    int m_value = 1;

    friend void B::doSomething(A &a);
};

// B클래스 외부에 정의를 분리
void B::doSomething(A &a)
{
    cout << a.m_value << endl;
}

int main(void)
{
    A a;
    B b;
    b.doSomething(a);
    return 0;
}
```
> ***B 클래스의 `doSomething(A& a)`를 선언과 정의를 분리한 이유는 클래스 전방선언을 통해 A 클래스에 대해 알렸지만, A 클래스의 멤버인 `m_value`의 존재는 알 수 없기 때문에 A 클래스를 위에 정의하고 `doSomething(A& a)` 함수의 정의 부분을 A 클래스 아래에 정의한 것***