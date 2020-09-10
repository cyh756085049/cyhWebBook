## LeetCode

#### [1. 两数之和](https://leetcode-cn.com/problems/two-sum/)

> 给定一个整数数组 `nums` 和一个目标值 `target`，请你在该数组中找出和为目标值的那 **两个** 整数，并返回他们的数组下标。
>
> 给定 nums = [2, 7, 11, 15], target = 9
>
> 因为 nums[0] + nums[1] = 2 + 7 = 9
> 所以返回 [0, 1]

思路：==哈希==、==数组==

- 求和转换为求差
- 借助Map结构将数组中每个元素及其索引相互对应
- 以空间换时间，将查找时间从O(N)降低到O(1)

代码：

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function(nums, target) {
  const map = new Map();
  for (let i = 0; i < nums.length; i++) {
    const res = target - nums[i];
    if (map.has(res)) {
      return [map.get(res), i];
    }
    map.set(nums[i], i);
  }
};
```

#### [15. 三数之和](https://leetcode-cn.com/problems/3sum/)

> 给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有满足条件且不重复的三元组。
>
> 注意：答案中不可以包含重复的三元组。
>
> 给定数组 nums = [-1, 0, 1, 2, -1, -4]，
>
> 满足要求的三元组集合为：
> [
>   [-1, 0, 1],
>   [-1, -1, 2]
> ]

思路：==排序==、==双指针==、==分治==

首先可以对数组进行排序，然后遍历数组作为外循环中的一个值，然后将问题转化为两个数相加等于给定值。总的时间复杂度为`O(n^2)`。



代码：

```js
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var threeSum = function(nums) {
  // 处理特殊情况，数组长度小于3
  if (nums.lenght < 3) return [];
  const list = [];
  // 对数组进行排序
  nums.sort((a, b) => a - b);
  for (let i = 0; i < nums.length; i++) {
    // 数组已经排序，如果当前数字大于0，则三数之和一定大于0，所以结束循环
    if (nums[i] > 0) break;
    // 去重
    if (i > 0 && nums[i] === nums[i - 1]) continue;
    let left = i + 1;
    let right = nums.length - 1;
    while (left < right) {
      if (nums[left] + nums[right] + nums[i] === 0) {
        list.push([nums[left], nums[right], nums[i]]);
        while (nums[left] === nums[left + 1]) {
          left++;
        }
        left++;
        while (nums[right] === nums[right -1]) {
          right--;
        }
        right--;
        continue;
      }else if (nums[left] + nums[right] + nums[i] > 0) {
        right--;
      } else {
        left++;
      }
    }
  }
  return list;
}
```

#### [18. 四数之和](https://leetcode-cn.com/problems/4sum/)

> 给定一个包含 n 个整数的数组 nums 和一个目标值 target，判断 nums 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 target 相等？找出所有满足条件且不重复的四元组。
>
> *注意*：答案中不可以包含重复的四元组。
>
> 给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。
>
> 满足要求的四元组集合为：
> [
>   [-1,  0, 0, 1],
>   [-2, -1, 1, 2],
>   [-2,  0, 0, 2]
> ]

思路：==排序==、==双指针==、==分治==

同三数之和类似，此题固定两个数，另外两个数同样用移动的双指针寻找，更新比较与 target 的大小，固定的两个数用双循环实现。注意剪枝和去重。

代码：

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[][]}
 */
var fourSum = function(nums, target) {
  let res = [];
  nums.sort((a,b) => a - b);
  let n = nums.length;
  for(let i = 0;i < n - 3;i++){
    if(i > 0 && nums[i] === nums[i-1]) continue;
    if(nums[i] + nums[i+1] + nums[i+2] + nums[i+3] > target) break;
    if(nums[i] + nums[n-1] + nums[n-2] + nums[n-3] < target) continue;
    for(let j = i + 1;j < n - 2;j++){
      if(j - i > 1 && nums[j] === nums[j-1]) continue;
      if(nums[i] + nums[j] + nums[j+1] + nums[j+2] > target) break;
      if(nums[i] + nums[j] + nums[n-1] + nums[n-2] < target) continue;
      let left = j + 1;
      let right = n - 1;
      while(left < right){
        let tmpRes = nums[i] + nums[j] + nums[left] + nums[right];
        if(tmpRes === target){
          res.push([nums[i],nums[j],nums[left],nums[right]]);
          while(left < right && nums[left] === nums[left + 1]) left++;
          while(left < right && nums[right] === nums[right - 1]) right--;
          left++;
          right--;
        }else if(tmpRes > target){
          right--;
        }else{
          left++;
        }
      }
    }
  }
  return res;
};
```

#### [20. 有效的括号](https://leetcode-cn.com/problems/valid-parentheses/)

> 给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。
>
> 有效字符串需满足：
>
> * 左括号必须用相同类型的右括号闭合。
> * 左括号必须以正确的顺序闭合。
> 注意空字符串可被认为是有效字符串。
>
> 示例 1:
>
> ```js
> 输入: "()"
> 输出: true
> ```

思路：**栈**

使用栈，遍历输入的字符串，如果当前字符为左半括号时，则将其压入栈中，如果遇到右半括号，则分类讨论：

