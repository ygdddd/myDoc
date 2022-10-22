# Algorithm
[OI-wiki](https://oi-wiki.org/)
## Dynamic Programming

动态规划是一种通过把原问题分解为相对简单的子问题的方式求解复杂问题的方法。

引入：斐波那契数列（Fibonacci）
```Java
//传统递归做法
public Fibonacci(int n){
    if(n <= 0){
        return 0;
    }
    if(n == 1){
        return 1;
    }
    return Fibonacci(n - 1) + Fibonacci(n - 2);
}
```
问题：过多次数调用方法会导致较小的数字重复执行。

解决方法1:使用数据结构存放已经计算的值
```Java
int array[];

public int FibArray(int n){
    if(n <= 0){
        return 0;
    }
    if(n == 1){
        return 1;
    }
    array = new int[n+1];
    return Fib(n);
}
public int FIb(int n){
    if(array[n]!=0){
        return array[n];
    }
    array[n] = array[n - 1] + array[n - 2];
    return array[n];
}
```
该方法还是利用了递归，由大数的计算引入小数的计算。
既然都要计算小数，则可以从小数开始算到大数->先计算子问题，再计算父问题。

解决方法2:动态规划
```Java
public int FibArray(int n){
    if(n <= 0){
        return 0;
    }
    if(n == 1){
        return 1;
    }
    int array[] = new int[n+1];
    array[1] = 1;
    for(int i = 2; i < n + 1;i++){
        if(array[i] == 0){
            array[i] = array[i-1] + array[i-2];
        }
    }
    return array[n];
}
```
