# PAT Basic 1002

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
# PAT Basic 1002. 写出这个数（20分）

 (C语言实现)
<!--more-->

### 题目

读入一个正整数 *n*，计算其各位数字之和，用汉语拼音写出和的每一位数字。



### 输入格式

每个测试输入包含 1 个测试用例，即给出自然数 *n* 的值。这里保证 *n* 小于 $${10}^{100}$$




### 输出格式

在一行内输出 *n* 的各位数字之和的每一位，拼音数字间有 1 空格，但一行中最后一个拼音数字后没有空格。



### 输入样例

```
1234567890987654321123456789
```

### 输出样例

```
yi san wu
```



### 思路

- 读取每一位上的数值,本是相对比较简单的操作(见注释)
- 但我们发现输入样例过长时,继续将输入值用int做,将无法通过测试.
- 故我们将输入值转换为字符串进行操作

### 实现代码

```c
#include <stdio.h>
/*
    while(num){
        i=num%10;
        s+=i;
        num/=10;
        printf("%d %d\n",i,num);
    }
*/
int main()
{
    int sum = 0;
    char c, *number[] = {"ling", "yi", "er", "san", "si",
                          "wu", "liu", "qi", "ba", "jiu"};
  	//提前存入数字对应对拼音

    while((c = getchar()) != '\n')
        sum += c - '0';
  	//遍历字符串每一个字符,并且利用ascii码将字符转为数字后累加至sum

    if(sum / 100)                           /* hundreds */
        printf("%s ", number[sum / 100]);
    if(sum / 10)                            /* tens */
        printf("%s ", number[sum / 10 % 10]);
    printf("%s", number[sum % 10]);        /* units */

    return 0;
}
```


