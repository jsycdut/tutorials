# leetcode中的位运算

## 预备知识
位运算一般指整数的移位运算，典型的位运算包括如下
|移运算|解释|
|:-:|:-:|
|`<<`|二元运算符，将目标操作数按二进制位向左移动指定位数，低位补0|
|`>>`|二元运算符，将目标操作数按二进制位向右移动指定位数，高位填充符号位|
|`>>>`|二元运算符，与`>>`类似，不过高位填充为0，而不是符号位|
|`|`|二元运算符，表示两数的按位或操作|
|`&`|二元运算符，表示两数的按位与操作|
|`^`|二元运算符，表示两数的按位异或操作|
|`~`|一元运算符，将整数按位取反|


## leetcode真题实战
* [leetcode-191 位1的个数](https://leetcode-cn.com/problems/number-of-1-bits/)
* [leetcode-231 2的幂](https://leetcode-cn.com/problems/power-of-two/)
* [leetcode-342 4的幂](https://leetcode-cn.com/problems/power-of-four/)
