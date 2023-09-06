# Chapter9_01 산술 연산자 오버로딩 하기

## 산술 연산자 오버로딩
**자주 사용하는 연산자 +, -, * 등을 사용자 정의 클래스에서 오버로딩하여 사용할 수 있다.**

```cpp
class Cents 
{
private:
	int value;

public:
	Cents(int cents = 0) { value = cents; }

	int getValue() const { return value; }
};

Cents add(const Cents& cents1, const Cents& cents2)
{
	return (cents1.getValue() + cents2.getValue());
}

int main()
{
	Cents cents1(5);
	Cents cents2(15);
	
	cout << add(cents1, cents2).getCents() << endl; // 20
}
```
Cents 클래스에서 두 Cents의 값을 더하는 기능을 만들고자 한다면 `add` 함수를 만들어서 값을 반환받을 수 있다.

> ***단순히 산술 연산을 구현한 것이기 때문에 산술 연산자 오버로딩을 이용하여 간편하게 표기할 수 있을 것***

### 연산자를 오버로딩하는 방법 <br>
`[반환 타입] operator  [연산자] (산술 대상..) {...}`

### 일반 함수로 구현하는 방법
**Cents 클래스 외부에서 일반 함수로 연산자를 오버로딩하여 value의 값을 연산할 수 있다.**
```cpp
Cents operator + (const Cents& cents1, const Cents& cents2)
{
	return Cents(cents1.getCents() + cents2.getCents());
}

int main()
{
	Cents cents1(5);
	Cents cents2(15);

	cout << (cents1 + cents2).getCents() << endl; // 20
}
```
> ***+ 연산자를 오버로딩하여 value의 합을 반환 받음***
### friend 함수로 구현하는 방법
접근하고자 하는 값이 private 멤버라면 friend로 등록하여 사용할 수 있다.
```cpp
class Cents 
{
private:
	int value;

public:
	Cents(int cents = 0) { value = cents; }
	int getCents() const { return value; }
	
    // friend로 등록
	friend Cents operator + (const Cents& cents1, const Cents& cents2);
};

Cents operator + (const Cents& cents1, const Cents& cents2)
{
	return Cents(cents1.value + cents2.value);
}

int main()
{
	Cents cents1(1);
	Cents cents2(9);

	cout << (cents1 + cents2).getCents() << endl; // 10
}
```
> ***+ 를 오버로딩한 함수를 Cents에 friend로 등록했기 때문에 `Cents(cents1.value + cents2.value)`에서 private 멤버인 value에 접근할 수 있게 된 것***

### 멤버함수로 구현하는 방법
**연산자를 오버로딩한 함수를 클래스 클래스 내부로 옮겨 캡슐화시킬 수 있다.**

***📌 클래스의 멤버로 연산자를 오버로딩할 경우 좌측 피연산자는 이 연산자를 호출하는 객체가 되고 우측 피연산자는 인자로 전달 받는 값이 된다.***

```cpp
Cents operator + (const Cents& cents2)
{
	return Cents(this->value + c2.value);
}

int main()
{
	Cents cents1(10);
	Cents cents2(20);

	cout << (cents1 + cents2).getCents() << endl; // 30
}
```
- `Cents operator + (const Cents& cents2)`
  - Cents 클래스의 멤버로 들어왔기 때문에 좌측 피연산자는 이 함수를 호출하는 객체가 되어 this로 생략된다.
  - **반환하는 부분인 `return Cents(this->value + c2.value)`를 보면 자기 자신의 value를 `this` 포인터로 호출하고 있음**

## 오버로딩을 할 수 없는 연산자
**`?:(조건 연산자)`, `::(범위 연산자)`, `sizeof(크기 연산자)`, `.(멤버 접근 연산자)`, `.*(포인터 멤버 접근 연산자)`는 오버로딩할 수 없다. **

## 멤버로만 오버로딩해야 하는 연산자
**`=(대입 연산자)`, `[](배열 접근 연산자)`, `()(함수 호출 연산자)`, `->(멤버 포인터 접근 연산자)`는 멤버로만 오버로딩 해야 한다.**

## 연산자의 우선순위
**오버로딩하여 사용한 연산자의 우선순위는 오버로딩 하기 전의 우선순위와 같다.**
> ***오버로딩한 `+`는 오버로딩한 `*`보다 우선순위가 낮음***<br>
> ***연산 순위가 애매하거나 매우 낮아 괄호를 신경써야 하는 경우 예제로 사용한 `add` 함수처럼 함수를 만들어서 사용하는 편이 더 나을 수 있음***