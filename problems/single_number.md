**找出只出现一次的数字**  

初期的思路是：  
1. 读取数组内所有数字
2. 对每个数字出现的次数进行计数
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
    return 1;
}
```  

但是这个解法因为使用了 *两层嵌套循环* 导致效率极低   
发现可以使用 *异或运算* 进行优化 理由如下： 
1. 利用性质 a ^ a = 0，可以抵消数组中出现偶数次的元素
2. 利用性质 0 ^ a = a，可以保留唯一出现奇数次的元素
3. 仅使用单层循环 时间复杂度从O(n2)降至 O(n)

```c
int singleNumber(int* nums, int numsSize) {   
int n = nums[0];

for(int i = 1; i < numsSize; i++) {
    n ^= nums[i];
}
return n;
}

```
