# leetcode

[TOC]



## 数据结构

### 217 存在重复元素

先排序，如果有重复元素，则true，反之则false



```c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
    sort(nums.begin(), nums.end());
    int n = nums.size();
    for(int i = 1; i < n; i++){
        if(nums[i-1] == nums[i]){   //i-1在先比i在先 执行用时会短百分之二十，内存消耗不变
            return true;            //可能与体系结构讲的局部性有关。
        }
    }
    return false;
    }
};
```

### 53 最大子序和

> 给你一个整数数组 nums ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。
>
> 子数组 是数组中的一个连续部分。
>
>  
>
> 示例 1：
>
> 输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
> 输出：6
> 解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
> 示例 2：
>
> 输入：nums = [1]
> 输出：1
> 示例 3：
>
> 输入：nums = [5,4,-1,7,8]
> 输出：23
>
>
> 提示：
>
> 1 <= nums.length <= 105
> -104 <= nums[i] <= 104

贪心法：

当前指针所指元素之前的和小于0，则丢弃之前的序列和，反之则加上当前所指元素，继续前进

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int res = nums[0];
        int sum = 0;
        for (int num : nums) {
            if (sum > 0)
                sum += num;
            else
                sum = num;
            res = max(res, sum);   //保存当前最大子序列和。
        }
        return res;
    }
};
```

动态规划：

当前一个元素大于0，则把他加到当前元素上。然后数组元素最大值即为结果

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
    int _maxAns = nums[0];
    for(int i = 1; i < nums.size(); i++){
        if(nums[i-1] > 0) nums[i]+=nums[i-1];
        _maxAns = max(_maxAns, nums[i]);
    }
    return _maxAns;
    }
};
```

### 88 合并两个有序数组

好像培培姐数据结构上讲过😁

给你两个按 非递减顺序 排列的整数数组 nums1 和 nums2，另有两个整数 m 和 n ，分别表示 nums1 和 nums2 中的元素数目。

请你 合并 nums2 到 nums1 中，使合并后的数组同样按 非递减顺序 排列。

注意：最终，合并后数组不应由函数返回，而是存储在数组 nums1 中。为了应对这种情况，nums1 的初始长度为 m + n，其中前 m 个元素表示应合并的元素，后 n 个元素为 0 ，应忽略。nums2 的长度为 n 。

 

示例 1：

输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
解释：需要合并 [1,2,3] 和 [2,5,6] 。
合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。
示例 2：

输入：nums1 = [1], m = 1, nums2 = [], n = 0
输出：[1]
解释：需要合并 [1] 和 [] 。
合并结果是 [1] 。
示例 3：

输入：nums1 = [0], m = 0, nums2 = [1], n = 1
输出：[1]
解释：需要合并的数组是 [] 和 [1] 。
合并结果是 [1] 。
注意，因为 m = 0 ，所以 nums1 中没有元素。nums1 中仅存的 0 仅仅是为了确保合并结果可以顺利存放到 nums1 中。


提示：

nums1.length == m + n
nums2.length == n
0 <= m, n <= 200
1 <= m + n <= 200
-109 <= nums1[i], nums2[j] <= 109

方法一：无脑

直接把数组2的换到数组1里面然后排序

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
    for(int i = 0; i < n; i++){
        nums1[m + i] = nums2[i];
    }
    sort(nums1.begin(), nums1.end());
    }
};
```

方法2： 复杂度m+n

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
    int _point = m-- + n-- - 1;
    while(n >= 0){
        nums1[_point--] = m>=0 && nums1[m] > nums2[n] ? nums1[m--] : nums2[n--];
    }
    }
};
//m和n作为两个数组的尾指针，进行对比，将最大的放于数组1尾部，然后依次比较放入，极端情况是当一个数组最小数比另一个数组最大数大时，到末尾放不进去。所以while条件为 n>=0 下面额外条件时 m>=0 两者位置可调换，不过 ? : 语句也要修改。
```

### 350 两个数组的交集 II

给你两个整数数组 nums1 和 nums2 ，请你以数组形式返回两数组的交集。返回结果中每个元素出现的次数，应与元素在两个数组中都出现的次数一致（如果出现次数不一致，则考虑取较小值）。可以不考虑输出结果的顺序。

 

示例 1：

输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2,2]
示例 2:

输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[4,9]


提示：

1 <= nums1.length, nums2.length <= 1000
0 <= nums1[i], nums2[i] <= 1000


