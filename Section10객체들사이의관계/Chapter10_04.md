# Chapter 10_04 제휴 관계(Association)

**관계 표**
| 관계        | 관계를 표현하는 동사 | 관계의 형태    | 다른 클래스에도 속할 수 있는가? | 멤버의 존재를 클래스가 관리하는가? | 방향성           |
| :---------- | :------------------- | :------------- | :------------------------------ | :--------------------------------- | :--------------- |
| 연계, 제휴  | Uses-a               | 용도 외엔 무관  | Yes                             | No                                 | 단항뱡 or 양방향 |

- 어느 한쪽이 확실히 주가 되지 않는 경우 연계 혹은 제휴 관계라고 할 수 있다.
  - ex) 의사 - 환자
- 환자는 병이 낫기 전까지 의사를 만나야 하지만 병이 낫는다면 더이상 만날 필요가 없음.
  - 용도 외엔 무관하다.
- 환자는 의사뿐 만 아니라 다른 클리닉에 담당자들을 가질 수 있음.
- 의사와 환자 클래스에서 서로를 관리할 수 없다.
  - 특정 시기에 생성하고 삭제할 수 없음.

### Patient Class
```cpp
class Patient
{
private:
	string m_name;
	vector<Doctor*> m_doctors; // 환자가 만나는 의사들

public:
	Patient(string name_in)
		: m_name(name_in)
	{}

	void addDoctor(Doctor* new_doctor)
	{
		m_doctors.push_back(new_doctor);
	}

	void meetDoctors()
    {
        for (auto& element : m_doctors)
        {
            cout << "Meet doctor : " << element->m_name << endl;;
        }
    }

	friend class Doctor;
};
```
- `string m_name;`
  - 환자의 이름
- `vector<Doctor*> m_doctors;`
  - 환자를 담당하는 의사들(doctors)의 포인터 벡터

- `void addDoctor(Doctor* new_doctor)`
  - 환자에게 담당 의사를 설정하는 함수
- `void meetDoctors()`
  - 담당 의사들의 이름을 출력하는 함수

- `friend class Doctor;`
  - `Patient` 클래스에서 `Doctor` 클래스의 `private` 멤버에 접근하기 위해 `Friend Class`로 등록 

### Doctor Class

```cpp
class Doctor
{
private:
	string m_name;
	vector<Patient*> m_patients; // 의사가 담당하는 환자들

public:
	Doctor(string name_in)
		: m_name(name_in)
	{}

	void addPatient(Patient* new_patient)
	{
		m_patients.push_back(new_patient);
	}

	void meetPatients()
	{
		for (auto& element :m_patients)
		{
			cout << "Meet patient : " << element->m_name << endl;;
		}
	}

	friend class Patient;
};
```
- `string m_name;`
  - 의사의 이름
- `vector<Patient*> m_patients;`
  - 의사가 담당하는 환자들(patients)의 포인터 벡터

- `void addPatient(Doctor* new_doctor)`
  - 환자에게 담당 의사를 설정하는 함수
- `void meetDoctors()`
  - 담당 환자들의 이름을 출력하는 함수

- `friend class Doctor;`
  - `Doctor` 클래스에서 `Patient` 클래스의 `private` 멤버에 접근하기 위해 `Friend Class`로 등록

### main.cpp
- 환자와 의사를 동적으로 할당하여 서로 담당할 환자와 의사를 설정
- 환자의 담당 의사 목록과 의사의 담당 환자 목록을 출력함 

```cpp
#include <iostream>
#include <vector>
#include <string>
using namespace std;

class Doctor; // 전방선언

int main()
{
    // 환자 생성
	Patient* p1 = new Patient("Jack Jack");
	Patient* p2 = new Patient("Dash");
	Patient* p3 = new Patient("Violet");

    // 의사 생성
	Doctor* d1 = new Doctor("Doctor K");
	Doctor* d2 = new Doctor("Doctor L");

    // 의사1, 환자1 연결
	p1->addDoctor(d1);
	d1->addPatient(p1);

    // 의사2, 환자2 연결
	p2->addDoctor(d2);
	d2->addPatient(p2);

    // 의사2, 환자1 연결
	p2->addDoctor(d1);
	d1->addPatient(p2);

	// patients meet doctors
	p1->meetDoctors();

	// doctors meet patients
	d1->meetPatients();

	delete p1;
	delete p2;
	delete p3;
	delete d1;
	delete d2;

	return 0;
}
```
- `Patient`입장에서 `Doctor`의 존재에 대해 알 수 없기 때문에 `Doctor`를 전방선언한다.

> 하지만 cpp파일을 실행하면 오류가 발생한다.
- `Doctor` 클래스는 전방선언 되어있어 존재는 알 수 있지만, `meetDoctors()`함수를 실행하는 시점에서 `Doctor`의 `m_name`에 대해 알 수 없기 때문이다.
 
#### 정의 부분을 빼서 작성하여 해결한다. 
```cpp
void Patient::meetDoctors()
{
	for (auto& element : m_doctors)
	{
		cout << "Meet doctor : " << element->m_name << endl;;
	}
}
```

```
// 실행 결과
Meet doctor : Doctor K
Meet patient : Jack Jack
Meet patient : Dash
```