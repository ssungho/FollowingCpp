# Chapter9_04 비교 연산자 오버로딩

## 비교 연산자 오버로딩
**비교 연산자**
- **`==`, `!=``, `<=`, `>=`, `<`, `>`**

### `==`, 오버로딩
**`==`는 두 객체가 같은지 비교하교 같으면 `true`를 다르면 `false`를 반환하며 `!=`는 반대의 결과를 반환한다.**

**`==` 연산을 객체에 대해 사용하면 두 객체가 같은 주소를 가지고 있는지에 대하여 결과값을 반환한다.**

**`==`을 이용해서 두 객체 사이의 특정 값을 비교할 경우 `==` 연산자를 오버로딩해서 사용할 수 있다.**

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
    int& getCents() { return value; }

    // 전역 함수를 friend 함수로 등록해서 오버로딩
    friend bool operator == (const Cents cents1, const Cents cents2)
    {
        return cents1.value == cents2.value;
    }

    friend std::ostream& operator << (ostream& out, const Cents& cents)
    {
        out << cents.value;
        return out;
    }
};

int main()
{
	Cents cents1 = Cents(5);
	Cents cents2 = Cents(10);

	if (cents1 == cents2)
		cout << "Equal" << endl;
	else
		cout << "Not Equal" << endl;

    return 0;
}
```
- friend 함수로 `==` 연산자를 오버로딩하여 `private` 멤버에 접근을 허락한다.
- 반환 타입은 `bool` 형식이며 `cents1`과 `cents2`의 `value`를 비교한 결과를 반환한다.
  - `5 == 10` -> `false`를 반환함

### `<` 오버로딩
**`<`는 두 값의 크기를 비교하여 우측 피연산자의 값이 더 클 경우 true를 반환하는 연산이다.**<br>

**이 연산 오버로딩하여 사용할 경우 객체가 가진 특정 멤버의 값을 비교하는 식으로 사용할 수 있다.**

> ***`>` 연산자는 오버로딩 할 수 없다.***<br>
> ***많은 c++ 라이브러리들은 `<` 연산자를 기준으로 정렬이나 비교 연산을 수행하며, `<` 연산자만으로도 다양한 비교 작업을 할 수 있다.***<br>
> ***`<` 연산자를 오버로딩하여 결과를 `>`로 리턴하는 방식으로 구현해야 한다.***

```cpp
friend bool operator < (const Cents cents1, const Cents cents2)
{
	return cents1.value < cents2.value;
}
```
`friend bool operator < (const Cents cents1, const Cents cents2)`
- friend 함수로 `<` 연산자를 오버로딩하여 `private` 멤버에 접근을 허락한다.
- 반환 타입은 `bool` 형식이며 `cents2`의 `value`가 `cents1`의 `value`보다 크다면 `true`를 반환한다.

### 멤버 함수로 오버로딩
**위에서 `friend` 키워드를 이용하여 전역 함수를 클래스 내부에 등록해서 사용했지만 클래스 내부에서 오버로딩해서 사용할 수 도 있다.**

```cpp
// 멤버 함수로 == 오버로딩
bool operator == (const Cents cents2)
{
	return this->value == cents2.value; 
}

// 멤버 함수로 < 오버로딩
bool operator < (const Cents cents2)
{
	return this->value < cents2.value;
}
```
> ***`this` 키워드는 생략 가능함***<br>
> ***비교 연산자는 피연산자 두 개를 필요로 하는데 멤버 함수로 오버로딩해서 사용할 경우 좌측 피연산자가 자신의 인스턴스가 되기 때문에 비교 대상만 매개변수로 받으면 됨***
> - ***`this`포인터를 이용하여 자신의 `value`와 매개변수로 받은 피연산자의 `value`를 비교하는 것***