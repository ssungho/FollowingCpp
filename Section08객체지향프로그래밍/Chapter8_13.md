# Chapter 8_13 익명 객체

## 익명 객체
**재사용될 일이 없는 인스턴스를 생성해야 할 때, 이름이 존재하지 않는 일회용 인스턴스를 생성하여 사용할 수 있다.**

### 사용 방법
**생성자를 호출하고 .을 이용하여 멤버를 호출한다.**
- ex) `Some().doSomething();` // Some 클래스의 doSomething() 함수 호출
```cpp
class A
{
public:
    A()
    {
        cout << "constructor" << endl;
    }

    ~A()
    {
        cout << "destructor" << endl;
    }

    void print()
    {
        cout << "Hello" << endl;
    }
};

int main(void)
{
    A a; // A 인스턴스 a 생성
    a.print();

    A().print(); // 여기서 생성된 A와
    A().print(); // 여기서 생성된 A는 같은 객체가 아님 (소멸자를 통해 확인 가능)
    return 0;
}
```
`a.print()`와 `A().print()`는 동일한 기능을 한다.<br>
`A().print()`를 사용할 때 마다 익명의 A 인스턴스를 생성하여 `print()` 함수를 호출하고 소멸한다.
> ***`A().print()`를 할 때마다 같은 객체를 사용하는 것이 아님***<br>
> ***R-Value처럼 작동함***

### 응용 예제
```cpp
class Cents
{
private:
    int m_cents;

public:
    Cents(int cents)
    {
        m_cent = cent;
    }
    int getCents() const
    {
        return m_cents;
    }
};

Cents add(const Cents &c1, const Cents &c2)
{
    return Cents(c1.getCents() + c2.getCents());
}

int main(void)
{
    cout << add(Cents(6), Cents(8)).getCents() << endl;

    return 0;
}
```
` add(Cents(6), Cents(8)).getCents()`
> ***`add` 함수 안에 들어갈 Cents를 익명 객체로 만들어 사용한 것***<br>
> ***여기서 반환된 Cents 객체 또한 익명 객체로 볼 수 있음***
> - ***반환된 객체에 대해 `getCents()`호출 후 사라지기 때문***