进阶：

如果给定的数组已经排好序呢？你将如何优化你的算法？
如果 nums1 的大小比 nums2 小，哪种方法更优？
如果 nums2 的元素存储在磁盘上，内存是有限的，并且你不能一次加载所有的元素到内存中，你该怎么办？

```c++
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
    vector<int> result;
    sort(nums1.begin(), nums1.end());
    sort(nums2.begin(), nums2.end());
    int _p_nums1 = 0,_p_nums2 = 0;
    while(_p_nums1 < nums1.size() && _p_nums2 < nums2.size()){
        if(nums1[_p_nums1] == nums2[_p_nums2]){
            result.push_back(nums1[_p_nums1]);
            ++_p_nums1;
            ++_p_nums2;
        }
        else nums1[_p_nums1] < nums2[_p_nums2] ? ++_p_nums1 : ++_p_nums2;
    }
    return result;
    }
};
//解法类似于88题，双指针，没必要遍历所有数组元素，只要有一个数组遍历完即可 tips：要排序好
```

```c++
//哈希表解法，使用unordered-map，内部由哈希表实现，先判断谁长，用短的做表，用大的差 哈希表++m[num] 应该有点像自动添加键值对。很实用 最后会erase 会省很多执行时间。

class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
    if(nums1.size() > nums2.size()){
        return intersect(nums2, nums1);
    }
    unordered_map<int, int> m;
    vector<int> _result;
    for(int num : nums1){
        ++m[num];
    }
    for(int num_1 : nums2){
        if(m[num_1]){
            --m[num_1];
            _result.push_back(num_1);
        }
        if (m[num_1] == 0) {
            m.erase(num_1);
            }
    }
    return _result;
    }
};
```

### 



### tips

string是个神奇的东西 sizeof(string) 不管string多长 sizeof(string) = 4; 这是在g++编译器环境下，不同的编译器结果可能不同，但和string长度无关





### 137. 只出现一次的数字

给你一个整数数组 nums ，除某个元素仅出现 一次 外，其余每个元素都恰出现 三次 。请你找出并返回那个只出现了一次的元素。

 

示例 1：

输入：nums = [2,2,3,2]
输出：3
示例 2：

输入：nums = [0,1,0,1,0,1,99]
输出：99


提示：

1 <= nums.length <= 3 * 104
-231 <= nums[i] <= 231 - 1
nums 中，除某个元素仅出现 一次 外，其余每个元素都恰出现 三次


进阶：你的算法应该具有线性时间复杂度。 你可以不使用额外空间来实现吗？



解法一 哈希表硬写 不满足进阶要求

```c++
//哈希表统计出现次数，然后遍历求解找到出现次数为1的元素，然后输出，xu'yo
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        unordered_map<int, int> m;
        for(int num : nums){
            ++m[num];
        }
        for(int num : nums){
            if(m[num] == 1) return num;
        }
        return 0;
    }
};
```

解法二 位运算求解

```c++
// 题目给出每个数字大小不超过32位，即int即可满足，然后每个数字在二进制形式下都有几个1存在，由于除特定元素外，每个元素出现三次，将其二进制形式下每个1累加，最后得到一个32位长度的整数数组，数组元素为3的倍数的，则不是出现一次元素所有的二进制位，不为3的倍数 即将二进制位对应大小加到result上。
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int result = 0;
        for(int i = 0; i < 32; ++i){
            int total = 0;
            for(int num : nums){
                total += ( (num >> i) & 1);
            }
            if(total % 3 == 1){
                result += 1 << i;
            }
        }
        return result;     
    }
};
```





### 560. 和为K的子数组

给你一个整数数组 nums 和一个整数 k ，请你统计并返回该数组中和为 k 的连续子数组的个数。

 

示例 1：

输入：nums = [1,1,1], k = 2
输出：2
示例 2：

输入：nums = [1,2,3], k = 3
输出：2


提示：

1 <= nums.length <= 2 * 104
-1000 <= nums[i] <= 1000
-107 <= k <= 107

```c++

class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int, int> mp;
        int sum = 0;
        int result = 0;
        mp[0] = 1;
        for(int i = 0; i < nums.size(); ++i){
            sum += nums[i];
            if(mp[sum - k] > 0) result += mp[sum - k];
            ++mp[sum];
        }
        return result;
    }
};
```







### 125 检查回文字符串

