# PAT Basic 1024


# PAT Basic 1024. 科学计数法（20分）

 (C语言实现)
<!--more-->

### 题目

科学计数法是科学家用来表示很大或很小的数字的一种方便的方法，其满足正则表达式[+-][1-9]`.`[0-9]+E[+-][0-9]+，即数字的整数部分只有 1 位，小数部分至少有 1 位，该数字及其指数部分的正负号即使对正数也必定明确给出。

现以科学计数法的格式给出实数 *A*，请编写程序按普通数字表示法输出 *A*，并保证所有有效位都被保留。



### 输入格式

每个输入包含 1 个测试用例，即一个以科学计数法表示的实数 *A*。该数字的存储长度不超过 9999 字节，且其指数的绝对值不超过 9999。



### 输出格式

对每个测试用例，在一行中按普通数字表示法输出 *A*，并保证所有有效位都被保留，包括末尾的 0。



### 输入样例1

```
+1.23400E-03
```

### 输出样例1

```
0.00123400
```

### 输入样例2

```
-1.2E+10
```

### 输出样例2

```
-12000000000
```



### 思路

- 将输入的字符分割,将E前面存为字符串,E后面存为整型,方便控制小数点的位置
- 用指针控制E前面的字符串
- 读取的时候用到了一种格式化字符串`%[^...]`，这和`%s`类似，不过会终止于`[^]`里面的字符，而不是空白字符，利用这个可以简单的读取’E’前后的两个数。

### 实现代码

```c
#include <stdio.h>
#include <string.h>


int main()
{
    int index;
    char n[10003] = {'\0'};
    scanf("%[^E]E%d", n, &index);
    if(n[0] == '-') printf("-");/* 负号前面输出-,正号前面不用 */
    if(index >= 0){
        movepoint(n, index);
        printf("%s\n", n+1);
    }
    else{/* index < 0时,控制输出格式 「0.」补0,后拼接 */
        printf("0.");
        for(index++; index; index++){
            printf("0");
        }
        printf("%c%s\n", n[1], n+3);
    }
    return 0;
}

int movepoint(char n[], int index)/* 移动,通过+0并且移动小数点 */
{
    char *p = n+2;
    for(; index; index--){
        if(*(p+1) != '\0') *p = *(p+1);/* 小数点后移 */        
        else               *p = '0';/* 补0 */
        p++;
        *p = '.';
    }
    if(*(p+1) == '\0') *p = '\0';
    return 0;
}

```

