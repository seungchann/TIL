# C programming basics  
> C에서 자주 헷갈리는 개념이나 함수를 정리해둔 페이지입니다.  

## Concepts
### uint8_t, uint16_t  
> https://dojang.io/mod/page/view.php?id=35  
* `uint8_t`  
  * 범위 : 0 ~ 255  
  * 크기 : 1 byte (8 bits)  
* `uint16_t`
  * 범위 : 0 ~ 65536  
  * 크기 : 2 byte (16 bits)  

***

## Functions  
### `atoi` : 문자 스트링을 정수로 변환  
> https://www.ibm.com/docs/ko/i/7.3?topic=functions-atoi-convert-character-string-integer  
* atoi() 함수는 소수점이나 지수를 인식하지 못합니다.  
* 변환할 수 없는 경우 리턴값은 `0`입니다.  
* 형식  
```c
#include <stdlib.h>
int atoi(const char *string);
```
* 예시  
```c
#include <stdlib.h>
#include <stdio.h>
 
int main(void)
{
    int i;
    char *s;
 
    s = " -9885";
    i = atoi(s);     /* i = -9885 */
 
    printf("i = %d\n",i);
}
 
/*******************  Output should be similar to:  ***************
 
i = -9885
*/
```

### `strtol` : 문자 스트링을 long integer 값으로 변환  
> https://www.ibm.com/docs/ko/i/7.3?topic=lf-strtol-strtoll-convert-character-string-long-long-long-integer  
* 형식  
```c
#include <stdlib.h>
long int strtol(const char *nptr, char **endptr, int base);
```
* 예시  
```c
#include <stdlib.h>
#include <stdio.h>
 
int main(void)
{
   char *string, *stopstring;
   long l;
   int bs;
 
   string = "10110134932";
   printf("string = %s\n", string);
   for (bs = 2; bs <= 8; bs *= 2)
   {
      l = strtol(string, &stopstring, bs);
      printf("   strtol = %ld (base %d)\n", l, bs);
      printf(" Stopped scan at %s\n\n", stopstring);
      }
}
 
/*****************  Output should be similar to:  *****************
 
string = 10110134932
   strtol = 45 (base 2)
   Stopped scan at 34932
 
   strtol = 4423 (base 4)
   Stopped scan at 4932
 ```
 
 
