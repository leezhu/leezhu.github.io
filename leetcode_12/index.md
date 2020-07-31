# Leetcode_12

<!--more-->

## [12. 整数转罗马数字](https://leetcode-cn.com/problems/integer-to-roman/)

罗马数字包含以下七种字符： `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 2 写做 `II` ，即为两个并列的 1。12 写做 `XII` ，即为 `X` + `II` 。 27 写做  `XXVII`, 即为 `XX` + `V` + `II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

- `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
- `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
- `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。

给定一个整数，将其转为罗马数字。输入确保在 1 到 3999 的范围内。

**示例 1:**

```
输入: 3
输出: "III"
```

**示例 2:**

```
输入: 4
输出: "IV"
```

**示例 3:**

```
输入: 9
输出: "IX"
```

**示例 4:**

```
输入: 58
输出: "LVIII"
解释: L = 50, V = 5, III = 3.
```

**示例 5:**

```
输入: 1994
输出: "MCMXCIV"
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```



#### 解题思路：

刚开始我打算用多个if else来区分4和9这种特殊情况，但是发觉过于繁琐，且代码量过多。也有许多同学提出很简单的python字符拼接方式，将所有情况都列举到二维数组里面，能够快速的找到罗马数字。后来经过评论区的朋友思维激发，题目中给的数值最大是3999，因此可以将1～1000中特殊的位置值全都罗列出来，再将要转换的数值从最高位进行拆分，把每一位从大到小进行整除。具体思路我还是通过代码进行讲解。

```go
//测试需要导入bytes包
func intToRoman(num int) string {
    resultStr:=bytes.NewBuffer([]byte(""))	//建立一个byte数组
    nums:=[13]int{1000,900,500,400,100,90,50,40,10,9,5,4,1}
    romanNums:=[13]string{"M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"}
    for i:=0;i<13;i++{
        temp:=num/nums[i]   //可以获取到每一位当前的倍数，例如3000就是3个1000
        if temp>0{
            			 resultStr.Write(bytes.Repeat([]byte(romanNums[i]),temp))    //倍数就是出现的次数，将结果写入到最终的字符串中
         num-=temp*nums[i]	//减去最高位，例如2900变成900
        }else{
            continue	//说明num比当前数组里面的值小，那么就找下一个进行比对
        }
    }
    return resultStr.String()   //bytes的字符串方法
}
```

