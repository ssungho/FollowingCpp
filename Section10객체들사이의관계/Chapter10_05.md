# Chapter 10_05 의존 관계(Depend-on)

**관계 표**
| 관계 | 관계를 표현하는 동사 | 관계의 형태    | 다른 클래스에도 속할 수 있는가? | 멤버의 존재를 클래스가 관리하는가? | 방향성 |
| :--- | :------------------- | :------------- | :------------------------------ | :--------------------------------- | :----- |
| 의존 | Depends-on           | 용도 외엔 무관 | Yes                             | Yes                                | 단항뱡 |

- 관계가 약하다.
- 다른 클래스의 기능을 잠시 사용한다는 느낌
- 멤버로도 사용될 수 있지만 그렇지 않은 경우도 많다.
- 환자가 목발을 사용할 때 목발은 환자에 대해 알 필요가 없음.
  - 환자가 잠시 목발을 이용하고 다리가 나으면 사용하지 않게됨.

### Timer.h
시간을 잴 수 있는 기능을 가진 Timer 클래스
```cpp
#pragma once

#include <iostream>
#include <vector>
#include <algorithm>
#include <random>
#include <chrono>

class Timer
{
    using clock_t = std::chrono::high_resolution_clock;
    using second_t = std::chrono::duration<double, std::ratio<1>>;

    std::chrono::time_point<clock_t> start_time = clock_t::now();

public:
    void elapsed()
    {
        std::chrono::time_point<clock_t> end_time = clock_t::now();

        std::cout << std::chrono::duration_cast<second_t>(end_time - start_time).count() << std::endl;
    }
};
```
- `void elapsed()`
  - 함수의 실행 시간을 출력해준다.

### Worker.h
`Worker` 클래스에는 `doSomething()`이라는 함수가 존재하며 실제로는 `Timer` 클래스를 이용하지만 선언 부분에서는 알 필요가 없다.
```cpp
#pragma once

class Worker
{
public:
	void doSomething();
};
```

### Worker.cpp
`Worker`클래스의 `doSomething()`이 정의된 cpp파일로 실제로 `Timer`클래스를 Include하여 사용하는 것을 확인할 수 있다.

```cpp
#pragma once

#include "Worker.h"
#include "Timer.h"

void Worker::doSomething()
{
	Timer timer; // 타이머 시작

	timer.elapsed(); // 타이머 종료
}
```

### main.cpp
```cpp
#include "Worker.h"

using  namespace std;

int main()
{
	Worker().doSomething();

	// do some work here

	return 0;
}
```
`Worker` 클래스에서 `doSomething()` 함수를 실행하면 `Timer` 클래스에 정의된 `elapsed()` 함수를 사용하게 된다.<br>
이때 `Timer` 클래스는  `Worker` 클래스에 속하지 않고 일종의 도구로써 사용된다.
- 의존관계