1、如果栈不为空且为对应的左半括号，则出栈，继续循环

2、如果此时栈为空，则直接返回false

3、如果不是对应的左半括号，则返回false

```js
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
    const stack = [];
    const arr = {
        "(": ")",
        "{": "}",
        "[": "]"
    };
    for (let i in s) {
        const value = s[i];
        if (["(", "{", "["].indexOf(value) > -1) {
            stack.push(value);
        } else {
            const peak = stack.pop();
            if (value !== arr[peak]) {
                return false;
            }
        }
    }
    if (stack.length > 0) return false;
    return true;
};
```

> 时间复杂度：O(n)，空间复杂度：O(n)

思路2：**正则匹配**

不断通过消除 '[]' ， '()', '{}' ，最后判断剩下的是否是空串即可。

```js
/**
 * @param {string} s
 * @return {boolean}
 */
var isValid = function(s) {
    while (s.includes("[]") || s.includes("()") || s.includes("{}")) {
        s = s.replace("[]", "").replace("()", "").replace("{}", "");
    }
    s = s.replace("[]", "").replace("()", "").replace("{}", "");
    return s.length === 0;
};
```

> 时间和空间复杂度：取决于正则引擎的实现

#### [剑指 Offer 25. 合并两个排序的链表](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/)

> 输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。
>
> 示例1：
>
> ```js
> 输入：1->2->4, 1->3->4
> 输出：1->1->2->3->4->4
> ```

思路：**递归、链表**

使用递归，将两个链表头部较小的一个与剩下的元素合并，并返回排好序的链表头，当两条链表中的一条为空时终止递归。

```js
/**
 * Definition for singly-linked list.
 * function ListNode(val) {
 *     this.val = val;
 *     this.next = null;
 * }
 */
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var mergeTwoLists = function(l1, l2) {
    if (l1 === null) return l2;
    if (l2 === null) return l1;
    if (l1.val < l2.val) {
        l1.next = mergeTwoLists(l1.next, l2);
        return l1;
    }else {
        l2.next = mergeTwoLists(l1, l2.next);
        return l2;
    }
};
```

> 时间复杂度：O(M+N)，空间复杂度：O(M+N)

#### [26. 删除排序数组中的重复项](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-array/)  

> 给定一个排序数组，你需要在 原地 删除重复出现的元素，使得每个元素只出现一次，返回移除后数组的新长度。
>
> 不要使用额外的数组空间，你必须在 原地 修改输入数组 并在使用 O(1) 额外空间的条件下完成。
>
> ```js
> 给定数组 nums = [1,1,2], 
> 函数应该返回新的长度 2, 并且原数组 nums 的前两个元素被修改为 1, 2。 
> 你不需要考虑数组中超出新长度后面的元素。
> ```

思路：**双指针、数组**

使用快慢指针来记录遍历的数组索引。

* 开始时快慢指针都指向第一个数字；

* 如果两个指针指向的数字相同，则快指针向前走一步；
* 如果不同，则两个指针都向前走一步；
* 当快指针走完整个数组后，慢指针当前的索引+1就是数组中不同数字的个数，即移除重复项后数组的新长度。

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var removeDuplicates = function(nums) {
    const size = nums.length;
  	if (size == 0) return 0;
  	let slow = 0;
  	for (let fast = 0; fast < size; fast++) {
      if (nums[fast] !== nums[slow]) {
        slow++;
        nums[slow] = nums[fast];
      }
    }
  return slow + 1;
};
```

#### [剑指 Offer 42. 连续子数组的最大和](https://leetcode-cn.com/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)  （✨）

> 输入一个整型数组，数组中的一个或连续多个整数组成一个子数组。求所有子数组的和的最大值。
>
> 要求时间复杂度为O(n)。
>
> ```js
> 输入: nums = [-2,1,-3,4,-1,2,1,-5,4]
> 输出: 6
> 解释: 连续子数组 [4,-1,2,1] 的和最大，为 6。
> ```

思路1：**前缀和、数组**

首先通过2个循环遍历出所有的子序列的首尾位置，然后计算首尾位置的序列和，用前缀和来优化，定义一个全局变量max，比较每次求解的子序列和。

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function(nums) {
    let sum = 0;
    let max = -Number.MAX_VALUE;
    const len = nums.length;
    for (let i = 0; i < len; i++) {
        sum = 0;
        for (let j = i; j < len; j++) {
            sum += nums[j];
            if (sum > max) {
                max = sum;
            }
        }
    }
    return max;
};
```

> 时间复杂度：O(n^2)，空间复杂度：O(1)

思路2：**动态规划、数组**    (✨)

初始化：`dp[0] = nums[0]`

动态转移方程：`dp[i] = max(dp[i - 1] + nums[i], nums[i])`

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
var maxSubArray = function(nums) {
    let max = nums[0];
    for (let i = 0; i < nums.length; i++) {
			if (nums[i - 1] > 0) {
        nums[i] += nums[i - 1];
      }
      if (nums[i] > max) {
        max = nums[i];
      }
    }
  	return max;
};
```

