# PAT Basic 1003

# PAT Basic 1003. 我要通过（20分）

 (C语言实现)
<!--more-->

### 题目

“**答案正确**”是自动判题系统给出的最令人欢喜的回复。本题属于 PAT 的“**答案正确**”大派送 —— 只要读入的字符串满足下列条件，系统就输出“**答案正确**”，否则输出“**答案错误**”。

得到“**答案正确**”的条件是：

1. 字符串中必须仅有 `P`、 `A`、 `T`这三种字符，不可以包含其它字符；
2. 任意形如 `xPATx` 的字符串都可以获得“**答案正确**”，其中 `x` 或者是空字符串，或者是仅由字母 `A` 组成的字符串；
3. 如果 `aPbTc` 是正确的，那么 `aPbATca` 也是正确的，其中 `a`、 `b`、 `c` 均或者是空字符串，或者是仅由字母 `A` 组成的字符串。

现在就请你为 PAT 写一个自动裁判程序，判定哪些字符串是可以获得“**答案正确**”的。



### 输入格式

每个测试输入包含 1 个测试用例。第 1 行给出一个正整数 *n* (<10)，是需要检测的字符串个数。接下来每个字符串占一行，字符串长度不超过 100，且不包含空格。



### 输出格式

每个字符串的检测结果占一行，如果该字符串可以获得“**答案正确**”，则输出 `YES`，否则输出 `NO`。。



### 输入样例

```
8
PAT
PAAT
AAPATAA
AAPAATAAAA
xPATx
PT
Whatever
APAAATAA
```

### 输出样例

```
YES
YES
YES
YES
NO
NO
NO
NO
```



### 思路

##### 审题

- 字符串只能包含‘P’ ‘A ’ ‘T’三种字符,如果存在其他字符则判错

- xPATx格式是正确的,这个条件需要正确理解,两个x是指代相同的字符串!!!

- aPbTc正确,aPbATca也正确.**本题的重点!**

  P和T中间每增加一个A,就需要将P之前的内容复制到字符串尾部.

##### 总结

- 注意看这八个测试样例
- 只存在‘P’ ‘ T’ ‘A’ 这三个字符
- ‘P’ 与 ‘T’ 的先后关系
- ‘P’ 与 ‘T’ 之间不能没有 ‘A’
- ‘T’ 之后 ‘A’ 的数量 =  ‘P’ 之前 ‘A’ 的数量 $*$ ‘P&T’ 中间 ‘A’ 的数量

##### 代码思路

- 用三个数分别记录三个位置的‘A’个数（用数组count
- 使用类似标记变量pos,检查‘P’,‘T’的出现及次序 (count[pos])
  - **其值在出现P之前为0（使用count[0]记录P之前的A）**
  - **只有在出现P且其值为0时，将值变为1（使用count[1]记录P&T之间的A）**
  - **只有在出现T且其值为1时，将其变为2（使用count[2]记录T之后的A）**

### 实现代码

```c
#include <stdio.h>

int main()
{
    char c;
    int num;
    scanf("%d", &num);
    while(getchar() != '\n');  /* 确保从读取每行时,从开头读取：‘\n’分割 */
    
    for(int i = 0; i < num; i++)
    {
        int pos = 0, count[3] = {0, 0, 0};
        while((c = getchar()) != '\n')
        {
            if(c == 'A')                  count[pos]++; /* count 'A's     */
            else if(c == 'P' && pos == 0) pos = 1;      /* one P before T */
            else if(c == 'T' && pos == 1) pos = 2;      /* one T after P  */
            else                          break;        /* 'wrong' string */
        }

        if(c == '\n' && pos == 2 && count[1] && count[2] == count[1] * count[0]) //见思路
            puts("YES");
        else
            puts("NO");

        /* read the rest of the line */
        if(c != '\n')
            while(getchar() != '\n');
    }

    return 0;
}
```


