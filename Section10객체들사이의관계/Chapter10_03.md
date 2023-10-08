# Chapter10_03 집합 관계

**관계 표**
| 관계        | 관계를 표현하는 동사 | 관계의 형태    | 다른 클래스에도 속할 수 있는가? | 멤버의 존재를 클래스가 관리하는가? | 방향성           |
| :---------- | :------------------- | :------------- | :------------------------------ | :--------------------------------- | :--------------- |
| 집합        | Has-a              | 전체/부품      | Yes                           | No                               | 단방향          |

- 강의(Lecture) 클래스는 선생님(Teacher)과, 학생(Student)으로 이루어 진다.
- Jack이라는 학생이 강의1과 강의2를 둘 다 듣는다면, 강의1를 듣는 Jack과 강의2를 듣는 Jack은 같은 인물이다.
  - *`Student` 인스턴스는 각각 다른 `Lecture` 인스턴스에 멤버로 존재할 수 있다.*
- 강의가 사라져도 학생은 사라지지 않는다.
  - *`Lecture` 클래스가 `Student` 인스턴스를 관리하지 않음.*

### Student.h
**Student 클래스를 정의한 헤더파일**

```cpp
#pragma once

#include <iostream>
#include <string>

class Student
{
private:
	std::string m_name;
	int m_intel; // intelligence;

public:
	Student(const std::string& name_in = "No Name", const int& intel_in = 0)
		: m_name(name_in), m_intel(intel_in)
	{}

	void setName(const std::string& name_in)
	{
		m_name = name_in;
	}

	void setIntel(const int& intel_in)
	{
		m_intel = intel_in;
	}

	int getIntel()
	{
		return m_intel;
	}

	friend std::ostream& operator << (std::ostream& out, const Student& student)
	{
		out << student.m_name << " " << student.m_intel;
		return out;
	}
};
```
- `Student`는 `m_name`(이름)과 `m_intel`(지능)을 멤버로 갖는다.

### Teacher.h
**Teacher 클래스를 정의한 헤더파일**

```cpp
#pragma once

#include <string>

class Teacher
{
private:
	std::string m_name;

public:
	Teacher(const std::string& name_in = "No Name") 
		: m_name(name_in)
	{}

	void setName(const std::string& name_in)
	{
		m_name = name_in;
	}

	std::string getName()
	{
		return m_name;
	}

	friend std::ostream& operator <<  (std::ostream& out, const Teacher& teacher)
	{
		out << teacher.m_name;
		return out;
	}
};
```
- `Teacher`는 `m_name`(이름)을 멤버로 갖는다.

### Lecture.h
**Lecture 클래스를 정의한 헤더파일**
- `Lecture` 클래스에서 `Student`와 `Teacher`의 포인터를 멤버로 갖는다.
  - *`Student`와 `Teacher`는 `Lecture`의 부품으로 사용되지만, 여러 `Lecture`에서 공용으로 사용될 수 있음.*

```cpp
#pragma once

#include <vector>
#include "Student.h"
#include "Teacher.h"

class Lecture
{
private:
	std::string m_name;

	Teacher* teacher;
	std::vector<Student*> students;

public:
	Lecture(const std::string& name_in)
		: m_name(name_in)
	{}

	~Lecture()
	{}

	void assignTeacher(Teacher* const teacher_input)
	{
		teacher = teacher_input;
	}

	void registerStudent(Student* const student_input)
	{
		students.push_back(student_input); // 복사해서 들어옴
	}

	void study()
	{
		std::cout << m_name << " Study " << std::endl << std::endl;

		for (auto element : students)
			element->setIntel(element->getIntel() + 1);
	}

	friend std::ostream& operator << (std::ostream& out, const Lecture& lecture)
	{
		out << "Lecture name : " << lecture.m_name << std::endl;

		out << *lecture.teacher << std::endl;
		for (auto element : lecture.students)
			out << *element << std::endl;

		return out;
	}
};
```
- Lecture.h에서 Student.h, Teacher.h 파일들을 Include하여 Student와 Teacher자료형을 사용할 수 있다.
- **`std::string m_name;`**
  - 강의 이름
- **`Teacher* teacher;`, `std::vector<Student*> students;`**
  - `Teacher`의 포인터와 `Student`의 포인터 벡터
    - 포인터를 사용하여 학생과 선생님 객체 자체를 강의에 구성시키지 않고 주소만 구성시켜 집합 관계를 나타냄.

> *`Lecture` 클래스의 멤버함수 `assignTeacher`, `registerStudent`는 포인터를 입력받으며, `const` 키워드를 포인터 앞에 사용하지 않는다.*
> - *포인터의 주소가 아닌 값을 변경시키기 때문.*

### main.cpp
```cpp
#include <iostream>
#include <string>
#include <vector>
#include "Lecture.h"

int main()
{
	using namespace std;

	// 선생님, 학생 생성
	Student *std1 = new Student("Jack Jack", 0);
	Student *std2 = new Student("Dash", 1);
	Student *std3 = new Student("Violet", 2);

	Teacher *teacher1 = new Teacher("Prof. Hong");
	Teacher *teacher2 = new Teacher("Prof. Good");

	Lecture lec1("lecture1");
	lec1.assignTeacher(teacher1);
	lec1.registerStudent(std1);
	lec1.registerStudent(std2);
	lec1.registerStudent(std3);

	Lecture lec2("lecture2");
	lec2.assignTeacher(teacher2);
	lec2.registerStudent(std1);

	{
		cout << lec1 << endl;
		cout << lec2 << endl;

		// event
		lec2.study();

		cout << lec1 << endl;
		cout << lec2 << endl;
	}

	// 소멸
	delete std1;
	delete std2;
	delete std3;
	delete teacher1;
	delete teacher2;

	return 0;
}
```
- `Teacher`과 `Student`의 포인터를 선언하여 동적으로 할당한다.
  - main함수가 아니라 다른 함수라면 `delete`를 반드시 사용해야 한다.
- Lecture의 인스턴스를 생성하고 선생님과 학생을 할당한다.
  - `lec1`과 `lec2`에 동시에 할당된 `std1(Jack Jack)`은 같은 주소를 가리킨다.
    - *교집합인 셈*
- `lec2`에서 `study()`를 실행하여 `std1`의 `m_intel`이 `1` 증가하고, `lec1`과 `lec2`에서 `std1`을 확인하면 동일한 값을 확인할 수 있다.
  - *같은 포인터를 가리키고 있기 때문!*