# Chapter7_12 재귀적 함수 호출

## ex 1
```cpp
#include <iostream>
using namespace std;

void countDown(int count)
{
	cout << count << endl;
	countDown(count - 1);
}

int main()
{
	countDown(5);
	return 0;
}
```

countDown() 함수는 함수 내에서 자기 자신을 호출한다. (재귀라고함)<br>

> **우리가 작성한 코드는 메모리에 저장되고 함수를 호출할 때는 함수가 저장된 메모리의 주소를 호출함**<br>
> **그렇기 때문에 함수 내에서 다시 같은 코드를 호출해서 사용할 수 있는 것**

종료하는 조건이 반드시 필요하고 너무 많이 호출하면 stackoverflow가 난다.

```cpp 
// 종료 조건 추가
void countDown(int count)
{
	cout << count << endl;
    
    if(count > 0)
	    countDown(count - 1);
}
```
5부터 1까지 출력하고 프로그램이 종료된다.<br>
> **종료 조건이 없으면 stackoverflow 오류가 남**

## ex 2
```cpp
#include <iostream>
using namespace std;

int sumTo(int sumto) 
{
	if (sumto <= 0)
		return 0;
	else if (sumto <= 1)
		return 1;
	else
		return sumTo(sumto - 1) + sumto;
}

int main()
{
	cout << sumTo(10) << endl;

	return 0;
}
```
위 코드를 실행하면 `sumto`가 0이 될때까지 자기 자신을 호출하고 결과는 10부터 1까지의 합이 된다.<br>
> **`0`이 종료조건인 셈**

```cpp
// 디버깅을 더 편하게 하기 위한 방법
int sumTo(int sumto) 
{
	if (sumto <= 0)
		return 0;
	else if (sumto <= 1)
		return 1;
	else
	{
		const int sum_minus_one = sumTo(sumto - 1);
		return sum_minus_one + sumto;
	}
}
```
> **반환식에서 `sumTO(sumto - 1)`부분을 상수로 빼서 확인하기 편해짐**


## ex 3
### 피보나치 수열
재귀를 사용하는 흔한 예제로 **피보나치 수열**이 있다.<br>

```cpp
int fibonacci(int number) 
{
	if (number == 1 || number == 2)
		return 1;

	return fibonacci(number - 1) + fibonacci(number - 2);
}
```
> **강의에서 피보나치 수열을 재귀 호출로 구현해 보는 것을 권장했기에 작성해봄**