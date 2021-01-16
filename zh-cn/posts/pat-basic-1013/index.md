# PAT Basic 1013


# PAT Basic 1013. 数素数（20分）

 (C语言实现)
<!--more-->

### 题目

令 $$ P_i $$表示第 i 个素数。现任给两个正整数 *M* ≤ *N* ≤$$ 10^4 $$,请输出$$ P_M $$到 $$ P_N $$ 的所有素数。



### 输入格式

输入在一行中给出 *M* 和 *N*,其间以空格分隔。



### 输出格式

输出从$$ P_M $$到 $$ P_N $$ 的所有素数,每 10 个数字占 1 行,其间以空格分隔,但行末不得有多余空格。



### 输入样例

```
5 27
```

### 输出样例

```
11 13 17 19 23 29 31 37 41 43
47 53 59 61 67 71 73 79 83 89
97 101 103
```



### 思路

- 还记得1007的素数判断条件吗？素数的性质！1,2,3,5,7,11,13...
- 用建立素数表的方法,同时能把被取余的那个数更改为素数表中的素数，以简便算法

### 实现代码

```c
#include <stdio.h>

int main()
{

    int M, N;
    scanf("%d %d", &M, &N);
    int primes[10000];

    for(int n = 2, count = 0; count < N; n++)
    {
        /* iprime标准变量：1素数/0非素数 */
        int iprime = 1;
        for(int j = 0; count > 0 && primes[j] * primes[j] <= n; j++)
            if(n % primes[j] == 0)	iprime = 0;
				/* 找到素数,并记录 */
        if(iprime)	primes[count++] = n;
    }

    /* 输出,注意输出格式 */
    for(int i = M; i < N; i++)
    {
        printf("%d", primes[i - 1]);
        printf((i - M + 1) % 10 ? " " : "\n");
    }
    printf("%d", primes[N - 1]);

    return 0;
}
```


