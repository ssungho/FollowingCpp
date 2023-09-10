# Chapter9_08 형변환(type casts) 오버로딩

## 형변환 오버로딩이 필요한 이유
```cpp
class Cents
{
private:
    int value;

public:
    Cents(int cents = 0) { value = cents; }
    int getCents() const { return value; }
    int &getCents() { return value; }
};
```
Cents라는 클래스의 객체가 존재하고 이 객체를 int 타입으로 형변환을 지원하지 않는다.
- 직접 형변환을 오버로딩해서 구현해야 함

### 형변환 오버로딩
> `operator 변환할 자료형 ()`
```cpp
#include <iostream>
using namespace std;

class Cents
{
private:
    int value;

public:
    Cents(int cents = 0) { value = cents; }
    int getCents() const { return value; }
    int &getCents() { return value; }
    
    // int 타입으로 형변환
    operator int() 
    {
        return value;
    }   
};

void printInt(const int &value)
{
    cout << value << endl;
}

int main(void)
{
    Cents cents(20);
    printInt(cents); // int 타입 O

    int icents = cents; // 직접 형변환 O
    cout << icents << endl;
    
    return 0;
}
```
- `printInt(cents);`
  - `printInt` 함수는 `int` 타입을 매개변수로 받는데 Cents 클래스에서 `int` 타입에 대해 형변환이 오버로딩되어있기 때문에 사용 가능하다.

- `int icents = cents;`
  - int 타입의 icents라는 변수에 Cents타입의 cents를 담을 수 있다. (형변환이 오버로딩 되어있기 때문)

### 형변환 확인하기
**Dollar -> Cents로 형변환, Cents -> int 형변환**
```cpp
#include <iostream>
using namespace std;

class Cents
{
private:
    int value;

public:
    Cents(int cents = 0) { value = cents; }
    int getCents() const { return value; }
    int &getCents() { return value; }

    operator int()
    {
        cout << "Cents to int!" << endl;
        return value;
    }
};

class Dollar
{
private:
    int value = 0;

public:
    Dollar(const int &input) : value(input) {}

    operator Cents()
    {
        cout << "Dollar to Cents!" << endl;
        return Cents(value * 100);
    }

};

void printInt(const int &value)
{
    cout << value << endl;
}

int main(void)
{
    Dollar dollar(5);
    Cents cents2 = dollar;
    printInt(cents2);
    
    return 0;
}
```
- Dollar 클래스 안에 Cents에 대한 형변환을 오버로딩해서 Cents 타입 변수에 Dollar 타입 객체를 담을 수 있다.
- 각 형변환시 로그를 찍어 어느 시점에 형변환이 일어나는지 확인할 수 있음
  - `Cents cents2 = dollar;`
  - `printInt(cents2);` 