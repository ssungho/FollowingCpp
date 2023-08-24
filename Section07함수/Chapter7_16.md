# Chapter7_16 생략부호 Ellipsis
함수를 구현할 때 매개변수의 개수가 정해져있지 않았으면 좋겠다라고 생각이 들 때 Ellipsis를 사용할 수 있다.

## ex 1
```cpp
#include <iostream>
#include <cstdarg> // for ellipsis
using namespace std;

double findAverage(int count, ...)
{
    double sum = 0;

    va_list list; // va_list는 Char*로 이루어져 있다. -> 문자열로 받음

    va_start(list, count); // argument의 개수를 count만큼 받는다.

    for (int arg = 0; arg < count; arg++)
        sum += va_arg(list, int); // va_list 사용시 va_arg사용한다. -> lsit의 내용을 int형으로 변환
    
    va_end(list); // 마칠때 va_end()을 선언한다.

    return sum / count;
}

int main()
{
    cout << findAverage(1, 1, 2, 3, "Hello", 'c') << endl; // 1개의 인자를 받아서 평균을 반환
    cout << findAverage(3, 1, 2, 3) << endl; // 3개의 인자를 받아서 평균을 반환
    cout << findAverage(5, 1, 2, 3, 4, 5) << endl; // 5개의 인자를 받아서 평균을 반환
    cout << findAverage(10, 1, 2, 3, 4, 5) << endl; // 인자의 개수가 10개가 안됨 -> 오류 발생
    return 0;
}
```
***output*** <br>
1<br>
2<br>
3<br>
**오류**

### Ellipsis 사용시 유의사항
- *개수가 틀리면 오류가 난다.*
- *타입을 미리 맞춰줘야 한다.*
- ***디버깅이 어렵고 사용하기 위험하다.***