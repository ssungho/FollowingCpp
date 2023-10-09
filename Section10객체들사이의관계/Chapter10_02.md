# Chapter10_02 구성 관계(Composition)

**관계 표**
| 관계        | 관계를 표현하는 동사 | 관계의 형태    | 다른 클래스에도 속할 수 있는가? | 멤버의 존재를 클래스가 관리하는가? | 방향성           |
| :---------- | :------------------- | :------------- | :------------------------------ | :--------------------------------- | :--------------- |
| 구성 (요소) | Part-of              | 전체/부품      | No                              | Yes                                | 단방향           |


- `Position2D`클래스는 `Monster`클래스의 멤버로 '구성'된다.
  - `m_location`(Position2D 자료형의 변수)는 `Monster`클래스에 대해 어떠한 정보도 필요로 하지 않는다. (구성품이기 때문)
- `Position2D`클래스는 다른 클래스의 구성품(멤버)로 사용될 수 있지만, `Monster`클래스의 멤버로 존재하는 `m_location`은 다른 클래스의 객체와는 별개의 존재이다.
- `m_location`은 이를 멤버로 가지고 있는 `Monster`객체가 사라질 경우 같이 사라지며 `Monster`객체의 필요에 의해 사용된다.

### Position2D Class
- Position.h에 정의된 Position Class
- x좌표와 y좌표를 이용하여 위치를 나타냄

> `Monster`클래스에서 `Position2D`를 멤버로 사용하여 위치를 나타낼 수 있음.

```cpp
class Position2D
{
private:
    int m_x;
    int m_y;

public:
    Position2D(const int &x_in, const int &y_in) : m_x(x_in), m_y(y_in)
    { }

    void set(const Position2D & pos_target)
    {
        set(pos_target.m_x, pos_target.m_y);
    }

    void set(const int & x_target, const int & y_target)
    {
        m_x = x_target;
        m_y = y_target;
    }

    friend std::ostream &operator<<(std::ostream &out, Position2D &pos2d)
    {
        out << pos2d.m_x << " " << pos2d.m_y;
        return out;
    }
};
```
- 멤버 변수로 x좌표와 y좌표를 가지고 있다.
- `void set(const Position2D & pos_target)`
  - `Position2D`객체를 파라미터로 사용해 내부에서 `set(const int & x_target, const int & y_target)`을 실행한다.
- `set(const int & x_target, const int & y_target)`
  - 입력받은 x좌표와 y좌표로 멤버 좌표를 설정한다.

> *`set`함수를 x좌표와 y좌표를 입력받아서 실행시킬 수 있지만 `Position2D`형태로 입력받을 수 있다.*
> *재사용 될 부분은 따로 빼두는 것이 좋다.(재사용 되는 부분을 수정하게 될 일이 있다면 따로 빼둔 부분만 수정하면 되기 때문)*

### Monster Class
- Monster.h 파일에 정의한 Monster Class
```cpp
class Monster
{
private:
    std::string m_name;
    Position2D m_location;

public:
    Monster(const std::string name_in, const Position2D &pos_in)
        : m_name(name_in), m_location(pos_in)
    {
    }

    void moveTo(const Position2D &pos_target)
    {
        m_location.set(pos_target);
    }

    friend std::ostream &operator<<(std::ostream &out, Monster &monster)
    {
        out << monster.m_name << " " << monster.m_location;
        return out;
    }
};
```
- `void moveTo(const Position2D &pos_target)`
  - 입력받은 위치로 자신의 위치(m_location)를 이동시킨다.
> *position2D.h파일에 iostream이 헤더파일로 선언되어 있다면 이 헤더파일을 선언한 Monster.h에서 iostream을 선언하지 않아도 된다.*
> `Monster`에서 `Position2D`를 멤버로 사용하고 있는데 `Position2D`에서는 `Monster`에 대한 정보를 필요로 하지 않는다.

### main.cpp
- `Monster` 객체를 생성하고 생성된 객체를 이동시킨다.

```cpp
#include "Monster.h"
using namespace std;

int main()
{
    Monster mon1("mon1", Position2D(0, 0));
    cout << mon1 << endl;

    // while (true) // game loop
    {
        // event
        mon1.moveTo(Position2D(1,1));
        cout << mon1 << endl;
    }

    return 0;
}
```
- `mon1.moveTo(Position2D(1,1));`
  - `mon1`의 m_location을 (1,1)로 변경한다.