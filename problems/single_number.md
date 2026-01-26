# 136.single_number

## 题目介绍

题目所给数字中，仅有一个数字会出现一次，找出这个“single_number”  
```
测试案例：[2,2,1]-->输出 1  
[4,2,4,2,3]-->输出 3  
```

## 解题思路

初期的思路(暴力解法)：  
1. 外层循环遍历数组每个元素
2. 内层循环统计当前元素出现的次数
3. 如果其中一个数字计数为1，则返回这个数字
```c
int singleNumber(int* nums, int numsSize) {

    for (int i = 0; i < numsSize; i++) {
        int count = 0;
        for (int j = 0; j < numsSize; j++) {
            if (nums[j] == nums[i]) {
            count++;
            }    
        }
       
        if (count==1) {
        return nums[i];
        }
    }
    return 0;
}
```  
**优化**
上面解法因为使用了 *两层嵌套循环* 导致效率极低   
发现可以使用 *异或运算* 进行优化 理由如下： 
1. 利用性质 a ^ a = 0，可以抵消数组中出现偶数次的元素(自反性)
2. 利用性质 0 ^ a = a，可以保留唯一出现奇数次的元素(恒等性)
3. 满足交换律和结合律 a ^b ^ c = a ^ c ^ b = a ^ ( b ^ c )
4. 仅使用单层循环 时间复杂度从O(n2)降至 O(n)

```c
int singleNumber(int* nums, int numsSize) {   
int n = nums[0];

for(int i = 1; i < numsSize; i++) {
    n ^= nums[i];
}
return n;
}

```
**关键知识**
位运算 异或

## 易错点

```c
int singleNumber(int* nums, int numsSize) {

    for (int i = 0; i < numsSize; i++) {
        int count = 0;
        for (int j = 0; j < numsSize; j++) {
            if (nums[j] == nums[i]) {
            count++;
            }    
        }
       
        if (count==1) {
        return nums[i];
        }
    }
    return 0;//丢掉此行(兜底返回)
}
```
C 语言的语法规则要求函数必须有返回值，即使逻辑上不会执行到这一行。  
singleNumber 函数的返回类型是 int，这意味着无论函数内部的逻辑如何，编译器都要求函数在所有可能的执行路径上都有明确的返回值。
从题目来讲数组中必然存在且仅存在一个符合条件的元素，所以逻辑上永远会执行到 return nums[i]，不会走到最后一行。但编译器并不知道题目规则，编译器会认为外层循环有可能遍历完所有元素都不触发 if (count==1)，如果没有最后的 return 0，编译器会判定存在无返回值的执行路径，直接报编译错误。