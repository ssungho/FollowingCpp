# Chapter7_12 방어적 프로그래밍의 개념

## 1. syntax error (문법 오류)
요즘은 Ide가 좋아져서 미리 잡아주는 경우도 있고 컴파일러가 잡아주는 경우도 있다.
> **컴파일러를 이용해서 오류를 잡도록 프로그래밍하자**

## 2. semantic errors (문맥 오류)
```cpp
int main(void)
{
    int x;
    cin >> x;
    if (x >= 5)
        cout << "x is greater than 5" << endl;

    return 0;
}
```
위 코드에서 작성자는 x가 5보다 클 경우 cout .. 구문이 실행되길 원했지만, `x >= 5`로 작성해 5일 경우에도 실행되는 문맥적 오류가 생긴다.<br>
> 이런 문제는 작성자 이외엔 알기 어렵기 때문에 **프로그래머가 항상 신경써서 코드를 작성해야 함**

## 3. violated assumption (의도한 것과 다르게 사용)
```cpp
int main(void)
{
    string hello = "Hello, my name is jack";

    int ix;
    cin >> ix;

    cout << hello[ix] << endl;
    return 0;
}
```
위와 같은 코드에서 우리는 `ix`를 `hello`에 저장된 문자열의 길이 이내의 값을 사용해야한다고 알 수 있겠지만, 이런 의도를 알아채지 못한다면 엉뚱한 값을 넣어 런타임 에러가 발생할 수 있다.<br>

이런 상황을 방지하기 위해서 몇 가지 방법이 있는데 하나는 사용자에게 범위를 알려주는 것이다.

```cpp
int main(void)
{
    string hello = "Hello, my name is jack";

    cout << "Input from 0 to " << hello.size() - 1 << endl;

    // ...
}
```

하지만 이렇게 범위를 알려주기만 해서는 사용가의 실수 혹은 불순한 의도에 의해서 런타임 에러가 난다.

그렇다면 의도한 입력 값을 사용하게 하기 위해 의도하지 않은 값에 대해 처리를 해주면 될 것이다.
```cpp
int main(void)
{
    string hello = "Hello, my name is jack";

    cout << "Input from 0 to " << hello.size() - 1 << endl;

    while(ture)
    {
        int ix;
        cin >> ix;

        if(ix > 0 && ix <= hello.size() -1)
            cout << hello[ix] << endl;
        else
            cout << "Please try again" << endl;
    }
    return 0;
}
```
이렇게 작성하면 사용자가 0 ~ 26의 값을 넣지 않았을 때 다시 값을 입력받아 작성자의 의도대로 사용할 수 있게 된다.<br>

> 프로그래밍을 할 때 사용자 테스트를 했을 때 문제가 없도록 여러가지 **방어장치**를 만들어야 하고, **컴파일러가 미리 오류를 방지하도록 코드를 작성하는 것**이 좋음