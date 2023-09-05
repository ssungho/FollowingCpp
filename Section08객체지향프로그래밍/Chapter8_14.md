# Chapter8_14 클래스 안에 포함된 자료형 nested types

### 클래스 안에서 중첩 자료형 사용
**특정 클래스에서만 사용될 자료형이라면 클래스 내부에 정의하는 편이 낫다.**
> ***반대로 공용으로 사용되는 자료형이라면 헤더파일에 정의하여 사용하는 것이 좋음***
```cpp
class Fruit
{
public:
    enum FruitType1 // enum
    {
        APPLE,
        BANANA,
        CHERRY,
    };

    enum class FruitType2 // enum class
    {
        APPLE,
        BANANA,
        CHERRY,
    };

    class InnerClass // class
    { };

    struct InnerStruct // struct
    { };
    

private:
    FruitType1 m_type1;
    FruitType2 m_type2;
    InnerClass m_type3;
    InnerStruct m_type4;

public:
    Fruit(FruitType type) : m_type1(type)
    {
    }

    FruitType getType() { return m_type1; }
};
```
> ***다양한 자료형을 클래스 내부에 정의해서 사용할 수 있음***