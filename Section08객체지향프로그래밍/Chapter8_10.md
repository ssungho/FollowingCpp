# Chapter8_10 정적 멤버 변수

### 정적 변수가 함수 안에 정의되어 있는 경우
```cpp
#include <iostream>
using namespace std;

int generateID()
{
	static int s_id = 0;
	return ++s_id;
}

int main()
{
	cout << generateID() << endl;
	cout << generateID() << endl;
	cout << generateID() << endl;

	return 0;
}
```
**출력 결과**
```
1
2
3
```
`s_id`가 `static`으로 선언되었기 때문에 모든 `generateID()`는 같은 `s_id`의 값을 증가시킨다.

### 멤버 변수를 static으로 선언
**일반 static 변수는 선언과 동시에 초기화 할 수 없다.**
```cpp
class Something 
{
public:
	static int s_value;
};

int Something::s_value = 1; // define in cpp

int main()
{
    Something st1;
    Something st2;
    
    Something::s_value = 1024;
    // ...

}
```
`static` 변수인 `s_value`는 클래스 내에서 초기화를 할 수 없고 헤더파일이 아닌 cpp파일에 정의해야 한다. <br>
st1의 s_value와 st2의 s_value는 같은 값을 가리킨다.
- st1의 s_value를 바꾸면 st2의 s_value도 바뀜
- `static`변수이기 때문에 바로 접근 가능 (`Something::s_value`)

### 멤버 변수를 static const로 선언
그냥 static으로 선언한 것과 다르게 const가 붙는다면 선언과 동시에 초기화 해야 한다.
```
static const int s_value = 1;
```
- 외부에서 변경할 수 없음
- `constexpr`도 똑같이 사용할 수 있음

