---
title: LeetCode-数据结构
date: 2021-07-16
tags: 记
comments: true
categories: 记
---



##  day1

1. **存在重复元素** 

> 给定一个整数数组，判断是否存在重复元素。
>
> 如果存在一值在数组中出现至少两次，函数返回 `true` 。如果数组中每个元素都不相同，则返回 `false` 。

```js
示例 1:

输入: [1,2,3,1]
输出: true
示例 2:

输入: [1,2,3,4]
输出: false
示例 3:

输入: [1,1,1,3,3,4,3,2,4,2]
输出: true

```



-  **方法1：Set结构**

> 利用Set结构一行解决

```typescript
function containsDuplicate(nums: number[]): boolean {
    const setNums: Array<any> = [...new Set(nums)]
    return setNums.length !== nums.length
};
```



- **方法2：优化版暴力解法**

> 暴力解法优化版，每次减少了已经检查过的值

```typescript
function containsDuplicate(nums: number[]): boolean {
    let newArr: Array<any> = [...nums]
    let res: boolean = false
    for(let i = 0; i <nums.length; i++){
        const item: any = newArr.splice(0, 1)[0]
        res = newArr.includes(item)
        if(res)break
    }
    return res
};
```



2. **最大子序和**

> 给定一个整数数组 `nums` ，找到一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

```json
示例 1：

输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
示例 2：

输入：nums = [1]
输出：1
示例 3：

输入：nums = [0]
输出：0
示例 4：

输入：nums = [-1]
输出：-1
示例 5：

输入：nums = [-100000]
输出：-100000

```

- **方法1：动态规划**

```typescript
function maxSubArray(nums: number[]): number {
    let sum = 0;
    let ans = nums[0]
    for(const num of nums) {
        if(sum > 0) {
            sum += num
        } else {
            sum = num
        }
        ans = Math.max(ans, sum)

    }
    return ans
};
```



---



## Day2

1. **两数之和**

```c
给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

 

示例 1：

输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
示例 2：

输入：nums = [3,2,4], target = 6
输出：[1,2]
示例 3：

输入：nums = [3,3], target = 6
输出：[0,1]

```



- **方法1：爆破**

  > 2 个 for 循环直接暴力解法

  ```typescript
  function twoSum(nums: number[], target: number): number[] {
      for (let i = 0; i < nums.length - 1; i++) {
          for (let j = i + 1; j < nums.length; j++) {
              if (target === nums[j] + nums[i]) {
                  return [i, j];
              }
          }
      }
  }
  ```


- **方法2：哈希表查询**

> 1. 建立一个`值`与`下标`的哈希表
> 2. 遍历一次数组用 `target - 数组的value`  = 结果，结果在我们的哈希表里面执行 `.has`找得到就返回下标

```typescript
function twoSum(nums: number[], target: number): number[] {
    let map = new Map<number, number>()
    for (let i = 0; i < nums.length; i++) {
       if(map.has(target - nums[i])){
           // 返回建立的哈希表的下标的值，和当前的下标
           return [map.get(target - nums[i]), i];
       } else {
           // 储存值为 key 下标为 index
           map.set(nums[i], i);
       }
    }
    return []
}

```



2. **合并两个有序数组**

```c
给你两个有序整数数组 nums1 和 nums2，请你将 nums2 合并到 nums1 中，使 nums1 成为一个有序数组。

初始化 nums1 和 nums2 的元素数量分别为 m 和 n 。你可以假设 nums1 的空间大小等于 m + n，这样它就有足够的空间保存来自 nums2 的元素。

 

示例 1：

输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
示例 2：

输入：nums1 = [1], m = 1, nums2 = [], n = 0
输出：[1]

```

- **方法1：合并数组排序**

```typescript
function merge(nums1: number[], m: number, nums2: number[], n: number): void {
    nums1.splice(m, nums1.length - m, ...nums2);
    nums1.sort((a, b) => a - b);
};
```



- **方法2：双指针**