```c++
char tolower(char c); //如果是大写字母 则改为小写
bool isalnum(char c); //检查是否是数字或者字母 是返回true f
```

 



### 430 扁平化多级链表

![image-20220315001434899](C:\Users\renjunsheng\AppData\Roaming\Typora\typora-user-images\image-20220315001434899.png)

```c++
//主要思想是采用递归，不知道为啥，递归出来效率很高 对于每个节点来说有四种情况，有孩子，无孩子 × 有弟弟，无弟弟
//递归增加了一个参数 die 是上一代留下来的弟弟，要缝合到我这一代最后一个人 也就是没孩子 没弟弟的后面 如果有孩子但是没有弟弟 直接是第一个else if的情况，没有新的die出现，则将上一个die作为参数，处理完直接返回即可 因为该题目一行中最多只有一个人有孩子 其他人都没有 所以只要缝了一个die 就直接返回头节点。 没缝合的话要继续
class Solution {
public:
    Node* flatter(Node* head, Node* die){
        Node* temp = head;
        while(temp){
        if(!(temp -> child) && temp->next == nullptr){
            temp -> next = die;
            if(die){
                die -> prev = temp;
            }
            return head;
        }
        else if(temp -> child && temp -> next == nullptr){
            temp -> next = flatter(temp -> child, die);
            temp -> child -> prev = temp;
            temp -> child = nullptr;
            return head;
        }
        else if(temp -> child && temp -> next){
            Node *will_die = temp -> next;
            temp ->child->prev = temp;
            temp -> next = flatter(temp -> child, will_die);
            temp -> child = nullptr;
            temp = will_die;
        }
        else temp = temp -> next;
         

        }
        return head;
    }
    Node* flatten(Node* head) {
        return flatter(head, nullptr);
    }
};
```





### 146 LRU缓存

```c++
list.splice()
    //有三种形式
void splice ( iterator position, list<T,Allocator>& x );
void splice ( iterator position, list<T,Allocator>& x, iterator it );
void splice ( iterator position, list<T,Allocator>& x, iterator first, iterator last );
/*对于一：会在position后把list&x所有的元素到剪接到要操作的list对象
  对于二：只会把it的值剪接到要操作的list对象中
  对于三：把first 到 last 剪接到要操作的list对象中 
*/

#include<bits/stdc++.h>
using namespace std;
int main()
{
	list<int>li1,li2;
	for(int i=1;i<=4;i++) li1.push_back(i),li2.push_back(i+10);
	// li1 1 2 3 4
	// li2 11 12 13 14
	list<int>::iterator it=li1.begin();
	it++;
	
	
	li1.splice(it,li2);//1 11 12 13 14 2 3 4
	if(li2.empty()) cout<<"li2 is empty"<<endl;
	
	
	li2.splice(li2.begin(),li1,it);
	cout<<*it<<"   chen"<<endl;
	/*
	li1 1 11 12 13 14 3 4
	li2 2
	这里的it的值还是2  但是指向的已经是li2中的了 
	*/
	
	it=li1.begin();
	advance(it,3);//advance 的意思是增加的意思，就是相当于 it=it+3;这里指向13
	li1.splice(li1.begin(),li1,it,li1.end()); //13 14 3 4 1 11 12 可以发现it到li1.end()被剪贴到li1.begin()前面了 
	for(list<int>::iterator it=li1.begin();it!=li1.end();++it) cout<<*it<<"  ";
	cout<<endl;
	for(list<int>::iterator it=li2.begin();it!=li2.end();++it) cout<<*it<<"  ";
	return 0;

```

[详解](https://blog.csdn.net/qjh5606/article/details/85881680)

题目分析，对于c++来说 由于要实现o（1）的时间复杂度，必然用到unordered_map,同时使用list用于作为cache的主体用于删除和更新添加元素。

但是对于list的更新来说 在确定要含有某个元素，但是要更新他的值的话 寻找这个元素的时间复杂度不满足o（1）所以哈希表中键是数据的键 value可以为指向list单元的指针 这样就可以实现o（1）复杂度的寻找元素然后使用list的splice进行拼接。也可以使用erase(itr)进行删除感觉这种会快一点





### 150. 逆波兰表达式求值

感觉不配是中等题，就一个栈就完事，或者数组模拟栈，数组大小取vector长度一半加1就行



### 735 小行星碰撞

多用while 多用while 多用while

别写恶心的代码 if语句条件可以复合 if语句条件可以复合 if语句条件可以复合 
