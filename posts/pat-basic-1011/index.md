# PAT Basic 1011


# PAT Basic 1011. A+B和C（15分）

 (C语言实现)
<!--more-->

### 题目

给定区间 [$$ -2^{31} $$,$$ 2^{31} $$] 内的 3 个整数 *A*、*B* 和 *C*，请判断 *A*+*B* 是否大于 *C*。



### 输入格式

输入第 1 行给出正整数 *T* (≤10)，是测试用例的个数。随后给出 *T* 组测试用例，每组占一行，顺序给出 *A*、*B* 和 *C*。整数间以空格分隔。



### 输出格式

对每组测试用例，在一行中输出 `Case #X: true` 如果 *A*+*B*>*C*，否则输出 `Case #X: false`，其中 `X` 是测试用例的编号（从 1 开始）。



### 输入样例

```
4
1 2 3
2 3 4
2147483647 0 2147483646
0 -2147483648 -2147483647
```

### 输出样例

```
Case #1: false
Case #2: true
Case #3: true
Case #4: false
```



### 思路

- 一道相对简单的题,需要注意的是数值范围,所以我们定义ABC用的是long int

- int可表达范围：-2147483648～2147483647 

- long int可表达范围：-9223372036854775808～9223372036854775807

  ```c
  #include <stdio.h>
  #include<limits.h>
  
  int main(void)
  
  {    
  
  	printf("%lld",LLONG_MAX);/* 可输出对应环境long int最大值 */
  	return 0;
  
  }
  ```

  

### 实现代码

```c
#include <stdio.h>

int main()
{
    int N;
    long int A, B, C;
    scanf("%d", &N);

    for(int i = 1; i <= N; i++)
    {
        scanf("%ld %ld %ld", &A, &B, &C);
        printf("Case #%d: %s\n", i, A + B > C ? "true" : "false");
    }

    return 0;
}
```


