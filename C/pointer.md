# C programming 포인터  
## Contents  
* [포인터 사용하기](#0-포인터-사용하기)  
* [포인터 변수 선언하기](#1-포인터-변수-선언하기)  
* [역참조 연산자 사용하기](#2-역참조-연산자-사용하기)  
* [다양한 자료형의 포인터 선언하기](#3-다양한-자료형의-포인터-선언하기)  
* [void 포인터 선언하기](#4-void-포인터-선언하기)  
* [잘못된 포인터 사용](#5-잘못된-포인터-사용)

## 0. 포인터 사용하기  
### 변수의 메모리 주소  
* 우리는 값을 저장할 때 변수를 사용합니다.  
* 변수는 어디에 생길까요?  
```c
int num1 = 10;
```
* 다음과 같이 변수는 **컴퓨터의 메모리**에 생성됩니다.  
* 메모리에 일정한 공간을 확보해두고 원하는 값을 저장하거나 가져오는 방식입니다.  

![메모리와 변수](https://dojang.io/pluginfile.php/337/mod_page/content/21/unit34-1.png)  

* 보통 변수는 `num1`과 같이 이름으로 사용합니다.  
* 하지만 메모리의 특정 장소에 위치하고 있으므로, 메모리 주소로도 표현할 수 있습니다.  
* 변수의 메모리 주소는 다음과 같이 구할 수 있습니다.  
```c
#include <stdio.h>

int main() {

  int num1 = 10;

  printf("%p\n", &num1); // 008AF7FC: num1의 메모리 주소를 출력
                         // 컴퓨터마다, 실행할 때마다 달라짐
  
  return 0
}

// 실행 결과
008AF7FC (메모리 주소. 컴퓨터마다, 실행할 때마다 달라짐)
```

* 변수의 메모리 주소를 구할 때는 변수 앞에 & (주소 연산자)를 붙이면 됩니다.  
* 메모리 주소는 16진수 형태이며, `printf`에서 서식 지정자 `%p`를 사용하여 출력합니다.  
* 메모리 주소는 고정된 것이 아니라 컴퓨터마다, 실행할 때마다 달라집니다.  

![변수의 메모리 주소](https://dojang.io/pluginfile.php/337/mod_page/content/21/unit34-2.png)  

## 1. 포인터 변수 선언하기  
* 메모리 주소는 포인터(pointer) 변수에 저장합니다.  
* 다음과 같이 포인터 변수는 `*`를 사용하여 선언합니다.  
  * 자료형 `* 포인터이름`;  
  * 포인터 = &변수;

```c
#include <stdio.h>

int main() {
  int *numPtr; // 포인터 변수 선언
  int num1 = 10; // int형 변수를 선언하고 10 저장
  
  numPtr = &num1; // num1의 메모리 주소를 포인터 변수에 저장
  
  printf("%p\n", numPtr); // 0055FC24: 포인터 변수 numPtr의 값 출력
                          // 컴퓨터마다, 실행할 때마다 달라짐  
  printf("%p\n", &num1); // 0055FC24: 변수 num1의 메모리 주소 출력
                         // 컴퓨터마다, 실행할 때마다 달라짐
                         
  return 0;
}

// 실행 결과
0055FC24 (메모리 주소. 컴퓨터마다, 실행할 때마다 달라짐)
0055FC24 (메모리 주소. 컴퓨터마다, 실행할 때마다 달라짐)
```
* 포인터 변수를 선언할 때는 자료형 뒤에 `*` (Asterisk, 애스터리스크)를 붙입니다.  
* `*`의 위치에 따른 차이는 없으며 모두 같은 뜻입니다.  

```c
int* numPtr; // 자료형 쪽에 *을 붙임
int * numPtr; // 자료형과 변수 가운데 *를 넣음
int *numPtr; // 변수 쪽에 *을 붙임
```

* 포인터 변수를 선언했으면 다음과 같이 &로 변수의 주소를 구해서 포인터 변수에 저장합니다.  
```c
numPtr = &num1; // num1의 메모리 주소를 포인터 변수에 저장  
```

* 이제 printf로 포인터 numPtr의 값을 출력해보면 변수 num1의 메모리 주소가 나옵니다.  
* 즉, 포인터와 메모리 주소는 같은 의미입니다.  
```c
printf("%p\n", numPtr); // 0055FC24: 포인터 변수 numPtr의 값 출력
                        // 컴퓨터마다, 실행할 때마다 달라짐
printf("%p\n", &num1);  // 0055FC24: 변수 num1의 메모리 주소 출력
                        // 컴퓨터마다, 실행할 때마다 달라짐
```
* 포인터 변수를 선언할 때는 자료형을 알려주고 `*`를 붙이는 방식을 사용합니다.  
* 여기서 `int *`는 영어로 pointer to int라고 읽는데, int형 공간을 가리키는 포인터라는 뜻입니다. (int 포인터라고도 부릅니다.)  

![int 포인터](https://dojang.io/pluginfile.php/338/mod_page/content/23/unit34-3.png)  
* 다음과 같이 포인터는 메모리의 특정 위치를 가리킬 때 사용합니다.  

![포인터와 메모리](https://dojang.io/pluginfile.php/338/mod_page/content/23/unit34-4.png)  

```c
int *numPtr; // 포인터 변수 선언
int num1 = 10;

numPtr = &num1; // num1의 메모리 주소를 포인터 변수에 저장 
```
* numPtr은 10이 저장된 메모리 공간을 가리킵니다.  
* 즉, 변수 num1이 있는 공간을 가리키게 되는 것입니다.  
![포인터에 변수의 메모리 주소 할당](https://dojang.io/pluginfile.php/338/mod_page/content/23/unit34-5.png)  

## 2. 역참조 연산자 사용하기  
* 포인터 변수에는 메모리 주소가 저장되어 있습니다.  
* 메모리 주소가 있는 곳으로 이동해서 값을 가져오고 싶다면 역참조(dereference) 연산자 `*`를 사용합니다.  
```c
#include <stdio.h>

int main() {
  int *numPtr; // 포인터 변수 선언
  int num1 = 10; // 정수형 변수를 선언하고 10 저장
  
  numPtr = &num1; // num1의 메모리 주소를 포인터 변수에 저장  
  
  printf("%d\n", *numPtr); // 10: 역참조 연산자로 num1의 메모리 주소에 접근하여 값을 가져옴  
  
  return 0;
}

// 실행 결과
10
```

* 역참조 연산자 `*`는 포인터 앞에 붙입니다.  
* 다음과 같이 numPtr앞에 `*`를 붙이면, numPtr에 저장된 메모리 주소로 가서 값을 가져옵니다.  
* 여기서는 numPtr이 num1의 메모리 주소를 저장하고 있으므로, num1의 값인 10이 출력됩니다.  
* 즉, 포인터는 변수의 주소만 가르키며 역참조는 주소의 접근하여 값을 가져옵니다.  

![](https://dojang.io/pluginfile.php/339/mod_page/content/30/unit34-6.png) 

### 포인터 선언과 역참조?  
* 포인터를 선언할 때도 `*`를 사용하고 역참조를 할 때도 `*`를 사용합니다.  
* 같은 `*` 기호를 사용해서 헷갈리기 쉽지만 선언과 사용을 구분해서 생각하면 됩니다.  
```c
int *numPtr; // 포인터. 포인터를 선언할 때 *
printf("%d\n", *numPtr); // 역참조. 포인터에 사용할 때 *
```

* 이번에는 포인터 변수에 역참조 연산자를 사용한 뒤 값을 저장(할당)해보겠습니다.  
```c
#include <stdio.h>

int main() {
  int *numPtr; // 포인터 변수 선언
  int num1 = 10; // 정수형 변수를 선언하고 10 저장
  
  numPtr = &num1; // num1의 메모리 주소를 포인터 변수에 저장
  
  *numPtr = 20 // 역참조 연산자로 메모리 주소에 접근하여 20을 저장
  
  printf("%d\n", *numPtr); // 20: 역참조 연산자로 메모리 주소에 접근하여 값을 가져옴
  printf("%d\n", num1); // 20: 실제 num1의 값도 바뀜
  
  return 0;
}

// 실행 결과
20
20
```

* 역참조 연산자는 값을 가져올 수도 있고 값을 저장할 수도 있습니다.  
* 여기서는 `*numPtr = 20;`과 같이 numPtr에 저장된 메모리 주소에 접근하여 20을 저장했습니다.  
* 또 한가지 중요한 점은 `*numPtr = 20;`으로 20을 저장한 뒤 num1 값을 출력해보면 20이 나온다는 것입니다.  
* 왜냐하면 numPtr에는 num1의 메모리 주소가 저장되어 있으므로 역참조 연산자로 값을 저장하면 결국 num1에 저장하게 됩니다.  

![](https://dojang.io/pluginfile.php/339/mod_page/content/30/unit34-7.png)  

* 역참조 연산자는 자료형을 바꾸는 효과를 냅니다.  
* 즉, `int *numPtr;`에서 `*numPtr`처럼 역참조하면 pointer to int에서 그냥 int로 만듭니다. (int 포인터 -> int)  
* 만약 포인터 numPtr에 변수 num1을 할당한다면 역참조 연산자로 자료형을 맞춰주면 됩니다.  
```c
int *numPtr;
int num1 = 10;

numPtr = num1; // 컴파일 경고, numPtr은 int 포인터형이고, num1은 int형이라 자료형이 일치하지 않음

*numPtr = num1; // *numPtr은 int형이고 num1도 int형이라 자료형이 일치함
```

* 물론 주소 연산자 `&`도 자료형을 맞춰주는 역할을 합니다.
* `&`을 붙이지 않으면 앞과 같은 경고가 발생합니다.  
```c
int *numPtr;
int num1;

numPtr = &num1 // numPtr은 int 포인터형이고, &num1은 int형 변수의 주소이므로 자료형이 일치함  
               // numPtr은 pointer to int, &num1은 address to int이므로 자료형이 일치함
```

* 즉, pointer to int와 address of int는 자료형이 같습니다.  
![포인터 자료형](https://dojang.io/pluginfile.php/339/mod_page/content/30/unit34-8.png)  

### 변수, 주소 연산자, 포인터의 차이  

![변수, 주소 연산자, 포인터의 차이](https://dojang.io/pluginfile.php/339/mod_page/content/30/unit34-9.png)  

#### 변수
* 메모리 주소를 몰라도 값을 가져오거나 저장할 수 있습니다.  
* 그냥 변수에 값을 할당하거나 그대로 출력하면 됩니다.  

#### 주소 연산자 `&`  
* 변수의 메모리 주소를 구합니다.  
* 이 그림에서 10을 감싸고 있는 상자는 메모리 공간을 뜻하는데, 주소 연산자는 메모리 공간이 어디에 있는지 위치만 알아낼 수 있습니다.  

#### 역참조 연산자 `*`  
* 그림에서 보면 상자 안까지 들어가서 값을 가져오거나 저장합니다.  
* 즉, 메모리 주소를 알고 있으므로 메모리 주소를 거쳐서 그 안에 있는 값을 가져오거나 저장합니다.  

#### 포인터  
* 변수의 메모리 주소만 가리킵니다.  
* 따라서 포인터는 메모리 공간이 어디에 있는지 위치만 알고 있습니다.  

## 3. 다양한 자료형의 포인터 선언하기  
* 이번에는 다양한 자료형의 포인터를 선언해보겠습니다.  
```c
#include <stdio.h>

int main() {
  long long *numPtr1; // long long형 포인터 선언
  float *numPtr2; // float형 포인터 선언
  char *cPtr1; // char형 포인터 선언
  
  long long num1 = 10;
  float num2 = 3.5f;
  char c1 = 'a';
  
  numPtr1 = &num1; // num1의 메모리 주소 저장
  numPtr2 = &num2; // num2의 메모리 주소 저장
  cPtr1 = &c1; // c1의 메모리 주소 저장
  
  return 0;
}

// 실행 결과
10
3.500000
a
```

* C 언어에서 사용할 수 있는 모든 자료형은 포인터로 만들 수 있습니다.  
* 포인터에 저장되는 메모리 주소값은 정수형으로 동일하지만 선언하는 자료형에 따라 메모리에 접근하는 방법이 달라집니다.  
* 즉, 다음과 같이 포인터를 역참조하면 선언한 자료형의 크기에 맞춰서 값을 가져오거나 저장하게 됩니다.  
![포인터의 자료형과 역참조 크기](https://dojang.io/pluginfile.php/340/mod_page/content/22/unit34-23.png)  

* 즉, long long 포인터는 8바이트 크기만큼 값을 가져오거나 저장하고, char 포인터는 1바이트 크기만큼 값을 가져오거나 저장합니다.  

### 상수와 포인터  
* 포인터에도 const 키워드를 붙일 수 있는데 const의 위치에 따라 특성이 달라집니다.  
* 먼저 상수를 가리키는 포인터(pointer to constant) 입니다.  
```c
const int num1 = 10; // int형 상수
const int *numPtr; // int형 상수를 가리키는 포인터. int sont *numPtr도 같음

numPtr = &num1;
*numPtr = 20; // 컴파일 에러. num1이 상수이므로 역참조로 값을 변경할 수 없음
```
* 먼저 num1이 const int이므로 이 변수의 주소를 넣을 수 있는 포인터는 `const int *`로 선언해야 합니다.  
* 그리고 num1의 주소를 numPtr에 넣을 때 역참조로 값을 변경하려고 해도 num1은 상수이므로 컴파일 에러가 발생합니다.  
* 즉, pointer to constant는 메모리 주소에 저장된 값을 변경할 수 없다는 뜻입니다.  
<br></br>
* 이번에는 포인터 자체가 상수인 상황입니다. (constant pointer)
* 이때는 `*` 뒤에 const를 붙입니다.  
```c
int num1 = 10; // int형 변수
int num2 = 20; // int형 변수
int * const numPtr = &num1; // int형 포인터 상수

numPtr = &num2; // 컴파일 에러. 포인터(메모리 주소)를 변경할 수 없음
```

* numPtr에 num1의 주소가 들어가 있는 상태에서 다시 num2의 주소를 넣으려고 하면 컴파일 에러가 발생합니다.  
* numPtr은 포인터 자체가 상수이므로 다른 포인터(메모리 주소)를 할당할 수 없습니다.  
* 즉, constant pointer는 메모리 주소를 변경할 수 없다는 뜻입니다.  
<br></br>
* 마지막으로 포인터가 상수이면서 상수를 가리키는 상황입니다. (constant pointer to constant)  
* 이때는 포인터를 선언하는 자료형에도 const를 붙이고 `*` 뒤에도 const를 붙입니다.  
```c
const int num1 = 10; // int형 상수
const int num2 = 20; // int형 상수
const int * const numPtr = &num1; // int형 상수를 가리키는 포인터 상수
                                  // int const * const numPtr도 같음

*numPtr = 30; // 컴파일 에러. num1의 상수이므로 역참조로 값을 변경할 수 없음
numPtr = &num2; // 컴파일 에러. 포인터(메모리 주소)를 변경할 수 없음  
```

* 여기서는 numPtr을 역참조한 뒤 값을 변경하려고 해도 num1은 상수이므로 컴파일 에러가 발생합니다.  
* 그리고 numPtr 자체도 상수이므로 num2의 주소를 넣으려고 하면 컴파일 에러가 발생합니다.  
* 즉, constant pointer to constant는 메모리 주소도 변경할 수 없고 메모리 주소의 저장된 값도 변경할 수 없다는 뜻입니다.  

## 4. void 포인터 선언하기  
* `long long *numPtr1;` 이나 `float *numPtr2`는 자료형이 정해진 포인터입니다.  
* 하지만 C 언어에서는 자료형이 정해지지 않은 포인터도 있습니다.  
* void 포인터라는 포인터인데 다음과 같이 void 키워드와 `*`로 선언합니다.  
  * `void *포인터이름;`
```c
#include <stdio.h>

int main() {
  int num1 = 10;
  char c1 = 'a';
  int *numPtr1 = &num1;
  char *cPtr1 = &c1;
  
  void *ptr; // void 포인터 선언  
  
  // 포인터 자료형이 달라도 컴파일 경고가 발생하지 않음  
  ptr = numPtr1; // void 포인터에 int 포인터 저장
  ptr = cPtr1; // void 포인터에 char 포인터 저장
  
  return 0;
}
```

* 기본적으로 C 언어는 자료형이 다른 포인터끼리 메모리 주소를 저장하면 컴파일 경고 (warning)가 발생합니다.  
* 하지만 void 포인터는 자료형이 정해지지 않은 특성 때문에 어떤 자료형으로 된 포인터든 모두 저장할 수 있습니다.  
* 반대로 다양한 자료형으로 된 포인터에도 void 포인터를 저장할 수도 있습니다.  
* 이런 특성 때문에 void 포인터는 범용 포인터라고 합니다.  
* 즉, 직접 자료형을 변환하지 않아도 암시적으로 자료형이 변환되는 방식입니다.  
```c
// 포인터 자료형이 달라도 컴파일 경고가 발생하지 않음  
ptr = numPtr1; // void 포인터에 int 포인터 저장
prt = cPtr1; // void 포인터에 char 포인터 저장

// 포인터 자료형이 달라도 컴파일 경고가 발생하지 않음  
numPtr1 = ptr; // int 포인터에 void 포인터 저장
cPtr1 = ptr;  // char 포인터에 void 포인터 저장
```

![void 포인터](https://dojang.io/pluginfile.php/341/mod_page/content/22/unit34-24.png)  

* 단, void 포인터는 자료형이 정해지지 않았으므로 값을 가져오거나 저장할 크기도 정해지지 않았습니다.  
* 따라서 void 포인터는 **역참조를 할 수 없습니다.**  
```c
ptr = numPtr1; // void 포인터에 int 포인터 저장  
printf("%d", *ptr); // void 포인터는 역참조할 수 없음. 컴파일 에러  

ptr = cPtr1; // void 포인터에 char 포인터 저장
printf("%c", *ptr); // void 포인터는 역참조할 수 없음. 컴파일 에러

// 컴파일 결과
error C2100: 간접 참조가 잘못되었습니다.
error C2100: 간접 참조가 잘못되었습니다.
```

* void 키워드로는 변수를 선언할 수도 없습니다.  
```c
void v1; // void로는 변수를 선언할 수 없음. 컴파일 에러

// 컴파일 결과
error C2182: 'v1': 'void' 형식을 잘못 사용했습니다.  
```

* 그런데 역참조도 할 수 없는 void 포인터는 왜 사용할까요?
  * 함수에서 다양한 자료형을 받아들일 때  
  * 함수의 반환 포인터를 다양한 자료형으로 된 포인터에 저장할 때  
  * 자료형을 숨기고 싶을 때  

## 5. 이중 포인터 선언하기  
* 이번에는 포인터의 메모리 주소를 저장하는 **포인터의 포인터**를 선언해보겠습니다.  
* 포인터를 선언할 때 `*`를 두 번 사용하면 포인터의 포인터(이중 포인터)를 선언합니다.  
  * `자료형 **포인터이름;`
```c
#include <stdio.h>

int main() {
  int *numPtr1; // 단일 포인터 선언
  int **numPtr2; // 이중 포인터 선언
  int num1 = 10;
  
  numPtr1 = &num1; // num1의 메모리 주소 저장
  
  numPtr2 = &numPtr1; // numPtr1의 메모리 주소 저장
  
  printf("%d\n", **numPtr2); // 20: 포인터를 두 번 역참조하여 num1의 메모리 주소에 접근
  
  return 0;
}

// 실행 결과
10
```

* 포인터도 실제로는 변수이기 때문에 메모리 주소를 구할 수 있습니다.  
* 하지만 포인터의 메모리 주소는 일반 포인터에 저장할 수 없고, `int **numPtr2`처럼 이중 포인터에 저장해야 합니다.  
* `int numPtr2;`를 영어로 읽으면 pointer to pointer to int가 됩니다. (numPtr2 -> numPtr1 -> num1).
* 여기서 이중 포인터 numPtr2를 끝까지 따라가서 실제 값을 가져오려면 `**numPtr2``처럼 변수 앞에 역참조 연산자를 두 번 사용하면 됩니다.  
* 즉, 역참조를 두 번 하므로 `numPtr2 <- numPtr1 <- num1`과 같은 모양이 되고 num1의 값을 가져올 수 있습니다.  
![이중 포인터의 역참조](https://dojang.io/pluginfile.php/342/mod_page/content/18/unit34-25.png)  
* 포인터를 선언할 때 `*`의 개수에 따라서 삼중 포인터, 사중 포인터 그 이상도 만들 수 있습니다.  
* 마찬가지로 역참조를 할 때도 `*`를 세 번, 네 번 또는 그 이상 사용할 수 있습니다.  

## 5. 잘못된 포인터 사용  
* 포인터는 메모리 주소를 저장하는 용도이므로 다음과 같이 값을 직접 저장하면 안 됩니다.  
```c
#include <stdio.h>

int main() {
  int *numPtr = 0x100; // 포인터에 0x100을 직접 저장
  
  printf("%d\n", *numPtr); // 0x100은 잘못된 메모리 주소이므로 실행 에러
  
  return 0;
}
```
* 소스 코드를 컴파일해서 실행해보면 제대로 실행이 되지 않습니다.  
* 왜냐면 `int *numPtr = 0x100;`과 같이 포인터에 직접 0x100을 저장했을 때 메모리에서 0x100은 잘못된 주소값이기 때문입니다.  
![잘못된 포인터 사용](https://dojang.io/pluginfile.php/343/mod_page/content/23/unit34-26.png)  

* 이 상태에서 numPtr을 역참조하여 메모리 주소에 접근해 봐야 에러만 발생합니다.
  * 운영체제는 프로그램이 잘못된 메모리 주소에 접근했을 때 에러를 발생시킵니다.  
* 만약 실제로 존재하는 메모리 주소라면 포인터에 직접 저장할 수 있습니다.  
```c
int *numPtr = 0x00CCFC2C; // 실제로 존재하는 메모리 주소라면 저장할 수 있음
```
* 보통 임베디드 시스템이나 마이크로 프로세서에서 제공하는 메모리 주소를 사용할 때 포인터에 직접 저장하기도 합니다.  

## 6. References
* https://dojang.io/mod/page/view.php?id=274
* https://dojang.io/mod/page/view.php?id=275  
* https://dojang.io/mod/page/view.php?id=276  
* https://dojang.io/mod/page/view.php?id=277  
* https://dojang.io/mod/page/view.php?id=278  
* https://dojang.io/mod/page/view.php?id=279  
* https://dojang.io/mod/page/view.php?id=280  
