# Chapter9_05 증감 연산자 오버로딩

## 증감 연산자 오버로딩

**증감 연산자**
- **`++`(증가 연산자), `--`(감소 연산자)**
증감 연산자가 앞에 붙냐 뒤에 붙냐에 따라서 전위 증감 연산자, 후위 증감 연산자로 나눌 수 있다.

### 전위 증감 연산자
**전위 증감 연산자는 연산 결과가 바로 반환된다.**
```cpp
int value = 5
cout << ++value << endl; // 6
cout << --value << endl; // 5
cout << value << endl;   // 5
```
`++value`를 통해 `value`는 `6`이 되었고 이 값이 바로 출력된다.

### 후위 증감 연산자
**후위 증감 연산자는 연산이 실행되는 것은 같지만, 자신이 가지고 있던 값을 반환한다.**

```cpp
int value = 5
cout << value++ << endl; // 5
cout << value-- << endl; // 6
cout << value << endl;   // 5
```
후위 연산을 실행하면 연산 자체는 진행되었지만 연산 되기 이전의 값을 출력한다.
- *3행의 `value--`를 진행했을 때는 `value = value -1`이 되기 전인 `6`이 출력되고 , 4행에서 `value`를 출력했을 때 `5`가 출력됨*

### 전위 증감 연산자 오버로딩
**증감 연산자는 피연산자를 자기 자신 1개만 필요로 하기 때문에 `friend` 키워드를 이용하여 전역 함수를 오버로딩 할 경우 매개변수는 1개를 필요로 하고 멤버 함수로 오버로딩할 경우 매개변수가 필요 없다.**

```cpp
class Digit
{
private:
    int value;

public:
    Digit(int digit = 0) { value = digit; }

    // 전위 증가 연산자 (멤버 함수로 오버로딩)
    Digit& operator++() // 파라미터 x
    {
        ++value;
        return *this;
    }

    // 전위 감소 연산자 (멤버 함수로 오버로딩)
    Digit& operator--() // 파라미터 x
    {
        --value;
        return *this;
    }

    friend std::ostream& operator<<(ostream& out, const Digit& digit)
    {
        out << digit.value;
        return out;
    }
};
```
> ***전위 증감 연산자는 자기 자신을 참조하여 연산을 진행하기 때문에 `Digit &`(객체 참조) 형식으로 값을 반환한다.***
> - ***`*this`를 반환하는 이유임***

### 후위 증감 연산자 오버로딩
**후위 증감 연산자는 전위 증감 연산자를 사용하기 때문에 먼저 전위 증감 연산자가 오버로딩되어 있어야 한다.**

```cpp
// 후위 증가 연산자
Digit operator++(int) // 파라미터로 더미값을 넣음
{
    Digit temp(value);
    ++(*this);

    // 값을 증가시키고 증가시키기 이전의 값을 반환
    return temp;
}

// 후위 감소 연산자
Digit operator--(int) // 파라미터로 더미값을 넣음
{
    Digit temp(value);
    --(*this);

    // 값을 감소시키고 감소시키기 이전의 값을 반환
    return temp;
}
```
> ***후위 증감 연산자는 연산을 진행하는 것은 전위 증감 연산자와 동일하지만, 증감 연산 이전의 값을 반환하기 때문에 증감 연산 이전의 값을 저장해두고 연산이 끝나면 이 값을 반환한다.***
> - ***객체 자신을 참조하여 전위 증감 연산을 실행하고 자신의 값을 저장했던 temp를 반환하는 것***