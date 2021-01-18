# PAT Basic 1015


# PAT Basic 1015. 德才论（25分）

 (C语言实现)
<!--more-->

### 题目

宋代史学家司马光在《资治通鉴》中有一段著名的“德才论”：“是故才德全尽谓之圣人，才德兼亡谓之愚人，德胜才谓之君子，才胜德谓之小人。凡取人之术，苟不得圣人，君子而与之，与其得小人，不若得愚人。”

现给出一批考生的德才分数，请根据司马光的理论给出录取排名。	



### 输入格式

输入第一行给出 3 个正整数，分别为：*N*（≤$$ 10^5 $$），即**考生总数**；*L*（≥60），为**录取最低分数线**，即德分和才分均不低于 *L* 的考生才有资格被考虑录取；*H*（<100），为**优先录取线**——**德分和才分均不低于**此线的被定义为“才德全尽”，此类考生按**德才总分从高到低**排序；才分不到但德分到线的一类考生属于“德胜才”，也按总分排序，但排在第一类考生之后；德才分均低于 *H*，但是德分不低于才分的考生属于“才德兼亡”但尚有“德胜才”者，按总分排序，但排在第二类考生之后；其他达到最低线 *L* 的考生也按总分排序，但排在第三类考生之后。

随后 *N* 行，每行给出一位考生的信息，包括：`准考证号 德分 才分`，其中`准考证号`为 8 位整数，德才分为区间 [0, 100] 内的整数。数字间以空格分隔。



### 输出格式

输出第一行首先给出达到最低分数线的考生人数 *M*，随后 *M* 行，每行按照输入格式输出一位考生的信息，考生按输入中说明的规则从高到低排序。当某类考生中有多人总分相同时，按其德分降序排列；若德分也并列，则按准考证号的升序输出。



### 输入样例

```
14 60 80
10000001 64 90
10000002 90 60
10000011 85 80
10000003 85 80
10000004 80 85
10000005 82 77
10000006 83 76
10000007 90 78
10000008 75 79
10000009 59 90
10000010 88 45
10000012 80 100
10000013 90 99
10000014 66 60
```

### 输出样例

```
12
10000013 90 99
10000012 80 100
10000003 85 80
10000011 85 80
10000004 80 85
10000007 90 78
10000006 83 76
10000005 82 77
10000002 90 60
10000014 66 60
10000008 75 79
10000001 64 90
```



### 思路（借鉴大佬,本题偏难）

##### 审题

- 第一类【才德全尽：德分/才分均高于H（优先录取线）,总分从高到低排序】
- 第二类【德胜才：才分不到但德分到线,按总分排序,排在第一类考生之后】
- 第三类【“才德兼亡”但尚有“德胜才”者：德才分均低于 *H*，但是德分不低于才分的考生】
- 第四类【其他达到最低线 *L的考生* 】
- 总分相同时,按其德分降序排列;若德分也并列,则按准考证号的升序输出

##### 构思

###### **qsort()函数**

```c
void qsort(void *base,size_t nitems,size_t size,int (*compar))
int compar(const void *a, const void *b)
```

- **base** -- 指向要排序的数组的第一个元素的指针。
- **nitems** -- 由 base 指向的数组中元素的个数。
- **size** -- 数组中每个元素的大小，以字节为单位。
- **compar** -- 用来比较两个元素的函数。

###### **compar()函数**

​		compar 参数指向一个比较两个元素的函数.比较函数的原型应该像下面这样.注意两个形参必须是 **const void \*** 型,同时在调用 compar 函数（compar 实质为函数指针,这里称它所指向的函数也为 compar）时,传入的实参也必须转换成**const void \*** 型.在 compar 函数内部会将 **const void \*** 型转换成实际类型,**如果 compar 返回值小于 0（< 0）,那么 p1 所指向元素会被排在p2所指向元素的前面如果 compar 返回值等于 0（= 0）,那么 p1 所指向元素与 p2 所指向元素的顺序不确定如果 compar 返回值大于 0（> 0）,那么 p1 所指向元素会被排在 p2 所指向元素的后面。**

### 实现代码

```c
#include <stdio.h>
#include <stdlib.h> //里有qsort()

typedef struct _Student{
    int ID;
    int D;      /** de2 : virtue  */
    int C;      /** cai2: ability */
    int rank;
    int sum;    /** sum = D + C   */
}sStudent, *Student;

/* larger the number, higher the rank */
int rank(Student s, int H, int L)
{/* 学生分类 */
    if(s->D < L || s->C < L)        return 0;   /* failed */
    else if(s->D >= H && s->C >= H) return 4;   /* best */
    else if(s->D >= H)              return 3;   /* second */
    else if(s->D >= s->C)           return 2;   /* third */
    else                            return 1;   /* fourth */
}

int comp(const void *a, const void *b)
{/* 继承学生结构并进行比较,定义排序规则,rank>sum>D>ID */
    Student s1 = *(Student*)a;
    Student s2 = *(Student*)b;

    if(s1->rank != s2->rank)    return s1->rank - s2->rank;
    else if(s1->sum != s2->sum) return s1->sum - s2->sum;
    else if(s1->D != s2->D)     return s1->D - s2->D;
    else if(s1->ID != s2->ID)   return s2->ID - s1->ID;
    else                        return 0;
}

int main()
{
    int N, L, H, M = 0;
    Student students[100000] = {0};
    sStudent buffer[100000];/* 最多10w学生 */

    scanf("%d %d %d", &N, &L, &H);
    for(int i = 0; i < N; i++)
    {
        Student s = buffer + i;/* 循环申请,记录学生 */
        scanf("%d %d %d", &s->ID, &s->D, &s->C);
        s->sum = s->D + s->C;
        if((s->rank = rank(s, H, L)) != 0) /* 记录过线的全部学生 */
            students[M++] = s;
    }

    qsort(students, M, sizeof(Student), comp);/* 从小到大排序 */

    printf("%d\n", M);
    for(int i = M - 1; i >= 0; i--)
        printf("%d %d %d\n", students[i]->ID, students[i]->D, students[i]->C);

    return 0;
}
```


