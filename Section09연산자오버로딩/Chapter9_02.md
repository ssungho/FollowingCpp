# Chapter9_02 입출력 연산자 오버로딩

## 입출력 연산자 오버로딩
**입력 연산자 `<<`와 출력 연산자 `>>`를 오버로딩하여 사용자 정의 클래스에 대해 사용할 수 있다.**

### 입출력 연산자 <<, >>

**출력 연산자 `<<`는 출력 관련 기능을 가진 `ostream` 클래스에, 입력 연산자 `>>`는 입력 관련 기능을 가진 `istream` 클래스에 정의되어 있다.** <br>
**`iostream`은 `ostream`과 `istream`을 모두 상속받고 있다.**
> ***`iostream` 헤더 파일을 include하여 입출력 기능을 모두 사용할 수 있음.***

### std::cout과 std::cin
**std::cout**
- **`std::cout`은 ostream 클래스의 객체이다.**
- ostream 클래스 내부에 여러 자료형에 대한 출력 연산자의 오버로딩이 되어있다.
- `cout << 데이터`
  - ostream 타입의 객체인 cout과 데이터에 대한 연산 결과는 출력 연산자 << 오른쪽에 있는 데이터를 출력하고, 반환형이 ostream 타입의 객체가 된다.
  - **반환형이 ostream 타입의 객체여서 연산자에 대한 체이닝이 가능하여 `cout << data1 << data2 << data3;`과 같이 여러 데이터를 연속으로 출력할 수 있다.**
  
**std::cin**
- **`std::cin`은 istream 클래스의 객체이다.**
- istream 또한 ostream과 같이 클래스 내부에 여러 자료형에 대한 입력 연산자의 오버로딩이 되어있다. 
- `cin >> 데이터`
  - istream 타입의 객체인 cin과 데이터에 대한 연산 결과는 입력 연산자 >> 오른쪽에 있는 데이터를 입력받고, 반환형이 istream 타입의 객체가 된다.
  - **반환형이 istream 타입의 객체여서 연산자에 대한 체이닝이 가능하여 `cin >> data1 >> data2 >> data3;`과 같이 연속으로 여러 데이터를 입력받을 수 있다.**

> ***cin과 cout이 istream과 ostream 타입의 객체라는 것이 중요함***

### 출력 연산자 << 오버로딩
**Point 클래스의 `private` 멤버에 접근하기 위해 `friend` 함수로 등록하여 오버로딩 한다.**
```cpp
#include <iostream>
using namespace std;

class Point
{
private:
	double m_x, m_y, m_z;

public:
	Point(double x = 0, double y = 0, double z = 0)
		: m_x(x), m_y(y), m_z(z)
	{}

	// 출력 연산자를 friend 함수로 등록 & 오버로딩
	friend std::ostream& operator << (std::ostream& out, const Point& point)
	{
		out << point.m_x << " " << point.m_y << " " << point.m_z;

		return out;
	}
};

int main(void)
{
	Point p1(0.0, 0.1, 0.2), p2(1.0, 1.5, 2.0);
	cout << p1 << " " << p2 << endl;

	return 0;
}
```
```
출력 결과
0.0 0.1 0.2
1.0 1.5 2.0
```
출력 연산자 `<<`는 Point 클래스의 멤버로 구현된 것이 아니라 `iostream-ostream`에 정의되어 있고 Point 클래스에서 friend 함수로 등록한 것이다.<br>
`ostream`타입의 참조 객체인 `out`을 반환하기 때문에 반환된 객체와 계속해서 연산을 진행하여 출력이 가능하다.
> ***`cout << p1`을 진행하고 다시 cout으로 반환하여 `cout << " "`이 진행되고, " "을 출력한 것이 다시 cout으로 반환되어 `cout << p2`가 실행되는 것***

### 입력 연산자 >> 오버로딩
**`<<`과 마찬가지로 `friend` 함수로 등록하여 오버로딩 한다.**<br>
**`<<` 오버로딩과 다르게 우측 피연산자에 `const` 키워드를 사용할 수 없다.**
> ***우측 피연산자의 값을 바꾸는 연산이기 때문에 `const` 키워드를 사용할 수 없는 것***
```cpp
friend std::istream& operator >> (std::istream& in, Point& point)
{
	in >> point.m_x >> point.m_y >> point.m_z;
	
	return in;
}

int main(void)
{
    Point p1, p2;
    cin >> p1 >> p2;
    
    return 0;
}
```
입력 연산자 `>>`는 `iostream-istream`에 정의되어 있기 때문에 `friend` 함수로 등록하여 오버로딩한 것이다.
`istream` 타입의 참조 객체인 `in`을 반환하기 때문에 반환된 객체와 계속해서 연산을 진행하여 입력이 가능하다.

## 파일 입출력
**`fstream` 헤더에 정의된 `ofstream` 클래스와 `ifstream` 클래스를 이용하여 콘솔이 아닌 파일에 값을 입출력할 수 있다.**
```cpp
#include <iostream>
#include <fstream>
#include <string>
using namespace std;

Class Point 
{
    // ...
};

int main(void)
{
    // out.txt 파일 생성
	ofstream of("out.txt");
	Point p1, p2;
	cin >> p1 >> p2;
	
    // out.txt에 입력한 p1과 p2을 작성
	of << p1 << " " << p2 << endl;
	of.close();

    // out.txt 파일 로드
	ifstream file("out.txt");
    string line;
	if (file.is_open())
	{
        // 각 줄을 출력
		while (getline(file, line))
		{
			cout << line << endl;
		}
		file.close();
	}

	return 0;
}
```
> *코드를 실행후 작업 폴더를 확인하면 out.txt가 생성되고 내용은 입력한 p1과 p2의 값이 들어가 있고 콘솔에도 이 내용이 출력됨*