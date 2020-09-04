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

思路：

同三叔之和类似，此题固定两个数，另外两个数同样用移动的双指针寻找，更新比较与 target 的大小，固定的两个数用双循环实现。注意剪枝和去重。

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

