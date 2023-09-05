# Chapter8_15 실행 시간 측정하기

## 프로그램 실행 시간 측정
**직접 만든 프로그램의 실행시간이 궁금하다면 아래 코드를 이용해서 측정할 수 있다.**<br>
> ***아래 코드는 따배씨++ Chapter8.15에서 사용한 코드입니다.***

### Timer Class
```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <random> // 난수 관련 라이브러리
#include <chrono> // 시간 관련 라이브러리
using namespace std;

class Timer
{
    using clock_t = std::chrono::high_resolution_clock;
    using second_t = std::chrono::duration<double, std::ratio<1>>;

    std::chrono::time_point<clock_t> start_time = clock_t::now();

public:
    void elapsed()
    {
        std::chrono::time_point<clock_t> end_time = clock_t:: now();

        cout << std::chrono::duration_cast<second_t>(end_time - start_time).count() << endl;
    }
};
```
- `start_time`
  - 시작 시간을 기록
- `elapsed()`
    - 프로그램의 시작 시간과 종료 시간의 차이를 초로 변환하여 출력하는 함수 

### 난수 생성 후 정렬 시간 확인
```cpp
int main(void)
{
    random_device rnd_device;
    mt19937 mersenne_engine{ rnd_device() };

    vector<int> vec(10);
    for(int i = 0; i < vec.size(); i++)
        vec[i] = i;
    
    // 난수 생성
    std::shuffle(vec.begin(), vec.end(), mersenne_engine);

    for(auto &element : vec)
        cout << element << " ";
    cout << endl;

    Timer timer;

    // 정렬
    std::sort(begin(vec), end(vec));

    // 시간 측정
    timer.elapsed();

    for(auto &element : vec)
        cout << element << " ";
    cout << endl;

    return 0;
}
```
> ***프로그램의 실행 시간은 여러 요인(하드웨어의 차이, 멀티 쓰레드 환경, 누적 캐시 사용, 등등)에 의해 차이가 남***<br>
> ***-> 시간을 측정할 때는 여러번 측정해야 하며 너무 디버그 모드가 아닌 릴리즈(실제 배포 환경)에서 측정해야함***<br>
> - *릴리즈 모드가 디버그 모드에 비해 훨씬 빠르게 측정된다.*