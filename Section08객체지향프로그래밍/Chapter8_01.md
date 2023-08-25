# 객체지향 프로그래밍과 클래스 (OOP & Class)

## ex 친구 정보 출력하기
친구의 정보를 저장하고 저장한 정보를 출력하는 프로그램을 작성한다고 가정해보자.

### 모든 데이터를 작성
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

// 정보 출력
void print(const string &name, const string &address, const int &age, const double &height, const double &weight)
{
    cout << name << " " << address << " " << age << " " << height << " " << weight << endl;
}

int main()
{
    // 친구의 정보를 작성한다고 가정
    string name;
    string address;
    int age;
    double height;
    double weight;

    // 각 데이터가 저장된 리스트
    vector<string> name_vec;
    vector<string> addr_vec;
    vector<int> age_vec;
    vector<double> height_vec;
    vector<double> weight_vec;

    // 첫 번째로 저장된 친구의 정보를 출력하는 코드
    print(name_vec[0], addr_vec[0], age_vec[0], height_vec[0], weight_vec[0]);

    return 0;
}
```
기존에 공부한 방식으로 친구에 대한 정보를 저장하고 출력하는 기능을 만든다면 위 코드처럼 작성할 수 있을 것이다.

저장된 정보를 출력하는 `print()`의 경우 저장된 정보를 하나씩 파라미터로 넘겨 굉작히 길고 복잡해진다.

### 구조체 사용
```cpp
// Friend 구조체를 만들어 관리
struct Friend
{
    string name;
    string address;
    int age;
    double height;
    double weight;
};

// 구조체 자체를 받는 print()
void print(const Friend &fr)
{
    cout << fr.name << " " << fr.address << " " << fr.age << " " << fr.height << " " << fr.weight << endl;
}

int main()
{
    // Friend struct jj 선언
    Friend jj{"Jack Jack","Uptown", 2, 30, 10};

    // jj의 정보 출력
    print(jj);

    return 0;
}
```
친구에 대한 정보를 `struct`로 만들어 사용한다면, 매개변수로 `struct`만 넘겨주면 되기 때문에 코드가 간결해진다.

### 기능 자체를 구조체의 멤버로 정의
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

// 구조체를 만들어 관리
struct Friend
{
    string name;
    string address;
    int age;
    double height;
    double weight;

    // print 함수를 Friend의 멤버로 정의
    void print()
    {
        // 같은 struct에 속하기 때문에 얼마든지 접근할 수 있음
        cout << name << " " << address << " " << age << " " << height << " " << weight << endl;
    }
};

int main()
{
    // Friend struct jj 선언
    Friend jj{"Jack Jack", "Uptown", 2, 30, 10};

    // jj의 print() 실행
    jj.print();

    return 0;
}
```
함수를 구조체의 멤버로 정의할 수 있는데 `print()`를 `Friend`의 멤버로 구현하면 사람이 이해하고 디자인 하기 훨씬 편해진다.

## 클래스 Class

**데이터와 기능이 묶여있는 것을 객체 `Object`라고 한다.**

**구조체(struct)에도 기능(함수)을 넣을 수 있는데 많은 객체지향 프로그래밍 기법들을 다룰 수 있게 해주는 것이 클래스(Class)이다.**

**일반적으로 구조체를 사용할 때는 데이터를 묶는데만 쓰고, 기능을 넣을 때는 클래스를 쓰는 것이 일반적이다.**

> **객체라는 것은 개념이고 이것을 프로그래밍 언어로 구현하는 키워드가 클래스(Class)다.**


### 클래스로 작성한 Friend
```cpp
// Class
class Friend
{
public: // access specifier (public, private, protected)
    string name;
    string address;
    int age;
    double height;
    double weight;

    void print()
    {
        cout << name << " " << address << " " << age << " " << height << " " << weight << endl;
    }
};
```

> **메모리를 차지하도록 선언해주는 것을 `instantiation`, 구현물을 `instance`라고 함**
> 
> Friend 클래스 자체는 메모리를 할당받은 것이 아니기 때문에 주소값을 확인할 수 없음

```cpp
int main()
{
    // Friend struct jj 선언
    Friend jj{"Jack Jack", "Uptown", 2, 30, 10};

    // 친구가 여러명이라고 가정
    vector<Friend> my_friends;
    my_friends.resize(2);

    // 모든 친구를 출력하는 코드도 간결하게 나타낼 수 있음  
    for(auto &ele : my_friends)
        ele.print();

    return 0;
}
```
> **클래스와 구조체의 차이점중 하나는 클래스를 사용할 때는 접근 한정자(access specifier)를 사용한다는 것**