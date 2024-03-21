
## 데이터 저장 방식
먼저 자료형이 무엇인지 이해하기 위해, 컴퓨터가 데이터를 어떻게 저장하는지 알아야한다.<br>
컴퓨터는 메모리에 데이터를 저장하기 위해 0과 1로 이루어진 bit로 저장하게 된다. <br> 연산 과정에서 bit가 길어지면 컴퓨터는 어디까지가 연산에 유효한 bit인지 효율적으로 판단하기 위해 8bit로 이루어진 바이트(byte)라는 최소 단위를 사용한다. <br>
> 1byte = 8bit


## 자료형
위처럼 컴퓨터는 데이터를 저장하기 위해 특정 메모리 공간을 할당받아야 하는데, 특정 공간을 할당받기 위해서는 얼마만큼의 공간을 할당받을 것인지 명시를 해줘야한다. 이를 표현하는 것이 자료형이다.
> 자료형은 **기본 자료형**과 **사용자 정의 자료형**으로 나뉜다.  

<br>
### 기본 자료형 (Premitive Bulit-in Types)
int, double, bool과 같이 컴파일러 차원에서 사용자에게 제공하는 자료형을 말한다. 각각의 자료형은 표현법과 크기가 정해져 있기에, 사용자가 상황에 맞게 최적의 자료형을 선택하게 된다. 

```
int a = 5;  //4byte
double b = 3.14;  //8byte
bool c = true;  //1byte
```
int a = 5;를 해석하자면, int는 4byte 크기의 자료형이므로 4byte 크기의 메모리 공간을 할당받고 할당 받은 메모리 공간의 이름을 a로 표현한다. 코드가 실행되면 a라는 메모리 공간에는 5가 저장된다.
<br>

컴퓨터 내부적으로는 어떻게 표현할까?
> 할당받은 4byte( = 32bit)에 5를 2진수로 표기하여 00000000 00000000 00000000 00000101 형태로 저장하게 된다.

<br><br>

* 어떤 자료형이 존재하고, 자료형의 크기를 확인하려면 다음 공식 사이트를 참고하면 된다.<br> <https://learn.microsoft.com/ko-kr/cpp/cpp/data-type-ranges?view=msvc-170>


<br>


### 사용자 정의 자료형
구조체나 클래스, 열거형 같이 사용자가 정의한 자료형을 **사용자 정의 자료형** 이라고 한다.<br>
사용자가 정의한 구조체나 클래스에서 사용한 멤버 변수의 크기를 종합하여 메모리 크기를 계산하고 할당받게 된다.

```
struct MyStruct
{
    int a = 2;
    int b = 3;
    bool c = false;
};

class MyClass
{
public:
    MyClass()
    {

    }
public:
    int a = 3;
    bool b = false;
};

int main()
{
    MyStruct ms;
    MyClass mc;
}
```

이번엔 디버그 모드(f5)를 눌러, 메모리 창을 열고 직접 확인하면 다음과 같다. 
![MyStruct 메모리](/assets\images\posts_img\cpp\MyStructure_memory.png)
위 주소는 주소 연산자를 이용하여 &ms(MyStruct의 주소)를 검색해서 나온 결과이다. MyStruct에서 멤버 변수로 a, b, c를 선언하여 초기화 했기 때문에, 메모리 상에 2(dec),8(dec),0(false)이 잡혀있는 것을 볼 수 있다.  <br><br>
MyClass도 마찬가지이다.
![MyClass 메모리](/assets\images\posts_img\cpp\MyClass_memory.png)

>참고 : 멤버 변수로 사용한 int는 4byte인데 디버그 메모리창에서 1블럭에 8자리만 표현되는 이유는 메모리 창이 16진수로 표기했기 때문! (16진수로 표기했을 때 2자리가 1byte를 나타냄)