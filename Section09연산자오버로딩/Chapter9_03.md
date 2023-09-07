# Chapter9_03 단항 연산자 오버로딩

## 단항 연산자
**단항 연산자는 하나의 피연산자를 가지고 연산을 한다.**
- 부호 연산자 `+`와 `-`
  - ex) -a  
    - ` a가 1이라면 결과는 -1`
- 논리 부정 연산자 `!`
  - ex) !a  
    - ` a가 true라면 결과는 0(false)`

### `-` 연산자 오버로딩
**부호를 반전시키는 `-` 연산자를 사용자가 정의한 클래스에 대해 멤버 함수로 오버로딩할 수 있다.**
```cpp
#include <iostream>
using namespace std;

class Cents
{
private:
	int value;

public:
	Cents(int cents = 0) { value = cents; }

    // 부호 연산자 - 오버로딩
	Cents operator - () const
	{
		return Cents(-value);
	}

    // 출력 연산자 << 오버로딩
	friend std::ostream& operator << (std::ostream& out, const Cents& cents)
	{
		out << cents.value;
		return out;
	}
};

int main(void)
{
	Cents cents1(5);
	Cents cents2(0);

	cout << cents1 << endl;
	cout << -cents1 << endl;
	cout << -Cents(10) << endl;

	return 0;
}
```
```
출력 결과
5
-5
-10
```
- `-` 연산자를 멤버 함수로 오버로딩할 경우 매개변수가 필요 없다.
  - 자기 자신이 피연산자가 되기 때문(`this` 포인터)
> ***출력 연산자도 오버로딩되어있기 때문에 `value`에 대해 부호가 바뀐것을 확인할 수 있음***

### `!` 연산자 오버로딩
**논리 부정 연산자 `!`를 오버로딩할 경우 어떤 논리에 의해서 결과를 반환하게 될 것인지 정의해야 한다.**
```cpp
class Cents
{
    // ...
    bool operator !() const
    {
	    // value가 0일 경우에 false(!true)를 반환
	    return (value == 0) ? true : false;
    }
};

int main
{
    Cents cents1(5);
    Cents cents2(0);

    cout << !cents1 << endl; // value == 5 -> !(true) -> false
    cout << !cents2 << endl; // value == 0 -> !(false) -> 1
}
```
`!`는 bool값을 반환하기 때문에 오버로딩하여 사용할 때 어떠한 조건으로 참, 거짓을 반환할지 문맥에 맞게 잘 작성해야 한다.
> ***`Cents` 클래스를 재화로 생각하고 `!`를 돈이 없는 경는지 확인하는 용도로 정의했을 때,<br>
> `cents1`은 `5`의 `value`를 가지고 있기 때문에 `거짓(0)`을 반환하고, <br>
>  `cents2`는 `0`의 `value`를 가지고 있기 때문에 `참(1)`을 반환한다.***