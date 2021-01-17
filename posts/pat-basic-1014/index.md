# PAT Basic 1014


# PAT Basic 1014. 福尔摩斯的约会（20分）

 (C语言实现)
<!--more-->

### 题目

大侦探福尔摩斯接到一张奇怪的字条：`我们约会吧！ 3485djDkxh4hhGE 2984akDfkkkkggEdsb s&hgsfdk d&Hyscvnm`。大侦探很快就明白了，字条上奇怪的乱码实际上就是约会的时间`星期四 14:04`，因为前面两字符串中第 1 对相同的大写英文字母（大小写有区分）是第 4 个字母 `D`，代表星期四；第 2 对相同的字符是 `E` ，那是第 5 个英文字母，代表一天里的第 14 个钟头（于是一天的 0 点到 23 点由数字 0 到 9、以及大写字母 `A` 到 `N` 表示）；后面两字符串第 1 对相同的英文字母 `s` 出现在第 4 个位置（从 0 开始计数）上，代表第 4 分钟。现给定两对字符串，请帮助福尔摩斯解码得到约会的时间。	



### 输入格式

输入在 4 行中分别给出 4 个非空、不包含空格、且长度不超过 60 的字符串。



### 输出格式

在一行中输出约会的时间，格式为 `DAY HH:MM`，其中 `DAY` 是某星期的 3 字符缩写，即 `MON` 表示星期一，`TUE` 表示星期二，`WED` 表示星期三，`THU` 表示星期四，`FRI` 表示星期五，`SAT` 表示星期六，`SUN` 表示星期日。题目输入保证每个测试存在唯一解。



### 输入样例

```
3485djDkxh4hhGE 
2984akDfkkkkggEdsb 
s&hgsfdk 
d&Hyscvnm
```

### 输出样例

```
THU 14:04
```



### 思路

- 让我们来直观点来理解题目 str1[index] = str2[index] 找到 index 的位置

  **两对字符串**

  - 第一对字符串包含两个信息：日期/时间 xx : 00 
  - 第二对字符串包含一个信息：时间 00 : xx

  - 都是通过找相同字符的**位置**,位置对应的解读信息的方式不同

- Ascii码的应用,可以百度去查看下对应规则

- 需要注意一些输出格式

### 实现代码

```c
#include <stdio.h>
#include <ctype.h>

int main()
{
    char str1[61], str2[61], str3[61], str4[61];
    scanf("%s %s %s %s", str1, str2, str3, str4);

    /* Find day */
    int DAY;
    char *weekdays[] = {"MON", "TUE", "WED", "THU", "FRI", "SAT", "SUN"};
    for(DAY = 0; str1[DAY] && str2[DAY]; DAY++)
        if(str1[DAY] == str2[DAY] && str1[DAY] >= 'A' && str1[DAY] <= 'G')
        {
            printf("%s", weekdays[str1[DAY] - 'A']);/* 利用Ascii码 */
            break;
        }

    /* Find hour [A-N|0-9] */
    int HH;
    for(HH = DAY + 1; str1[HH] && str2[HH]; HH++)
        if(str1[HH] == str2[HH])
        {
            if(str1[HH] >= 'A' && str1[HH] <= 'N')
            {
                printf(" %02d", str1[HH] - 'A' + 10);
                break;
            }
            if(isdigit(str1[HH]))
            {
                printf(" %02d", str1[HH] - '0');
                break;
            }
        }

    /* Find minute */
    int MM;
    for(MM = 0; str3[MM] && str4[MM]; MM++)
        if(str3[MM] == str4[MM] && isalpha(str3[MM]))
        {
            printf(":%02d", MM);
            break;
        }

    return 0;
}
```


