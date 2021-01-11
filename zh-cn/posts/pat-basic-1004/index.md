# PAT Basic 1004

# PAT Basic 1004. 成绩排名（20分）

 (C语言实现)
<!--more-->

### 题目

读入 *n*（>0）名学生的姓名、学号、成绩，分别输出成绩最高和成绩最低学生的姓名和学号。



### 输入格式

第 1 行：正整数 n 

第 2 行：第 1 个学生的姓名 学号 成绩 

第 3 行：第 2 个学生的姓名 学号 成绩 

 ... ... ...

 第 n+1 行：第 n 个学生的姓名 学号 成绩

其中`姓名`和`学号`均为不超过 10 个字符的字符串，成绩为 0 到 100 之间的一个整数，这里保证在一组测试用例中没有两个学生的成绩是相同的



### 输出格式

对每个测试用例输出 2 行，第 1 行是成绩最高学生的姓名和学号，第 2 行是成绩最低学生的姓名和学号，字符串间有 1 空格。



### 输入样例

```
3
Joe Math990112 89
Mike CS991301 100
Mary EE990830 95
```

### 输出样例

```
Mike CS991301
Joe Math990112
```



### 思路

- 相比于1003,1004是一道相对简单的题
- 我们可以使用数据结构中找Max or Min的思想,如果满足条件,不断交换至找到最值
- 注意题目所给三种数据的范围

### 实现代码

```c
#include<stdio.h>
#include<string.h>

int main(){
    int N;
    scanf("%d",&N);
  //Max—最大值的各种数据项
  //Min-最小值的各种数据项
  //cur-当前值的各种数据项
    char maxname[11],minname[11],curname[11],
         maxid[11],minid[11],curid[11];
    int max = -1, min = 101, cur;
    for(int i = 0;i < N; i++){
        scanf("%s %s %d",curname,curid,&cur);
      //cur-字符串不给赋值符号&
        if(cur > max){
            strcpy(maxname,curname);
            strcpy(maxid,curid);
            max=cur;
        }
        if(cur < min){
            strcpy(minname,curname);
            strcpy(minid,curid);
            min=cur;
        }
    }
    printf("%s %s\n",maxname,maxid);
    printf("%s %s",minname,minid);
    return 0;
}
```