![双指针](https://i.loli.net/2021/08/02/9kMhd8ZF27jsLC3.gif)

```typescript
/**
 Do not return anything, modify nums1 in-place instead.
 */
function merge(nums1: number[], m: number, nums2: number[], n: number): void {
   // 定义2个从0 开始的指针，分别用来遍历 nums1 和 nums2
   let p1 = 0, p2 = 0;
   // 空数组储存排序后的值
    const sorted = new Array(m + n).fill(0);
    // 当次排序的值
    var cur: number;
    
    // 开始遍历2个数组
    while (p1 < m || p2 < n) {
        if (p1 === m) {
            cur = nums2[p2++];
        } else if (p2 === n) {
            cur = nums1[p1++];
        } else if (nums1[p1] < nums2[p2]) {
            cur = nums1[p1++];
        } else {
            cur = nums2[p2++];
        }

        sorted[p1 + p2 - 1] = cur;
    }
    for (let i = 0; i != m + n; ++i) {
        nums1[i] = sorted[i];
    }
};
```



**方法3：逆向双指针**

> 方法2的优化版，节省一个内存空间的位置，我们已知` mums1 的长度 = m + n`，后半部分的元素都是空的所有`nums `和 `nums2` 永远不会相交所有从尾部可以直接给`nums1`赋值

```typescript
/**
 Do not return anything, modify nums1 in-place instead.
 */
function merge(nums1: number[], m: number, nums2: number[], n: number): void {
    // 指针从尾部开始
    let p1 = m - 1
    let p2 = n - 1
    // 总长度
    let tail = m + n - 1;
    // 当次排序的值
    let cur: number;

    while (p1 >= 0 || p2 >= 0) {
        if (p1 === -1) {
            cur = nums2[p2--];
        } else if (p2 === -1) {
            cur = nums1[p1--];
        } else if (nums1[p1] > nums2[p2]) {
            cur = nums1[p1--];
        } else {
            cur = nums2[p2--];
        }
        nums1[tail--] = cur;
    }
};
```



---





## Day3



1. **两个数组的交集 II**

```c

给定两个数组，编写一个函数来计算它们的交集。

 

示例 1：

输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2,2]
示例 2:

输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[4,9]
 

说明：

输出结果中每个元素出现的次数，应与元素在两个数组中出现次数的最小值一致。
我们可以不考虑输出结果的顺序。
进阶：

如果给定的数组已经排好序呢？你将如何优化你的算法？
如果 nums1 的大小比 nums2 小很多，哪种方法更优？
如果 nums2 的元素存储在磁盘上，内存是有限的，并且你不能一次加载所有的元素到内存中，你该怎么办？

```



- **方法1：哈希表查询**

> O(n)
>
> 1. 简历哈希表，值做key次数做value
> 2. 查询哈希表，按次数判断交集

```typescript
function intersect(nums1: number[], nums2: number[]): number[] {
    // 创建 哈希表
    let map = new Map()
    // 结果
    let res = []

    // 为nums1 建立 哈希表
    for (let v of nums1) {
        map.has(v) ? map.set(v, map.get(v) + 1) : map.set(v, 1)
    }

    // 遍历 nums2 查找nums1 的哈希表 找到交集
    for (let v2 of nums2) {
        if(map.has(v2)) {
            res.push(v2)
            // 找到了，次数大于 1 减一次，否则删除
            map.get(v2) > 1 ? map.set(v2, map.get(v2) - 1) : map.delete(v2)
        }
    }
    
    return res
};
```



- **方法2：双指针**

```typescript
function intersect(nums1: number[], nums2: number[]): number[] {
    // 创建2个数组指针
    let p1 = 0
    let p2 = 0
    // 排序2个数组
    nums1 = nums1.sort((a, b) => a - b)
    nums2 = nums2.sort((a, b) => a - b)

    let res = []
    // 取数组长度最小的一个数组做判断条件
    while (p1 < nums1.length && p2 < nums2.length ) {
        // 找到交集
        if(nums1[p1] === nums2[p2]){
            res.push(nums1[p1])
            p1 ++
            p2 ++
        // 数组1当前指针的值小于数组2的话，指针加一，用下一个数来比较
        } else if (nums1[p1] < nums2[p2]) {
            p1 ++
        // 数组2当前指针的值小于数组1的话，指针加一，用下一个数来比较
        } else {
            p2 ++
        }
    }

    return res
};
```



2. **买卖股票的最佳时机**

```c
给定一个数组 prices ，它的第 i 个元素 prices[i] 表示一支给定股票第 i 天的价格。

你只能选择 某一天 买入这只股票，并选择在 未来的某一个不同的日子 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 0 。

 

示例 1：

输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
示例 2：

输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
 

提示：

1 <= prices.length <= 105
0 <= prices[i] <= 104

```



- **方法1：贪心算法**

>1. 拟定一个最小值和当前利润
>
>2. 遍历更新最小值和利润




```typescript
function maxProfit(prices: number[]): number {
    // 利润
    let profit = 0
    // 最小值
    let min = prices[0]

    for (let v of prices) {
        if(v < min){
            min = v

        // 如果出现比最大利润还要大的就进行重新赋值
        }else if(v  - min > profit){ 
            profit = v  - min
        }
    }

    return profit
};
```



- **方法2：动态规划**

```ty
function maxProfit(prices: number[]): number {
    // 存放最小的值
    let minprice = Number.MAX_VALUE
    // 存放差值
    let maxprofit = 0
    
    for (let v of prices) {
        // 每次更新最小的值
        minprice = Math.min(v, minprice)
        // 每次计算差值
        maxprofit = Math.max(v - minprice, maxprofit)
    }

    return maxprofit
};
```



---



## Day4



1. **重塑矩阵**

```js
在MATLAB中，有一个非常有用的函数 reshape，它可以将一个矩阵重塑为另一个大小不同的新矩阵，但保留其原始数据。

给出一个由二维数组表示的矩阵，以及两个正整数r和c，分别表示想要的重构的矩阵的行数和列数。

重构后的矩阵需要将原始矩阵的所有元素以相同的行遍历顺序填充。

如果具有给定参数的reshape操作是可行且合理的，则输出新的重塑矩阵；否则，输出原始矩阵。

示例 1:

输入: 
nums = 
[[1,2],
 [3,4]]
r = 1, c = 4
输出: 
[[1,2,3,4]]
解释:
行遍历nums的结果是 [1,2,3,4]。新的矩阵是 1 * 4 矩阵, 用之前的元素值一行一行填充新矩阵。
示例 2:

输入: 
nums = 
[[1,2],
 [3,4]]
r = 2, c = 4
输出: 
[[1,2],
 [3,4]]
解释:
没有办法将 2 * 2 矩阵转化为 2 * 4 矩阵。 所以输出原矩阵。
注意：

给定矩阵的宽和高范围在 [1, 100]。
给定的 r 和 c 都是正数。
```



<img src="https://i.loli.net/2021/08/04/8Fg2MZnfI5XiRYV.jpg" alt="重塑矩阵1" style="zoom:67%;" />

<img src="https://i.loli.net/2021/08/04/jRmgOXuD43Q2FKU.jpg" alt="重塑矩阵2" style="zoom:67%;" />

- **方法1：双循环**

> 1. 拍平数组
> 2. 按照行数`r`，列数`c` 重新排列

```typescript
function matrixReshape(mat: number[][], r: number, c: number): number[][] {
    // 拍平二维数组
    let newMat: any = mat.flat()
    
    // 判断个数是否一致，一致才能重塑
    if (r * c !== newMat.length) return mat

    // r 行
    for (let i = 0; i < r; i++) {
        const item: number[] = []
        // 每行c个
        for (let j = 0; j < c; j ++){
            // 将c个元素从头部拿出，并放入暂存的item数组
            item.push(newMat.shift(newMat[i]))
        }
        // 当前行收集完毕，推入新数组的尾部
        newMat.push(item);
    }

    return newMat
};
```



- **方法2：二维数组一维表示**

```typescript
function matrixReshape(mat: number[][], r: number, c: number): number[][] {
    const m = mat.length;
    const n = mat[0].length;
    if (m * n != r * c) {
        return mat;
    }

    const ans = new Array(r).fill(0).map(() => new Array(c).fill(0));
    for (let x = 0; x < m * n; ++x) {
        ans[Math.floor(x / c)][x % c] = mat[Math.floor(x / n)][x % n];
    }
    return ans;
};
```



2. **杨辉三角**

```js
给定一个非负整数 numRows，生成杨辉三角的前 numRows 行。

示例:

输入: 5
输出:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]
```

![PascalTriangleAnimated2](https://tva1.sinaimg.cn/large/005u3PONly8gsonaoir33g307806o3z0.gif)



> 杨辉三角非常有意思，我们很容易就可以找到呀的规律
> 1. 每行的第一个元素和最后一个元素永远不会是上一层的和，因为他永远都是1
> 2. 下一层的值(x),位置(y) 取决上一层的和
> 得出公式：

```
x = 当前值
y = 层数
z = 位置

x = y - 1 [ z -1 ] + y - 1 [ z + 1 ]
```

```typescript
function generate(numRows: number): number[][] {
    const ret = []
    for(let i = 0; i < numRows; i++) {
        // 创建行，用 1 填充
        const row = new Array(i + 1).fill(1);
        // 填充每一行数据，左右2边不填充, 从 1 开始，结束于倒数第二个
        for (let j = 1; j < row.length - 1; j++){
            // 当前的值 = (上一行的当前个 - 1 [上一个] 值  ) + ( 上一行的当前个数 [下一个] 值 )
            row[j] = ret[i - 1][j - 1] + ret[i - 1][j];
        }
        // 添加当前行
        ret.push(row);
    }

    return ret;
};
```



---



## day5

1. **有效的数独**

```js
请你判断一个 9x9 的数独是否有效。只需要 根据以下规则 ，验证已经填入的数字是否有效即可。

数字 1-9 在每一行只能出现一次。
数字 1-9 在每一列只能出现一次。
数字 1-9 在每一个以粗实线分隔的 3x3 宫内只能出现一次。（请参考示例图）
数独部分空格内已填入了数字，空白格用 '.' 表示。

注意：

一个有效的数独（部分已被填充）不一定是可解的。
只需要根据以上规则，验证已经填入的数字是否有效即可。

```



**示例1：**

![250px-sudoku-by-l2g-20050714svg](https://i.loli.net/2021/07/22/SNa4FQg2EqmAfcB.png)

```json
输入：board = 
[["5","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
输出：true
示例 2：

输入：board = 
[["8","3",".",".","7",".",".",".","."]
,["6",".",".","1","9","5",".",".","."]
,[".","9","8",".",".",".",".","6","."]
,["8",".",".",".","6",".",".",".","3"]
,["4",".",".","8",".","3",".",".","1"]
,["7",".",".",".","2",".",".",".","6"]
,[".","6",".",".",".",".","2","8","."]
,[".",".",".","4","1","9",".",".","5"]
,[".",".",".",".","8",".",".","7","9"]]
输出：false

解释：除了第一行的第一个数字从 5 改为 8 以外，空格内其他数字均与 示例1 相同。 但由于位于左上角的 3x3 宫内有两个 8 存在, 因此这个数独是无效的。

```



- **方法1：双层遍历-[哈希表]; 【空间换时间】**

```typescript
function isValidSudoku(board: string[][]): boolean {
    /** 初始化横、纵、小九宫格
     *  每个元素 用 0 表示未被使用,再赋值为一个 map 哈希表
     */
    
    // 每行
    let rows = new Array(9).fill(0).map(() => new Map())
    // 每列
    let columns = new Array(9).fill(0).map(() => new Map())
    // 每个九宫格
    let boxes = new Array(9).fill(0).map(() => new Map())

    for(let i = 0; i<9; i++) {
        for(let j = 0; j<9; j++) {
            // 遇到 . 跳过
            if(board[i][j] === '.')continue
            // 当前元素
            let num = board[i][j]
            // 九宫格下标
            let box_idx = Math.floor(i/3)*3 + Math.floor(j/3)

            // 判断重复直接 false
            if(rows[i].has(num)) return false
            if(columns[j].has(num)) return false
            if(boxes[box_idx].has(num)) return false

            // 每次填充当前元素，1表示为 true； 被使用过了
            rows[i].set(num, 1)
            columns[j].set(num, 1)
            boxes[box_idx].set(num, 1)
        }
    }

    return true

}
```





2. **矩阵置零**

```js
给定一个 m x n 的矩阵，如果一个元素为 0 ，则将其所在行和列的所有元素都设为 0 。请使用 原地 算法。

进阶：

一个直观的解决方案是使用  O(mn) 的额外空间，但这并不是一个好的解决方案。
一个简单的改进方案是使用 O(m + n) 的额外空间，但这仍然不是最好的解决方案。
你能想出一个仅使用常量空间的解决方案吗？

```

**示例1：**

<img src="https://i.loli.net/2021/07/22/gqn6vlQV1RKHN9U.jpg" alt="mat1" style="zoom:67%;" />

> 输入：matrix = [[1,1,1],[1,0,1],[1,1,1]]
> 输出：[[1,0,1],[0,0,0],[1,0,1]]



**示例2：**

<img src="https://i.loli.net/2021/07/22/HrCI5ouEWh3OxpJ.jpg" alt="mat2" style="zoom:67%;" />



> 输入：matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]
> 输出：[[0,0,0,0],[0,4,5,0],[0,3,1,0]]



- **方法1：数组标记法**

```typescript
/**
 Do not return anything, modify matrix in-place instead.
 */
function setZeroes(matrix: number[][]): void {
    // 储存 行、列 出现过0的位置 并且去重 
    const rowsIndex = new Set()
    const columnsIndex = new Set()

    // 矩阵 长、宽
    const m = matrix.length
    const n = matrix[0].length

    // 筛选出 x,y 坐标出现过 0 的位置
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if(!matrix[i][j]) {
                rowsIndex.add(i)
                columnsIndex.add(j)
            }
        }
    } 

    // 置为 0 
    for (let i = 0; i < m; i++) {
        for (let j = 0; j < n; j++) {
            if(Array.from(rowsIndex).includes(i) || Array.from(columnsIndex).includes(j)) {
                matrix[i][j] = 0
            }
        }
    } 

};

```



---



## day6

1. **字符串中的第一个唯一字符**

```js
给定一个字符串，找到它的第一个不重复的字符，并返回它的索引。如果不存在，则返回 -1。

 

示例：

s = "leetcode"
返回 0

s = "loveleetcode"
返回 2
```



- **方法1：哈希表查询**

> 比较简单不贴更多解法了



```typescript
function firstUniqChar(s: string): number {
   let map = {}
   
   for (let i of s) {
       map[i] ? map[i] += 1 : map[i] = 1
   }
   
   for(let i = 0; i < s.length; i++) if(map[s[i]] === 1) return i;

   return -1
};
```



2. **赎金信**

```js
给定一个赎金信 (ransom) 字符串和一个杂志(magazine)字符串，判断第一个字符串 ransom 能不能由第二个字符串 magazines 里面的字符构成。如果可以构成，返回 true ；否则返回 false。

(题目说明：为了不暴露赎金信字迹，要从杂志上搜索各个需要的字母，组成单词来表达意思。杂志字符串中的每个字符只能在赎金信字符串中使用一次。)

 

示例 1：

输入：ransomNote = "a", magazine = "b"
输出：false
示例 2：

输入：ransomNote = "aa", magazine = "ab"
输出：false
示例 3：

输入：ransomNote = "aa", magazine = "aab"
输出：true
 

提示：
 你可以假设两个字符串均只含有小写字母。

```



- **方法1：操作字符串**

```typescript
function canConstruct(ransomNote: string, magazine: string): boolean {
    for (let s of ransomNote) {
        // 杂志中不包含赎金信中的字直接返回 false 
        if(magazine.indexOf(s) === -1) return false
        // 移除包含的这个字符串
        magazine = magazine.replace(s, '')
    }

    return true
};
```



- **方法2：哈希表查询**

```typescript
function canConstruct(ransomNote: string, magazine: string): boolean {
    // 建立哈希表
    const map = new Map();
    // 写入赎金信的值
    for (let s of ransomNote) {
        map.has(s) ? map.set(s, map.get(s) + 1) : map.set(s, 1);
    }
    // 杂志的值填满 减去每个值
    for (let s of magazine) {
        if (map.get(s)) map.set(s, map.get(s) - 1);
    }

    // 没有填满返回 false
    for (let v of map.values()) {
        if (v) return false;
    }

    return true;
}

```



3. **有效的字母异位词**

```js
给定两个字符串 s 和 t ，编写一个函数来判断 t 是否是 s 的字母异位词。

注意：若 s 和 t 中每个字符出现的次数都相同，则称 s 和 t 互为字母异位词。

 

示例 1:

输入: s = "anagram", t = "nagaram"
输出: true
示例 2:

输入: s = "rat", t = "car"
输出: false
 

提示:

1 <= s.length, t.length <= 5 * 104
s 和 t 仅包含小写字母
 

进阶: 如果输入字符串包含 unicode 字符怎么办？你能否调整你的解法来应对这种情况？

```



- **方法1： 排序**

```typescript
function isAnagram(s: string, t: string): boolean {
    // 转换成 -> 数组 -> 排序 -> 转换成字符串; 判断是否相等
    return s.length == t.length && [...s].sort().join('') === [...t].sort().join('')
};
```



- **方法2：哈希表[unicode 编码]**

> 进阶：处理 unicode 字符

```typescript
function isAnagram(s: string, t: string): boolean {
    // 长度不相等直接 返回 false
    if (s.length !== t.length) {
        return false;
    }

    // 初始化一个26个字母的数组
    const arr = new Array(26).fill(0);

    /**
     * 填入 unicode 码点 减去第一个 英文字母的码点
     * 例如 字母 'a' -> 97; 'b' -> 98 = 1; 不会出现负数
     */
    for (let i = 0; i < s.length; ++i) {
        arr[s.codePointAt(i) - 'a'.codePointAt(0)]++;
    }

    // 用 t 的剩余 unicode 码点 来清空；出现 负数说明不一致 返回 false
    for (let i = 0; i < t.length; ++i) {
        arr[t.codePointAt(i) - 'a'.codePointAt(0)]--;
        if (arr[t.codePointAt(i) - 'a'.codePointAt(0)] < 0) {
            return false;
        }
    }

    return true
};
```



---



## day7



1. **环形链表**

```typescript
给定一个链表，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，我们使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

如果链表中存在环，则返回 true 。 否则，返回 false 。
```

![环形链表](https://i.loli.net/2021/07/27/lFPWVR3bahmpiw6.png)



> 输入：head = [3,2,0,-4], pos = 1
> 输出：true
> 解释：链表中有一个环，其尾部连接到第二个节点。



![环形链表2](https://i.loli.net/2021/08/09/j3vuFWbxcKGmDCY.png)

> 输入：head = [1], pos = -1
> 输出：false
> 解释：链表中没有环。



**单链表的构造方法：👇🏻**

```typescript
 class ListNode {
      val: number
      next: ListNode | null
      constructor(val?: number, next?: ListNode | null) {
          this.val = (val===undefined ? 0 : val)
          this.next = (next===undefined ? null : next)
      }
  }
```



- **方法1：哈希表查询**

> 1. 建一个哈希表储存已经处理过的值
> 2. 遍历链表；当前值内链表包含则表示有换

```typescript
function hasCycle(head: ListNode | null): boolean {
    // 建表记录
    const MAP = []

    while(head) {
        // 存即有环
        if(MAP.includes(head)){
            return true
        }
        // 判断过的计入哈希表
        MAP.push(head)
        // 判断下一个 链路
        head = head.next
    }

    // 无环
    return false
};
```



- **方法：哈希表延升法：链表记录法，也叫污链表法**

> 1. 找过的链路增加一个 `flag` 标识符
> 2. 遇到标识符说明有环
>
> - 缺点：会改变原有数据结构
> - 优点：不需要单独展位一个内存

```typescript
function hasCycle(head: any): boolean {
    while (head) {
        if(head.flag) {
            return true
        }
        head.flag = true;
        head = head.next;
    }

    return false
};
```



- **方法2：快慢指针**



![快慢指针](https://i.loli.net/2021/07/27/f9CL3qNk1lbUvOI.gif)



> 快慢指针和我们之前做的`双指针`的题原理大致是一样的：
>
> - 一个是用2个指针缩短路径，一个是用2个指针减少占用内存（对比哈希表解法）
>
> - 快慢指针的原理很简单就是遍历一次链表，快指针比慢指针快一步，指针重叠则说明有环



```typescript
function hasCycle(head: ListNode | null): boolean {
    if (!head) return false

    // 初始化 快指针和慢指针
    let slow_p = head
    let fast_p = head

    while (fast_p.next !== null && fast_p.next.next !== null) {
        // 快指针比慢指针快一步
        slow_p = slow_p.next
        fast_p = fast_p.next.next
        // 如果快指针追上了慢指针则证明有环
        if (slow_p === fast_p) return true
    }

    return false
};
```



- **方法3：JS语言特性**

> `JSON.stringify`方法会自动检测传入的对象是否为环，如果`JSON.stringify`成功执行，那说明传入的对象一定不是环

```typescript
function hasCycle(head: ListNode | null): boolean {
    try {
        JSON.stringify(head)
        return false
    } catch {
        return true
    }
};
```





2. **合并两个有序链表**

将两个升序链表合并为一个新的 **升序** 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

![合并两个有序链表](https://i.loli.net/2021/07/28/k5VZjp3CLYrsJ7R.jpg)



```js
输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]
示例 2：

输入：l1 = [], l2 = []
输出：[]
示例 3：

输入：l1 = [], l2 = [0]
输出：[0]


提示：

两个链表的节点数目范围是 [0, 50]
-100 <= Node.val <= 100
l1 和 l2 均按 非递减顺序 排列
```



- **方法1：递归**

```typescript
function mergeTwoLists(l1: ListNode | null, l2: ListNode | null): ListNode | null {
    
    // 其中一个空了证明排序 排完了
    if (!l1 || !l2) {
        return l1 || l2
    } 

    if (l1.val < l2.val) {
        l1.next = mergeTwoLists(l1.next, l2);
        return l1;
    } else {
        l2.next = mergeTwoLists(l1, l2.next);
        return l2;
    }
};
```



- **方法2：迭代**

```typescript
function mergeTwoLists(l1: ListNode | null, l2: ListNode | null): ListNode | null {
    // 已知范围最小的是 0，nwe 一个出来做比较
    const newList = new ListNode(0);
    let cur = newList;

    while (l1 && l2) {
        if (l1.val <= l2.val) {
            cur.next = l1;
            l1 = l1.next;
        } else {
            cur.next = l2;
            l2 = l2.next;
        }
        cur = cur.next;
    }

    // 合并后 l1 和 l2 最多只有一个还未被合并完，我们直接将链表末尾指向未合并完的链表即可
    cur.next = l1 ? l1 : l2;
    return newList.next;
};
```



3. **移除链表元素**



- **方法1：递归**



```typescript
function removeElements(head: ListNode | null, val: number): ListNode | null {
    if (head === null) return head

    head.next = removeElements(head.next, val); 
    // 满足条件跳过当前 链表
    return head.val === val ? head.next : head;

};
```



**方法2：迭代**



```typescript
function removeElements(head: ListNode | null, val: number): ListNode | null {
   const preHead = new ListNode(0);
    preHead.next = head;

    let cur = preHead;

    while (cur.next !== null) {
        if (cur.next.val == val) {
            cur.next = cur.next.next;
        } else {
            cur = cur.next;
        }
    }
    return preHead.next;
};
```



---



## day8



1. **反转链表**

> 给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。



![反转链表](https://i.loli.net/2021/08/10/LSzUfeAIQMslZKX.jpg)





```typescript
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```



- 通用的解题思路：链表的题目通常情况下都是用递归去解决，这道题可以用`递归`和`双指针`来解决，其实思路都是一样的 看图 👇🏻



<img src="https://i.loli.net/2021/07/29/rgiR8BPeD5cONu7.gif" alt="反转链表思路" style="zoom:67%;" />



- **方法1：递归**

> 递归理解起来可能有些难理解，其实他和`双指针`的做法是一样的，都是把下一`指针`向后指

```typescript
function reverseList(head: ListNode | null): ListNode | null {
    // 如果当前链表和下一个链表不存在返回当前 head
    if (head == null || head.next == null) {
        return head;
    }
    // 递归
    const newHead = reverseList(head.next);
    // 让链表指针向后指
    head.next.next = head;
    head.next = null;

    return newHead;
};
```





- **方法2：双指针**

> 双指针的代码比较清晰了。
>
> 1. 定义2个指针和寄存器
> 2. 让第一个头部 指向 null，代表最后一个
> 3. 每次移动2个指针的位置
> 4. tmp ：寄存器 储蓄下一个 链表
> 5. 2个链表交换位置

```typescript
function reverseList(head: ListNode | null): ListNode | null {
    if(!head || !head.next) return head;

    // 指针 pre 【上一个】
    let pre = null
    // 定义寄存器
    let tmp = null
    // 指针 cur 【下一个】
    let cur = head

    // 移动指针
    while(cur) {
        tmp = cur.next;
        cur.next = pre;
        pre = cur;
        cur = tmp;
    }
    return pre;
};
```





2. **删除排序链表中的重复元素**

**示例1：**

```
存在一个按升序排列的链表，给你这个链表的头节点 `head` ，请你删除所有重复的元素，使每个元素 **只出现一次** 。

返回同样按升序排列的结果链表。
```

![img](https://i.loli.net/2021/08/10/S5OeloaEzf1XjDw.jpg)



```
输入：head = [1,1,2]
输出：[1,2]
```





**示例2：**

![img](https://i.loli.net/2021/08/10/FizIRkQZP5641lX.jpg)



```
输入：head = [1,1,2,3,3]
输出：[1,2,3]
```





- **方法1：一次遍历**

> 注意📢：链表是`排序`好的!
>
> - 我们只需要遍历一次，不断地和后面的元素做比较，发现相等跳过即可

```typescript
function deleteDuplicates(head: ListNode | null): ListNode | null {
    if (!head) return head

    let item = head
    
    // 因为是升序的我们可以拿当前的链表不断的和下一个比较，发现重复就跳过
    while (item.next) {
        if (item.val === item.next.val) {
            // 重复，跳过
            item.next = item.next.next;
        } else {
            item = item.next
        }
    }

    return head
};
```



---



## day9



1. **有效的括号**

> 给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。
>
> 有效字符串需满足：
>
> 左括号必须用相同类型的右括号闭合。
> 左括号必须以正确的顺序闭合。
>
>
> 示例 1：
>
> 输入：s = "()"
> 输出：true
> 示例 2：
>
> 输入：s = "()[]{}"
> 输出：true
> 示例 3：
>
> 输入：s = "(]"
> 输出：false
> 示例 4：
>
> 输入：s = "([)]"
> 输出：false
> 示例 5：
>
> 输入：s = "{[]}"
> 输出：true



- **方法1：栈匹配法**

> 栈的特性：`先进先出`
>
> 1. 建立一个括号的映射: 括号`左边`  =>  匹配`右边`
> 2. 遍历判断左边进`栈`
> 3. 判断右边 取`栈 `- 判断是否匹配



```typescript
function isValid(s: string): boolean {
    // 奇数直接 返回 false
    if (s.length % 2 !== 0) return false

    // 新建一个栈
    const stack = []
    // 映射 三个括号
    const map = {
        '(':')',
        '[':']',
        '{':'}',
    }

    for (let str of s) {
        // 是左括号的话 就进栈 判断
        if ( str in map ){
            stack.push(str)
            continue;
        }
        // 右括号的话，判断栈的最后一个是否匹配【完整的括号】
        if (map[stack.pop()] !== str) return false
    }

    return !stack.length
};
```





2. **用栈实现队列**



>
>
>你仅使用两个栈实现先入先出队列。队列应当支持一般队列支持的所有操作（push、pop、peek、empty）：
>
>实现 MyQueue 类：
>
>void push(int x) 将元素 x 推到队列的末尾
>int pop() 从队列的开头移除并返回元素
>int peek() 返回队列开头的元素
>boolean empty() 如果队列为空，返回 true ；否则，返回 false
>
>
>说明：
>
>你只能使用标准的栈操作 —— 也就是只有 push to top, peek/pop from top, size, 和 is empty 操作是合法的。
>你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。
>
>
>进阶：
>
>你能否实现每个操作均摊时间复杂度为 O(1) 的队列？换句话说，执行 n 个操作的总时间复杂度为 O(n) ，即使其中一个操作可能花费较长时间。
>
>
>示例：
>
>输入：
>
>```js
>["MyQueue", "push", "push", "peek", "pop", "empty"]
>[[], [1], [2], [], [], []]
>输出：
>[null, null, null, 1, 1, false]
>
>解释：
>MyQueue myQueue = new MyQueue();
>myQueue.push(1); // queue is: [1]
>myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
>myQueue.peek(); // return 1
>myQueue.pop(); // return 1, queue is [2]
>myQueue.empty(); // return false
>```
>
>
>
>
>提示：
>
>1 <= x <= 9
>最多调用 100 次 push、pop、peek 和 empty
>假设所有操作都是有效的 （例如，一个空的队列不会调用 pop 或者 peek 操作





- **方法1：双栈实现**

```typescript
class MyQueue {
    // 进栈
    public stack: number[] = []
    // 出栈
    public stackOut: number[] = []

    // 吧进栈数据倒入到出栈
    inOut (): void {
        while (this.stack.length) {
            this.stackOut.push(this.stack.pop());
        }
    }

    push(x: number): void {
        this.stack.push(x)
    }

    pop(): number {
        if (!this.stackOut.length) {
            this.inOut();
        }
        return this.stackOut.pop();
    }

    peek(): number {
        if (!this.stackOut.length) {
            this.inOut();
        }
        return this.stackOut[this.stackOut.length - 1];
    }

    empty(): boolean {
        return this.stackOut.length === 0 && this.stack.length === 0;
    }
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * var obj = new MyQueue()
 * obj.push(x)
 * var param_2 = obj.pop()
 * var param_3 = obj.peek()
 * var param_4 = obj.empty()
 */
```



