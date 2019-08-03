$C_{n+1}= \sum_{k=0}^{n}C_{k}C_{n-k}$

```cpp
#define M 35
ll C[M+1];
void catalan()
{
    C[0]=C[1]=1;
    for(int i=2; i<=M; i++)
    {
        C[i]=0;
        for(int j=0; j<i; j++)
            C[i] += C[j]*C[i-j-1];
    }
}
```
