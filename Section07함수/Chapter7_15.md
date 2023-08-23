# Chapter7_15 명령줄 인수 command line argument
## command line
프로그램을 command line에서 실행해야 하는 경우가 있다.

main()함수를 호출할 때 전달할 수 있는 파라미터
- **int자료형과 문자열 배열**

```cpp
#include <iostream>
using namespace std;

int main(int argc, char *argv[])
{
	for (int i = 0; i < argc; i++)
	{
		cout << argv[i] << endl;
	}
}
```
 `argc` 만큼 반복하여 `argv[]`의 내용을 꺼내보면 최종적으로 프로그램의 이름이 출력된다.

여기서 `argc`는 `argv[]`의 길이를 나타내고, `argv[]`의 0 번째 문자열은 **프로그램의 이름**, 그 이후 1 번부터는 **사용자가 입력한 문자열**이 된다. 


> 프로그램의 이름, 100, 1024, 3.14를 순서대로 입력한다면 아래와 같은 결과를 얻을 수 있음
> ![**Alt text**](/Section07함수/iamges/7_15/image.png)
> **`argv[]`에는 프로그램의 이름, 입력받은 문자열들이 저장되어 있는 것**


## command line 입력 방법
**command line을 입력하는 방법 두 가지**
-  명령 프롬프트(cmd)를 이용하여 프로젝트의 경로로 이동하여 명령줄 인수를 입력
- VisualStudio의 프로젝트 > 속성 > 디버깅 > 명령 인수 여기에 명령줄 인수를 입력 

## 명령줄 인수 사용
아래 코드는 명령줄 인수로 들어온 문자열을 사용하는 예시임
```cpp
int main(int argc, char *argv[])
{
	for (int i = 0; i < argc; i++)
	{
		std::string argv_single = argv[i];
		
		if (i == 1)
		{
			int input_number = std::stoi(argv_single);
			cout << input_number + 1 << endl;
		}
		else
			cout << argv_single << endl;
	}
}
```
명령줄 인수에 1024를 넣을 경우 1025가 출력된다.
> **입력 받은 값은 문자열로 저장되기 때문에 1024에 +1을 하기 위해 `stoi()`를 사용** <br>
> **`argv[0]`은 프로그램의 이름이기 때문에 입력받은 첫 번째 문자를 사용하기 위해 `argv[1]`, `i == 1`이라는 조건을 둔 것**

오류처리를 매번 자세하게 해줘야하는 번거로움 등의 불편함이 있을 수 있는데 직접 구현하거나 Boost 라이브러리를 이용해서 해결할 수 있음
