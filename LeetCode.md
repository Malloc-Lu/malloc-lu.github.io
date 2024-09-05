## 406. 根据身高重建队列

### Introduction: 

假设有打乱顺序的一群人站成一个队列，数组 people 表示队列中一些人的属性（不一定按顺序）。每个 people[i] = [hi, ki] 表示第 i 个人的身高为 hi ，前面 **正好** 有 ki 个身高大于或等于 hi 的人。

请你重新构造并返回输入数组 `people` 所表示的队列。返回的队列应该格式化为数组 queue ，其中 queue[j] = [hj, kj] 是队列中第 j 个人的属性（queue[0] 是排在队列前面的人）。

#### Example 1:

> **输入**：people = [[7,0],[4,4],[7,1],[5,0],[6,1],[5,2]]
> **输出**：[[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]]
> **解释**：
> 编号为 0 的人身高为 5 ，没有身高更高或者相同的人排在他前面。
> 编号为 1 的人身高为 7 ，没有身高更高或者相同的人排在他前面。
> 编号为 2 的人身高为 5 ，有 2 个身高更高或者相同的人排在他前面，即编号为 0 和 1 的人。
> 编号为 3 的人身高为 6 ，有 1 个身高更高或者相同的人排在他前面，即编号为 1 的人。
> 编号为 4 的人身高为 4 ，有 4 个身高更高或者相同的人排在他前面，即编号为 0、1、2、3 的人。
> 编号为 5 的人身高为 7 ，有 1 个身高更高或者相同的人排在他前面，即编号为 1 的人。
> 因此 [[5,0],[7,0],[5,2],[6,1],[4,4],[7,1]] 是重新构造后的队列。

#### Example 2:

> 输入：people = [[6,0],[5,0],[4,0],[3,2],[2,2],[1,4]]
> 输出：[[4,0],[5,0],[2,2],[3,2],[1,4],[6,0]]



### Solution

**minds**

- 首先将`ki`相同的人放入相同的数组中，并进行排序。

- 按照`ki`从小到大的顺序，逐个将其插入第一个数组。

  插入时需要注意，源数组需按照`hi`***降序***来进行插入。

### code

**`list<int>`**

```cpp
// list<int> 解决办法
// 注意，list中没有迭代器和整形相加的函数。
class Solution {
public:
    static bool com(vector<int>& a, vector<int>& b){
        if(a[0] > b[0]){
            return true;
        }
        if(a[0] == b[0]){
            return a[1] < b[1];
        }
        else{
            return false;
        }
    }
    // static bool insert_com(vector<int>& a, vector<int>& b){

    // }
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        //将队列按照hi进行倒序排列
        sort(people.begin(), people.end(), com);
        //将排序过后的数组，按照ki升序再插入到新的数组中
        list<vector<int>> result;
        for(auto i = people.begin(); i != people.end(); ++i){
            if(result.empty()){
                result.push_back(*i);
                continue;
            }
            if((*i)[1] < result.size()){
                int pos = (*i)[1];
                auto h = result.begin();
                while(pos){
                    ++h;
                    --pos;
                }
                result.emplace(h, (*i));
            }
            else{
                result.emplace(result.end(), (*i));
            }
        }
        return vector<vector<int>>(result.begin(), result.end());
    }
       
};
```

**`vector<int>`**

```cpp
// vector<int> 解决办法
class Solution {
public:
    static bool com(vector<int>& a, vector<int>& b){
        if(a[0] > b[0]){
            return true;
        }
        if(a[0] == b[0]){
            return a[1] < b[1];
        }
        else{
            return false;
        }
    }

    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        //将队列按照hi进行倒序排列
        sort(people.begin(), people.end(), com);
        //将排序过后的数组，按照ki升序再插入到新的数组中
        vector<vector<int>> result;
        for(auto i = people.begin(); i != people.end(); ++i){
            if(result.empty()){
                result.push_back(*i);
                continue;
            }
            if((*i)[1] < result.size()){
                result.emplace(result.begin() + (*i)[1], (*i));
            }
            else{
                result.emplace(result.end(), (*i));
            }
        }
        return result;
    }
};
```

## 665. 非递减数列

### Introduction:

给你一个长度为 n 的整数数组` nums `，请你判断在 最多 改变 1 个元素的情况下，该数组能否变成一个非递减数列。

我们是这样定义一个非递减数列的： 对于数组中任意的 i (0 <= i <= n-2)，总满足 nums[i] <= nums[i + 1]。

#### example1:

```
输入: nums = [4,2,3]
输出: true
解释: 你可以通过把第一个 4 变成 1 来使得它成为一个非递减数列。
```

#### example2:

```
输入: nums = [4,2,1]
输出: false
解释: 你不能在只改变一个元素的情况下将其变为非递减数列。
```

**minds**

>- 以三个为一组，若`nums[i-1] > nums[i]`则进行判断
>  - `i-2 >= 0`, if `nums[i-2] > nums[i]`, then `nums[i] = nums[i-1]`; if `nums[i-2] <= nums[i]`, then `nums[i-1] = nums[i]`.
>  - `i-2 < 0`, `nums[i-1] = nums[i]`.

## TOP100

### 3.无重复字符的最长子串

给定一个字符串 `s` ，请你找出其中不含有重复字符的 **最长子串** 的长度。

**示例 1:**

```
输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

**示例 2:**

```
输入: s = "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。
```

**示例 3:**

```
输入: s = "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
     请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串。
```

### 206.[反转链表](https://leetcode.cn/problems/reverse-linked-list/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (73.97%) | 3520  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/linked-list" title="https://leetcode.com/tag/linked-list" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你单链表的头节点 `head` ，请你反转链表，并返回反转后的链表。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex1.jpg)

```
输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev1ex2.jpg)

```
输入：head = [1,2]
输出：[2,1]
```

**示例 3：**

```
输入：head = []
输出：[]
```

**My solution**

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* pnext;
        while(head) {
            pnext = head->next;
            head->next = prev;
            prev = head;
            head = pnext;
        }
        return prev;
    }
};
```

做链表题时，不要想节点本身，要想空的地方。每个节点都由三个部分构成：*前世*，*今生*，*后世*。弄清楚每个节点的三部分分别是什么。

本题应理解为修改节点的*前世今生*，而不是去想如何逆转链表。

### [LRU 缓存](https://leetcode.cn/problems/lru-cache/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (53.69%) | 3100  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/design" title="https://leetcode.com/tag/design" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

请你设计并实现一个满足 [LRU (最近最少使用) 缓存](https://baike.baidu.com/item/LRU) 约束的数据结构。

实现 `LRUCache` 类：

- `LRUCache(int capacity)` 以 **正整数** 作为容量 `capacity` 初始化 LRU 缓存
- `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `-1` 。
- `void put(int key, int value)` 如果关键字 `key` 已经存在，则变更其数据值 `value` ；如果不存在，则向缓存中插入该组 `key-value` 。如果插入操作导致关键字数量超过 `capacity` ，则应该 **逐出** 最久未使用的关键字。

函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行。

 

**示例：**

```
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4
```

**My solution**

```go
class LRUCache {
public:
    LRUCache(int capacity) {
        m_capacity = capacity;
    }
    
    int get(int key) {
        if (frequency.count(key)){
            // * 将刚刚访问的元素放到尾部
            cache.splice(cache.begin(), cache, frequency[key]);
            // cache.splice(cache.end(), cache, frequency[key]);
            // return cache.back().second;
            return cache.front().second;
        } else {
            return -1;
        }
    }
    
    void put(int key, int value) {
        if (frequency.count(key)) {
            // cache.splice(cache.end(), cache, frequency[key]);
            cache.splice(cache.begin(), cache, frequency[key]);
            // cache.back().second = value;
            cache.front().second = value;
        } else {
            // * insert a new element in cache.end()
            // cache.emplace_back(std::pair<int, int>{key, value});
            cache.emplace_front(std::pair<int, int>(key, value));
            // * add new element in `frequency`
            // frequency[key] = cache.end()--;
            frequency[key] = cache.begin();
        }
        if (cache.size() > m_capacity) {
            // frequency.erase(cache.front().first);
            frequency.erase(cache.back().first);
            // cache.pop_front();
            cache.pop_back();
        }
    }
private:
    int m_capacity;
    std::list<std::pair<int, int>> cache;
    std::map<int, std::list<std::pair<int, int>>::iterator> frequency;      // ! here what to be mapped is ITERATOR
};
```

构造两个数据结构

* `std::list<std::pair<int, int>>`，用于存储节点
* `std::map<int, std::list<std::pair<int, int>>::iterator>`，用于映射`key`和存储器的`迭代器`,这样在查询特定`key`是否在存储器中，可以大幅降低搜索时间

另外，如果把*最近最常使用*的节点放在存储器的末尾，无法通过测试；若把其放在最前面，则可以通过测试。

### [数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (61.99%) | 2423  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/divide-and-conquer" title="https://leetcode.com/tag/divide-and-conquer" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/heap" title="https://leetcode.com/tag/heap" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定整数数组 `nums` 和整数 `k`，请返回数组中第 `**k**` 个最大的元素。

请注意，你需要找的是数组排序后的第 `k` 个最大的元素，而不是第 `k` 个不同的元素。

你必须设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

 

**示例 1:**

```
输入: [3,2,1,5,6,4], k = 2
输出: 5
```

**示例 2:**

```
输入: [3,2,3,1,2,4,5,5,6], k = 4
输出: 4
```

**My solution**

本题可以理解为找到升序排序后第`n-k`个元素，而快排每一次都能确定一个元素（枢纽元素）的最终位置，故使用**快排**。

```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        int len = nums.size();
        int target = len - k;

        int left = 0, right = len - 1;

        while(left <= right) {
            int pivotIndex = partiton(nums, left, right);
            if (pivotIndex == target) {
                return nums[pivotIndex];
            } else if (pivotIndex < target) {
                left = pivotIndex + 1;
            } else {
                right = pivotIndex - 1;
            }
        }
        return left;
    }

    void swap (std::vector<int>& nums, int i, int j){
        // * exchange value of position i and position j
        if (i != j) {
            nums[i] = nums[i] + nums[j];
            nums[j] = nums[i] - nums[j];
            nums[i] = nums[i] - nums[j];
    }
    }

    int partiton(std::vector<int>& nums, int left, int right) {
        // std::minstd_rand0 e;
        // int pivotRandom = left + e() % (right - left);
        // swap(nums, left, pivotRandom); 
        int pivotRandom = (left + right) / 2;   
        swap(nums, left, pivotRandom);
        int pivot = nums[left];
        while (left < right) {
            while(nums[right] >= pivot && left < right){
                --right;
            }
            nums[left] = nums[right];
            while(nums[left] <= pivot && left < right) {
                ++left;
            }
            nums[right] = nums[left];
        }
        nums[left] = pivot;
        return left;
    }
};
```

这里使用了**快排**的思想，⚠️快排在选择枢纽元素的时候，选择**中位数**，也就是`(left + right) / 2`，运行速度会快很多。

在`partiton()`函数中，对数组元素和枢纽元素进行大小判断时，应该加上等号，即`while(nums[right] >= pivot && left < right)`。

### 15.[三数之和](https://leetcode.cn/problems/3sum/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (37.80%) | 6837  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/two-pointers" title="https://leetcode.com/tag/two-pointers" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数数组 `nums` ，判断是否存在三元组 `[nums[i], nums[j], nums[k]]` 满足 `i != j`、`i != k` 且 `j != k` ，同时还满足 `nums[i] + nums[j] + nums[k] == 0` 。请

你返回所有和为 `0` 且不重复的三元组。

**注意：**答案中不可以包含重复的三元组。

 

 

**示例 1：**

```
输入：nums = [-1,0,1,2,-1,-4]
输出：[[-1,-1,2],[-1,0,1]]
解释：
nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0 。
nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0 。
nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0 。
不同的三元组是 [-1,0,1] 和 [-1,-1,2] 。
注意，输出的顺序和三元组的顺序并不重要。
```

**示例 2：**

```
输入：nums = [0,1,1]
输出：[]
解释：唯一可能的三元组和不为 0 。
```

**示例 3：**

```
输入：nums = [0,0,0]
输出：[[0,0,0]]
解释：唯一可能的三元组和为 0 。
```

**My solution**

一开始想到的是用*回溯法*，但*回溯法*会超时。

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<int> path;
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        backtrace(nums, res, path);

        return res;
    }
  	void backtrace(vector<int> nums, vector<vector<int>>& res, vector<int>& path) {
        if (path.size() == 3 && accumulate(path.begin(), path.end(), 0) != 0) {
            return;
        }
        if (path.size() == 3 && accumulate(path.begin(), path.end(), 0) == 0) {
            res.push_back(path);
        }
        if (path.size() > 3) {
            return;
        }
        if (nums.empty()) {
            return;
        }
        unordered_map<int, bool> used;
        while (!nums.empty()) {
            if (used.count(nums[0]) != 0) {
                nums.erase(nums.begin());
                continue;
            }
            used[nums[0]] = true;
            path.push_back(nums[0]);
            nums.erase(nums.begin());
            backtrace(nums, res, path);
            path.pop_back();
        }
    }
};
```

看了题解才知道，还能用*双指针*法

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<int> path;
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        // backtrace(nums, res, path);

        //return res;

        // * double pointer solution
        if (nums.size() < 3) {
            return res;
        }
        int i = 0, j = 0, k = 0;
        for(;k < nums.size();++k) {
            // while (k > 0 && k < nums.size() && nums[k] == nums[k-1]) {
            //     ++k;
            // }
            // if (k >= nums.size()-2 || nums[k] > 0) {
            //     return res;
            // }
            if (k > 0 && nums[k] == nums[k-1]) {
                continue;
            }
            if (nums[k] > 0) {
                break;;
            }
            i = k+1;
            j = nums.size()-1;
            while (i < j) {
                int sum = nums[i] + nums[j] + nums[k];
                if (sum == 0) {
                    res.push_back(vector<int>{nums[i], nums[j], nums[k]});
                    ++i;
                    --j;
                    while(i < nums.size() && nums[i] == nums[i-1]) {
                        ++i;
                    } 
                    while(-1 < j && nums[j] == nums[j+1]) {
                        --j;
                    } 
                } else if (sum < 0) {
                    ++i;
                    while(i < nums.size() && nums[i] == nums[i-1]) ++i;
                } else if (sum > 0) {
                    --j;
                    while(-1 < j && nums[j] == nums[j+1]) --j;
                }
            }
        }
        return res;
    }
}
```

使用双指针法有一个点需要注意：对于每次遍历的三个数字，**需要考虑三种情况**

- `sum < 0`

  此时需要增大sum的值，所以要递增i的值，同时跳过相同的值

- `sum > 0`

  此时需要减小sum的值，所以要递减i的值，同时跳过相同的值

- `sum = 0`

  此时**既要**递增i的值，**也要**递减j的值，同时二者都要跳过相同的值

### 5.[最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (38.17%) | 7176  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/string" title="https://leetcode.com/tag/string" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个字符串 `s`，找到 `s` 中最长的回文子串。

如果字符串的反序与原始字符串相同，则该字符串称为回文字符串。

 

**示例 1：**

```
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
```

**示例 2：**

```
输入：s = "cbbd"
输出："bb"
```

**My solution**

本题最开始想用的是回溯法，果不其然，*回溯法*会超时，接着想使用*动态规划*法，但想不出来。最后看题解，使用的所谓*中心扩散法*

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        string path("");
        int res = 0;
        // backtrace(s, path, res);
        return centrediffusion(s); 
    }

    void backtrace(string s, string& path, int& res) {
        if (s.length() <= 0) {
            return;
        }
        for (int i = 0;i < s.length();++i) {
            string t = s.substr(0, i+1);
            if (isback(t) && t.length() > path.length()) {
                path = t;
            }
            backtrace(s.substr(i+1), path, res);
        }
    }
    bool isback(string t) {
        int n = t.length();
        for(int i = 0, j = n-1;i <= j;++i, --j) {
            if (t[i] != t[j]) {
                return false;
            }
        }
        return true;
    }

    string centrediffusion(string s) {
        int n = s.length();
        int start = 0, end = 0;
        for (int i = 0;i < n;++i) {
            pair<int, int> res1 = computeback(s, i, i);
            pair<int, int> res2 = computeback(s, i, i+1);
            if (res1.second - res1.first > end - start) {
                start = res1.first;
                end = res1.second;
            }
            if (res2.second - res2.first > end - start) {
                start = res2.first;
                end = res2.second;
            }
        //     if (start1 - end1 > start - end) {
        //         start = start1;
        //         end = end1;
        //     }
        //     if (start2 - end2 > start - end) {
        //         start = start2;
        //         end = end2;
        //     }
        }
        return s.substr(start, end - start +1);
    }

    pair<int, int> computeback(string s, int left, int right) {
        while (left >= 0 && right < s.length() && s[left] == s[right]) {
            --left;
            ++right;
        }
        return {left+1, right-1};
    }
};
```

需要⚠️的是：

* 边界情况

  就是最小的回文子串。可以怎么理解，给你一个回文子串，不断的去除*首字符*和*尾字符*，那么最后剩下的**要么是单个字符，要么是两个字符**

  所以在从最短的回文子串扩散的时候，也应该从**单个字符或两字符**扩散

* `substr`用法

  `substr()`的用法只有`substr(startindex, count)`没有别的用法

### 121.[买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (57.73%) | 3474  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个数组 `prices` ，它的第 `i` 个元素 `prices[i]` 表示一支给定股票第 `i` 天的价格。

你只能选择 **某一天** 买入这只股票，并选择在 **未来的某一个不同的日子** 卖出该股票。设计一个算法来计算你所能获取的最大利润。

返回你可以从这笔交易中获取的最大利润。如果你不能获取任何利润，返回 `0` 。

 

**示例 1：**

```
输入：[7,1,5,3,6,4]
输出：5
解释：在第 2 天（股票价格 = 1）的时候买入，在第 5 天（股票价格 = 6）的时候卖出，最大利润 = 6-1 = 5 。
     注意利润不能是 7-1 = 6, 因为卖出价格需要大于买入价格；同时，你不能在买入前卖出股票。
```

**示例 2：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 没有交易完成, 所以最大利润为 0。
```

**My solution**

做这题时，没有看清楚题目，题目要求股票只能买卖一次，所以只要找到股票价格的最低点和最高点即可。

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int low = __INT_MAX__;
        int high = 0;
        int profit = 0;
        int n = prices.size();
        for (int i = 0;i < n;++i) {
            low = min(low, prices[i]);
            profit = max(profit, prices[i] - low);
            
        }
        return profit;
    }
};
```

上述代码只需要*不断更新最低点价格*即可。

另外，本题可以用**动态规划**

*动态规划五步曲*

* 确定dp数组含义

  这里dp数组大小为`dp[n][2]`

  `dp[i][0]`表示第i天持有股票时，手中的**最大**现金；`dp[i][1]`表示第i天不持有股票时，手中的**最大**现金。

* 确定递推公式

  `dp[i][0] = max(dp[i-1][0], -price[i])`

  `dp[i][1] = max(dp[i-1][1], dp[i-1][0] + price[i])`

* 初始化dp数组

  从上面的递推公式可以看到，`dp[i][0]`, `dp[i][1]`需要从`dp[0][0]`和`dp[0][1]`中推出。

  `dp[0][0] = -price[i]`, `dp[0][1] = 0`

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        // * dynamic programming solution
        int n = prices.size();
        vector<vector<int>> dp(n, vector<int>(2, 0));
        
        // * initiate dp array
        dp[0][0] = -prices[0];
        dp[0][1] = 0;

        for (int i = 1;i < n;++i) {
            dp[i][0] = max(dp[i-1][0], -prices[i]);
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] + prices[i]);
        }

        return dp[n-1][1];
    }
};
```

### 88.[合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (53.49%) | 2411  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/two-pointers" title="https://leetcode.com/tag/two-pointers" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你两个按 **非递减顺序** 排列的整数数组 `nums1` 和 `nums2`，另有两个整数 `m` 和 `n` ，分别表示 `nums1` 和 `nums2` 中的元素数目。

请你 **合并** `nums2` 到 `nums1` 中，使合并后的数组同样按 **非递减顺序** 排列。

**注意：**最终，合并后数组不应由函数返回，而是存储在数组 `nums1` 中。为了应对这种情况，`nums1` 的初始长度为 `m + n`，其中前 `m` 个元素表示应合并的元素，后 `n` 个元素为 `0` ，应忽略。`nums2` 的长度为 `n` 。

 

**示例 1：**

```
输入：nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
输出：[1,2,2,3,5,6]
解释：需要合并 [1,2,3] 和 [2,5,6] 。
合并结果是 [1,2,2,3,5,6] ，其中斜体加粗标注的为 nums1 中的元素。
```

**示例 2：**

```
输入：nums1 = [1], m = 1, nums2 = [], n = 0
输出：[1]
解释：需要合并 [1] 和 [] 。
合并结果是 [1] 。
```

**示例 3：**

```
输入：nums1 = [0], m = 0, nums2 = [1], n = 1
输出：[1]
解释：需要合并的数组是 [] 和 [1] 。
合并结果是 [1] 。
注意，因为 m = 0 ，所以 nums1 中没有元素。nums1 中仅存的 0 仅仅是为了确保合并结果可以顺利存放到 nums1 中。
```

**My solution**

这题刚看第一眼觉得应该先确定数组末端，再看一眼又觉得不行，准备看题解。

结果瞄一眼看到有个题解就是先确定数组末端，于是立马开始自己写。

```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int i = m-1;
        int j = n-1;
        int k = m+n-1;
        while (i >= 0 && j >= 0) {
            if (nums2[j] >= nums1[i]) {
                nums1[k--] = nums2[j];
                j--;
            } else if (nums2[j] < nums1[i]) {
                nums1[k--] = nums1[i];
                i--;
            }
        }
        while (i >= 0) nums1[k--] = nums1[i--];
        while (j >= 0) nums1[k--] = nums2[j--];

    }
};
```

### 46.[全排列](https://leetcode.cn/problems/permutations/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (79.16%) | 2875  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/backtracking" title="https://leetcode.com/tag/backtracking" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个不含重复数字的数组 `nums` ，返回其 *所有可能的全排列* 。你可以 **按任意顺序** 返回答案。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**示例 2：**

```
输入：nums = [0,1]
输出：[[0,1],[1,0]]
```

**示例 3：**

```
输入：nums = [1]
输出：[[1]]
```

**My solution**

这题使用回溯法

因为是排列，每个元素用到不止一次，需要加入一个`unordered_map<int, bool> used`记录元素的使用情况

**卡了我很久**，因为我没有想到*回溯的时候，同时将`used`重置

```cpp
class Solution {
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<int> path;
        vector<vector<int>> res;
        unordered_map<int, bool> used;

        backtrace(nums, path, res, used);
        return res;
    }

    void backtrace(vector<int>& nums, vector<int>& path, vector<vector<int>>& res, unordered_map<int, bool>& used) {
        if (path.size() == nums.size()) {
            res.push_back(path);
            return;
        }
        for (int i = 0;i < nums.size();++i) {
            if (used.count(i) != 0 && used[i] == true) {
                continue;
            }
            used[i] = true;
            path.push_back(nums[i]);
            backtrace(nums, path, res, used);
            path.pop_back();
            used[i] = false;
        }
    }
};
```





### 236.[二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (71.08%) | 2676  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

[百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出：3
解释：节点 5 和节点 1 的最近公共祖先是节点 3 。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出：5
解释：节点 5 和节点 4 的最近公共祖先是节点 5 。因为根据定义最近公共祖先节点可以为节点本身。
```

**示例 3：**

```
输入：root = [1,2], p = 1, q = 2
输出：1
```

**My solution**

首先确定一下，这题使用的是后序遍历.

对后序遍历的节点处理，如果遍历的节点是要查找的节点或空节点就直接返回。

```cpp
if (root == p || root == q || root == nullptr) return root;
```

如果不是则*递归遍历*

分成三种情况

* 如果返回的左右子树中有一个不是空节点，那么说明对应子树中有*要寻找的节点*或*最近公共父节点*
* 如果左右子树返回的都不是空节点，就说明该`root`就是*最近公共父节点*

```cpp
if (left != nullptr && right == nullptr) return left;
if (left == nullptr && right != nullptr) return right;
if (left != nullptr && right != nullptr) return root;
```

```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (root == p || root == q || root == nullptr) return root;
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        if (left != nullptr && right == nullptr) return left;
        if (left == nullptr && right != nullptr) return right;
        if (left != nullptr && right != nullptr) return root;
        return nullptr;
    }
```

### 103.[二叉树的锯齿形层序遍历](https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (58.86%) |  882  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/stack" title="https://leetcode.com/tag/stack" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/breadth-first-search" title="https://leetcode.com/tag/breadth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你二叉树的根节点 `root` ，返回其节点值的 **锯齿形层序遍历** 。（即先从左往右，再从右往左进行下一层遍历，以此类推，层与层之间交替进行）。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：[[3],[20,9],[15,7]]
```

**示例 2：**

```
输入：root = [1]
输出：[[1]]
```

**示例 3：**

```
输入：root = []
输出：[]
```

**My solution**

比较简单的做法是将`queue`直接做*reverse*操作

这里我用了两个栈

```cpp
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        vector<vector<int>> res;
        if (root == nullptr) return res;

        queue<TreeNode*> que;
        stack<TreeNode*> st1;
        stack<TreeNode*> st2;
        st1.push(root);
        int depth = -1;
        while(!st1.empty() || !st2.empty()) {
            depth++;
            vector<int> vec;
            if (!st1.empty()) {
                int size = st1.size();
                for (int i = 0;i < size;++i) {
                    TreeNode* cur = st1.top();
                    st1.pop();
                    vec.push_back(cur->val);
                    if (depth % 2 == 0) {
                        if (cur->left) st2.push(cur->left);
                        if (cur->right) st2.push(cur->right);
                    } else {
                        if (cur->right) st2.push(cur->right);
                        if (cur->left) st2.push(cur->left);
                    }
                }
            } else if (!st2.empty()) {
                int size = st2.size();
                for (int i = 0;i < size;++i) {
                    TreeNode* cur = st2.top();
                    st2.pop();
                    vec.push_back(cur->val);
                    if (depth % 2 == 0) {
                        if (cur->left) st1.push(cur->left);
                        if (cur->right) st1.push(cur->right);
                    } else {
                        if (cur->right) st1.push(cur->right);
                        if (cur->left) st1.push(cur->left);
                    }
                }
            }
            res.push_back(vec);
        }
        return res;
    }
```

对于*子节点的入栈顺序*我使用的`depth`变量来记录，判断本层高度是奇数还是偶数来确定*入栈顺序*

### 92.[反转链表 II](https://leetcode.cn/problems/reverse-linked-list-ii/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (56.14%) | 1775  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/linked-list" title="https://leetcode.com/tag/linked-list" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你单链表的头指针 `head` 和两个整数 `left` 和 `right` ，其中 `left <= right` 。请你反转从位置 `left` 到位置 `right` 的链表节点，返回 **反转后的链表** 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/rev2ex2.jpg)

```
输入：head = [1,2,3,4,5], left = 2, right = 4
输出：[1,4,3,2,5]
```

**示例 2：**

```
输入：head = [5], left = 1, right = 1
输出：[5]
```

**My solution**

首先本题的*关键代码*是链表原地掉头的代码

```cpp
void reverse(ListNode* begin, ListNode* end) {
        ListNode* pre = nullptr;
        ListNode* cur = begin;
        ListNode* last = end->next;
        while(cur != last) {
            ListNode* next = cur->next;
            cur->next = pre;
            pre = cur;
            cur = next;
        }
    }
```

找到需要掉头的链表区间后，还需要**重新连接**翻转完成的链表。所以，需要在进入翻转函数之前使用`ListNode* hair`来记录`head`之前的链表。该*变量*需要在堆上分配内存。

*完整代码*

```cpp
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        // ! LeetCode 这里要在堆上分配内存
        ListNode* hair = (ListNode*)malloc(sizeof(ListNode));
        hair->next = head;
        ListNode* pre = hair;
        ListNode* cur = head;
        ListNode* tail = head;
        for(int i = 0;i < left - 1;++i) {
            pre = cur;
            cur = cur->next;
        }
        for (int i = 0;i < right - 1;++i) {
            tail = tail->next;
        }
        // ! record the next pointer of tail as address of tail will change
        ListNode* tailnext = tail->next;
        reverse(cur, tail);
        pre->next = tail;
        cur->next = tailnext;
        return hair->next;
    }

    void reverse(ListNode* begin, ListNode* end) {
        ListNode* pre = nullptr;
        ListNode* cur = begin;
        ListNode* last = end->next;
        while(cur != last) {
            ListNode* next = cur->next;
            cur->next = pre;
            pre = cur;
            cur = next;
        }
    }
};
```

### 54.[螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (50.72%) | 1670  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个 `m` 行 `n` 列的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/13/spiral1.jpg)

```
输入：matrix = [[1,2,3],[4,5,6],[7,8,9]]
输出：[1,2,3,6,9,8,7,4,5]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/11/13/spiral.jpg)

```
输入：matrix = [[1,2,3,4],[5,6,7,8],[9,10,11,12]]
输出：[1,2,3,4,8,12,11,10,9,5,6,7]
```

**My solution**

本题说要螺旋遍历矩阵，其实就要要规定矩阵遍历顺序：右、下、左、上。遍历顺序循环。

```cpp
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        int dir[4][2] = {0, 1, 1, 0, 0, -1, -1, 0};
        queue<pair<int, int>> que;
        que.push({0, 0});
        int m = matrix.size();
        int n = matrix[0].size();
        vector<vector<bool>> visited(m, vector<bool>(n, false));
        vector<int> res;

        int dir_index = 0;

        while(!que.empty()) {
            int x = que.front().first;
            int y = que.front().second;
            visited[x][y] = true;
            que.pop();
            res.push_back(matrix[x][y]);
            for (int i = 0;i < 5;++i) {
                int nextx = x + dir[dir_index][0];
                int nexty = y + dir[dir_index][1];
                if (nextx < 0 || nextx >= m || nexty < 0 || nexty >= n || visited[nextx][nexty]) {
                    dir_index = (dir_index + 1) % 4;
                    continue;
                }
                if (i == 4) {
                    return res;
                }
                if (!visited[nextx][nexty]) {
                    que.push({nextx, nexty});
                    break;
                }
            }
        }

        return res;
    }
};
```

### 23.[合并 K 个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Hard (59.46%) | 2811  |    -     |

<details style="color: rgb(204, 204, 204); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/linked-list" title="https://leetcode.com/tag/linked-list" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/divide-and-conquer" title="https://leetcode.com/tag/divide-and-conquer" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/heap" title="https://leetcode.com/tag/heap" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(204, 204, 204); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

 

**示例 1：**

```
输入：lists = [[1,4,5],[1,3,4],[2,6]]
输出：[1,1,2,3,4,4,5,6]
解释：链表数组如下：
[
  1->4->5,
  1->3->4,
  2->6
]
将它们合并到一个有序链表中得到。
1->1->2->3->4->4->5->6
```

**示例 2：**

```
输入：lists = []
输出：[]
```

**示例 3：**

```
输入：lists = [[]]
输出：[]
```

**My solution**

使用一个`ListNode* mini`，每次遍历`lists`记录此次遍历的最小`ListNode*`，把它插入到最终输出的列表。

⚠️

可能出现环状链表现象

> 内存无法访问问题
>
> 使用`malloc`动态分配内存

> 当`lists`中出现*空指针*，将其移除之后，记得**重新遍历`lists`**

```cpp
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        int n = lists.size();
        if (lists.size() == 0) {
            return nullptr;
        }
        ListNode* hair = (ListNode*)malloc(sizeof(ListNode));
        ListNode* head = (ListNode*)malloc(sizeof(ListNode));
        head->val = INTMAX_MAX;
        head->next = nullptr;

        ListNode* mini = (ListNode*)malloc(sizeof(ListNode));
        mini->val = INTMAX_MAX;
        int mini_index;
        hair->next = head;

        bool deleted = false;
        while(lists.size() > 1) {
            mini = lists[0];
            mini_index = 0;
            for (int i = 0;i < lists.size();++i) {
                if (lists[i] != nullptr && lists[i]->val <= mini->val) {
                    mini = lists[i];
                    mini_index = i;
                }
                if (nullptr == lists[i]) {
                    lists.erase(lists.begin() + i);
                    deleted = true;
                    break;
                }
            }
            if (deleted) {
                deleted = false;
                continue;
            }
            lists[mini_index] = lists[mini_index]->next;
            head->next = mini;
            head = head->next;
        }
        head->next = lists[0];
        return hair->next->next;

    }
};
```

### 160.[相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (64.86%) | 2444  |    -     |

<details style="color: rgb(59, 59, 59); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/linked-list" title="https://leetcode.com/tag/linked-list" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(59, 59, 59); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你两个单链表的头节点 `headA` 和 `headB` ，请你找出并返回两个单链表相交的起始节点。如果两个链表不存在相交节点，返回 `null` 。

图示两个链表在节点 `c1` 开始相交**：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_statement.png)

题目数据 **保证** 整个链式结构中不存在环。

**注意**，函数返回结果后，链表必须 **保持其原始结构** 。

**自定义评测：**

**评测系统** 的输入如下（你设计的程序 **不适用** 此输入）：

- `intersectVal` - 相交的起始节点的值。如果不存在相交节点，这一值为 `0`
- `listA` - 第一个链表
- `listB` - 第二个链表
- `skipA` - 在 `listA` 中（从头节点开始）跳到交叉节点的节点数
- `skipB` - 在 `listB` 中（从头节点开始）跳到交叉节点的节点数

评测系统将根据这些输入创建链式数据结构，并将两个头节点 `headA` 和 `headB` 传递给你的程序。如果程序能够正确返回相交节点，那么你的解决方案将被 **视作正确答案** 。

 

**示例 1：**

[![img](https://assets.leetcode.com/uploads/2021/03/05/160_example_1_1.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_1.png)

```
输入：intersectVal = 8, listA = [4,1,8,4,5], listB = [5,6,1,8,4,5], skipA = 2, skipB = 3
输出：Intersected at '8'
解释：相交节点的值为 8 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [4,1,8,4,5]，链表 B 为 [5,6,1,8,4,5]。
在 A 中，相交节点前有 2 个节点；在 B 中，相交节点前有 3 个节点。
— 请注意相交节点的值不为 1，因为在链表 A 和链表 B 之中值为 1 的节点 (A 中第二个节点和 B 中第三个节点) 是不同的节点。换句话说，它们在内存中指向两个不同的位置，而链表 A 和链表 B 中值为 8 的节点 (A 中第三个节点，B 中第四个节点) 在内存中指向相同的位置。
```

 

**示例 2：**

[![img](https://assets.leetcode.com/uploads/2021/03/05/160_example_2.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_2.png)

```
输入：intersectVal = 2, listA = [1,9,1,2,4], listB = [3,2,4], skipA = 3, skipB = 1
输出：Intersected at '2'
解释：相交节点的值为 2 （注意，如果两个链表相交则不能为 0）。
从各自的表头开始算起，链表 A 为 [1,9,1,2,4]，链表 B 为 [3,2,4]。
在 A 中，相交节点前有 3 个节点；在 B 中，相交节点前有 1 个节点。
```

**示例 3：**

[![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/14/160_example_3.png)](https://assets.leetcode.com/uploads/2018/12/13/160_example_3.png)

```
输入：intersectVal = 0, listA = [2,6,4], listB = [1,5], skipA = 3, skipB = 2
输出：null
解释：从各自的表头开始算起，链表 A 为 [2,6,4]，链表 B 为 [1,5]。
由于这两个链表不相交，所以 intersectVal 必须为 0，而 skipA 和 skipB 可以是任意值。
这两个链表不相交，因此返回 null 。
```

**My solution**

这里我的想法是用`map`数据结构，存储其中一个链表的所有指针。接着遍历第二个链表指针，如果`map`中有相同元素，则是二者交点。

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        map<ListNode*, bool> occurrence;
        ListNode* p = headA;
        while (p) {
            occurrence[p] = true;
            p = p->next;
        }
        ListNode* cur = headB;
        while (cur) {
            if (occurrence[cur]) {
                return cur;
            }
            cur = cur->next;
        }
        return nullptr;
    }
};
```

*巧妙方法*

可以用双指针。

两个指针都要遍历一遍两个链表，这样两个指针所走的长度都相同，也就能够找到二者的交点。

### 143.[重排链表](https://leetcode.cn/problems/reorder-list/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (66.20%) | 1476  |    -     |

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/linked-list" title="https://leetcode.com/tag/linked-list" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个单链表 `L` 的头节点 `head` ，单链表 `L` 表示为：

```
L0 → L1 → … → Ln - 1 → Ln
```

请将其重新排列后变为：

```
L0 → Ln → L1 → Ln - 1 → L2 → Ln - 2 → …
```

不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

 

**示例 1：**

![img](https://pic.leetcode-cn.com/1626420311-PkUiGI-image.png)

```
输入：head = [1,2,3,4]
输出：[1,4,2,3]
```

**示例 2：**

![img](https://pic.leetcode-cn.com/1626420320-YUiulT-image.png)

```
输入：head = [1,2,3,4,5]
输出：[1,5,2,4,3]
```

**My solution**

一开始想的是把链表奇数位置和偶数位置结点分别记录，组成两个链表，后面也要涉及到链表的反转。

接着，想到用`map<int, ListNode*>`把链表映射一下。

后面在按照题目规则连接结点时，出现了问题，*没有考虑最后一个结点*（最后一个结点的next应该置为nullptr。

```cpp
class Solution {
public:
    void reorderList(ListNode* head) {
        if (head == nullptr) {
            return;
        }
        map<int, ListNode*> listmap;
        ListNode* cur = head;
        int i = 0;
        while(cur) {
            ++i;
            listmap[i] = cur;
            cur = cur->next;
            listmap[i]->next = nullptr;
        }
        // listmap[4]->next = listmap[2];
        for (int j = 1;j <= (i+1)/2;++j) {
            if (j >= 2) {
                listmap[i-(j-1)+1]->next = listmap[j];
                if (i % 2 == 0) {

                }
            }
            listmap[j]->next = listmap[i-j+1];
          // ! don't forget this
            if (j == (i+1)/2 && i % 2 == 1) {
                listmap[j]->next = nullptr;
            }
        }

    }
};
```

*题解*

使用到了*反转链表*，*找链表中点*，*合并链表*

这里*合并链表*的思想很值得参考

```cpp
void mergeList(ListNode* l1, ListNode* l2) {
        ListNode* l1_tmp;
        ListNode* l2_tmp;
        while (l1 != nullptr && l2 != nullptr) {
            l1_tmp = l1->next;
            l2_tmp = l2->next;

            l1->next = l2;
            l1 = l1_tmp;

            l2->next = l1;
            l2 = l2_tmp;
        }
    }
```

### 56.[合并区间](https://leetcode.cn/problems/merge-intervals/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (49.93%) | 2333  |    -     |

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/sort" title="https://leetcode.com/tag/sort" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

以数组 `intervals` 表示若干个区间的集合，其中单个区间为 `intervals[i] = [starti, endi]` 。请你合并所有重叠的区间，并返回 *一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间* 。

 

**示例 1：**

```
输入：intervals = [[1,3],[2,6],[8,10],[15,18]]
输出：[[1,6],[8,10],[15,18]]
解释：区间 [1,3] 和 [2,6] 重叠, 将它们合并为 [1,6].
```

**示例 2：**

```
输入：intervals = [[1,4],[4,5]]
输出：[[1,5]]
解释：区间 [1,4] 和 [4,5] 可被视为重叠区间。
```

**My solution**

这里我的第一想法是先对`intervals`排序，数组第一位的权重高于第二位。

```cpp
 sort(intervals.begin(), intervals.end(), [](vector<int> a, vector<int> b) {
                     if (a[0] != b[0]) {
                          return a[0] < b[0];
                     } else {
                          return a[1] < b[1];
                     }
 });
```

接着遍历排序好的`intervals`，进行判断。

```cpp
int i = 0;
        while (i+1 < intervals.size()) {
            times ++;
            if (intervals[i][1] >= intervals[i+1][0]) {
                int head = intervals[i][0];
                int tail = intervals[i+1][1];
                if (intervals[i][1] >= intervals[i+1][1]) {
                    tail = intervals[i][1];
                }
                intervals.erase(intervals.begin()+i);
                intervals.erase(intervals.begin()+i);
                intervals.emplace(intervals.begin()+i, vector<int>{head, tail});
            }
            else {
                ++i;
            }
            // if (times == 3) {
            //     return intervals;
            // }
        }
```

这里逻辑的关键点有三个：

1. 前一个数组范围可能会完全包含后一个数组，也就是后一个数组是前一个数组的子集
2. 更新`intervals`时注意要erase元素的位置，另外erase一个元素之后，后一个元素位置会变成前一个元素的位置
3. 重新构造元素的位置

**完整代码**

```cpp
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), [](vector<int> a, vector<int> b) {
                                                    if (a[0] != b[0]) {
                                                        return a[0] < b[0];
                                                    } else {
                                                        return a[1] < b[1];
                                                    }
        });
        int i = 0;
        while (i+1 < intervals.size()) {
            times ++;
            if (intervals[i][1] >= intervals[i+1][0]) {
                int head = intervals[i][0];
                int tail = intervals[i+1][1];
                if (intervals[i][1] >= intervals[i+1][1]) {
                    tail = intervals[i][1];
                }
                intervals.erase(intervals.begin()+i);
                intervals.erase(intervals.begin()+i);
                intervals.emplace(intervals.begin()+i, vector<int>{head, tail});
            }
            else {
                ++i;
            }
            // if (times == 3) {
            //     return intervals;
            // }
        }
        return intervals;
    }
};
```

### 19.[删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (48.20%) | 2866  |    -     |

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/linked-list" title="https://leetcode.com/tag/linked-list" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/two-pointers" title="https://leetcode.com/tag/two-pointers" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个链表，删除链表的倒数第 `n` 个结点，并且返回链表的头结点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/remove_ex1.jpg)

```
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

**示例 2：**

```
输入：head = [1], n = 1
输出：[]
```

**示例 3：**

```
输入：head = [1,2], n = 1
输出：[1]
```

**My solution**

本题我的思路很简单，就是一次遍历出链表的长度，第二次遍历删除指定的结点。

需要注意的是一些`corner case`

```cpp
if (n == i) {
            return head->next;
        }
```

上面的情况是：要删除的结点是第一个结点

*完整代码*

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* p = head;
        int i = 0;
        while(p) {
            i++;
            p = p->next;
        }
        if (n == i) {
            return head->next;
        }
        int delindex = i - n + 1;
        p = head;
        i = 1;
        while (p) {
            if (i == delindex-1) {
                p->next = p->next->next;
                return head;
            }
            p = p->next;
            i++;
        }
        return head;
    }
};
```

**链表重要思想**

设置一个*哑结点*指向链表的*头结点*

**题解**

使用*栈*数据结构解决。

### 93. [复原 IP 地址](https://leetcode.cn/problems/restore-ip-addresses/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (59.50%) | 1419  |    -     |

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/string" title="https://leetcode.com/tag/string" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/backtracking" title="https://leetcode.com/tag/backtracking" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

**有效 IP 地址** 正好由四个整数（每个整数位于 `0` 到 `255` 之间组成，且不能含有前导 `0`），整数之间用 `'.'` 分隔。

- 例如：`"0.1.2.201"` 和` "192.168.1.1"` 是 **有效** IP 地址，但是 `"0.011.255.245"`、`"192.168.1.312"` 和 `"192.168@1.1"` 是 **无效** IP 地址。

给定一个只包含数字的字符串 `s` ，用以表示一个 IP 地址，返回所有可能的**有效 IP 地址**，这些地址可以通过在 `s` 中插入 `'.'` 来形成。你 **不能** 重新排序或删除 `s` 中的任何数字。你可以按 **任何** 顺序返回答案。

 

**示例 1：**

```
输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]
```

**示例 2：**

```
输入：s = "0000"
输出：["0.0.0.0"]
```

**示例 3：**

```
输入：s = "101023"
输出：["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
```

**My solution**

第二次做这道题竟然被`s.substr()`坑了

```cpp
if (isLegal(tem)) {
                path.push_back(tem);
                // ！ 第二次做这题竟然还是被`substr()`坑了，应该是`s.substr(i+1)`
                backtrace(res, path, s.substr(i+1, s.length()));
                path.pop_back();
            }
```

这里进入下一层递归的应该是`s.substr(i+1)`，因为`s[i]`已经被遍历过。否则递归的字符串长度不可能是0。

*完整代码*

```cpp
vector<string> restoreIpAddresses(string s) {
        vector<string> path;
        vector<string> res;

        backtrace(res, path, s);

        return res;
    }

    // * 24.May.29
    void backtrace(vector<string>& res, vector<string>& path, string s) {
        if (s.length() == 0 && path.size() == 4) {
            string address = path[0];
            for(int i = 1;i < path.size();++i) {
                address = address + "." + path[i];
            }
            res.push_back(address);
            return;
        }
        if (s.length() == 0) {
            return;
        }
        if (s.length() != 0 && path.size() == 4) {
            return;
        }

        for (int i = 0;i < s.length();++i) {
            string tem = s.substr(0, i+1);
            if (isLegal(tem)) {
                path.push_back(tem);
                // ！ 第二次做这题竟然还是被`substr()`坑了，应该是`s.substr(i+1)`
                backtrace(res, path, s.substr(i+1, s.length()));
                path.pop_back();
            }
        }
    }
    
    bool isLegal(string& t) {
        if (t.length() > 3) {
            return false;
        }
        long long int a = stoll(t);
        char x = t[0];
        if (x == '0' && t.length() > 1) {
            return false;
        }
        if (a >= 0 && a <= 255) {
            return true;
        } else {
            return false;
        }
    }
};
```



### 82. [删除排序链表中的重复元素 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-list-ii/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (54.36%) | 1295  |    -     |

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/linked-list" title="https://leetcode.com/tag/linked-list" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个已排序的链表的头 `head` ， *删除原始链表中所有重复数字的节点，只留下不同的数字* 。返回 *已排序的链表* 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/04/linkedlist1.jpg)

```
输入：head = [1,2,3,3,4,4,5]
输出：[1,2,5]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/01/04/linkedlist2.jpg)

```
输入：head = [1,1,1,2,3]
输出：[2,3]
```

**My solution**

一开始看错题目了，以为是删除重复的元素，使链表中不存在重复的元素。就写了下面的代码。

```cpp
ListNode* hair = (ListNode*)malloc(sizeof(ListNode));
        hair->next = head;
        ListNode* cur = head;
        while(cur) {
            if (cur->next) {
                if (cur->next->val == cur->val) {
                    cur->next = cur->next->next;
                    identical[cur->val] = true;
                } else {
                    cur = cur->next;
                }
            } else {
                break;
            }
        }
```

但是题目要求要把重复的元素全部删除。于是想到用`unordered_map`结构记录上面代码中相同元素的值，重新遍历链表，遇到相同的元素就删除。

⚠️这里用到了*在链表头结点之前再加入一个结点*的思想

*完整代码*

```cpp
ListNode* deleteDuplicates(ListNode* head) {
        unordered_map<int, bool> identical;
        ListNode* hair = (ListNode*)malloc(sizeof(ListNode));
        hair->next = head;
        ListNode* cur = head;
        while(cur) {
            if (cur->next) {
                if (cur->next->val == cur->val) {
                    cur->next = cur->next->next;
                    identical[cur->val] = true;
                } else {
                    cur = cur->next;
                }
            } else {
                break;
            }
        }
        cur = hair->next;
        ListNode* pre = hair;
        while(cur) {
            if (identical.count(cur->val) != 0) {
                cur = cur->next;
                pre->next = pre->next->next;
            } else {
                cur = cur->next;
                pre = pre->next;
            }
        }
        return hair->next;
    }
```

### 704.[二分查找](https://leetcode.cn/problems/binary-search/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (55.41%) | 1579  |    -     |

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/Unknown" title="https://leetcode.com/tag/Unknown" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个 `n` 个元素有序的（升序）整型数组 `nums` 和一个目标值 `target` ，写一个函数搜索 `nums` 中的 `target`，如果目标值存在返回下标，否则返回 `-1`。


**示例 1:**

```
输入: nums = [-1,0,3,5,9,12], target = 9
输出: 4
解释: 9 出现在 nums 中并且下标为 4
```

**示例 2:**

```
输入: nums = [-1,0,3,5,9,12], target = 2
输出: -1
解释: 2 不存在 nums 中因此返回 -1
```

**My solution**

简单，但要注意*移位*问题。

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        // * 24.Jun.3
        int n = nums.size();
        int left = 0, right = n;
        int mid;
        while (left < right) {
            mid = left + ((right - left) >> 1);
            if (nums[mid] == target) {
                return mid;
            } else if (nums[mid] < target) {
                left = mid + 1;
            } else if (nums[mid] > target) {
                right = mid;
            }
        }
        return -1;
    }
}
```

### 199.[二叉树的右视图](https://leetcode.cn/problems/binary-tree-right-side-view/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (67.10%) | 1071  |    -     |

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/breadth-first-search" title="https://leetcode.com/tag/breadth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个二叉树的 **根节点** `root`，想象自己站在它的右侧，按照从顶部到底部的顺序，返回从右侧所能看到的节点值。

 

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/02/14/tree.jpg)

```
输入: [1,2,3,null,5,null,4]
输出: [1,3,4]
```

**示例 2:**

```
输入: [1,null,3]
输出: [1,3]
```

**示例 3:**

```
输入: []
输出: []
```

**My solution**

简单。就是一个二叉树*广度遍历*

```cpp
class Solution {
public:
    vector<int> rightSideView(TreeNode* root) {
        // * 24.Jun.3
        vector<int> res;
        queue<TreeNode*> que;

        if(nullptr != root) {
            que.push(root);
        }

        while (!que.empty()) {
            int size = que.size();
            for (int i = 0;i < size;++i) {
                TreeNode* cur = que.front();
                que.pop();
                if (i == size-1) {
                    res.push_back(cur->val);
                }
                if (cur->left) que.push(cur->left);
                if (cur->right) que.push(cur->right);
            }
        }

        return res;
    }
}
```

### 4. [寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Hard (42.27%) | 7138  |    -     |

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/binary-search" title="https://leetcode.com/tag/binary-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/divide-and-conquer" title="https://leetcode.com/tag/divide-and-conquer" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定两个大小分别为 `m` 和 `n` 的正序（从小到大）数组 `nums1` 和 `nums2`。请你找出并返回这两个正序数组的 **中位数** 。

算法的时间复杂度应该为 `O(log (m+n))` 。

 

**示例 1：**

```
输入：nums1 = [1,3], nums2 = [2]
输出：2.00000
解释：合并数组 = [1,2,3] ，中位数 2
```

**示例 2：**

```
输入：nums1 = [1,2], nums2 = [3,4]
输出：2.50000
解释：合并数组 = [1,2,3,4] ，中位数 (2 + 3) / 2 = 2.5
```

**My solution**

本题我的思想就是使用*双指针*。两个指针分别遍历数组1和数组2。

记录当前遍历的序号，如何和*索要查找的中位数序号相同*就记录下来，返回结果。具体看代码吧。

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n1 = nums1.size(), n2 = nums2.size();
        int i = 0, j = 0;
        int mid = -1, mid2 = -1;
        if ((n1 + n2) % 2 == 1) {
            mid = n1 + ((n2 - n1) >> 1);
        } else {
            mid = n1 + ((n2 - n1) >> 1) - 1;
            mid2 = mid + 1;
        }
        int k = 0;
        int res = -1;
        while (i < n1 && j < n2) {
            if (nums1[i] <= nums2[j] && i < n1) {
                if (k == mid || k == mid2) {
                    if (mid2 == -1) {
                        return nums1[i];
                    }
                    if (res == -1) {
                        res = nums1[i];
                    } else {
                        return (nums1[i] + res) / 2.0;
                    }
                }
                i++;
                // i = (i + 1) < n1 ? (i + 1) : i;
            } else if (nums1[i] > nums2[j] && j < n2) {
                if (k == mid || k == mid2) {
                    if (mid2 == -1) {
                        return nums2[j];
                    }
                    if (res == -1) {
                        res = nums2[j];
                    } else {
                        return (nums2[j] + res) / 2.0;
                    }
                }
                j++;
                // j = (j + 1) < n2 ? (j + 1) : j;
            }
            k++;
        }
        while(i < n1) {
            if (k == mid || k == mid2) {
                if (mid2 == -1) {
                    return nums1[i];
                }
                if (res == -1) {
                    res = nums1[i];
                } else {
                    return (nums1[i] + res) / 2.0;
                }
            }
            i++;
            k++;
        }

        while(j < n2) {
            if (k == mid || k == mid2) {
                if (mid2 == -1) {
                    return nums2[j];
                }
                if (res == -1) {
                    res = nums2[j];
                } else {
                    return (nums2[j] + res) / 2.0;
                }
            }
            j++;
            k++;
        }
        return -1;

    }
};
```

后面可以精简一下代码，用一个函数解决。

### 232.[用栈实现队列](https://leetcode.cn/problems/implement-queue-using-stacks/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (68.05%) | 1112  |    -     |

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/stack" title="https://leetcode.com/tag/stack" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/design" title="https://leetcode.com/tag/design" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

请你仅使用两个栈实现先入先出队列。队列应当支持一般队列支持的所有操作（`push`、`pop`、`peek`、`empty`）：

实现 `MyQueue` 类：

- `void push(int x)` 将元素 x 推到队列的末尾
- `int pop()` 从队列的开头移除并返回元素
- `int peek()` 返回队列开头的元素
- `boolean empty()` 如果队列为空，返回 `true` ；否则，返回 `false`

**说明：**

- 你 **只能** 使用标准的栈操作 —— 也就是只有 `push to top`, `peek/pop from top`, `size`, 和 `is empty` 操作是合法的。
- 你所使用的语言也许不支持栈。你可以使用 list 或者 deque（双端队列）来模拟一个栈，只要是标准的栈操作即可。

 

**示例 1：**

```
输入：
["MyQueue", "push", "push", "peek", "pop", "empty"]
[[], [1], [2], [], [], []]
输出：
[null, null, null, 1, 1, false]

解释：
MyQueue myQueue = new MyQueue();
myQueue.push(1); // queue is: [1]
myQueue.push(2); // queue is: [1, 2] (leftmost is front of the queue)
myQueue.peek(); // return 1
myQueue.pop(); // return 1, queue is [2]
myQueue.empty(); // return false
```

**My solution**

这题一开始的思路也是用两个栈。数据流是先到第一个栈，再从第一个栈出栈到第二个栈，实现*先进先出*。

但是一考虑到元素并不是一次全部进栈，如何保证第二次以及后面入栈的元素*先进先出*难住了我。

看了一眼**题解**。

*输出栈*在输出的时候，要判断是否是否为空。如果为空就把*输入栈*的所有元素都入栈到*输出栈*。这样就保证了*先进先出*

```cpp
int pop() {
        int top;
        if (stout.empty()) {
            while(!stin.empty()) {
                int tem = stin.top();
                stin.pop();
                stout.push(tem);
            }
        }
        if (!stout.empty()) {
            top = stout.top();
            stout.pop();
        }
        if (stout.empty()) {
            while(!stin.empty()) {
                int tem = stin.top();
                stin.pop();
                stout.push(tem);
            }
        }
        return top;

    }
```

*完整代码*

```cpp
class MyQueue {
public:
    MyQueue() {

    }
    
    void push(int x) {
        stin.push(x);
    }
    
    int pop() {
        int top;
        if (stout.empty()) {
            while(!stin.empty()) {
                int tem = stin.top();
                stin.pop();
                stout.push(tem);
            }
        }
        if (!stout.empty()) {
            top = stout.top();
            stout.pop();
        }
        if (stout.empty()) {
            while(!stin.empty()) {
                int tem = stin.top();
                stin.pop();
                stout.push(tem);
            }
        }
        return top;

    }
    
    int peek() {
        if (stout.empty()) {
            while(!stin.empty()) {
                int tem = stin.top();
                stin.pop();
                stout.push(tem);
            }
        }
        return stout.top();
    }
    
    bool empty() {
        return stin.empty() && stout.empty();
    }

private:
    stack<int> stin;
    stack<int> stout;
};
```

### 148.[排序链表](https://leetcode.cn/problems/sort-list/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (65.63%) | 2312  |    -     |

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/linked-list" title="https://leetcode.com/tag/linked-list" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/sort" title="https://leetcode.com/tag/sort" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(225, 228, 232); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你链表的头结点 `head` ，请将其按 **升序** 排列并返回 **排序后的链表** 。



 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/14/sort_list_1.jpg)

```
输入：head = [4,2,1,3]
输出：[1,2,3,4]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/09/14/sort_list_2.jpg)

```
输入：head = [-1,5,3,4,0]
输出：[-1,0,3,4,5]
```

**示例 3：**

```
输入：head = []
输出：[]
```

**My solution**

这题我投机取巧了。直接将链表所有值拷贝到容器中，对容器直接排序。接着再用链表把容器中的值全部记录下来。

```cpp
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        ListNode* hair = (ListNode*)malloc(sizeof(ListNode));
        hair->next = head;

        vector<int> vec;
        ListNode* cur = head;
        while (head) {
            vec.push_back(head->val);
            head = head->next;
        }
        sort(vec.begin(), vec.end());
        cur = hair->next;
        for (auto& i : vec) {
            cur->val = i;
            cur = cur->next;
        }
        return hair->next;
    }
};
```

### 31.[下一个排列](https://leetcode.cn/problems/next-permutation/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (39.35%) | 2502  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

整数数组的一个 **排列** 就是将其所有成员以序列或线性顺序排列。

- 例如，`arr = [1,2,3]` ，以下这些都可以视作 `arr` 的排列：`[1,2,3]`、`[1,3,2]`、`[3,1,2]`、`[2,3,1]` 。

整数数组的 **下一个排列** 是指其整数的下一个字典序更大的排列。更正式地，如果数组的所有排列根据其字典顺序从小到大排列在一个容器中，那么数组的 **下一个排列** 就是在这个有序容器中排在它后面的那个排列。如果不存在下一个更大的排列，那么这个数组必须重排为字典序最小的排列（即，其元素按升序排列）。

- 例如，`arr = [1,2,3]` 的下一个排列是 `[1,3,2]` 。
- 类似地，`arr = [2,3,1]` 的下一个排列是 `[3,1,2]` 。
- 而 `arr = [3,2,1]` 的下一个排列是 `[1,2,3]` ，因为 `[3,2,1]` 不存在一个字典序更大的排列。

给你一个整数数组 `nums` ，找出 `nums` 的下一个排列。

必须**[ 原地 ](https://baike.baidu.com/item/原地算法)**修改，只允许使用额外常数空间。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[1,3,2]
```

**示例 2：**

```
输入：nums = [3,2,1]
输出：[1,2,3]
```

**示例 3：**

```
输入：nums = [1,1,5]
输出：[1,5,1]
```

**My solution**

本题思路最重要，实现起来不难。

思路：**从后往前**搜索，找到*递增*对（`nums[i], nums[i+1]`）。接着继续**从后往前**搜索，找到第一个比`nums[i]`大的数`nums[k]`。交换`nums[i]`和`nums[k]`的值，最后对`i`之后的数组排序。

```cpp
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        int n = nums.size();
        int max_value;
        int i = n-2;
        int max_index = 0;
        
        while(i >= 0) {
            int j = i + 1;
            if (nums[i] <= nums[j]) {
                for (int k = n-1;k >= j;--k) {
                    if (nums[k] > nums[i]) {
                        swap(nums[i], nums[k]);
                        sort(nums.begin() + j, nums.end());
                        return;
                    }
                }
            }
            i--;

        }
        sort(nums.begin(), nums.end());
        return;

    }
};
```

### 2.[两数相加](https://leetcode.cn/problems/add-two-numbers/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (43.77%) | 10627 |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/linked-list" title="https://leetcode.com/tag/linked-list" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/math" title="https://leetcode.com/tag/math" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你两个 **非空** 的链表，表示两个非负的整数。它们每位数字都是按照 **逆序** 的方式存储的，并且每个节点只能存储 **一位** 数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0 开头。

 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/01/02/addtwonumber1.jpg)

```
输入：l1 = [2,4,3], l2 = [5,6,4]
输出：[7,0,8]
解释：342 + 465 = 807.
```

**示例 2：**

```
输入：l1 = [0], l2 = [0]
输出：[0]
```

**示例 3：**

```
输入：l1 = [9,9,9,9,9,9,9], l2 = [9,9,9,9]
输出：[8,9,9,9,0,0,0,1]
```

**My solution**

这题和之前的*字符串转数字题*相似。

每种情况需要维护三个变量`result`, `add`, `prev1`。分别代表*两数和进位相加之和*，*进位*，*链表1的前结点*

*完整代码*

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* hair = (ListNode*)malloc(sizeof(ListNode));
        hair->next = l1;
        // ListNode* another = (ListNode*)malloc(sizeof(ListNode));
        ListNode* another = new ListNode();

        ListNode* cur1 = l1;
        ListNode* cur2 = l2;

        ListNode* prev1 = nullptr;
        int add = 0;

        while (cur1 != nullptr || cur2 != nullptr || add != 0) {
            int result;
            if (nullptr != cur1 && nullptr != cur2) {
                result = cur1->val + cur2->val + add;
                add = result / 10;
                cur1->val = result % 10;

                prev1 = cur1;
                cur1 = cur1->next;
                cur2 = cur2->next;
            } else if (nullptr != cur1 && nullptr == cur2) {
                result = cur1->val + add;
                add = result / 10;
                cur1->val = result % 10;

                prev1 = cur1;
                cur1 = cur1->next;
            } else if (nullptr == cur1 && nullptr != cur2) {
                result = cur2->val + add;
                add = result / 10;
                cur2->val = result % 10;

                prev1->next = cur2;
                prev1 = cur2;
                cur2 = cur2->next;
            // } else {
            } else if (nullptr == cur1 && nullptr == cur2 && add != 0) {
                result = add;
                add = result / 10;
                another->val = result % 10;
                another->next = nullptr;
                prev1->next = another;

                prev1 = another;
                another = another->next; 
            } else {
                return hair->next;
            }
        }
        return hair->next;

    }
};
```

**代码随想录**

代码中有一个很巧妙的思想，就是将短的链表后面位数补0。即如果链表指针为空，就将其值视为0。

这样可以节省很多代码。

```cpp
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *head = nullptr, *tail = nullptr;
        int carry = 0;
        while (l1 || l2) {
            int n1 = l1 ? l1->val: 0;
            int n2 = l2 ? l2->val: 0;
            int sum = n1 + n2 + carry;
            if (!head) {
                head = tail = new ListNode(sum % 10);
            } else {
                tail->next = new ListNode(sum % 10);
                tail = tail->next;
            }
            carry = sum / 10;
            if (l1) {
                l1 = l1->next;
            }
            if (l2) {
                l2 = l2->next;
            }
        }
        if (carry > 0) {
            tail->next = new ListNode(carry);
        }
        return head;
    }
};
```

### 165.[比较版本号](https://leetcode.cn/problems/compare-version-numbers/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (52.32%) |  418  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/string" title="https://leetcode.com/tag/string" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你两个 **版本号字符串** `version1` 和 `version2` ，请你比较它们。版本号由被点 `'.'` 分开的修订号组成。**修订号的值** 是它 **转换为整数** 并忽略前导零。

比较版本号时，请按 **从左到右的顺序** 依次比较它们的修订号。如果其中一个版本字符串的修订号较少，则将缺失的修订号视为 `0`。

返回规则如下：

- 如果 `*version1* < *version2*` 返回 `-1`，
- 如果 `*version1* > *version2*` 返回 `1`，
- 除此之外返回 `0`。

 

**示例 1：**

**输入：**version1 = "1.2", version2 = "1.10"

**输出：**-1

**解释：**

version1 的第二个修订号为 "2"，version2 的第二个修订号为 "10"：2 < 10，所以 version1 < version2。

**示例 2：**

**输入：**version1 = "1.01", version2 = "1.001"

**输出：**0

**解释：**

忽略前导零，"01" 和 "001" 都代表相同的整数 "1"。

**示例 3：**

**输入：**version1 = "1.0", version2 = "1.0.0.0"

**输出：**0

**解释：**

version1 有更少的修订号，每个缺失的修订号按 "0" 处理。

> **My solution**

这题借鉴了上一题*二者其中一者待比较位空缺就置为0*的思想。

用到了标准库中`std::string`的处理方法，`substr`, `find`

*完整代码*

```cpp
class Solution {
public:
    int compareVersion(string version1, string version2) {
        while (!version1.empty() || !version2.empty()) {
            int ver1 = version1.empty() ? 0 : convertVersion2Value(version1);
            int ver2 = version2.empty() ? 0 : convertVersion2Value(version2);

            if (ver1 == ver2) continue;
            if (ver1 > ver2) {
                return 1;
            } else {
                return -1;
            }
        }
        return 0;

    }

    int convertVersion2Value(string& s) {
        string v;
        if (s.find(".") != string::npos) {
            v = s.substr(0, s.find("."));
            s = s.substr(s.find(".") + 1);
        } else {
            v = s;
            s = "";
        }
        return atoi(v.c_str());
    }
};
```

**official solution**

官方题解的方法是遍历字符串，计算每个由`.`分割开的值。然后进行比较。

```cpp
class Solution {
public:
    int compareVersion(string version1, string version2) {
        int n = version1.length(), m = version2.length();
        int i = 0, j = 0;
        while (i < n || j < m) {
            long long x = 0;
            for (; i < n && version1[i] != '.'; ++i) {
                x = x * 10 + version1[i] - '0';
            }
            ++i; // 跳过点号
            long long y = 0;
            for (; j < m && version2[j] != '.'; ++j) {
                y = y * 10 + version2[j] - '0';
            }
            ++j; // 跳过点号
            if (x != y) {
                return x > y ? 1 : -1;
            }
        }
        return 0;
    }
};
```

### 239.[滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Hard (48.94%) | 2800  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/heap" title="https://leetcode.com/tag/heap" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/sliding-window" title="https://leetcode.com/tag/sliding-window" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数数组 `nums`，有一个大小为 `k` 的滑动窗口从数组的最左侧移动到数组的最右侧。你只可以看到在滑动窗口内的 `k` 个数字。滑动窗口每次只向右移动一位。

返回 *滑动窗口中的最大值* 。

 

**示例 1：**

```
输入：nums = [1,3,-1,-3,5,3,6,7], k = 3
输出：[3,3,5,5,6,7]
解释：
滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

**示例 2：**

```
输入：nums = [1], k = 1
输出：[1]
```

> **My solution**

一开始想的解决办法是直接每移动一次窗口就使用`max_element`找出最大值。果不其然，提交超时。

后面就使用`max_element`和*只比较新加入值与最大值大小*结合的思想。当最大值的下标不在当前窗口内，就使用`max_element`重新计算最大值。

后面我好像自己实现了`max_element`。

*完整代码*

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        // // * determine meaning of dp array
        // // ? dp[i] means the maximum value in the range of nums[i, i+k-1]
        // vector<int> dp(n+1, INT_MIN);

        // // * initiate dp array
        // dp[0] = *(max_element(nums.begin(), nums.begin() + k));

        vector<int> res;
        int n = nums.size();
        if (k >= n) {
            res.push_back(*(max_element(nums.begin(), nums.end())));
            return res;
        }
        int start = 0, max_index = 0, end = start+k-1;
        int max_value = INT_MIN;
        findmaxVI(nums, start, end, max_value, max_index);
        res.push_back(max_value);
        ++start;
        ++end;

        while (end < n) {
            if (max_index >= start && max_index <= end) {
                if (nums[end] >= max_value) {
                    max_value = nums[end];
                    max_index = end;
                } 
            } else {
                max_value = INT_MIN;
                findmaxVI(nums, start, end, max_value, max_index);
            }
            res.push_back(max_value);
            ++start;
            ++end;
        }

        return res;
    }

    void findmaxVI(vector<int>& nums, int start, int end, int& max_val, int& max_inde) {
        for (int i = start;i <= end;++i) {
            if (nums[i] >= max_val) {
                max_val = nums[i];
                max_inde = i;
            }
        }
    }
};
```

> **official solution**

官解中有一个做法特别巧妙。它先将数组按窗口大小分组，每个组内计算两个数组`prefix`和`suffix`。

假定每个组的第一个元素下标为`groupHead`，最后一个元素下表为`groupTail`

* `prefix[i]`表示`nums[groupHead : i]`的最大值
* `suffix[i]`表示`nums[gourpTail : i]`的最大值

![image.png](https://pic.leetcode.cn/1698553966-EHilcU-image.png)

以`suffix[1]`和`prefix[3]`为例。

`suffix[1]`表示的是`nums[1 : 2]`最大值，`prefix[3]`表示的是`nums[3 : 3]`最大值，他两的组合正好就是窗口`[1 : 3]`的最大值。

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int n = nums.size();
        vector<int> prefixMax(n), suffixMax(n);
        for (int i = 0; i < n; ++i) {
            if (i % k == 0) {
                prefixMax[i] = nums[i];
            }
            else {
                prefixMax[i] = max(prefixMax[i - 1], nums[i]);
            }
        }
        for (int i = n - 1; i >= 0; --i) {
            if (i == n - 1 || (i + 1) % k == 0) {
                suffixMax[i] = nums[i];
            }
            else {
                suffixMax[i] = max(suffixMax[i + 1], nums[i]);
            }
        }

        vector<int> ans;
        for (int i = 0; i <= n - k; ++i) {
            ans.push_back(max(suffixMax[i], prefixMax[i + k - 1]));
        }
        return ans;
    }
};
```



### 322.[零钱兑换](https://leetcode.cn/problems/coin-change/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (48.46%) | 2829  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数数组 `coins` ，表示不同面额的硬币；以及一个整数 `amount` ，表示总金额。

计算并返回可以凑成总金额所需的 **最少的硬币个数** 。如果没有任何一种硬币组合能组成总金额，返回 `-1` 。

你可以认为每种硬币的数量是无限的。

 

**示例 1：**

```
输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1
```

**示例 2：**

```
输入：coins = [2], amount = 3
输出：-1
```

**示例 3：**

```
输入：coins = [1], amount = 0
输出：0
```

> **My solution**

使用动态规划。

说实话，在动态规划的*初始化*和*递推公式确定*两步骤卡住我了。

* 初始化的时候只要将`dp`初始化可能出现的最大值`amount+1`即可

* 递推公式如何与之前的`dp`建立联系。

  `dp[i-coin]`就是所得到的最少硬币个数。

*完整代码*

```cpp
class Solution {
public:
    int coinChange(vector<int>& coins, int amount) {
        // * 24.Jun.12
        
        // * determine meaning of dp array
        // ? dp[i] represent the minimum coins of which the sum is i
        vector<int> dp(amount+1, amount + 1);

        // * initiate the dp array
        dp[0] = 0;

        // * decide the recurrence formula
        for (int i = 1;i <= amount;++i) {
            for (auto& coin : coins) {
                if (coin > i) continue;
                dp[i] = min(dp[i], dp[i-coin] + 1);
            }
        }
        return dp[amount] == amount+1 ? -1 : dp[amount];
    }
};
```

### 76.[最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Hard (45.82%) | 2918  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/hash-table" title="https://leetcode.com/tag/hash-table" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/two-pointers" title="https://leetcode.com/tag/two-pointers" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/string" title="https://leetcode.com/tag/string" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/sliding-window" title="https://leetcode.com/tag/sliding-window" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个字符串 `s` 、一个字符串 `t` 。返回 `s` 中涵盖 `t` 所有字符的最小子串。如果 `s` 中不存在涵盖 `t` 所有字符的子串，则返回空字符串 `""` 。

 

**注意：**

- 对于 `t` 中重复字符，我们寻找的子字符串中该字符数量必须不少于 `t` 中该字符数量。
- 如果 `s` 中存在这样的子串，我们保证它是唯一的答案。

 

**示例 1：**

```
输入：s = "ADOBECODEBANC", t = "ABC"
输出："BANC"
解释：最小覆盖子串 "BANC" 包含来自字符串 t 的 'A'、'B' 和 'C'。
```

**示例 2：**

```
输入：s = "a", t = "a"
输出："a"
解释：整个字符串 s 是最小覆盖子串。
```

**示例 3:**

```
输入: s = "a", t = "aa"
输出: ""
解释: t 中两个字符 'a' 均应包含在 s 的子串中，
因此没有符合条件的子字符串，返回空字符串。
```

> **My solution**

这题也是看了官方题解才有思路。采用`哈希`+`遍历`

不过最后超时了。

```cpp
class Solution {
public:
    bool check(unordered_map<char, int> tmap) {
        for (auto& item : tmap) {
            if (item.second > 0) {
                return false;
            }
        }
        return true;
    }

    string minWindow(string s, string t) {
        unordered_map<char, int> tmap;
        for (auto& c : t) {
            if (tmap.count(c) != 0) {
                tmap[c]++;
            } else {
                tmap[c] = 1;
            }
        }
        pair<int, int> res{0, s.length()};
        int l = 0, r = 0;
        bool success = false;
        while (l <= r && r < s.length()) {
            if (tmap.count(s[r]) != 0 ) {
                tmap[s[r]]--;
                while(check(tmap)) {
                    success = true;
                    if (res.second - res.first > r - l + 1) {
                        res.first = l;
                        res.second = r+1;
                    }
                    if (tmap.count(s[l]) != 0) {
                        tmap[s[l]]++;
                    }
                    l++;
                }
            }
            r++;
        }
        return success ? s.substr(res.first, res.second-res.first) : "";

    }
};
```

### 41.[缺失的第一个正数](https://leetcode.cn/problems/first-missing-positive/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Hard (44.80%) | 2137  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个未排序的整数数组 `nums` ，请你找出其中没有出现的最小的正整数。

请你实现时间复杂度为 `O(n)` 并且只使用常数级别额外空间的解决方案。

 

**示例 1：**

```
输入：nums = [1,2,0]
输出：3
解释：范围 [1,2] 中的数字都在数组中。
```

**示例 2：**

```
输入：nums = [3,4,-1,1]
输出：2
解释：1 在数组中，但 2 没有。
```

**示例 3：**

```

输入：nums = [7,8,9,11,12]
输出：1
解释：最小的正数 1 没有出现。
```

> **My Solution**

直接排序。然后遍历，设置一个*比较值*（从0开始）。

```cpp
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        sort(nums.begin(), nums.end());

        int positive = 1;
        for (auto& i : nums) {
            if(i < positive) {
                continue;
            } else if (i == positive) {
                positive++;
            } else {
                return positive;
            }
        }
        return positive;
    }
};
```

> **offical solution**

官方题解非常巧妙。

首先将数组中所有的负数去除（设置成n+1，因为所有正整数都会落在[1, n]）。接着，遍历数组，标记下标为该值的元素（标记为负数）。这么一来所有下标为出现过正整数的元素都为负数，再次遍历数组，如果遇到不为负数的元素，*其下标就是未出现的正整数*。

```cpp
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();
        for (int& num: nums) {
            if (num <= 0) {
                num = n + 1;
            }
        }
        for (int i = 0; i < n; ++i) {
            int num = abs(nums[i]);
            if (num <= n) {
                nums[num - 1] = -abs(nums[num - 1]);
            }
        }
        for (int i = 0; i < n; ++i) {
            if (nums[i] > 0) {
                return i + 1;
            }
        }
        return n + 1;
    }
};
```

### 78.[子集](https://leetcode.cn/problems/subsets/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (81.34%) | 2302  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/backtracking" title="https://leetcode.com/tag/backtracking" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/bit-manipulation" title="https://leetcode.com/tag/bit-manipulation" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

> **My Solution**

现在做这种题目简直是轻车熟路。要记住关键要点：把递归当成一个树，递归就是进入下一层，遍历就是本层。

```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        //* 24.Jun.13
        vector<int> path;
        vector<vector<int>> res;
        vector<int> empty;
        res.push_back(empty);
        backtrace(nums, res, path);
        return res;

    }

    void backtrace(vector<int> nums, vector<vector<int>>& res, vector<int>& path) {
        if (nums.size() == 0) {
            return;
        }

        while (!nums.empty()) {
            int tem = nums[0];
            nums.erase(nums.begin());
            path.push_back(tem);
            res.push_back(path);
            backtrace(nums, res, path);
            path.pop_back();
        }
    }
};
```

### 106. [从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (72.24%) | 1230  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定两个整数数组 `inorder` 和 `postorder` ，其中 `inorder` 是二叉树的中序遍历， `postorder` 是同一棵树的后序遍历，请你构造并返回这颗 *二叉树* 。

 

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
输入：inorder = [9,3,15,20,7], postorder = [9,15,7,20,3]
输出：[3,9,20,null,null,15,7]
```

**示例 2:**

```
输入：inorder = [-1], postorder = [-1]
输出：[-1]
```

> **My solution**

也是参考了*代码随想录*。

首先要考虑corner case，当后序遍历数组中只有一个值，则为叶子节点。

大致思路就是

* 找到*后序遍历*的最后一个元素/*前序遍历*的第一个元素，作为*切分元素*。**并移除后序遍历的最后一个元素**
* 在*中序遍历*中找到*切分元素*并切分为左右两部分
* 按照上一部分切分的左右两部分对后序遍历/前序遍历，切分为相同大小的左右两部分
* 最后进入递归

*完整代码*

```cpp
class Solution {
public:
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        if (postorder.empty()) {
            return nullptr;
        }
        int delimiter = postorder.back();
        TreeNode* root = new TreeNode(delimiter);
        
        if (postorder.size() == 1) return root;
        // * 全部采用左闭右开
        // * find position in inorder to split
        auto i = find(inorder.begin(), inorder.end(), delimiter);
        vector<int> leftInorder(inorder.begin(), i);
        vector<int> rightInorder(i+1, inorder.end());

        postorder.pop_back();
        vector<int> leftPostorder(postorder.begin(), postorder.begin()+leftInorder.size());
        vector<int> rightPostorder(postorder.begin()+leftInorder.size(), postorder.end());

        root->left = buildTree(leftInorder, leftPostorder);
        root->right = buildTree(rightInorder, rightPostorder);

        return root;
    }
};
```

### 105.[从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (71.67%) | 2309  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定两个整数数组 `preorder` 和 `inorder` ，其中 `preorder` 是二叉树的**先序遍历**， `inorder` 是同一棵树的**中序遍历**，请构造二叉树并返回其根节点。

 

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/02/19/tree.jpg)

```
输入: preorder = [3,9,20,15,7], inorder = [9,3,15,20,7]
输出: [3,9,20,null,null,15,7]
```

**示例 2:**

```
输入: preorder = [-1], inorder = [-1]
输出: [-1]
```

> **My solution**

和*106.从中序与后序遍历序列构造二叉树*类似。

*完整代码*

```cpp
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        // * find delimiter from preorder
        if (preorder.empty()) {
            return nullptr;
        }
        int delimiter = preorder.front();
        TreeNode* root = new TreeNode(delimiter);
        if (preorder.size() == 1) {
            return root;
        }

        // * split the inorder into left and right depending on the delimiter
        auto i = find(inorder.begin(), inorder.end(), delimiter);
        vector<int> leftInorder(inorder.begin(), i);
        vector<int> rightInorder(i+1, inorder.end());

        // * split the preorder into left and right, which is recourse to the size of the last step
        // ! remember to remove the first element of preorder
        preorder.erase(preorder.begin());
        vector<int> leftPreorder(preorder.begin(), preorder.begin()+leftInorder.size());
        vector<int> rightPreorder(preorder.begin()+leftInorder.size(), preorder.end());

        root->left = buildTree(leftPreorder, leftInorder);
        root->right = buildTree(rightPreorder, rightInorder);

        return root;
    }
};
```

### 43.[字符串相乘](https://leetcode.cn/problems/multiply-strings/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (44.34%) | 1343  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/math" title="https://leetcode.com/tag/math" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/string" title="https://leetcode.com/tag/string" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定两个以字符串形式表示的非负整数 `num1` 和 `num2`，返回 `num1` 和 `num2` 的乘积，它们的乘积也表示为字符串形式。

**注意：**不能使用任何内置的 BigInteger 库或直接将输入转换为整数。

 

**示例 1:**

```
输入: num1 = "2", num2 = "3"
输出: "6"
```

**示例 2:**

```
输入: num1 = "123", num2 = "456"
输出: "56088"
```

> **My solution**

这题还是有点小难度的。涉及到字符串的加减，以及字符转数字。

主要思想就是模拟*竖式乘法*

```cpp
class Solution {
public:
    string multiply(string num1, string num2) {
        reverse(num1.begin(), num1.end());
        reverse(num2.begin(), num2.end());

        int n2 = num2.length();
        string tem_res("");

        if ((num1.length() == 1 && num1[0] == '0') || (num2.length() == 1 && num2[0] == '0')) {
            tem_res.push_back('0');
            return tem_res;
        }
        
        int i = 0;
        while (i < num2.length()) {
            string single{num2[i]};
            string tem = singleMultiply(num1, single);
            string zero(i, '0');
            tem = zero + tem;
            tem_res = numsAdd(tem, tem_res);
            i++;
        }
        reverse(tem_res.begin(), tem_res.end());
        return tem_res;
    }

    string singleMultiply(string n1, string n2) {
        // * n1为被乘数，n2为乘数
        string res("");
        int n1_length = n1.length();
        // reverse(n1.begin(), n1.end());
        // reverse(n2_mul.begin(), n2_mul.end());

        int add = 0;
        int i = 0;
        while(i < n1.length()) {
            // int result = atoi(n1[i]) * atoi(n2[j]) + add;
            int result = (n1[i] - '0') * (n2[0] - '0') + add;
            add = result / 10;
            res.push_back('0' + (result % 10));
            i++;
        }
        if (add > 0) {
            res.push_back('0' + add);
        }
        return res;
    }

    string numsAdd(string num1, string num2) {
        if (num1.length() < num2.length()) {
            string addition(num2.length()-num1.length(), '0');
            num1 = num1 + addition;
        } else {
            string addition(num1.length()-num2.length(), '0');
            num2 = num2 + addition;
        }
        // reverse(num1.begin(), num1.end());
        // reverse(num2.begin(), num2.end());
        assert(num1.length() == num2.length());

        string res("");
        int i = 0;
        int add = 0;
        while (i < num1.length()) {
            // int result = atoi(num1[i]) + atoi(num2[i]) + add;
            int result = (num1[i] - '0') + (num2[i] - '0') + add;
            add = result / 10;
            res.push_back('0' + (result % 10));
            i++;
        }
        if (add > 0) {
            res.push_back('0' + add);
        }
        return res;
    }
};
```

### 155.[最小栈](https://leetcode.cn/problems/min-stack/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (59.89%) | 1775  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/stack" title="https://leetcode.com/tag/stack" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/design" title="https://leetcode.com/tag/design" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

设计一个支持 `push` ，`pop` ，`top` 操作，并能在常数时间内检索到最小元素的栈。

实现 `MinStack` 类:

- `MinStack()` 初始化堆栈对象。
- `void push(int val)` 将元素val推入堆栈。
- `void pop()` 删除堆栈顶部的元素。
- `int top()` 获取堆栈顶部的元素。
- `int getMin()` 获取堆栈中的最小元素。

 

**示例 1:**

```
输入：
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

输出：
[null,null,null,null,-3,null,0,-2]

解释：
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.getMin();   --> 返回 -2.
```

> **My solution**

这题我的想法很简单。就是使用一个`min_val_`变量来记录每次更改栈之后栈内的最小值。`min_vals_`用来存储每次栈更改后的最小值。

有一个需要注意的是：

当`min_vals_`为空时，需手动将`min_val_`设置为为最大值`__INT_MAX__`。否则，`min_val_`被置为0，其他数字无法进入`min_vals`

*完整代码*

```cpp
class MinStack {
public:
    MinStack() {
        min_val_ = __INT_MAX__;
    }
    
    void push(int val) {
        if (val <= min_val_) {
            min_vals_.push_back(val);
            min_val_ = val;
        }
        st.push(val);
    }
    
    void pop() {
        if (st.top() == min_val_) {
            min_vals_.pop_back();
            // auto i = find(min_vals_.begin(), min_vals_.end(), min_val_);
            // min_vals_.erase(i);
            min_val_ = *(min_element(min_vals_.begin(), min_vals_.end()));
            if (min_vals_.empty()) {
                min_val_ = __INT_MAX__;
            }
        }
        st.pop();
    }
    
    int top() {
        return st.top();
    }
    
    int getMin() {
        return min_val_;
    }

private:
    stack<int> st;
    vector<int> min_vals_;
    int min_val_;
};
```

> **Official solution**

官方题解又是很巧妙。

*官方题解*使用了两个栈，`x_stack`和`min_stack`。每次入栈操作时，会将当前栈最小元素同时入栈`min_stack`。如果入栈元素小大于`min_stack`栈顶元素，此时**仍会将`min_stack`栈顶元素重复入栈**

```cpp
class MinStack {
    stack<int> x_stack;
    stack<int> min_stack;
public:
    MinStack() {
        min_stack.push(INT_MAX);
    }
    
    void push(int x) {
        x_stack.push(x);
        min_stack.push(min(min_stack.top(), x));
    }
    
    void pop() {
        x_stack.pop();
        min_stack.pop();
    }
    
    int top() {
        return x_stack.top();
    }
    
    int getMin() {
        return min_stack.top();
    }
};
```

### 151.[反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (55.47%) | 1160  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/string" title="https://leetcode.com/tag/string" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个字符串 `s` ，请你反转字符串中 **单词** 的顺序。

**单词** 是由非空格字符组成的字符串。`s` 中使用至少一个空格将字符串中的 **单词** 分隔开。

返回 **单词** 顺序颠倒且 **单词** 之间用单个空格连接的结果字符串。

**注意：**输入字符串 `s`中可能会存在前导空格、尾随空格或者单词间的多个空格。返回的结果字符串中，单词间应当仅用单个空格分隔，且不包含任何额外的空格。

 

**示例 1：**

```
输入：s = "the sky is blue"
输出："blue is sky the"
```

**示例 2：**

```
输入：s = "  hello world  "
输出："world hello"
解释：反转后的字符串中不能存在前导空格和尾随空格。
```

**示例 3：**

```
输入：s = "a good   example"
输出："example good a"
解释：如果两个单词间有多余的空格，反转后的字符串需要将单词间的空格减少到仅有一个。
```

> **My solution**

本题的难点其实在于最后如何*去除空格*。有很多种情况，比如字符串开头有很多连续空格，单词之间有多个连续空格，字符串末尾有多个连续空格。

*去除空格*思路

*快慢指针*。*快指针*每次从*空格元素*遍历到*非空格元素*时，说明进入到了下一个单词，此时如果*慢指针*不指向第一个算数就需要令慢指针所指元素为**一个空格**。这样一来，两两单词之间就只有一个空格。

```cpp
void removeSpace(string& s) {
        int slow = 0, fast = 0;
        int n = s.length();
        while (fast < n) {
            if (s[fast] == ' ') {
                fast++;
              	// ! 要判断fast<n，不然s[fast]可能不会报错
                if (fast < n && s[fast] != ' ' && slow != 0) {
                    s[slow++] = ' ';
                }
                continue;
            }
            s[slow++] = s[fast++];
        }
        s.resize(slow);
    }
```

**完整代码**

```cpp
class Solution {
public:
    string reverseWords(string s) {
        // * 24.Jun.14
        int n = s.length();
        reverse(s, 0, n-1);

        // * begin to rever every single word
        for (int i = 0;i < n;++i) {
            if (s[i] == ' ') {
                continue;
            }
            int j = i;
            while(j < n && s[j] != ' ') {
                j++;
            }
            reverse(s, i, j-1);
            i = j;
            if (j == n) {
                break;
            }
        }
        removeSpace(s);
        return s;

    }

    void reverse(string& s, int i, int j) {
        while(i <= j) {
            char tem = s[i];
            s[i] = s[j];
            s[j] = tem;
            i++;
            j--;
        }
    }

    void removeSpace(string& s) {
        int slow = 0, fast = 0;
        int n = s.length();
        while (fast < n) {
            if (s[fast] == ' ') {
                fast++;
                if (fast < n && s[fast] != ' ' && slow != 0) {
                    s[slow++] = ' ';
                }
                continue;
            }
            s[slow++] = s[fast++];
        }
        s.resize(slow);
    }
};
```

### 129.[求根节点到叶节点数字之和](https://leetcode.cn/problems/sum-root-to-leaf-numbers/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (70.64%) |  741  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个二叉树的根节点 `root` ，树中每个节点都存放有一个 `0` 到 `9` 之间的数字。

每条从根节点到叶节点的路径都代表一个数字：

- 例如，从根节点到叶节点的路径 `1 -> 2 -> 3` 表示数字 `123` 。

计算从根节点到叶节点生成的 **所有数字之和** 。

**叶节点** 是指没有子节点的节点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/19/num1tree.jpg)

```
输入：root = [1,2,3]
输出：25
解释：
从根到叶子节点路径 1->2 代表数字 12
从根到叶子节点路径 1->3 代表数字 13
因此，数字总和 = 12 + 13 = 25
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/02/19/num2tree.jpg)

```
输入：root = [4,9,0,5,1]
输出：1026
解释：
从根到叶子节点路径 4->9->5 代表数字 495
从根到叶子节点路径 4->9->1 代表数字 491
从根到叶子节点路径 4->0 代表数字 40
因此，数字总和 = 495 + 491 + 40 = 1026
```

> **My solution**

其实这题不难，很明显是用回溯。

对每个节点分为两种情况：*叶子节点*和*非叶子节点*

```cpp
class Solution {
public:
    int sumNumbers(TreeNode* root) {
        vector<int> res;
        int path = 0;

        backtrace(root, res, path);
        return accumulate(res.begin(), res.end(), 0);
    }

    void backtrace(TreeNode* root, vector<int>& res, int& path) {
        if (nullptr == root->left && nullptr == root->right) {
            path = path * 10 + root->val;
            res.push_back(path);
            return;
        }

        path = path * 10 + root->val;
        if (root->left) {
            backtrace(root->left, res, path);
            path = (path - root->left->val) / 10;
        }
        if (root->right) {
            backtrace(root->right, res, path);
            path = (path - root->right->val) / 10;
        } 

    }
};
```

需要注意的是**回溯**操作。

`path - root->left->val` 而不是`path - root->val`。因为进入递归的是`root->left`节点。

> **offical solution**

官方题解除了`深度优先遍历`还给了一种`广度优先遍历`。其实就可以理解为*层序遍历*，但是不需要每层输出，去除了`size`这个变量。另外， 加入第二个队列`sumqueue`，用来记录每个节点对应的和。

*完整代码*

```cpp
class Solution {
public:
    int sumNumbers(TreeNode* root) {
        if (root == nullptr) {
            return 0;
        }
        int sum = 0;
        queue<TreeNode*> nodeQueue;
        queue<int> numQueue;
        nodeQueue.push(root);
        numQueue.push(root->val);
        while (!nodeQueue.empty()) {
            TreeNode* node = nodeQueue.front();
            int num = numQueue.front();
            nodeQueue.pop();
            numQueue.pop();
            TreeNode* left = node->left;
            TreeNode* right = node->right;
            if (left == nullptr && right == nullptr) {
                sum += num;
            } else {
                if (left != nullptr) {
                    nodeQueue.push(left);
                    numQueue.push(num * 10 + left->val);
                }
                if (right != nullptr) {
                    nodeQueue.push(right);
                    numQueue.push(num * 10 + right->val);
                }
            }
        }
        return sum;
    }
};
```

### 104.[二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (77.61%) | 1836  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个二叉树 `root` ，返回其最大深度。

二叉树的 **最大深度** 是指从根节点到最远叶子节点的最长路径上的节点数。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/26/tmp-tree.jpg)

 

```
输入：root = [3,9,20,null,null,15,7]
输出：3
```

**示例 2：**

```
输入：root = [1,null,2]
输出：2
```

> **My solution**

简单题没什么好说的。就是使用*深度优先遍历*或*广度优先遍历*

*完整代码*

```cpp
class Solution {
public:
    int maxDepth(TreeNode* root) {
        int res = 0;
        if (nullptr == root) {
            return 0;
        }
        int maxDepth = 1;
        // * depth first
        // dfs(root, res, maxDepth);
        // return maxDepth;
        // * width first
        queue<TreeNode*> que;
        que.push(root);
        while (!que.empty()) {
            int size = que.size();
            for (int i = 0;i < size;++i) {
                TreeNode* cur = que.front();
                que.pop();
                if (i == size-1) {
                    ++res;
                }
                if (cur->left) que.push(cur->left);
                if (cur->right) que.push(cur->right);
            }
        }
        return res;
    }

    void dfs(TreeNode* root, int& res, int& maxDepth) {
        if (!root->left && !root->right) {
            ++res;
            maxDepth = max(maxDepth, res);
            return;
        }

        ++res;
        if (root->left) {
            dfs(root->left, res, maxDepth);
            --res;
        }
        if (root->right) {
            dfs(root->right, res, maxDepth);
            --res;
        }

    }
};
```

### 101.[对称二叉树](https://leetcode.cn/problems/symmetric-tree/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (60.38%) | 2726  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/breadth-first-search" title="https://leetcode.com/tag/breadth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个二叉树的根节点 `root` ， 检查它是否轴对称。

 

**示例 1：**

![img](https://pic.leetcode.cn/1698026966-JDYPDU-image.png)

```
输入：root = [1,2,2,3,4,4,3]
输出：true
```

**示例 2：**

![img](https://pic.leetcode.cn/1698027008-nPFLbM-image.png)

```
输入：root = [1,2,2,null,3,null,3]
输出：false
```

> **My solution**

同样，一题简单题。

采用的是递归，深度优先遍历。因为是对称，需要注意进行比较的节点。`left->left`和`right->right`以及`left->right`和`right->left`

```cpp
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        // * 24.Jun.17
        if (nullptr == root) return true;
        return compare(root->left, root->right);
    }

    bool compare(TreeNode* left, TreeNode* right) {
        if (!left && !right) {
            return true;
        } else if (!left && right) {
            return false;
        } else if (left && !right)   {
            return false;
        } else if (left->val != right->val) {
            return false; 
        }

        bool inside = compare(left->right, right->left);
        bool outside = compare(left->left, right->right);
        return (inside && outside);
    }
};
```

> **Official solution**

官解思路一样，但是代码写的很简洁。

```cpp
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        return root == nullptr || recur(root->left, root->right);
    }
private:
    bool recur(TreeNode* L, TreeNode* R) {
        if (L == nullptr && R == nullptr) return true;
        if (L == nullptr || R == nullptr || L->val != R->val) return false;
        return recur(L->left, R->right) && recur(L->right, R->left);
    }
};
```

### 144.[二叉树的前序遍历](https://leetcode.cn/problems/binary-tree-preorder-traversal/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (71.92%) | 1254  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/stack" title="https://leetcode.com/tag/stack" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你二叉树的根节点 `root` ，返回它节点值的 **前序** 遍历。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg)

```
输入：root = [1,null,2,3]
输出：[1,2,3]
```

**示例 2：**

```
输入：root = []
输出：[]
```

**示例 3：**

```
输入：root = [1]
输出：[1]
```

**示例 4：**

![img](https://assets.leetcode.com/uploads/2020/09/15/inorder_5.jpg)

```
输入：root = [1,2]
输出：[1,2]
```

> **My solution**

三种写法

* 递归

  很简单

  ```cpp
  vector<int> preorderTraversal(TreeNode* root) {
    // * 24.Jun.17
    if (nullptr == root) {
      return vector<int>{};
    }
    vector<int> res;
    preorder(root, res);
    return res;
  }
  ```

* 用栈模拟递归

  一般写法。前序遍历顺序是*中左右*。所以入栈的时候需要注意顺序（右左中）

  ```cpp
  vector<int> res;
          stack<TreeNode*> st;
          if (nullptr != root) st.push(root);
          while (!st.empty()) {
              TreeNode* cur = st.top();
              st.pop();
              if(cur->right) st.push(cur->right);
              if(cur->left) st.push(cur->left);
              res.push_back(cur->val);
          } 
  ```

  统一写法。该写法先从栈顶取出一个元素，判断该元素是否为空指针：是，说明下一个元素需要添加到`res`；否则，按照*一般写法*进行入栈，但是在`cur`入栈之后要记得入栈一个*空指针*

  ```cpp
  	vector<int> res;
    stack<TreeNode*> st;
    if (nullptr != root) st.push(root);
    while (!st.empty()) {
      TreeNode* cur = st.top();
      if (nullptr != cur) {
        st.pop();
        if (cur->right) st.push(cur->right);
        if (cur->left) st.push(cur->left);
        st.push(cur);
        st.push(nullptr);
      } else if (nullptr == cur) {
        st.pop();
        cur = st.top();
        st.pop();
        res.push_back(cur->val);
      }
    }
  ```



### 110.[平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (58.47%) | 1522  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个二叉树，判断它是否是 平衡二叉树 

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：true
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg)

```
输入：root = [1,2,2,3,3,null,null,4,4]
输出：false
```

**示例 3：**

```
输入：root = []
输出：true
```

> **My solution**

这题别看它是个简单题。花了我很长时间的，最终采取的方案还是*直接判断当前结点的左右子树是不是平衡二叉树*

不知道为啥返回最小深度和最大深度总会出错。

```cpp
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        // * 24.Jun.19
        int minDepth = getMinDep(root);
        // if (nullptr != root && (nullptr == root->left || nullptr == root->right)) {
        //     minDepth = 1;
        // } else if (nullptr == root) {
        //     minDepth = 0;
        // }
        // int maxDepth = getMaxDepth(root);
        int depth1 = depth(root);
        return depth1 == -1 ? false : true;

        //  return (abs(maxDepth - minDepth) >= 2) ? false : true;
    }

    int depth(TreeNode* root) {
        if (nullptr == root) return 0;
        int res = 1;
        int leftDepth = depth(root->left);
        int rightDepth = depth(root->right);
        if (leftDepth == -1 || rightDepth == -1) return -1;
        if (abs(leftDepth - rightDepth) > 1) return -1;
        return max(leftDepth, rightDepth) + res;
    }

    int getMinDep(TreeNode* root) {
        if (nullptr == root) return 0;
        int res = 1;
        if (nullptr == root->left && nullptr == root->right) {
            return res;
        }
        int leftDepth = getMinDep(root->left);
        int rightDepth = getMinDep(root->right);
        return min(leftDepth, rightDepth) + res;
    }

    int getMaxDepth(TreeNode* root) {
        if (nullptr == root) return 0;
        int res = 1;
        if (nullptr == root->left && nullptr == root->right) {
            return res;
        }
        int leftDepth = getMaxDepth(root->left);
        int rightDepth = getMaxDepth(root->right);
        return max(leftDepth, rightDepth) + res;
    }
};
```

### 543.[二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (60.47%) | 1552  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一棵二叉树的根节点，返回该树的 **直径** 。

二叉树的 **直径** 是指树中任意两个节点之间最长路径的 **长度** 。这条路径可能经过也可能不经过根节点 `root` 。

两节点之间路径的 **长度** 由它们之间边数表示。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/06/diamtree.jpg)

```
输入：root = [1,2,3,4,5]
输出：3
解释：3 ，取路径 [4,2,1,3] 或 [5,2,1,3] 的长度。
```

**示例 2：**

```
输入：root = [1,2]
输出：1
```

> **My solution**

思路很简单：计算当前结点左右两个子树的高度，相加就是路径长度。并选出最大的路径长度。

```cpp
class Solution {
public:
    int diameterOfBinaryTree(TreeNode* root) {
        int res = 0;
        maxDepth(root, res);
        return res;
    }

    int maxDepth(TreeNode* root, int& maxLength) {
        if(nullptr == root) return 0;
        int res = 1;
        int leftDepth = maxDepth(root->left, maxLength);
        int rightDepth = maxDepth(root->right, maxLength);
        maxLength = max(maxLength, leftDepth + rightDepth);
        return max(leftDepth, rightDepth) + res;
    }
};
```

### 34.[在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (43.55%) | 2720  |    -     |

<details style="color: rgb(59, 59, 59); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/binary-search" title="https://leetcode.com/tag/binary-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(59, 59, 59); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个按照非递减顺序排列的整数数组 `nums`，和一个目标值 `target`。请你找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 `target`，返回 `[-1, -1]`。

你必须设计并实现时间复杂度为 `O(log n)` 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```

**示例 2：**

```
输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]
```

**示例 3：**

```
输入：nums = [], target = 0
输出：[-1,-1]
```

> **My solution**

第二次做这题，还是卡住了，稍微瞄了一眼之前写的代码。对之前代码的理解加深了。

思路：使用二分查找，分两次找到*左边界*和*右边界*

以*左边界*为例，无非有三种情况：数组中有两个`target`，一个`target`，没有`target`。

* 两个`target`

  此时不能确保第一次找到的`leftborder`是最终的`leftborder`，所以除了令`leftborder = middle`外，还要更进一步令`right = middle`，让其继续查找（可能还有一个`target`）。

*完整代码*

```cpp
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        // * 24.Jun.26
        int leftborder = -1, rightborder = -1;
        int left = 0, right = nums.size();

        // * find left border
        // * continually find left part, so definitely can find left border, despite the firs left border is not expected
        while (left < right) {
            int middle = left + ((right - left) >> 1);
            if (nums[middle] > target) {
                right = middle;
            } else if (nums[middle] < target) {
                left = middle + 1;
            } else {
                leftborder = middle;
                right = middle;
            }
        }
        left = 0, right = nums.size();

        // * find right border
        while (left < right) {
            int middle = left + ((right - left) >> 1);
            if (nums[middle] > target) {
                right = middle;
            } else if (nums[middle] < target) {
                left = middle + 1;
            } else {
                rightborder = middle;
                left = middle + 1;
            }
        }
        return vector<int>{leftborder, rightborder};

    }
```

### 113.[路径总和 II](https://leetcode.cn/problems/path-sum-ii/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (63.28%) | 1116  |    -     |

<details><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你二叉树的根节点 `root` 和一个整数目标和 `targetSum` ，找出所有 **从根节点到叶子节点** 路径总和等于给定目标和的路径。

**叶子节点** 是指没有子节点的节点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsumii1.jpg)

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：[[5,4,11,2],[5,8,4,5]]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg)

```
输入：root = [1,2,3], targetSum = 5
输出：[]
```

**示例 3：**

```
输入：root = [1,2], targetSum = 0
输出：[]
```

> **My solution**

这题要打印路径，很明显使用回溯法。但需要注意的是：

* 路径必须是从*根结点*到*叶结点*

* 写的过程中了出现了打印两次正确结果的情况。原因是我对返回条件的判断`targetSum==0`，因为在传参时传的是`targetSum - root->val`，这样的判断条件会让函数在*左结点*和*右结点*都返回，故返回了两次。

  正确的判断条件应该是`targetSum - root->val == 0`

*完整代码*

```cpp
class Solution {
public:
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        vector<vector<int>> res;
        vector<int> path;
        backtrace(root, res, path, targetSum);

        return res;
    }

    void backtrace(TreeNode* root, vector<vector<int>>& res, vector<int>& path, int targetSum) {
        if (nullptr == root) {
            return;
        }
        if (nullptr == root->left && nullptr == root->right) {
            path.push_back(root->val);
            if (targetSum - root->val == 0) {
                res.push_back(path);
            }
            path.pop_back();
            return;
        }
        path.push_back(root->val);
        backtrace(root->left, res, path, targetSum - root->val);
        backtrace(root->right, res, path, targetSum - root->val);
        path.pop_back();
        
    }
};
```







### 39.[组合总和](https://leetcode.cn/problems/combination-sum/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (72.77%) | 2820  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/backtracking" title="https://leetcode.com/tag/backtracking" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个 **无重复元素** 的整数数组 `candidates` 和一个目标整数 `target` ，找出 `candidates` 中可以使数字和为目标数 `target` 的 所有 **不同组合** ，并以列表形式返回。你可以按 **任意顺序** 返回这些组合。

`candidates` 中的 **同一个** 数字可以 **无限制重复被选取** 。如果至少一个数字的被选数量不同，则两种组合是不同的。 

对于给定的输入，保证和为 `target` 的不同组合数少于 `150` 个。

 

**示例 1：**

```
输入：candidates = [2,3,6,7], target = 7
输出：[[2,2,3],[7]]
解释：
2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。
7 也是一个候选， 7 = 7 。
仅有这两种组合。
```

**示例 2：**

```
输入: candidates = [2,3,5], target = 8
输出: [[2,2,2,2],[2,3,3],[3,5]]
```

**示例 3：**

```
输入: candidates = [2], target = 1
输出: []
```

> **My solution**

纯粹，回溯法。

```cpp
class Solution {
public:
    // * 24.Jun.20
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<int> sequence;
        vector<vector<int>> res;

        backtrace(candidates, res, sequence, target);
        return res;

    }

    void backtrace(vector<int> candidates, vector<vector<int>>& res, vector<int>& sequence, int target) {
        if (accumulate(sequence.begin(), sequence.end(), 0) == target) {
            res.push_back(sequence);
            return;
        }
        if (accumulate(sequence.begin(), sequence.end(), 0) > target) {
            return;
        }
        if (candidates.empty()) {
            return;
        }

        while (!candidates.empty()) {
            sequence.push_back(candidates[0]);
            backtrace(candidates, res, sequence, target);
            sequence.pop_back();
            candidates.erase(candidates.begin());
        }
    }
};
```

这里我对加入`res`数组和函数返回的判断条件都是调用`accumulate`函数。一个开销更小的方法是每次进入递归时用`target`减去刚刚加入的元素，判断`target==0`或`target<0`。

### 240.[搜索二维矩阵 II](https://leetcode.cn/problems/search-a-2d-matrix-ii/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (53.87%) | 1486  |    -     |

<details style="color: rgb(59, 59, 59); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/binary-search" title="https://leetcode.com/tag/binary-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/divide-and-conquer" title="https://leetcode.com/tag/divide-and-conquer" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(59, 59, 59); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

编写一个高效的算法来搜索 `*m* x *n*` 矩阵 `matrix` 中的一个目标值 `target` 。该矩阵具有以下特性：

- 每行的元素从左到右升序排列。
- 每列的元素从上到下升序排列。

 

**示例 1：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/25/searchgrid2.jpg)

```
输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5
输出：true
```

**示例 2：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2020/11/25/searchgrid.jpg)

```
输入：matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 20
输出：false
```

> **My solution**

这题本来想用两次*二分查找*。第一次查找出所在列，第二次查找出所在行。但实际操作下来是不对的，因为有些情况下，`target`比第一行的所有数都大。（或许对每行都做一次二分查找可以解决）

后面就用了另一种想法：按行按列找出所有符合范围的行和列，并把它们记录到行数组和列数组

检查每一行元素

```cpp
for (int i = 0;i < row_len;++i) {
        if (matrix[i][0] < target && matrix[i][column_len-1] > target) {
            rows.push_back(i);
        } else if (matrix[i][0] == target || matrix[i][column_len-1] == target) {
            return true;
        }
    }
    for (int i = 0;i < column_len;++i) {
        if (matrix[0][i] < target && matrix[row_len-1][i] > target) {
            columns.push_back(i);
        } else if (matrix[0][i] == target || matrix[row_len-1][i] == target) {
            return true;
        }
    }
```

知道了符合条件的*行*和*列*之后，就可以确定矩阵中是否有该元素了。

*完整代码*

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        // * 24.Jun.27
        // int left = 0, right = matrix[0].size();
        // int column = -1;
        // // while (left < right) {
        // //     column = left + ((right - left) >> 1);
        // //     if (matrix[0][column] > target) {
        // //         right = column;
        // //     } else if (matrix[0][column] < target) {
        // //         left = column + 1;
        // //     } else {
        // //         return true;
        // //     }
        // // }
        // if (matrix.size() == 0) return false;
        // // if (matrix[0].size() == 1) return matrix[0][0] == target ? true : false;
        // for (int i = 0;i < matrix.size();++i) {
        //     int len = matrix[i].size();
        //     if (matrix[i][0] < target && matrix[i][len - 1] > target) {
        //         column = i;
        //         // break;
        //     } else if (matrix[i][0] == target || matrix[i][len - 1] == target) {
        //         return true;
        //     }
        // }
        // if (column == -1) return false;
        // cout << "column is " << column << endl;

        // left = 0, right = matrix[column].size();
        // while (left < right) {
        //     int middle = left + ((right - left) >> 1);
        //     if (matrix[column][middle] < target) {
        //         left = middle + 1;
        //     } else if (matrix[column][middle] > target) {
        //         right = middle;
        //     } else {
        //         return true;
        //     }
        // }
        // return false;
    // }

    vector<int> rows;
    vector<int> columns;
    int row_len = matrix.size();
    int column_len = matrix[0].size();

    for (int i = 0;i < row_len;++i) {
        if (matrix[i][0] < target && matrix[i][column_len-1] > target) {
            rows.push_back(i);
        } else if (matrix[i][0] == target || matrix[i][column_len-1] == target) {
            return true;
        }
    }
    for (int i = 0;i < column_len;++i) {
        if (matrix[0][i] < target && matrix[row_len-1][i] > target) {
            columns.push_back(i);
        } else if (matrix[0][i] == target || matrix[row_len-1][i] == target) {
            return true;
        }
    }
    for (int i = 0;i < rows.size(); ++i) {
        for (int j = 0;j < columns.size();++j) {
            if (matrix[rows[i]][columns[j]] == target) {
                return true;
            }
        }
    }
    return false;
    }
};
```

> **official solution**

* 直接遍历整个矩阵
* 对每行或者每列，都使用*二分法*

### 64.[最小路径和](https://leetcode.cn/problems/minimum-path-sum/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (70.46%) | 1688  |    -     |

<details style="color: rgb(59, 59, 59); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(59, 59, 59); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个包含非负整数的 `*m* x *n*` 网格 `grid` ，请找出一条从左上角到右下角的路径，使得路径上的数字总和为最小。

**说明：**每次只能向下或者向右移动一步。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/05/minpath.jpg)

```
输入：grid = [[1,3,1],[1,5,1],[4,2,1]]
输出：7
解释：因为路径 1→3→1→1→1 的总和最小。
```

**示例 2：**

```
输入：grid = [[1,2,3],[4,5,6]]
输出：12
```

> **My solution**

这题拿到手先愣了一下。后面察觉到这题是典型的*动态规划*题。

直接*动态规划五部曲*

```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        // * 24.Jun.30
        int m = grid.size(), n = grid[0].size();
        // * determine the meaning of dp array
        // ? dp[i][j] represents the mini value, from dp[0][0] to dp[i][j]
        vector<vector<int>> dp(m+1, vector<int>(n+1, __INT_MAX__));
        
        // * initiate the dp array
        dp[0][0] = grid[0][0];
        for (int i = 1;i < m;++i) {
            dp[i][0] = grid[i][0] + dp[i-1][0];
        }
        for (int j = 1;j < n;++j) {
            dp[0][j] = grid[0][j] + dp[0][j-1];
        }

        for (int i = 1;i < m;++i) {
            for (int j = 1;j < n;++j) {
                dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
            }
        }
        return dp[m-1][n-1];
    }
};
```

### 394.[字符串解码](https://leetcode.cn/problems/decode-string/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (57.72%) | 1776  |    -     |

<details style="color: rgb(59, 59, 59); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/stack" title="https://leetcode.com/tag/stack" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(59, 59, 59); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个经过编码的字符串，返回它解码后的字符串。

编码规则为: `k[encoded_string]`，表示其中方括号内部的 `encoded_string` 正好重复 `k` 次。注意 `k` 保证为正整数。

你可以认为输入字符串总是有效的；输入字符串中没有额外的空格，且输入的方括号总是符合格式要求的。

此外，你可以认为原始数据不包含数字，所有的数字只表示重复的次数 `k` ，例如不会出现像 `3a` 或 `2[4]` 的输入。

 

**示例 1：**

```
输入：s = "3[a]2[bc]"
输出："aaabcbc"
```

**示例 2：**

```
输入：s = "3[a2[c]]"
输出："accaccacc"
```

**示例 3：**

```
输入：s = "2[abc]3[cd]ef"
输出："abcabccdcdcdef"
```

**示例 4：**

```
输入：s = "abc3[cd]xyz"
输出："abccdcdcdxyz"
```

> **My solution**

本题一开始的思路不是用栈，而是简单地碰到`]`就去找*`[`和重复数字*。但是无法通过测试用例*嵌套中括号*的情况。

后面还是用栈去处理。同样地，遇到`]`就*出栈找`[`和重复数字*。把内容展开后再*入栈*。最后，栈内元素就是最终结果。

```cpp
class Solution {
public:
    string decodeString(string s) {
    // ? 递归做法不适合没有嵌套的字符串
    //     int begin = s.find('[');
    //     int end = s.rfind(']');
    //     if (begin == string::npos) {
    //         return "";
    //     }
    //     string tem = s.substr(begin+1, end - begin - 1);
    //     string remain = s.substr(0, begin-1);
    //     int rep = s[begin-1] - '0';
    //     res = decodeString(tem);
    //     return remain + res;
    // }

    // string unCompactString(string s) {
    //     int begin = s.find('[');
    //     int end = s.find(']');
    //     if (begin == string::nops) {
    //         return "";
    //     }
    //     string tem = s.substr(begin + 1, end - begin - 1);
    //     string remain = s.substr(0, begin - 1);

    // }
        string res;
        stack<char> st;
        for (int i = 0;i < s.length();++i) {
            if (s[i] == ']') {
                string tem;
                while (st.top() != '[') {
                    tem.push_back(st.top());
                    st.pop();
                }
                st.pop();
                string rep;
                while (!st.empty() && st.top() >= '0' && st.top() <= '9') {
                    rep.push_back(st.top());
                    st.pop();
                }
                reverse(rep.begin(), rep.end());
                int rep_int = atoi(rep.c_str());
                for (int j = 0;j < rep_int;++j) {
                    for (int k = tem.length() - 1;k >= 0;--k) {
                        st.push(tem[k]);
                    }
                }
            } else {
                st.push(s[i]);
            }
        }
        while (!st.empty()) {
            res.push_back(st.top());
            st.pop();
        }
        reverse(res.begin(), res.end());
        return res;
    }
};
```

### 221.[最大正方形](https://leetcode.cn/problems/maximal-square/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (50.39%) | 1670  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

在一个由 `'0'` 和 `'1'` 组成的二维矩阵内，找到只包含 `'1'` 的最大正方形，并返回其面积。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/26/max1grid.jpg)

```
输入：matrix = [["1","0","1","0","0"],["1","0","1","1","1"],["1","1","1","1","1"],["1","0","0","1","0"]]
输出：4
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/11/26/max2grid.jpg)

```
输入：matrix = [["0","1"],["1","0"]]
输出：1
```

**示例 3：**

```
输入：matrix = [["0"]]
输出：0
```

> **My solution**

说是*my solution*，其实都看过了官方题解。

* 暴力解法（超时）

  遍历整个矩阵，遇到`1`，就以该元素作为左上角，不断地扩大边长，直到某次扩大之后出现`0`

  ```cpp
  class Solution {
  public:
      int maximalSquare(vector<vector<char>>& matrix) {
          // * 24.Jul.2
          int m = matrix.size(), n = matrix[0].size();
          int res = 0;
  
          vector<vector<bool>> visited(m, vector<bool>(n, false));
  
          for (int i = 0;i < m;++i) {
              for (int j = 0;j < n;++j) {
                  if (matrix[i][j] == '1') {
                      int tem = bfs(matrix, i, j);
                      res = max(res, tem + 1);
                  }
              }
          }
          return res * res;
      }
    int bfs(vector<vector<char>>& matrix, int x, int y) {
          int i = 1;
          int max_len = 0;
          while ((x + i) < matrix.size() && (y + i) < matrix[0].size()) {
              cout << "i is " << i << endl;
              for (int h = 0;h <= i;++h) {
                  for (int w = 0;w <= i;++w) {
                      if (matrix[x+h][y+w] != '1') {
                          cout << "max_len_in is " << max_len << endl;
                          return max_len;
                      }
                  }
              }
              max_len = i;
              cout << "max_len is " << max_len << endl;
              ++i;
          }
          return max_len;
      }
  };
  ```

* 动态规划算法

  直接给出递推公式可能好理解一点

  `dp[i][j] = min(dp[i-1][j-1], dp[i-1][j], dp[i][j-1]) + 1`

  这里`dp[i][j]`表示的是以`matrix[i][j]`为右下角的正方形的最大边长，其长度可以由三个位置得到

  ```cpp
  class Solution {
  public:
      int maximalSquare(vector<vector<char>>& matrix) {
          // * 24.Jul.2
          // * dynamic programming
          // * determine the meaning of dp array
          // ? dp[i][j] represents that the max length of a rectangle whose bottom right corner is matrix[i][j]
          int m = matrix.size(), n = matrix[0].size();
          vector<vector<int>> dp(m, vector<int>(n, 0));
  
          // * initiate the dp array 
          int res = 0;
          for (int i = 0;i < m;++i) {
              for (int j = 0;j < n;++j) {
                  if (matrix[i][j] == '1') {
                      dp[i][j] = 1;
                  }
                  res = max(res, dp[i][j]);
              }
          }
          // * decide the recurrence formula
          for (int i = 1;i < m;++i) {
              for (int j = 1;j < n;++j) {
                  if (matrix[i][j] == '1') {
                      dp[i][j] = min(dp[i-1][j-1], min(dp[i-1][j], dp[i][j-1])) + 1;
                      cout << i << "," << j << " is " << dp[i][j] << endl;
                  }
                  res = max(res, dp[i][j]);
              }
          }
          return res * res;
      }
  ```

### 162. [寻找峰值](https://leetcode.cn/problems/find-peak-element/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (49.50%) | 1288  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/binary-search" title="https://leetcode.com/tag/binary-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

峰值元素是指其值严格大于左右相邻值的元素。

给你一个整数数组 `nums`，找到峰值元素并返回其索引。数组可能包含多个峰值，在这种情况下，返回 **任何一个峰值** 所在位置即可。

你可以假设 `nums[-1] = nums[n] = -∞` 。

你必须实现时间复杂度为 `O(log n)` 的算法来解决此问题。

 

**示例 1：**

```
输入：nums = [1,2,3,1]
输出：2
解释：3 是峰值元素，你的函数应该返回其索引 2。
```

**示例 2：**

```
输入：nums = [1,2,1,3,5,6,4]
输出：1 或 5 
解释：你的函数可以返回索引 1，其峰值元素为 2；
     或者返回索引 5， 其峰值元素为 6。
```

> **My solution**

暴力

```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        if (nums.size() == 1) {
            return 0;
        }
        if (nums.size() > 1) {
            if (nums[0] > nums[1]) return 0;
            if (nums[nums.size()-1] > nums[nums.size()-2]) return nums.size() - 1;
        }
        
        for (int i = 1;i < nums.size()-1;++i) {
            cout << nums[i-1] << " " << nums[i] << " " << nums[i+1] << endl;
            if (nums[i] > nums[i-1] && nums[i] > nums[i+1]) {
                return i;
            }
        }
        return -1;
    }
};
```

*官解是直接找出数组最大值*

> **Official solution**

*人往高处走，水往低处流*

从`[l, r]`范围随机选个下标`i`，判断`nums[i]`和`nums[i+1]`的大小关系，决定舍弃哪边的值

```cpp
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int n = nums.size();

        // 辅助函数，输入下标 i，返回一个二元组 (0/1, nums[i])
        // 方便处理 nums[-1] 以及 nums[n] 的边界情况
        auto get = [&](int i) -> pair<int, int> {
            if (i == -1 || i == n) {
                return {0, 0};
            }
            return {1, nums[i]};
        };

        int left = 0, right = n - 1, ans = -1;
        while (left <= right) {
            int mid = (left + right) / 2;
            if (get(mid - 1) < get(mid) && get(mid) > get(mid + 1)) {
                ans = mid;
                break;
            }
            if (get(mid) < get(mid + 1)) {
                left = mid + 1;
            }
            else {
                right = mid - 1;
            }
        }
        return ans;
    }
};
```

### 128. [最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (51.69%) | 2122  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/union-find" title="https://leetcode.com/tag/union-find" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个未排序的整数数组 `nums` ，找出数字连续的最长序列（不要求序列元素在原数组中连续）的长度。

请你设计并实现时间复杂度为 `O(n)` 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [100,4,200,1,3,2]
输出：4
解释：最长数字连续序列是 [1, 2, 3, 4]。它的长度为 4。
```

**示例 2：**

```
输入：nums = [0,3,7,2,5,8,4,6,0,1]
输出：9
```

> **My solution**

本题我的思路是先将`nums`排序，然后遍历寻找最长连续序列。但是连续不一定连续，可能有重复元素，所以排完序后我又将重复元素去除。

```cpp
 sort(nums.begin(), nums.end());
for (int i = 1;i < nums.size();++i) {
  if (nums[i] == nums[i-1]) {
    nums.erase(nums.begin() + i);
    --i;
  }
}
```

*完整代码*

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        for (int i = 1;i < nums.size();++i) {
            if (nums[i] == nums[i-1]) {
                nums.erase(nums.begin() + i);
                --i;
            }
        }
        int res = 0;
        int i = 0;
        while (i < nums.size()) {
            pair<int, int> tem = getSequenceLen(nums, i);
            res = max(tem.first, res);
            i = tem.second;
        }
        return res;
    }

    // * pair<maxlen, index>
    pair<int, int> getSequenceLen(vector<int>& nums, int i) {
        for (int j = i+1;j < nums.size();++j) {
            if (nums[j] - nums[j-1] != 1) {
                return pair<int, int>{j-i, j};
            }
        }
        return pair<int, int>{nums.size()-i, nums.size()};
    }
};
```

> **Official solution**

官方题解用到了*哈希*。

先把所有的元素映射到*哈希表*（去重）。然后遍历哈希表元素，判断其是不是连续序列的第一个数（判断`value-1`在不在哈希表中）

*完整代码*

```cpp
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> num_set;
        for (const int& num : nums) {
            num_set.insert(num);
        }

        int longestStreak = 0;

        for (const int& num : num_set) {
            if (!num_set.count(num - 1)) {
                int currentNum = num;
                int currentStreak = 1;

                while (num_set.count(currentNum + 1)) {
                    currentNum += 1;
                    currentStreak += 1;
                }

                longestStreak = max(longestStreak, currentStreak);
            }
        }

        return longestStreak;           
    }
};
```

> **summarization**

遇到这种需要去除*重复元素*的，可以考虑*哈希表*。

### 234.[回文链表](https://leetcode.cn/problems/palindrome-linked-list/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (54.60%) | 1921  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/linked-list" title="https://leetcode.com/tag/linked-list" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/two-pointers" title="https://leetcode.com/tag/two-pointers" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个单链表的头节点 `head` ，请你判断该链表是否为回文链表。如果是，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal1linked-list.jpg)

```
输入：head = [1,2,2,1]
输出：true
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/03/03/pal2linked-list.jpg)

```
输入：head = [1,2]
输出：false
```

> **My solution**

简单题。

想法很简单，把链表转换为数组，使用*双指针*判断是不是回文。

*完整代码*

```cpp
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        // * 24.Jul.3
        vector<int> vec;
        ListNode* cur = head;
        while (nullptr != cur) {
            vec.push_back(cur->val);
            cur = cur->next;
        }
        int i = 0, j = vec.size()-1;
        while (i <= j) {
            if (vec[i++] != vec[j--]) {
                return false;
            }
        }
        return true;
    }
};
```

### 112.[路径总和](https://leetcode.cn/problems/path-sum/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (54.33%) | 1366  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你二叉树的根节点 `root` 和一个表示目标和的整数 `targetSum` 。判断该树中是否存在 **根节点到叶子节点** 的路径，这条路径上所有节点值相加等于目标和 `targetSum` 。如果存在，返回 `true` ；否则，返回 `false` 。

**叶子节点** 是指没有子节点的节点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsum1.jpg)

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,null,1], targetSum = 22
输出：true
解释：等于目标和的根节点到叶节点路径如上图所示。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg)

```
输入：root = [1,2,3], targetSum = 5
输出：false
解释：树中存在两条根节点到叶子节点的路径：
(1 --> 2): 和为 3
(1 --> 3): 和为 4
不存在 sum = 5 的根节点到叶子节点的路径。
```

**示例 3：**

```
输入：root = [], targetSum = 0
输出：false
解释：由于树是空的，所以不存在根节点到叶子节点的路径。
```

> **My solution**

*简单题*

思路很简单，就是*递归*。分为两种情况，是叶子节点和非叶子节点。

```cpp
class Solution {
public:
    bool hasPathSum(TreeNode* root, int targetSum) {
        if (nullptr == root) return false;
        // if (targetSum < 0) {
        //     return false;
        // }
        if (nullptr == root->left && nullptr == root->right) {
            if (root->val - targetSum == 0) {
                cout << "root->val is " << root->val << "targetSum is " << targetSum << endl;
                return true;
            } else {
                return false;
            }
        } else {
            bool left = hasPathSum(root->left, targetSum - root->val);
            bool right = hasPathSum(root->right, targetSum - root->val);
            return (left || right);
        }
        
    }

};
```

*注意*：上面注释掉的代码，本来想*剪枝*。但是没有考虑到`targetSum`本事就是一个负值。

### 169.[多数元素](https://leetcode.cn/problems/majority-element/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (66.37%) | 2225  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/divide-and-conquer" title="https://leetcode.com/tag/divide-and-conquer" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/bit-manipulation" title="https://leetcode.com/tag/bit-manipulation" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个大小为 `n` 的数组 `nums` ，返回其中的多数元素。多数元素是指在数组中出现次数 **大于** `⌊ n/2 ⌋` 的元素。

你可以假设数组是非空的，并且给定的数组总是存在多数元素。

 

**示例 1：**

```
输入：nums = [3,2,3]
输出：3
```

**示例 2：**

```
输入：nums = [2,2,1,1,1,2,2]
输出：2
```

> **My solution**

使用`unordered_map`

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        unordered_map<int, int> maps;
        for (int i = 0;i < nums.size();++i) {
            if (maps.count(nums[i]) == 0) {
                maps[nums[i]] = 1;
            } else {
                maps[nums[i]]++;
            }
            if (maps[nums[i]] > (nums.size()/ 2)) {
                return nums[i];
            }
        }
        return -1;
    }
};
```

> **Official solution**

官解给了很多方法，挑了一种。

思路：先对数组排序，下表为`n/2`的元素一定是众数。

```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        return nums[nums.size() / 2];
    }
};
```



### 718.[最长重复子数组](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (56.72%) | 1090  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/hash-table" title="https://leetcode.com/tag/hash-table" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/binary-search" title="https://leetcode.com/tag/binary-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给两个整数数组 `nums1` 和 `nums2` ，返回 *两个数组中 **公共的** 、长度最长的子数组的长度* 。

 

**示例 1：**

```
输入：nums1 = [1,2,3,2,1], nums2 = [3,2,1,4,7]
输出：3
解释：长度最长的公共子数组是 [3,2,1] 。
```

**示例 2：**

```
输入：nums1 = [0,0,0,0,0], nums2 = [0,0,0,0,0]
输出：5
```

> **My solution**

这题我的思路是*暴力解法* 没解出来。

```cpp
class Solution {
public:
    int findLength(vector<int>& nums1, vector<int>& nums2) {
        int res = 0;
        for (int i = 0;i < nums1.size();++i) {
            int tem = i;
            int j = 0;
            cout << "\n i is " << i << endl;
            while (j < nums2.size() && tem < nums1.size()) {
                if (nums2[j] == nums1[tem]) {
                    ++j;
                    ++tem;
                } else {
                    res = max(res, tem - i);
                    cout << "i is " << i << "  j is " << j << "  tem is " << tem << "  res is " << res << endl;
                    tem = i;
                    ++j;
                    
                }
            }
            res = max(res, tem - i);
        }
        return res;
    }
};
```

> **Official solution**

官方题解给了两种方案：*滑动窗口*和*动态规划*

* 滑动窗口

  把两个数组按照不同偏移对齐，接着一次遍历找出相同队列。

  但是要**注意**，需要进行**两个方向**的偏移对齐。

  ```cpp
  class Solution {
  public:
      int findLength(vector<int>& nums1, vector<int>& nums2) {
          int res = 0;
          for (int offset = 0;offset < nums1.size();++offset) {
              // cout << "offset is " << offset << endl;
              res = max(res, getMaxCommon(nums1, nums2, offset));
          }
          for (int offset = 0;offset < nums1.size();++offset) {
              res = max(res, getMaxCommon(nums2, nums1, offset));
          }
          return res;
  };
    
    int getMaxCommon(vector<int>& nums1, vector<int>& nums2, int offset) {
      int res = 0;
      int i = offset, j = 0, tem = 0;
      while (i < nums1.size() && j < nums2.size()) {
        if (nums1[i++] == nums2[j++]) {
          // cout << "nums[i-1] is " << nums1[i-1] << "\tnums[j-1] is " << nums2[j-1] << endl;
          tem++;
        } else {
          // cout << "i is " << i << "\tj is " << j << "\ttem is " << tem << endl;
          res = max(res, tem);
          tem = 0;
        }
      }
      res = max(res, tem);
      return res;
    }
  ```

* 动态规划

  这题的思路和`1147.最长公共序列`类似，比那个简单些。

  关键的是递推公式

  ```cpp
  if (nums1[i-1] == nums2[j-1]) {
    dp[i][j] = dp[i-1][j-1] + 1;
  }
  ```

  *完整代码*

  ```cpp
  class Solution {
  public:
      int findLength(vector<int>& nums1, vector<int>& nums2) {
  
          // * dyanmic programming
          int res = 0;
          // * determine the meaning of dp array
          // ? dp[i][j] represents the max length between two sequence ending with nums1[i-1] and nums2[j-1] respectively
          int n1 = nums1.size(), n2 = nums2.size();
          vector<vector<int>> dp(n1+1, vector<int>(n2 + 1, 0));
  
          // * initiate the dp array
          for (int i = 0;i < n1;++i) {
              dp[i][0] = 0;
          }
          for (int j = 0;j < n2;++j) {
              dp[0][j] = 0;
          }
  
          // * deceive the recurrence formula
          for (int i = 1;i <= n1;++i) {
              for (int j = 1;j <= n2;++j) {
                  if (nums1[i-1] == nums2[j-1]) {
                      dp[i][j] = dp[i-1][j-1] + 1;
                      res = max(res, dp[i][j]);    
                  }
              }
          }
          return res;
      }
  };
  ```

### 179.[最大数*](https://leetcode.cn/problems/largest-number/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (41.07%) | 1275  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/sort" title="https://leetcode.com/tag/sort" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一组非负整数 `nums`，重新排列每个数的顺序（每个数不可拆分）使之组成一个最大的整数。

**注意：**输出结果可能非常大，所以你需要返回一个字符串而不是整数。

 

**示例 1：**

```
输入：nums = [10,2]
输出："210"
```

**示例 2：**

```
输入：nums = [3,30,34,5,9]
输出："9534330"
```

> **My solution**

暴力解法。

每次遍历都找出一个“最大”的数。实现了一个*比较*函数来确定两个数的“大小”

```cpp
int compare(vector<int>& nums, int a, int b) {
        string a_str = to_string(nums[a]);
        string b_str = to_string(nums[b]);
        int a_tem = nums[a];
        int b_tem = nums[b];
        int sub = a_str.length() - b_str.length();
        if (sub > 0) {
            for (int i = 0;i < sub;++i) {
                b_tem = b_tem * 10;
            }
        } else {
            for (int i = 0;i < abs(sub);++i) {
                a_tem = a_tem * 10;
            }
        }
        if (a_tem == b_tem) {
            return sub > 0 ? b : a;
        }
        return a_tem > b_tem ? a : b;
    }
```

后面提交发现，这种判断“大小”的方法只能解决两个数没有相同数字的情况。其他情况，如测试用例中的`[111311, 1113]`就无法得到正确答案。

*完整代码*

```cpp
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        // * 24.Jul.4
        // string res = "";
        // while (nums.size() != 0) {
        //     int max_idx = 0;
        //     for (int i = 0;i < nums.size();++i) {
        //         max_idx = compare(nums, max_idx, i);
        //     }
        //     res = res + to_string(nums[max_idx]);
        //     nums.erase(nums.begin() + max_idx);
        // }
        // return res;
    }

    int compare(vector<int>& nums, int a, int b) {
        string a_str = to_string(nums[a]);
        string b_str = to_string(nums[b]);
        int a_tem = nums[a];
        int b_tem = nums[b];
        int sub = a_str.length() - b_str.length();
        if (sub > 0) {
            for (int i = 0;i < sub;++i) {
                b_tem = b_tem * 10;
            }
        } else {
            for (int i = 0;i < abs(sub);++i) {
                a_tem = a_tem * 10;
            }
        }
        if (a_tem == b_tem) {
            return sub > 0 ? b : a;
        }
        return a_tem > b_tem ? a : b;
    }
};
```

> **Official solution**

官解是把两个数字组合到一起再进行比较。

很难想，说实话。

```cpp
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        // * 24.Jul.4

        sort(nums.begin(), nums.end(), [](const int &x, const int &y) {
            return to_string(x) + to_string(y) > to_string(y) + to_string(x);
        });
        if (nums[0] == 0) {
            return "0";
        }
        string ret;
        for (int &x : nums) {
            ret += to_string(x);
        }
        return ret;

    }
};
```

### 62.[不同路径](https://leetcode.cn/problems/unique-paths/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (68.30%) | 2041  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

一个机器人位于一个 `m x n` 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

问总共有多少条不同的路径？

 

**示例 1：**

![img](https://pic.leetcode.cn/1697422740-adxmsI-image.png)

```
输入：m = 3, n = 7
输出：28
```

**示例 2：**

```
输入：m = 3, n = 2
输出：3
解释：
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右
3. 向下 -> 向右 -> 向下
```

**示例 3：**

```
输入：m = 7, n = 3
输出：28
```

**示例 4：**

```
输入：m = 3, n = 3
输出：6
```

> **My solution**

经典的动态规划题。

直接动态规划五部曲。

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        // * 24.Jul.7
        // * dynamic programming
        // * determine the meaning of dp array
        // ? dp[i][j] means the number of ways to get grid[m][n]
        vector<vector<int>> dp(m+1, vector<int>(n+1, 0));

        // * initiate the dp array
        for (int i = 0;i <= m;++i) {
            dp[i][0] = 1;
        }
        for (int i = 0;i <= n;++i) {
            dp[0][i] = 1;
        }

        // * determine the recurrence formula
        for (int i = 1;i < m;++i) {
            for (int j = 1;j < n;++j) {
                dp[i][j] = dp[i-1][j] + dp[i][j-1]; 
            }
        }
        return dp[m-1][n-1];
    }
};
```

### 227.[基本计算器 II*](https://leetcode.cn/problems/basic-calculator-ii/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (44.95%) |  775  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/string" title="https://leetcode.com/tag/string" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个字符串表达式 `s` ，请你实现一个基本计算器来计算并返回它的值。

整数除法仅保留整数部分。

你可以假设给定的表达式总是有效的。所有中间结果将在 `[-231, 231 - 1]` 的范围内。

**注意：**不允许使用任何将字符串作为数学表达式计算的内置函数，比如 `eval()` 。

 

**示例 1：**

```
输入：s = "3+2*2"
输出：7
```

**示例 2：**

```
输入：s = " 3/2 "
输出：1
```

**示例 3：**

```
输入：s = " 3+5 / 2 "
输出：5
```

> **My solution**

这题被我想得太复杂了，想到了之前的*后缀表达式*，用栈来解决，但其实不用。

下面代码最后也是超时了。

*完整代码*

```cpp
class Solution {
public:
    int calculate(string s) {
        // * 24.Jul.7
        stack<char> st;
        stack<long> nums;
        bool hasOperator = false;

        for (int i = 0;i < s.length();++i) {
            long tem;
            long tem2;
            if (isNumber(s[i])) {
                // cout << "i is " << i << endl;
                long tem = getNum(s, i);
                --i;
                // cout << "i is " << i << endl;
                // cout << "tem is " << tem << endl;
                nums.push(tem);
                // cout << "nums.size() is " << nums.size() << endl;
            } else {
                if (s[i] != ' ') {
                    switch (s[i])
                    {
                    case '+':
                        if (!st.empty() && (st.top() == '+' || st.top() == '-')) {
                            tem = nums.top();
                            nums.pop();
                            tem2 = nums.top();
                            nums.pop();
                            if (st.top() == '+') {
                                st.pop();
                                nums.push(tem + tem2);
                            } else if (st.top() == '-') {
                                st.pop();
                                nums.push(tem2 - tem);
                            }
                        }
                        st.push(s[i]);
                        
                        continue;;
                    case '-':
                        if (!st.empty() && (st.top() == '+' || st.top() == '-')) {
                                tem = nums.top();
                                nums.pop();
                                tem2 = nums.top();
                                nums.pop();
                                if (st.top() == '+') {
                                    st.pop();
                                    nums.push(tem + tem2);
                                } else if (st.top() == '-') {
                                    st.pop();
                                    nums.push(tem2 - tem);
                                }
                            }
                            st.push(s[i]);
                            
                        continue;;
                    case '*':
                        i = i+1;
                        tem = getNum(s, i);
                        --i;
                        tem2 = nums.top();
                        nums.pop();
                        nums.push(tem * tem2);
                        // cout << "nums.size() is " << nums.size() << endl;
                        continue;;
                    case '/':
                        i = i+1;
                        // cout << "i is " << i <<endl;
                        tem = getNum(s, i);
                        --i;
                        // cout << "i is " << i <<endl;
                        tem2 = nums.top();
                        nums.pop();
                        // cout << "nums.size() is " << nums.size() << endl;
                        // cout << "tem is " << tem << "\ttem2 is " << tem2 << endl;
                        nums.push(tem2 / tem);
                        // cout << "nums.size() is " << nums.size() << endl;
                        continue;
                    default:
                        break;
                    }
                }
            }
        }
        cout << "st.size() is " << st.size() << endl;
        cout << "nums.size() is " << nums.size() << endl;
        while (st.size() >= 1) {
            char fun = st.top();
            st.pop();
            long res;
            long tem = nums.top();
            nums.pop();
            long tem2 = nums.top();
            nums.pop();
            switch (fun)
            {
            case '+':
                res = tem + tem2;
                // cout << "tem is " << tem << "\ttem2 is " << tem2 << endl;
                nums.push(res);
                continue;
            case '-':
                res = tem2 - tem;
                nums.push(res);
                continue;
            default:
                cout << "execute to default" << endl;
                break;
            }
        }

        return nums.top();

        
    }

    bool isNumber(char c) {
        if (c != ' ' && c != '+' && c != '-' && c != '*' && c != '/') {
            return true;
        } else {
            return false;
        }
    }
    
    long getNum(string s, int& i){
        string res = "";
        for (;i < s.length();++i) {
            if (s[i] == ' ') {
                continue;
            }
            if (s[i] >= '0' && s[i] <= '9') {
                res.push_back(s[i]);
            } else {
                break;
            }
        }
        // reverse(res.begin(), res.end());
        return atol(res.c_str());
    }
};
```

> **Official solution**

官方题解想法简单的多，因为题目中的算式只有*空格*，*操作符*和*整数*。所以官方题解就先把算式中的乘除法提前计算，并将结果替换，最后统一相加得到结果。

总结以下几点是我没有想到的：

* 每次遍历到*操作符*，处理的是上一个*操作符*
* 除了遇到*操作符*时要处理，遍历到字符串末尾（`i == n-1`）时也要处理
* 当*操作符*是**符号**时，向结果队列中推入**负数**

*完整代码*

```cpp
class Solution {
public:
    int calculate(string s) {
        // 24.Jul.7     official solution
        int num = 0;
        vector<int> adds;
        char presign = '+';
        int n = s.length();
        for (int i = 0;i < n;++i) {
            if (isdigit(s[i])) {
                num  = num * 10 + (s[i] - '0');
            }
            if (!isdigit(s[i]) && s[i] != ' ' || i == n-1) {
                switch (presign) {
                case '+':
                    adds.push_back(num);
                    num = 0;
                    break;;
                case '-':
                    adds.push_back(-num);
                    num = 0;
                    break;;
                case '*':
                    adds.back() = adds.back() * num;
                    num = 0;
                    break;;
                default:
                    adds.back() = adds.back() / num;
                    num = 0;
                    break;;
                }
                presign = s[i];
            }
        }
        int res = accumulate(adds.begin(), adds.end(), 0);
        return res;
    }
};
```

### 122.[买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (73.68%) | 2506  |    -     |

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/greedy" title="https://leetcode.com/tag/greedy" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(191, 189, 182); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数数组 `prices` ，其中 `prices[i]` 表示某支股票第 `i` 天的价格。

在每一天，你可以决定是否购买和/或出售股票。你在任何时候 **最多** 只能持有 **一股** 股票。你也可以先购买，然后在 **同一天** 出售。

返回 *你能获得的 **最大** 利润* 。

 

**示例 1：**

```
输入：prices = [7,1,5,3,6,4]
输出：7
解释：在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4。
随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6 - 3 = 3。
最大总利润为 4 + 3 = 7 。
```

**示例 2：**

```
输入：prices = [1,2,3,4,5]
输出：4
解释：在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4。
最大总利润为 4 。
```

**示例 3：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 交易无法获得正利润，所以不参与交易可以获得最大利润，最大利润为 0。
```

> **My solution**

经典老题。

* 贪心算法

  比较今天和明天的股票价格，如果明天的股票价格高于今天，就买入。直接利润加上差值。

  ```cpp
  class Solution {
  public:
      int maxProfit(vector<int>& prices) {
          // * 24.Jul.7
          int profit = 0;
          for (int i = 0;i < prices.size()-1;++i) {
              if (prices[i+1] - prices[i] > 0) {
                  profit += prices[i+1] - prices[i];
              }
          }
          return profit;
          
      }
  };
  ```

* 动态规划

  dp数组是n个大小为2的数组，分别表示*持有股票*和*未持有股票*时手里的现金数量。

  ```cpp
  class Solution {
  public:
      int maxProfit(vector<int>& prices) {
          // * dynamic programming
          // * determine the meaning of dp array
          // ? `dp[i][0]` denotes the money if do not have stack on the i'th day and `dp[i][1]` denotes the opposite
          int n = prices.size();
          vector<vector<int>> dp(n, vector<int>(2, 0));
  
          // * initiate the dp array
          dp[0][0] = 0;
          dp[0][1] = -prices[0];
  
          for (int i = 1;i < prices.size();++i) {
              dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i]);
              dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i]);
          }
  
          return dp[n-1][0];
          
      }
  };
  ```



### 226.[翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (80.51%) | 1826  |    -     |

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一棵二叉树的根节点 `root` ，翻转这棵二叉树，并返回其根节点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/14/invert1-tree.jpg)

```
输入：root = [4,2,7,1,3,6,9]
输出：[4,7,2,9,6,3,1]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/03/14/invert2-tree.jpg)

```
输入：root = [2,1,3]
输出：[2,3,1]
```

**示例 3：**

```
输入：root = []
输出：[]
```

> **My solution**

简单题。但是我又可以把二叉树的几个遍历写法，好好复习一遍了。

* 层序遍历

  ```cpp
  queue<TreeNode*> que;
          if (nullptr != root) que.push(root);
          while (!que.empty()) {
              int size = que.size();
              for (int i = 0;i < size;++i) {
                  TreeNode* cur = que.front();
                  que.pop();
                  if (cur->left) que.push(cur->left);
                  if (cur->right) que.push(cur->right);
                  TreeNode* tem = cur->left;
                  cur->left = cur->right;
                  cur->right = tem;
              }
          }
  ```

* 迭代遍历（一般写法）

  ```cpp
  stack<TreeNode*> st;
          // * 如果本来就是空集，会导致`TreeNode* cur = st.top()`出错
          // if (nullptr != root) st.push(root);
          // TreeNode* cur = st.top();
          // st.pop();
          TreeNode* cur = root;
          while (!st.empty() || nullptr != cur) {
              if (nullptr != cur) {
                  st.push(cur);
                  cur = cur -> left;
              } else if (nullptr == cur) {
                  cur = st.top();
                  st.pop();
                  swap(cur);
                  // * 因为这里调换了左右子树，所有本来遍历右子树应该遍历左子树
                  cur = cur->left;
              }
          }
  ```

* 迭代遍历（通用写法）

  ```cpp
  stack<TreeNode*> st;
  if (nullptr != root) st.push(root);
  while (!st.empty()) {
    TreeNode* cur = st.top();
    st.pop();
    if (nullptr != cur) {
      if (cur->right) st.push(cur->right);
      if (cur->left) st.push(cur->left);
      st.push(cur);
      st.push(nullptr);
    } else if (nullptr == cur) {
      cur = st.top();
      st.pop();
      swap(cur);
    }
  }
  ```

  调整下面代码顺序即可实现*前中后序*，对于本题来说，三种遍历顺序均能通过。

  ```cpp
  if (nullptr != cur) {
    if (cur->right) st.push(cur->right);
    if (cur->left) st.push(cur->left);
    st.push(cur);
    st.push(nullptr);
  ```

### 695.[岛屿的最大面积](https://leetcode.cn/problems/max-area-of-island/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (68.10%) | 1076  |    -     |

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个大小为 `m x n` 的二进制矩阵 `grid` 。

**岛屿** 是由一些相邻的 `1` (代表土地) 构成的组合，这里的「相邻」要求两个 `1` 必须在 **水平或者竖直的四个方向上** 相邻。你可以假设 `grid` 的四个边缘都被 `0`（代表水）包围着。

岛屿的面积是岛上值为 `1` 的单元格的数目。

计算并返回 `grid` 中最大的岛屿面积。如果没有岛屿，则返回面积为 `0` 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/05/01/maxarea1-grid.jpg)

```
输入：grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
输出：6
解释：答案不应该是 11 ，因为岛屿只能包含水平或垂直这四个方向上的 1 。
```

**示例 2：**

```
输入：grid = [[0,0,0,0,0,0,0,0]]
输出：0
```

> **My solution**

经典题。

*深度优先遍历*和*广度优先遍历*的两种

* 深度优先遍历

  和递归的思路差不多，要注意*返回条件*

  ```cpp
  class Solution {
  public:
      int maxAreaOfIsland(vector<vector<int>>& grid) {
          // * 24.Jul.10
          int dir[4][2] = {0, 1, 1, 0, 0, -1, -1, 0};
          int m = grid.size(), n = grid[0].size();
  
          vector<vector<bool>> visited(m, vector<bool>(n, false));
          int res = 0;
  
          for (int i = 0;i < m;++i) {
              for (int j = 0;j < n;++j) {
                  if (grid[i][j] == 1 && !visited[i][j]) {
                      int tem = 0;
                      dfs(grid, visited, dir, i, j, tem);
                      res = max(res, tem);
                  }
              }
          }
          return res;
      }
  
      void dfs(vector<vector<int>>& grid, vector<vector<bool>>& visited, int (*dir)[2], int x, int y, int& res) {
          if (grid[x][y] == 1 && !visited[x][y]) {
              ++res;
          }
          if (grid[x][y] == 0 || visited[x][y]) {
              return;
          }
          visited[x][y] = true;
          for (int i = 0;i < 4;++i) {
              int nextx = x + dir[i][0];
              int nexty = y + dir[i][1];
              if (nextx >= 0 && nextx < grid.size() && nexty >= 0 && nexty < grid[0].size() && !visited[nextx][nexty]) {
                  dfs(grid, visited, dir, nextx, nexty, res);
              }
          }
      }
  	}
  };
  ```

* 广度优先遍历

  和二叉树的层序遍历类似，也是需要用`queue`来管理，但是不需要确定每层的二叉树节点数量。

  ```cpp
  class Solution {
  public:
      int maxAreaOfIsland(vector<vector<int>>& grid) {
          // * 24.Jul.10
          int dir[4][2] = {0, 1, 1, 0, 0, -1, -1, 0};
          int m = grid.size(), n = grid[0].size();
  
          vector<vector<bool>> visited(m, vector<bool>(n, false));
          int res = 0;
          for (int i = 0;i < m;++i) {
              for (int j = 0;j < n;++j) {
                  if (grid[i][j] == 1 && !visited[i][j]) {
                      int tem = 1;
                      bfs(grid, visited, dir, i, j, tem);
                      res = max(res, tem);
                  }
              }
          }
  
          return res;
      }
  
      void bfs(vector<vector<int>>& grid, vector<vector<bool>>& visited, int (*dir)[2], int x, int y, int& res) {
          queue<pair<int, int>> que;
          que.push(pair<int, int>{x, y});
          visited[x][y] = true;
          while (!que.empty()) {
              auto cur = que.front();
              que.pop();
              
              for (int i = 0;i < 4;++i) {
                  int nextx = cur.first + dir[i][0];
                  int nexty = cur.second + dir[i][1];
                  if (nextx >= 0 && nextx < grid.size() && nexty >= 0 && nexty < grid[0].size() && !visited[nextx][nexty]) {
                      if (grid[nextx][nexty] == 1) {
                          que.push(pair<int, int>{nextx, nexty});
                          visited[nextx][nexty] = true;
                          ++res;
                      }
                  }
              }
          }
      }
  ```

### 83.[删除排序链表中的重复元素](https://leetcode.cn/problems/remove-duplicates-from-sorted-list/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (53.89%) | 1135  |    -     |

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/linked-list" title="https://leetcode.com/tag/linked-list" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个已排序的链表的头 `head` ， *删除所有重复的元素，使每个元素只出现一次* 。返回 *已排序的链表* 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/04/list1.jpg)

```
输入：head = [1,1,2]
输出：[1,2]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/01/04/list2.jpg)

```
输入：head = [1,1,2,3,3]
输出：[1,2,3]
```

> **My solution**

简单。

```cpp
class Solution {
public:
    ListNode* deleteDuplicates(ListNode* head) {
        ListNode* hair = new ListNode();
        hair->next = head;
        if (nullptr == head) return head;
        
        ListNode* cur = head;
        while (cur->next) {
            if (cur->val == cur->next->val) {
                cur->next = cur->next->next;
            } else {
                cur = cur->next;
            }
        }
        return hair->next;
    }
};
```

### 152.[乘积最大子数组](https://leetcode.cn/problems/maximum-product-subarray/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (42.61%) | 2265  |    -     |

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数数组 `nums` ，请你找出数组中乘积最大的非空连续子数组（该子数组中至少包含一个数字），并返回该子数组所对应的乘积。

测试用例的答案是一个 **32-位** 整数。

 

**示例 1:**

```
输入: nums = [2,3,-2,4]
输出: 6
解释: 子数组 [2,3] 有最大乘积 6。
```

**示例 2:**

```
输入: nums = [-2,0,-1]
输出: 0
解释: 结果不能为 2, 因为 [-2,-1] 不是子数组。
```

> **My solution**

这题真的是 唉。

本来想用*回溯法*直接暴力解的，但是超时了。

```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        // * 24.Jul.11
        int res = INT_MIN;
        int tem = 1;

        backtrace(nums, tem, res); 
        return res;

    void backtrace(vector<int> nums, int tem, int& res) {
        if (nums.size() == 0) {
            return;
        }
        // cout << "tem is " << tem << endl;
        while(nums.size() != 0) {
            int tem2 = nums[0];
            tem = tem * tem2;
            res = max(res, tem);
            // cout << "res is " << res << endl;
            nums.erase(nums.begin());
            backtrace(nums, 1, res);
        }
    }
};
```

后面又想用动态规划，但是后来再看发现也就是个*暴力解法*

```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        // * 24.Jul.11
        // * dynamic programming
        // * determine the meaning of dp array
        // ? dp[i][j] denotes the max value of the subsequence beginning with nums[i], ending with nums[j]
        int res = INT_MIN;
        int n = nums.size();
        vector<vector<int>> dp(n, vector<int>(n, INT_MIN));

        // * initiate dp array
        for (int i = 0;i < n;++i) {
            dp[i][i] = nums[i];
            res = max(res, dp[i][i]);
        }

        // * determine the recurrence formula
        for (int i = 0;i < n;++i) {
            for (int j = i + 1;j < n;++j) {
                dp[i][j] = dp[i][j-1] * nums[j];
                res = max(res, dp[i][j]);
            }
        }
        return res;
    }
};
```

> **Official solution**

最后还是看了官方题解。

因为数组中会有负数，所有官方题解的*动态规划*用了**两个dp数组**，分别记录最小值和最大值。这样如果`nums[i]`是负数的话，负数乘以记录的最小值（也是负数）就可以得到最大值。

*完整代码*

```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        // * 24.Jul.12
        // * determine the meaning of dp array
        // ? min_array[i] represents the minimum value for subsequence ending with nums[i], max_array[i] denotes the maximum value
        
        // * initiate the dp array. 
        // ? As the initial value is just itself, so just assign the original nums to it
        vector<int> min_array(nums);
        vector<int> max_array(nums);
        int res = 0;
        
        cout << min_array.size() << endl;
        cout << max_array.size() << endl;

        // * deceive the recurrence formula
        for (int i = 1;i < nums.size();++i) {
            // max_array[i] = max(max_array[i], max(max_array[i-1] * nums[i], nums[i]));
            // min_array[i] = min(min_array[i], min(min_array[i-1] * nums[i], nums[i]));
            max_array[i] = max(max_array[i-1] * nums[i], max(min_array[i-1] * nums[i], nums[i]));
            min_array[i] = min(min_array[i-1] * nums[i], min(max_array[i-1] * nums[i], nums[i]));
            // res = max(res, max_array[i]);
        }
        res = *max_element(max_array.begin(), max_array.end());

        return res;
};
```

### 198.[打家劫舍](https://leetcode.cn/problems/house-robber/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (55.17%) | 3015  |    -     |

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。

给定一个代表每个房屋存放金额的非负整数数组，计算你 **不触动警报装置的情况下** ，一夜之内能够偷窃到的最高金额。

 

**示例 1：**

```
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

**示例 2：**

```
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

> **My solution**

这题思路和*买股票*很相似。

就是用一个二维数组来记录*偷*与*不偷*两种状态下，小偷能偷到的最多钱数。

*完整代码*

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        // * 24.Jul.15
        // * determine the meaning of dp array
        // ? dp[i][1] represents having stolen, while dp[i][0] denotes not stolen
        int n = nums.size();
        vector<vector<int>> dp(n, vector<int>(2, 0));
        int res = 0;

        // * initiate the dp array
        dp[0][1] = nums[0];
        dp[0][0] = 0;
        res = nums[0];

        // * determine the recurrence formula
        for (int i = 1;i < n;++i) {
            dp[i][0] = max(dp[i-1][0], dp[i-1][1]);
            dp[i][1] = dp[i-1][0] + nums[i];
            res = max(res, max(dp[i][0], dp[i][1]));
        }

        return res;
    }
}
```

### 337.[打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (61.66%) | 1988  |    -     |

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为 `root` 。

除了 `root` 之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果 **两个直接相连的房子在同一天晚上被打劫** ，房屋将自动报警。

给定二叉树的 `root` 。返回 ***在不触动警报的情况下** ，小偷能够盗取的最高金额* 。

 

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg)

```
输入: root = [3,2,3,null,3,null,1]
输出: 7 
解释: 小偷一晚能够盗取的最高金额 3 + 3 + 1 = 7
```

**示例 2:**

![img](https://assets.leetcode.com/uploads/2021/03/10/rob2-tree.jpg)

```
输入: root = [3,4,5,1,3,null,1]
输出: 9
解释: 小偷一晚能够盗取的最高金额 4 + 5 = 9
```

> **My solution**

再次做这个题可以凭借我自己做出来一点了。

```cpp
class Solution {
public:
    // * memoralize the process
    unordered_map<TreeNode*, int> computed;
    int rob(TreeNode* root) {
        // * 24.Jul.16
        int res = 0;
        if (nullptr == root) return res;

        res = backtrace(root, true);
        return res;
    }

    int backtrace(TreeNode* root, bool steal_able) {
        if (nullptr == root) return 0;
        int res = 0;
        if (computed.count(root) != 0) {
            return computed[root];
        }
        if (steal_able) {
            int steal = backtrace(root->left, false) + backtrace(root->right, false) + root->val;
            int not_steal = backtrace(root->left, true) + backtrace(root->right, true);
            res = max(steal, not_steal);
            // cout << "true, res is " << res << endl;
        } else {
            int not_steal = backtrace(root->left, true) + backtrace(root->right, true);
            res = max(res, not_steal);
            // cout << "false, res is " << res << endl;
        }
        res = max(steal, not_steal);
        computed[root] = max(steal, not_steal);
        return res;
    }
};
```

上面的代码最后超时了，而且无法*剪枝*。无法*剪枝*的原因在于对于当前节点，我无法比较*偷*和*不偷*当前节点所得到的结果。而最后可以得到正确结果是因为对于根节点或者父节点来说，它是知道*偷*与*不偷*的结果的。

> **Official solution**

```cpp
class Solution {
public:
    // * memoralize the process
    unordered_map<TreeNode*, int> computed;
    int rob(TreeNode* root) {
        // * 24.Jul.16
        int res = 0;
        if (nullptr == root) return res;

        res = backtrace(root);
        return res;
    }

    int backtrace(TreeNode* root) {
        if (nullptr == root) return 0;
        int res = 0;
        if (computed.count(root) != 0) {
            return computed[root];
        }
        int steal = root->val;
        if (root->left) {
            steal += backtrace(root->left->left) + backtrace(root->left->right);
        }
        if (root->right) {
            steal += backtrace(root->right->left) + backtrace(root->right->right);
        }

        int not_steal = 0;
        not_steal += backtrace(root->left) + backtrace(root->right);

        res = max(steal, not_steal);
        computed[root] = max(steal, not_steal);
        return res;
    }
};
```

### 912. [排序数组](https://leetcode.cn/problems/sort-an-array/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (49.05%) | 1008  |    -     |

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/binary-search" title="https://leetcode.com/tag/binary-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/random" title="https://leetcode.com/tag/random" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数数组 `nums`，请你将该数组升序排列。

 



**示例 1：**

```
输入：nums = [5,2,3,1]
输出：[1,2,3,5]
```

**示例 2：**

```
输入：nums = [5,1,1,2,0,0]
输出：[0,0,1,1,2,5]
```

> **My solution**

这次使用的是*堆排序*，大体分成两个步骤

* 构建大顶堆

  构建时，是从下往上，从右往左，从第一个*非叶子节点*开始。如果有变动（父子结点交换）则继续检查被交换后的子结点

* 排序

  排序的过程就是不断地将第一个元素（顶）换到最后一个位置，再检查第一个元素（原末尾元素）。⚠️此时数组长度要做减法

上面的两个过程用到的`adjustHeap()`方法都相同。

```cpp
void adjustHeap(vector<int>& nums, int i, int len) {
        int tem = nums[i];
        int k = i * 2 + 1;
        while (k < len) {
        // for(int k=i*2+1;k<len;k=k*2+1){
            if ((k + 1 < len) && (nums[k] < nums[k+1])) {
                ++k;
            }
            if (nums[k] > tem) {
                nums[i] = nums[k];
                i = k;
                k = 2 * k + 1;
            } else {
                break;
            }
        }
        nums[i] = tem;
    }
```

*完整代码*

```cpp
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        // * 24.Jul.17
        int n = nums.size();
        for (int i = n/2 - 1;i >= 0;--i) {
            adjustHeap(nums, i, n);
            // cout << "i is " << i << endl;
            // for (auto& x : nums) {
            //     cout << x << " ";
            // }
            // cout << endl;
        }

        for (int j = n - 1;j > 0;--j) {
            swap(nums, 0, j);
            adjustHeap(nums, 0, j);
        }

        return nums;
    }

    void adjustHeap(vector<int>& nums, int i, int len) {
        int tem = nums[i];
        int k = i * 2 + 1;
        while (k < len) {
        // for(int k=i*2+1;k<len;k=k*2+1){
            if ((k + 1 < len) && (nums[k] < nums[k+1])) {
                ++k;
            }
            if (nums[k] > tem) {
                nums[i] = nums[k];
                i = k;
                k = 2 * k + 1;
            } else {
                break;
            }
        }
        nums[i] = tem;
    }

    void swap(vector<int>& nums, int i, int j) {
        int tem = nums[i];
        nums[i] = nums[j];
        nums[j] = tem;
    }
};
```

### 24.[两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (72.50%) | 2239  |    -     |

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/linked-list" title="https://leetcode.com/tag/linked-list" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/swap_ex1.jpg)

```
输入：head = [1,2,3,4]
输出：[2,1,4,3]
```

**示例 2：**

```
输入：head = []
输出：[]
```

**示例 3：**

```
输入：head = [1]
输出：[1]
```

> **My solution**

简单，轻轻松松。

```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        // * 24.Jul.17
        ListNode* hair = new ListNode();
        hair->next = head;
        if (nullptr == head) {
            return head;
        }
        ListNode* pre = hair;
        ListNode* cur = head;
        ListNode* cur_next = head->next;
        while (nullptr != cur && nullptr != cur_next) {
            cur->next = cur_next->next;
            cur_next->next = cur;
            pre->next = cur_next;
            pre = cur;

            cur = cur->next;
            if (cur) cur_next = cur->next;
        }
        return hair->next;
    }
};
```

### 560.[和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (44.15%) | 2406  |    -     |

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/hash-table" title="https://leetcode.com/tag/hash-table" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 *该数组中和为 `k` 的子数组的个数* 。

子数组是数组中元素的连续非空序列。

 

**示例 1：**

```
输入：nums = [1,1,1], k = 2
输出：2
```

**示例 2：**

```
输入：nums = [1,2,3], k = 3
输出：2
```

> **My solution**

本来想用*回溯法*，后面发现*回溯法*解决不了，因为题目要求的是**连续非空序列**。*回溯法*是相当于树结构，所以向下递归到右子树的话就构不成*连续非空序列*

```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        // * 24.Jul.21
        int res = 0;
        int sub_sum = 0;
        backtrace(nums, res, k);
        return res;
    }

    void backtrace(vector<int> nums, int& res, int k) {
        if (k == 0) {
            ++res;
            return;
        }
        if (k < 0) {
            return;
        }
        while (!nums.empty()) {
            cout << "k is " << k << endl;
            for (auto& x : nums) {
                cout << x << " ";
            }
            cout << endl;
            int tem = nums[0];
            k = k - tem;
            nums.erase(nums.begin());
            backtrace(nums, res, k);
            k += tem;
        }
    }
};
```

后面又使用*暴力解法*，显然超时了

```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        // * 24.Jul.21

        int res = 0;
        for (int i = 0;i < nums.size();++i) {
            int sum = 0;
            for (int j = i;j >= 0;--j) {
                sum += nums[j];
                if (sum == k) ++res;
            }
        }
        return res;

    }
};
```

> **Official solution**

这是一道*前缀和*的题目。

假设`pre[i]`表示前i个数的总和，那么`pre[j]-pre[i]`就是`nums[i-j]`连续序列的和。

官方解法中还加入了*哈希表*来优化。

```cpp
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        // * 24.Jul.21
        unordered_map<int, int> mp;
        int sum = 0;
        int count = 0;
        mp[0] = 1;
        for (int i = 0;i < nums.size();++i) {
            sum += nums[i];
            if (mp.find(sum-k) != mp.end()) {
                count += mp[sum-k];
            } 
            mp[sum]++;
        }
        return count;

    }

};
```

### 209.[长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (46.50%) | 2174  |    -     |

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/two-pointers" title="https://leetcode.com/tag/two-pointers" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/binary-search" title="https://leetcode.com/tag/binary-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个含有 `n` 个正整数的数组和一个正整数 `target` **。**

找出该数组中满足其总和大于等于 `target` 的长度最小的 **子数组** `[numsl, numsl+1, ..., numsr-1, numsr]` ，并返回其长度**。**如果不存在符合条件的子数组，返回 `0` 。

 

**示例 1：**

```
输入：target = 7, nums = [2,3,1,2,4,3]
输出：2
解释：子数组 [4,3] 是该条件下的长度最小的子数组。
```

**示例 2：**

```
输入：target = 4, nums = [1,4,4]
输出：1
```

**示例 3：**

```
输入：target = 11, nums = [1,1,1,1,1,1,1,1]
输出：0
```

> **My solution**

这题乍一看是一个*前缀和*题目，但其实不是，因为题目要求的子数组和*只要大于*`target`即可。而*560*题中要求的是`等于`

首先想到的还是*回溯法*，但是不对，因为子数组必须是连续的。

*暴力解法*

```cpp
vector<int> pre(nums.size(), 0);
        // unordered_map<int, int> mp;

        int sum = 0;
        bool exist = false;
        int mini_len = nums.size();
        for (int i = 0;i < nums.size();++i) {
            sum += nums[i];
            pre[i] = sum;
            int k = -1;
            if (sum - target >= 0) {
                exist = true;
                for (int j = 0;j <= i;++j) {
                    if (pre[i] - pre[j] >= target) {
                        k = j;
                        // cout << "k is " << k << endl;
                    }
                }
                // cout << "i is " << i << endl;
                mini_len = min(mini_len, i - k);
            }
        }
        if (exist) {
            return mini_len;
        } else {
            return 0;
        }
```

> **Official solution**

*哈希表*+*二分*

```cpp
int n = nums.size();
if (n == 0) {
  return 0;
}
int ans = INT_MAX;
vector<int> sums(n + 1, 0); 
// 为了方便计算，令 size = n + 1 
// sums[0] = 0 意味着前 0 个元素的前缀和为 0
// sums[1] = A[0] 前 1 个元素的前缀和为 A[0]
// 以此类推
for (int i = 1; i <= n; i++) {
  sums[i] = sums[i - 1] + nums[i - 1];
}
for (int i = 1; i <= n; i++) {
  int target = s + sums[i - 1];
  auto bound = lower_bound(sums.begin(), sums.end(), target);
  if (bound != sums.end()) {
    ans = min(ans, static_cast<int>((bound - sums.begin()) - (i - 1)));
  }
}
return ans == INT_MAX ? 0 : ans;
```

*滑动窗口*

```cpp
int start = 0, end = 0;
int sum = 0;
int res = __INT_MAX__;
for (;end < nums.size();++end) {
  sum += nums[end];
  while (sum >= target) {
    res = min(res, end - start + 1);
    sum -= nums[start];
    ++start;
  }
}
return res == __INT_MAX__ ? 0 : res;
```

### 662.[二叉树最大宽度](https://leetcode.cn/problems/maximum-width-of-binary-tree/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (44.02%) |  642  |    -     |

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(51, 51, 51); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一棵二叉树的根节点 `root` ，返回树的 **最大宽度** 。

树的 **最大宽度** 是所有层中最大的 **宽度** 。

每一层的 **宽度** 被定义为该层最左和最右的非空节点（即，两个端点）之间的长度。将这个二叉树视作与满二叉树结构相同，两端点间会出现一些延伸到这一层的 `null` 节点，这些 `null` 节点也计入长度。

题目数据保证答案将会在 **32 位** 带符号整数范围内。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/05/03/width1-tree.jpg)

```
输入：root = [1,3,2,5,3,null,9]
输出：4
解释：最大宽度出现在树的第 3 层，宽度为 4 (5,3,null,9) 。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2022/03/14/maximum-width-of-binary-tree-v3.jpg)

```
输入：root = [1,3,2,5,null,null,9,6,null,7]
输出：7
解释：最大宽度出现在树的第 4 层，宽度为 7 (6,null,null,null,null,null,7) 。
```

**示例 3：**

![img](https://assets.leetcode.com/uploads/2021/05/03/width3-tree.jpg)

```
输入：root = [1,3,2,5]
输出：2
解释：最大宽度出现在树的第 2 层，宽度为 2 (3,2) 。
```

> **Official solution**

把*树结构*想象成用数组存储，每次遍历到下一层就记录该层在数组中的最小值，并每次计算每层的最大宽度。

*广度遍历*

```cpp
class Solution {
public:
    unordered_map<int, unsigned long long> mp;

    int widthOfBinaryTree(TreeNode* root) {
        // * 24.Jul.25
        vector<pair<TreeNode*, unsigned long long>> vec;
        if (nullptr != root) vec.push_back({root, 1});
        while (!vec.empty()) {
            vector<pair<TreeNode*, unsigned long long>> tem;
            for (auto [node, index] : vec) {
                if (node->left) {
                    tem.push_back({node->left, 2 * index});
                }
                if (node->right) {
                    tem.push_back({node->right, 2 * index + 1});
                }
            }
            res = max(res, vec.back().second - vec[0].second + 1);
            vec = move(tem);
        }
        return res;
    }
};
```

*深度遍历*

```cpp
class Solution {
public:
    unordered_map<int, unsigned long long> mp;

    int widthOfBinaryTree(TreeNode* root) {
        // * 24.Jul.25
        int depth = 1;
        
        unsigned long long res = 1;
        return dfs(root, depth, 1ll);
    }

    unsigned long long dfs(TreeNode* root, int depth, unsigned long long index) {
        if (root == nullptr) {
            return 0ll;
        }
        if (mp.count(depth) == 0) mp[depth] = index;
        unsigned long long left = dfs(root->left, depth+1, index * 2);
        unsigned long long right = dfs(root->right, depth+1, index * 2 + 1);
        unsigned long long res = max(left, right);
        res = max(index - mp[depth] + 1, res);
        return res;
    }
};
```

做完之后想想，其实就是在*广度遍历*和*深度遍历*中加入记录当前层节点序号的代码



## 哈希表

### 242.[有效的字母异位词](https://leetcode.cn/problems/valid-anagram/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (66.37%) |  914  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/hash-table" title="https://leetcode.com/tag/hash-table" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/sort" title="https://leetcode.com/tag/sort" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定两个字符串 `*s*` 和 `*t*` ，编写一个函数来判断 `*t*` 是否是 `*s*` 的字母异位词。

**注意：**若 `*s*` 和 `*t*` 中每个字符出现的次数都相同，则称 `*s*` 和 `*t*` 互为字母异位词。

 

**示例 1:**

```
输入: s = "anagram", t = "nagaram"
输出: true
```

**示例 2:**

```
输入: s = "rat", t = "car"
输出: false
```

**My solution**

这里本来的想法是用个`unordered_map`来记录每个每个字母出现次数。

由于本题特殊，可以用一个**数组**对字符串中的字母映射。

```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        int sn = s.length();
        int tn = t.length();
        vector<int> ss(26,0);
        vector<int> tt(26,0);
        for (int i = 0;i < sn;++i) {
            int index = s[i] - 'a';
            ss[index]++;
        }
        for (int j = 0;j < tn;++j) {
            int index = t[j] - 'a';
            tt[index]++;
        }
        for (int i = 0;i < 26;++i) {
            if (ss[i] != tt[i]) {
                return false;
            }
        }
        return true;
    }
};
```

### 349.[两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (74.44%) |  901  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/hash-table" title="https://leetcode.com/tag/hash-table" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/two-pointers" title="https://leetcode.com/tag/two-pointers" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/binary-search" title="https://leetcode.com/tag/binary-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/sort" title="https://leetcode.com/tag/sort" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定两个数组 `nums1` 和 `nums2` ，返回 *它们的 交集* 。输出结果中的每个元素一定是 **唯一** 的。我们可以 **不考虑输出结果的顺序** 。

 

**示例 1：**

```
输入：nums1 = [1,2,2,1], nums2 = [2,2]
输出：[2]
```

**示例 2：**

```
输入：nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出：[9,4]
解释：[4,9] 也是可通过的
```

**My solution**	

使用一个`unordered_map`来记录`nums1`中的元素是否出现，再遍历`nums2`中的元素，若其在`unordered_map`中出现，就要输出，但是要记得一旦nums2中的元素遍历过且在`unordered_map`中出现，就要将其在`unordered_map`中移除。

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int, bool> umap;
        vector<int> res;
        int n1 = nums1.size();
        int n2 = nums2.size();
        for(int i = 0;i < n1;++i) {
            umap[nums1[i]] = true;
        }
        for(int j = 0;j < n2;++j) {
            if (umap[nums2[j]]) {
                umap[nums2[j]] = false;
                res.push_back(nums2[j]);
            }
        }
        return res;
    }
};
```

*代码随想录*解法

利用`unordered_set`中不会有重复元素的特点，直接用`nums`初始化

```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result_set; // 存放结果，之所以用set是为了给结果集去重
        unordered_set<int> nums_set(nums1.begin(), nums1.end());
        for (int num : nums2) {
            // 发现nums2的元素 在nums_set里又出现过
            if (nums_set.find(num) != nums_set.end()) {
                result_set.insert(num);
            }
        }
        return vector<int>(result_set.begin(), result_set.end());
    }
};
```



























## 回溯法

**模版**

```cpp
void backtracking(参数) {
    if (终止条件) {
        存放结果;
        return;
    }

    for (选择：本层集合中元素（树中节点孩子的数量就是集合的大小）) {
        处理节点;
        backtracking(路径，选择列表); // 递归
        回溯，撤销处理结果
    }
}
```

### 77.[组合](https://leetcode.cn/problems/combinations/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (77.04%) | 1597  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/backtracking" title="https://leetcode.com/tag/backtracking" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定两个整数 `n` 和 `k`，返回范围 `[1, n]` 中所有可能的 `k` 个数的组合。

你可以按 **任何顺序** 返回答案。

**示例 1：**

```
输入：n = 4, k = 2
输出：
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

**示例 2：**

```
输入：n = 1, k = 1
输出：[[1]]
```



**My solution**

这题的对我来说有两个难点：

* 如何保存结果并返回

  除了声明一个`vector<vector<int>> res`之外，还需要声明一个`vector<int> path`，用`path`来保存每个叶节点的结果。

* 边界条件（终止条件）

  终止条件并不是*待选择的元素数量小于k*，应该是**`path.size() == k`**

* 如何处理节点

  对我来说，我不太清晰具体如何对`path`操作，正确的操作应该是先`path.push_back()`，接着进行**回溯**，再`path.pop_back()`，**撤销处理结果**。

```cpp
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> res;
        vector<int> path;
        vector<int> nums;
        

        for (int i = 1; i <= n; ++i) {
            nums.push_back(i);
        }
        backtrace(res, path, nums, k);
        return res;
    }

    void backtrace(vector<vector<int>>& res, vector<int>& path, vector<int> nums, int k) {
        int n = nums.size();
        if (path.size() == k) {
            res.push_back(path);
            return;
        }
      	// 剪枝
        if (nums.size() < (k - path.size())) {
            return;
        }
        while(nums.size() != 0) {
            path.push_back(nums[0]);
            nums.erase(nums.begin());
            backtrace(res, path, nums, k);
            path.pop_back();
        }
    }
};
```

### 216.[组合总和 III](https://leetcode.cn/problems/combination-sum-iii/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (71.15%) |  812  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/backtracking" title="https://leetcode.com/tag/backtracking" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

找出所有相加之和为 `n` 的 `k` 个数的组合，且满足下列条件：

- 只使用数字1到9
- 每个数字 **最多使用一次** 

返回 *所有可能的有效组合的列表* 。该列表不能包含相同的组合两次，组合可以以任何顺序返回。

 

**示例 1:**

```
输入: k = 3, n = 7
输出: [[1,2,4]]
解释:
1 + 2 + 4 = 7
没有其他符合的组合了。
```

**示例 2:**

```
输入: k = 3, n = 9
输出: [[1,2,6], [1,3,5], [2,3,4]]
解释:
1 + 2 + 6 = 9
1 + 3 + 5 = 9
2 + 3 + 4 = 9
没有其他符合的组合了。
```

**示例 3:**

```
输入: k = 4, n = 1
输出: []
解释: 不存在有效的组合。
在[1,9]范围内使用4个不同的数字，我们可以得到的最小和是1+2+3+4 = 10，因为10 > 1，没有有效的组合。
```



**My solution**

和*77题组合*相似，⚠️题目要求**所有相加之和为 `n` 的 `k` 个数的组合**，而不是在n个数里选组合，另外只能在**1-9**中进行选择。

```cpp
class Solution {
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<vector<int>> res;
        vector<int> comb;
        vector<int> nums;

        for(int i = 1; i <= 9; ++i) {
            nums.push_back(i);
        }

        backtrace(res, comb, nums, k, n);
        return res;
    }

    void backtrace(vector<vector<int>>& res, vector<int>& path, vector<int> nums, int k, int n) {
        if (accumulate(path.begin(), path.end(), 0) == n && path.size() == k) {
            res.push_back(path);
            return;
        }

        while(nums.size() != 0) {
            path.push_back(nums[0]);
            nums.erase(nums.begin());
            backtrace(res, path, nums, k, n);
            path.pop_back();
        }
    }
};
```

### 17.[电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (59.10%) | 2778  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/string" title="https://leetcode.com/tag/string" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/backtracking" title="https://leetcode.com/tag/backtracking" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。答案可以按 **任意顺序** 返回。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2021/11/09/200px-telephone-keypad2svg.png)

 

**示例 1：**

```
输入：digits = "23"
输出：["ad","ae","af","bd","be","bf","cd","ce","cf"]
```

**示例 2：**

```
输入：digits = ""
输出：[]
```

**示例 3：**

```
输入：digits = "2"
输出：["a","b","c"]
```



**My solution**

需要注意的点是：

1. 定义全局变量`map<string, string>`是*const 对象*，而操作符`[]`是**非const成员函数**无法访问map元素，故需使用`at()`函数。
2. 如何将单个字符转变为`string`，不能用`to_string()`因为其是将`int`类型转换为`string`
3. ⚠️向树的深层次遍历用**递归**遍历，而不是`for`循环，`for`只在同一层次遍历

```cpp
class Solution {
public:
    map<string, string> lettermap{
        {"2", "abc"},
        {"3", "def"},
        {"4", "ghi"},
        {"5", "jkl"},
        {"6", "mno"},
        {"7", "pqrs"},
        {"8", "tuv"},
        {"9", "wxyz"}
    };

    vector<string> letterCombinations(string digits) {
        int n = digits.length();
        vector<string> res;
        string comba("");

        backtrace(digits, res, comba);
        return res;
    }

    void backtrace(string digits, vector<string>& res, string& comba) {
        if (digits.length() == 0) {
            res.push_back(comba);
            return;
        }

        string index{digits[0]};
        string letters = lettermap[index];
        for (int j = 0; j < letters.length(); ++j) {
            comba = comba + letters[j];
            backtrace(digits.substr(1), res, comba);
            comba = comba.substr(0, comba.length()-1);
        }
        // for (int i = 0;i < digits.length();++i) {
        //     string letters = lettermap[to_string(digits[i])];
        //     for (int j = 0; j < letters.length(); j++) {
        //         comba = comba + letters[j];
        //         backtrace(digits.substr(i+1), res, comba);
        //         comba = comba.substr(0, comba.length() - 1);
        //     }
        // }


    }

};
```

### 39.[组合总和](https://leetcode.cn/problems/combination-sum/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (72.44%) | 2741  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/backtracking" title="https://leetcode.com/tag/backtracking" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个 **无重复元素** 的整数数组 `candidates` 和一个目标整数 `target` ，找出 `candidates` 中可以使数字和为目标数 `target` 的 所有 **不同组合** ，并以列表形式返回。你可以按 **任意顺序** 返回这些组合。

`candidates` 中的 **同一个** 数字可以 **无限制重复被选取** 。如果至少一个数字的被选数量不同，则两种组合是不同的。 

对于给定的输入，保证和为 `target` 的不同组合数少于 `150` 个。

 

**示例 1：**

```
输入：candidates = [2,3,6,7], target = 7
输出：[[2,2,3],[7]]
解释：
2 和 3 可以形成一组候选，2 + 2 + 3 = 7 。注意 2 可以使用多次。
7 也是一个候选， 7 = 7 。
仅有这两种组合。
```

**示例 2：**

```
输入: candidates = [2,3,5], target = 8
输出: [[2,2,2,2],[2,3,3],[3,5]]
```

**示例 3：**

```
输入: candidates = [2], target = 1
输出: []
```



**My solution**

需要注意的是：

由于题目描述中提到

> 至少一个数字的被选数量不同，则两种组合是不同的。

所以在同一层的遍历中，**已经选用过的元素就要在下一轮`for`循环中剔除**

```cpp
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> res;
        vector<int> sequence;

        backtrace(candidates, res, sequence, target);

        return res;
        
    }

    void backtrace(vector<int> candidates, vector<vector<int>>& res, vector<int>& sequence, int target) {
        // if (::accumulate(sequence.begin(), sequence.end(), 0) == target) {
        //     res.push_back(sequence);
        //     return;
        // }
        if (target == 0) {
            res.push_back(sequence);
            return;
        }
        if (target < 0) {
            return;
        }

        // for (auto i : candidates){
        //     sequence.push_back(i);
        //     backtrace(candidates, res, sequence, target - i);
        //     sequence.pop_back();
        // }
        while(candidates.size() != 0) {
            sequence.push_back(candidates[0]);
            backtrace(candidates, res, sequence, target - candidates[0]);
          	// remove element that has been chosen
            candidates.erase(candidates.begin());
            sequence.pop_back();
        }
    }
```

### 131.[分割回文串](https://leetcode.cn/problems/palindrome-partitioning/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (73.49%) | 1748  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/backtracking" title="https://leetcode.com/tag/backtracking" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个字符串 `s`，请你将 `s` 分割成一些子串，使每个子串都是 **回文串** 。返回 `s` 所有可能的分割方案。

 

**示例 1：**

```
输入：s = "aab"
输出：[["a","a","b"],["aa","b"]]
```

**示例 2：**

```
输入：s = "a"
输出：[["a"]]
```



**My solution**

本题首先要用双指针写一个*回文字符串判断*函数

```cpp
bool isback(string s) {
        int i = 0, j = s.length() - 1;
        for (;i < j;++i, --j){
            if (s[i] != s[j]) {
                return false;
            }
        }
        return true;
    }
```

需要注意的点是：

* **切割方式**

  切割是取前i个元素作为字符串进行判断，剩下的字符串作为下一层递归

* 对切割出来的字符串处理

  如果该字符串不是回文字符串就跳过

* **回溯返回后，需撤销操作！**

```cpp
public:
    vector<vector<string>> partition(string s) {
        vector<vector<string>> res;
        vector<string> path;

        backtrace(s, res, path);
        return res;
    }

    void backtrace(string s, vector<vector<string>>& res, vector<string>& path) {
        if (s.length() == 0) {
            res.push_back(path);
            return;
        }
        for (int i = 0; i < s.length(); ++i) {
            string t = s.substr(0, i+1);
            if (!isback(t)) {
                continue;
            }
            if (isback(t)) {
                path.push_back(t);
            }
            backtrace(s.substr(i + 1), res, path);
            path.pop_back();
        }
        

    }

    bool isback(string s) {
        int i = 0, j = s.length() - 1;
        for (;i <= j;++i, --j){
            if (s[i] != s[j]) {
                return false;
            }
        }
        return true;
    }
```



### 93.[复原 IP 地址](https://leetcode.cn/problems/restore-ip-addresses/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (59.01%) | 1403  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/string" title="https://leetcode.com/tag/string" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/backtracking" title="https://leetcode.com/tag/backtracking" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

**有效 IP 地址** 正好由四个整数（每个整数位于 `0` 到 `255` 之间组成，且不能含有前导 `0`），整数之间用 `'.'` 分隔。

- 例如：`"0.1.2.201"` 和` "192.168.1.1"` 是 **有效** IP 地址，但是 `"0.011.255.245"`、`"192.168.1.312"` 和 `"192.168@1.1"` 是 **无效** IP 地址。

给定一个只包含数字的字符串 `s` ，用以表示一个 IP 地址，返回所有可能的**有效 IP 地址**，这些地址可以通过在 `s` 中插入 `'.'` 来形成。你 **不能** 重新排序或删除 `s` 中的任何数字。你可以按 **任何** 顺序返回答案。

 

**示例 1：**

```
输入：s = "25525511135"
输出：["255.255.11.135","255.255.111.35"]
```

**示例 2：**

```
输入：s = "0000"
输出：["0.0.0.0"]
```

**示例 3：**

```
输入：s = "101023"
输出：["1.0.10.23","1.0.102.3","10.1.0.23","10.10.2.3","101.0.2.3"]
```



**My solution**

本题和分割回文串类似，最主要的思想就是对**字符串切割**

* 确定好回溯返回的条件，一般来说就是**到达叶节点**

* 对切割字符串是否符合要求的判断函数，细节会比较多

  ```cpp
  bool isLegal(string s) {
          if (s.length() > 3) {
              return false;
          }
          char c = s[0];
          if (c == '0' && s.length() > 1) {
              return false;
          }
          long long a = stol(s.c_str());
          if (a > 255) {
              return false;
          } else {
              return true;
          }
      }
  ```



```cpp
public:
    vector<string> restoreIpAddresses(string s) {
        vector<string> path;
        vector<string> res;

        backtrace(res, path, s);

        return res;
    }

    void backtrace(vector<string>& res, vector<string>& path, string s) {
        if (s.length() == 0) {
            if (path.size() == 4) {
                string ip{""};
                for (string i : path) {
                    ip += i + ".";
                }
                res.push_back(ip.substr(0, ip.rfind(".")));
            }
        }
        // * 满足ip要求，但是还有字符串没有使用
        if (s.length() !=0 && path.size() == 4) {
            return;
        }

        if (s.length() + path.size() < 4) {
            return;
        }

        for (int i = 0; i < s.length(); ++i) {
            string t = s.substr(0, i+1);
            if (!isLegal(t)) {
                continue;
            }
            path.push_back(t);
            backtrace(res, path, s.substr(i+1));
            path.pop_back();
        }
    }

    bool isLegal(string s) {
        if (s.length() > 3) {
            return false;
        }
        char c = s[0];
        if (c == '0' && s.length() > 1) {
            return false;
        }
        long long a = stol(s.c_str());
        if (a > 255) {
            return false;
        } else {
            return true;
        }
    }
```



### 78.[子集](https://leetcode.cn/problems/subsets/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (81.24%) | 2265  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/backtracking" title="https://leetcode.com/tag/backtracking" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/bit-manipulation" title="https://leetcode.com/tag/bit-manipulation" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。

解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回解集。

 

**示例 1：**

```
输入：nums = [1,2,3]
输出：[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]
```

**示例 2：**

```
输入：nums = [0]
输出：[[],[0]]
```

**My solution**

这题需要注意的是如何去构造一个树结构，以及如何去处理每个节点

* 选择一个元素就代表一个分支，将选择元素放入`subset`数组中，就**直接**添加到`res`数组，也就是说**每个节点都是我们想要的结果**。

  最后记得回溯结束，**撤销操作**

```cpp
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<int> subset;
        vector<int> empty;
        vector<vector<int>> res;

        backtrace(res, subset, nums);

        res.push_back(empty);
        return res;

    }
    // void backtrace(vector<vector<int>>& res, vector<int> nums) {
    //     if (nums.size() == 0) {
    //         return;
    //     }

    //     for (int i = 1; i < nums.size() + 1; ++i) {
    //         for (int j = 0; j < nums.size(); ++j) {
    //             vector<int> a(nums.begin(), nums.begin() + j + 1);
    //             if (a.size() == i) {
    //                 res.push_back(a);
    //             }
    //             vector<int> b(nums.begin() + j + 1, nums.end());
    //             backtrace(res, b, i);
    //         }
    //     } 
    // }

    void backtrace(vector<vector<int>>& res, vector<int>& path, vector<int> nums) {
        if (nums.size() == 0) {
            return;
        }

        while(nums.size() != 0) {
            path.push_back(nums[0]);
            res.push_back(path);
            nums.erase(nums.begin());
            backtrace(res, path, nums);
            path.pop_back();
        }
    }
```

### 47.[全排列 II](https://leetcode.cn/problems/permutations-ii/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (65.89%) | 1597  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/backtracking" title="https://leetcode.com/tag/backtracking" style="color: var(--vscode-textLink-foreground); text-decoration: var(--text-link-decoration);"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个可包含重复数字的序列 `nums` ，***按任意顺序*** 返回所有不重复的全排列。

 

**示例 1：**

```
输入：nums = [1,1,2]
输出：
[[1,1,2],
 [1,2,1],
 [2,1,1]]
```

**示例 2：**

```
输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

**My solution**

类似题目，需要去重的，可以分为*同一层去重*和*整体去重*

> 同一层去重

有两种方法：

1. 使用`startindex`

   ```cpp
   for (int i = startindex;i < n;++i) {
     if (i > startindex && nums[i] == nums[i-1]) continue;
   }
   ```

2. 使用`map<int, bool> used`

   该方法不要使用全局的`used`数组，而是在`backtrace()`中定义一个局部变量来存储信息，需要注意最后回溯的时候**不要设置`used[i] = false`**

> 整体去重

去重需要注意：最后回溯时，**需要设置`used[i] = false`**

## 动态规划

### 322.[零钱兑换](https://leetcode.cn/problems/coin-change/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (47.40%) | 2736  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数数组 `coins` ，表示不同面额的硬币；以及一个整数 `amount` ，表示总金额。

计算并返回可以凑成总金额所需的 **最少的硬币个数** 。如果没有任何一种硬币组合能组成总金额，返回 `-1` 。

你可以认为每种硬币的数量是无限的。

 

**示例 1：**

```
输入：coins = [1, 2, 5], amount = 11
输出：3 
解释：11 = 5 + 5 + 1
```

**示例 2：**

```
输入：coins = [2], amount = 3
输出：-1
```

**示例 3：**

```
输入：coins = [1], amount = 0
输出：0
```

**My solution**

> 回溯法

假设回溯法中运算的函数为`result(nums, i)`，关键点要**想象`result(num,i)`就是状态i的结果**

本题中，状态`result(num, i)`表示的就是`amount=i`时，所需的最小硬币个数。

*回溯法*函数

```cpp
int dp(vector<int>& coins, int amount) {
        if (0 == amount) {
            return 0;
        }
        if (0 > amount) {
            return -1;
        }
        int res = __INT_MAX__;  // * sometimes no solution for dp(coins, amount)
        for (auto i : coins) {
            int subproblem = dp(coins, amount - i);
            if (subproblem == -1) continue;
            res = min(res, subproblem + 1);
        }
        return res == __INT_MAX__ ? -1 : res; // * if no solution, return -1
    }
```

写时需要注意：**有些情况没有硬币组合方式**，这种情况不能忽略！！

> 动态规划法

动态规划法，最重要的是要想明白**每个状态代表的是什么意思**和**状态转移方程**

* *状态含义*

  本题只需要*一维dp数组*，`dp[i]`表示的就是`amount=i`时，所需的最小硬币个数。（**对应回溯法中回溯函数的结果**）

* 状态转移方程

  找寻`dp[i]`和`dp[i-1]`甚至`dp[i-2]`的关系。本题中关系如下：

  `dp[i]`的值就是所有`dp[i-coin]`的最小值**+1**，`+1`是因为有且仅有加上被减去`coin`的硬币才到达到`i=amount`。

⚠️：动态规划的**边界处理**和**初始化**

*动态规划*函数

```cpp
int coinChange(vector<int>& coins, int amount) {
        // vector<int> dp(amount + 1, 0);
        vector<int> dp(amount + 1, amount + 1);
        dp[0] = 0;
        for (int i = 1; i <= amount; ++i) {
            // ! 初始化为这个会在`dp[i-coin]`中溢出
            // dp[i] = __INT_MAX__;
            for (auto coin : coins) {
                if (i - coin < 0) {
                    continue;
                }
                dp[i] = min(dp[i], dp[i-coin] + 1);
            }
            // dp[i] = dp[i] == __INT_MAX__ ? -1 : dp[i];
        }
        return dp[amount];

        // return dp(coins, amount);
    }
```

暂时经验总结（不一定对）：

* *dp数组初始化*

  1. 若题目求*最大，最长*，则全部初始化为**最小**，如0
  2. 若题目要求*最小，最短*，则全部初始化为**最大**，如`__INT_MAX__`（结合题目要求，本题为`amount+1`，每个amount所需最多的硬币个数就是`amount+1`，**`__INT_MAX__`易溢出**

* *边界处理*

  一般就是`i=0`时，`dp[i]`为多少

### 300.[最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (55.47%) | 3594  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/binary-search" title="https://leetcode.com/tag/binary-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数数组 `nums` ，找到其中最长严格递增子序列的长度。

**子序列** 是由数组派生而来的序列，删除（或不删除）数组中的元素而不改变其余元素的顺序。例如，`[3,6,2,7]` 是数组 `[0,3,1,6,2,2,7]` 的子序列。

 

**示例 1：**

```
输入：nums = [10,9,2,5,3,7,101,18]
输出：4
解释：最长递增子序列是 [2,3,7,101]，因此长度为 4 。
```

**示例 2：**

```
输入：nums = [0,1,0,3,2,3]
输出：4
```

**示例 3：**

```
输入：nums = [7,7,7,7,7,7,7]
输出：1
```

**My solution**

> 回溯法

1. 确定状态

   `reverse(nums, i)`表示的以`nums[i]`结尾的*最长递增子序列长度*

   该状态的确定可以通过*举例法*总结得出，找一个较为简单的案例，然后穷举。

2. 确定边界

   很明显，当`i=0`时，*最长递增子序列*就是其本身，长度为1

3. 确定更新逻辑

   也就是如何得到`reverse(nums, i)`

   找所有*小于i的j，所对应的`reverse(nums, j)`*，找到后如果`num[i] > num[j]`，则`reverse(nums, i) = reverse(nums, j) + 1`；否则`reverse(nums, i) = reverse(nums, j)`。

   最后取最长。

*回溯法*函数

```cpp
int reverse (vector<int>& nums, map<int, int>& mem, int n, int i) {
        if (i == 0) {
            return 1;
        }
        int res = 1;
        if (mem.count(i)) {
            return mem[i];
        }
        for (int j = 0; j < i; ++j) {
            if (nums[j] < nums[i]) {
                res = max(res, reverse(nums, mem, n, j) + 1);
            }
        }
        mem.insert({i, res});
        return res;
    }
```

这里在查询前**设置`res=1`**，因为如果所有`nums[j] > nums[i]`，则以`nums[i]`结尾的最长递增子序列就只有它自己。

### 70.[爬楼梯](https://leetcode.cn/problems/climbing-stairs/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (54.44%) | 3481  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

假设你正在爬楼梯。需要 `n` 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

 

**示例 1：**

```
输入：n = 2
输出：2
解释：有两种方法可以爬到楼顶。
1. 1 阶 + 1 阶
2. 2 阶
```

**示例 2：**

```
输入：n = 3
输出：3
解释：有三种方法可以爬到楼顶。
1. 1 阶 + 1 阶 + 1 阶
2. 1 阶 + 2 阶
3. 2 阶 + 1 阶
```

**My solution**

动态规划五步走：

1. 确定`dp`数组下标含义

   *`dp[i]`表示爬到第i层楼梯，有`dp[i]`种方法*

2. 确定递推公式

   思考如何到达第i层楼梯，也就是思考在到达第i层楼梯之前，到达了哪一层楼梯。

   显然，要么是从第i-1层楼梯经过，要么是从第i-2层楼梯经过。

   所以，递推公式为

   `dp[i] = dp[i-1] + dp[i-2]`

3. dp数组如何初始化

   `dp[1] = 1, dp[2] = 2`

   很好理解

4. 确定遍历顺序

   从前往后

5. 举例推到dp数组

   <img src="https://code-thinking-1253855093.file.myqcloud.com/pics/20210105202546299.png" alt="70.爬楼梯" style="zoom:67%;" />

```cpp
class Solution {
public:
    int climbStairs(int n) {
        // * ascertain meaning of dp array
        // ? dp[i] represent ways to arrive i'th stairs
        vector<int> dp(n+1, 0);

        if (n == 1 || n == 2) {
            return n;
        }
        // * ascertain the recurrence formula
        // ? dp[i] = dp[i-1] + dp[i-2]

        // * ascertain the initiation of dp array
        // ? dp[1] = 1, dp[2] = 2
        dp[1] = 1;
        dp[2] = 2;

        for (int i = 3; i <= n; ++i) {
            dp[i] = dp[i-1] + dp[i-2];
        }
        return dp[n];
    }
};
```

### 746.[使用最小花费爬楼梯](https://leetcode.cn/problems/min-cost-climbing-stairs/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (66.40%) | 1459  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/trie" title="https://leetcode.com/tag/trie" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数数组 `cost` ，其中 `cost[i]` 是从楼梯第 `i` 个台阶向上爬需要支付的费用。一旦你支付此费用，即可选择向上爬一个或者两个台阶。

你可以选择从下标为 `0` 或下标为 `1` 的台阶开始爬楼梯。

请你计算并返回达到楼梯顶部的最低花费。

 

**示例 1：**

```
输入：cost = [10,15,20]
输出：15
解释：你将从下标为 1 的台阶开始。
- 支付 15 ，向上爬两个台阶，到达楼梯顶部。
总花费为 15 。
```

**示例 2：**

```
输入：cost = [1,100,1,1,1,100,1,1,100,1]
输出：6
解释：你将从下标为 0 的台阶开始。
- 支付 1 ，向上爬两个台阶，到达下标为 2 的台阶。
- 支付 1 ，向上爬两个台阶，到达下标为 4 的台阶。
- 支付 1 ，向上爬两个台阶，到达下标为 6 的台阶。
- 支付 1 ，向上爬一个台阶，到达下标为 7 的台阶。
- 支付 1 ，向上爬两个台阶，到达下标为 9 的台阶。
- 支付 1 ，向上爬一个台阶，到达楼梯顶部。
总花费为 6 。
```

**My solution**

动态规划五步曲

1. 确定dp数组含义

   `dp[i]`表示到达第i个台阶所需最小cost

2. 确定dp数组初始化

   由于可以从第0阶台阶或第1阶台阶开始，所以`dp[0] = dp[1] = 0`

3. 确定递推公式

   `dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])`

   很好理解

4. 确定遍历顺序

   从前往后

5. 举例推算dp数组

   在草稿纸上演算

```cpp
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {
        // * determine the meaning of dp array
        // ? dp[i] means the minimum cost to get to corresponding stair
        int n = cost.size();
        vector<int> dp(n+1, 0);
        // * determine the initiation of dp array
        // ? dp[0] = 0, dp[1] = 0
        dp[0] = 0, dp[1] = 0;
        
        // * determine the formula for recurrence 
        // ? dp[i] = min(dp[i-1]+cost[i-1], dp[i-2]+cost[i-2])
        for (int i = 2; i <= n; ++i) {
            dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2]);
        }

        return dp[n];
    }
};
```

*回溯法*

```cpp
class Solution {
public:
    int minCostClimbingStairs(vector<int>& cost) {

        vector<int> dp(n+1,-1); 				// used to downsize time
        return bracktrace(cost, n, dp);
    }

    int bracktrace(const vector<int>& cost, int n, vector<int>& dp) {
        if (n==0 || n==1) return 0;
        if(dp[n] != -1) {
            return dp[n];
        }
        int res;
        res = min(bracktrace(cost, n-1, dp) + cost[n-1], bracktrace(cost, n-2, dp) + cost[n-2]);
        dp[n] = res;
        return res;
    }
};
```

### 62.[不同路径](https://leetcode.cn/problems/unique-paths/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (68.10%) | 2008  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

一个机器人位于一个 `m x n` 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish” ）。

问总共有多少条不同的路径？

 

**示例 1：**

![img](https://pic.leetcode.cn/1697422740-adxmsI-image.png)

```
输入：m = 3, n = 7
输出：28
```

**示例 2：**

```
输入：m = 3, n = 2
输出：3
解释：
从左上角开始，总共有 3 条路径可以到达右下角。
1. 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右
3. 向下 -> 向右 -> 向下
```

**示例 3：**

```
输入：m = 7, n = 3
输出：28
```

**示例 4：**

```
输入：m = 3, n = 3
输出：6
```

**My solution**

和前面几题类似，用*动态规划五步曲*解决

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        // * determine meaning of dp array
        // ? dp[i][j] represents ways to get coordinate (i, j)
        vector<vector<int>> dp(m+1, vector<int>(n+1, 0));

        // * determine initiation of dp array
        // ? ways to get coordinate on the border only have one
        for (int i = 0; i <= m; ++i) {
            dp[i][0] = 1;
        }
        for (int j = 0; j <= n; ++j) {
            dp[0][j] = 1;
        }

        // * determine recurrence formula
        // ? dp[i][j] = dp[i-1][j] + dp[i][j-1]
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                dp[i][j] = dp[i-1][j] + dp[i][j-1];  
            }
        }
        return dp[m-1][n-1];
    }
};
```

*回溯法*

```cpp
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m+1, vector<int>(n+1, 0));
        return backtrace(m, n, dp);
    }

    int backtrace(int m, int n, vector<vector<int>>& dp) {
        if(m == 1 || n ==1 ) return 1;
        if (dp[m][n] != 0) {
            return dp[m][n];
        }
        int res;
        res = backtrace(m-1, n, dp) + backtrace(m, n-1, dp);
        dp[m][n] = res;
        return res;
    }
};
```



### 63.[不同路径 II](https://leetcode.cn/problems/unique-paths-ii/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (41.28%) | 1226  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

一个机器人位于一个 `m x n` 网格的左上角 （起始点在下图中标记为 “Start” ）。

机器人每次只能向下或者向右移动一步。机器人试图达到网格的右下角（在下图中标记为 “Finish”）。

现在考虑网格中有障碍物。那么从左上角到右下角将会有多少条不同的路径？

网格中的障碍物和空位置分别用 `1` 和 `0` 来表示。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/04/robot1.jpg)

```
输入：obstacleGrid = [[0,0,0],[0,1,0],[0,0,0]]
输出：2
解释：3x3 网格的正中间有一个障碍物。
从左上角到右下角一共有 2 条不同的路径：
1. 向右 -> 向右 -> 向下 -> 向下
2. 向下 -> 向下 -> 向右 -> 向右
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/11/04/robot2.jpg)

```
输入：obstacleGrid = [[0,1],[0,0]]
输出：1
```

**My solution**

这里需要注意*动态规划五步曲*中对dp数组的**初始化**和**递推公式**

- 初始化

  由于路径上会出现*障碍*，之前边界的路径只有一条，但如果边界上出现障碍，那么在边界上的障碍之后都是路径条数都是0

  ```cpp
  bool bar = false;
          for (int i = 0;i < m; ++i) {
              if (!obstacleGrid[i][0] && !bar) {
                  dp[i][0] = 1;
              } else if (obstacleGrid[i][0]) {
                  dp[i][0] = 0;
                  bar = true;        
              } else if (bar == true) {
                  dp[i][0] = 0;
              }
          }
  ```

- 递推公式

  由于路径上会有障碍，所以如果该坐标上有障碍，`dp[i][j]=0`

  ```cpp
  for (int i = 1; i < m; ++i) {
              for (int j = 1; j < n; ++j) {
                  if (obstacleGrid[i][j]) {
                      dp[i][j] = 0;
                  } else {
                      dp[i][j] = dp[i-1][j] + dp[i][j-1];
                  }
              }
          }
  ```

```cpp
class Solution {
public:
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        // * determine meaning of dp array
        // ? dp[i][j] represents ways to get to coordinate (i,j)
        int m = obstacleGrid.size();
        int n = obstacleGrid[0].size();

        vector<vector<int>> dp(m+1, vector<int>(n+1, 0));
        // * determine initiation of dp array
        // ? ways of coordinate on borders only have one, and have zero if there is handicap on borders
        bool bar = false;
        for (int i = 0;i < m; ++i) {
            if (!obstacleGrid[i][0] && !bar) {
                dp[i][0] = 1;
            } else if (obstacleGrid[i][0]) {
                dp[i][0] = 0;
                bar = true;        
            } else if (bar == true) {
                dp[i][0] = 0;
            }
        }
        bar = false;
        for (int j = 0;j < n; ++j) {
            if (!obstacleGrid[0][j] && !bar) {
                dp[0][j] = 1;
            } else if (obstacleGrid[0][j]) {
                dp[0][j] = 0;
                bar = true;
            } else if (bar == true) {
                dp[0][j] = 0;
            }
        }

        // * determine the recurrence formula
        // ? dp[i][j] = dp[i-1][j] + dp[i][j-1], obstacleGrid[i][j] == 0
        // ? dp[i][j] = 0, obstacleGrid[i][j] == 1
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                if (obstacleGrid[i][j]) {
                    dp[i][j] = 0;
                } else {
                    dp[i][j] = dp[i-1][j] + dp[i][j-1];
                }
            }
        }
        return dp[m-1][n-1];
    }
};
```

### 343.[整数拆分](https://leetcode.cn/problems/integer-break/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (63.10%) | 1361  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/math" title="https://leetcode.com/tag/math" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个正整数 `n` ，将其拆分为 `k` 个 **正整数** 的和（ `k >= 2` ），并使这些整数的乘积最大化。

返回 *你可以获得的最大乘积* 。

 

**示例 1:**

```
输入: n = 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1。
```

**示例 2:**

```
输入: n = 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36。
```

**My solution**

*动态规划五步曲*

1. 确定dp数组含义

   `dp[i]`表示数字i的最大乘积

2. 确定dp数组初始化

   这里初始化时要注意，`n >= 2`，所以只需要初始化`dp[2]`就行了

   `dp[2] = 1`

3. 确定递推公式（难点）

   这里的递推公式，我本来想的是

   `dp[i] = max(dp[i], j * dp[i-j])`，这里有个问题就是少计算了一种情况，就是将i拆分为两个数`j*(i-j)`

   可以理解为将数字i拆分为*两个数*或*两个及两个以上数*

   比如`dp[4]`, 2*2 就比`2*dp[2]`更大

```cpp
class Solution {
public:
    int integerBreak(int n) {
        if (n == 2 || n == 3) {
            return n-1;
        }
        // * determine meaning of dp array
        // ? dp[i] means value which is max value of multiplying splits of number i
        vector<int> dp(n + 1, 0);
        
        // * determine the initiation of dp array
        // ? dp[2] = 1, dp[3] = 2, n>=2
        dp[2] = 1, dp[3] = 2;

        // * determine the recurrence fomula
        // ? dp[i] = max(dp[i], j * dp[i-j]),j belongs to [1, j]
        for (int i = 4;i <= n;++i) {
            for (int j = 1; j < i-1; ++j) {
                dp[i] = max(dp[i], max(j * (i-j), j * dp[i-j]));
            }
        }
        return dp[n];
    }
};
```

### 96.[不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (71.02%) | 2479  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数 `n` ，求恰由 `n` 个节点组成且节点值从 `1` 到 `n` 互不相同的 **二叉搜索树** 有多少种？返回满足题意的二叉搜索树的种数。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/18/uniquebstn3.jpg)

```
输入：n = 3
输出：5
```

**示例 2：**

```
输入：n = 1
输出：1
```

**My solution**

本题主要难点是如何去想到*递推关系式*

通过观察可以发现，当`n=3`时，这些二叉搜索树的构成分别是*右子树为2*，*左子树为1，右子树为1*，*左子树为2*，也就是`dp[2] + dp[1] * dp[1] + dp[2]`

这里需要注意的是⚠️左右子树都不为0时，其种类是**左子树种类 * 右子树种类**

```cpp
class Solution {
public:
    int numTrees(int n) {
        if (n==1 || n==2) {
            return n;
        }
        // * determine meaning of dp array
        // ? dp[i] 表示i个节点能组成的二叉搜索树
        vector<int> dp(n+1, 0);

        // * determine the initiation of dp array
        // ? dp[1] = 1, dp[2] = 2
        dp[0] = 0;              // * to make it convenient to handle
        dp[1] = 1, dp[2] = 2;

        // * determine the recurrence formula
        // ? dp[i] = dp[j] + dp[i-j], 0 <= j < i
        for (int i = 3; i <= n; ++i) {
            for (int j = 0; j < i; ++j) {
              	// remind the way to handle this
                if (j == 0 || j == i-1) {
                    dp[i] = dp[i] + dp[j] + dp[i-j-1];
                } else {
                    dp[i] += dp[j] * dp[i-j-1];
                }
            }
        }
        return dp[n];
    }
};
```

### 416.[分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (52.38%) | 2046  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个 **只包含正整数** 的 **非空** 数组 `nums` 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

 

**示例 1：**

```
输入：nums = [1,5,11,5]
输出：true
解释：数组可以分割成 [1, 5, 5] 和 [11] 。
```

**示例 2：**

```
输入：nums = [1,2,3,5]
输出：false
解释：数组不能分割成两个元素和相等的子集。
```

**My solution**

这题的思路是*套0-1背包问题*

*动态规划五步曲*

1. 确定dp数组含义（先选用二维数组）

   `dp[i][j]`，i表示物品可选择的范围为0-i，j表示背包容量，`dp[i][j]`表示的就是背包容量为j是，在物品0-i范围内，能达到的最大值

2. 初始化dp数组

   首先将dp数组全部初始化为0

   接着初始化dp数组的边界：当i=0时，只能选择物品0，所有当`j > nums[0]`时，就将其初始化为`nums[0]`；当j=0时，背包容量为0，所以都为0。

   ```cpp
   // * initiate dp array
           for (int j = nums[0]; j <= sum/2; ++j) {
               dp[0][j] = nums[0];
           }
           for (int i = 0; i < n; ++i) {
               dp[i][0] = 0;
           }
   ```

3. 确定递推公式

   在确定递推公式之前，要想明白本题中的价值是什么？

   本题中的价值就是*物品值的和*，其中物品的值和物品的重量是等价的。

   ```cpp
   if (j < nums[i]) {
     dp[i][j] = dp[i-1][j];				// *背包容量不够，无法放入
   } else {
     dp[i][j] = dp[i-1][j-nums[i]] + nums[i];
   }
   ```

   

```cpp
class Solution {
    public:
        bool canPartition(vector<int>& nums) {
            int sum = accumulate(nums.begin(), nums.end(), 0);
            int n = nums.size();
            vector<vector<int>> dp(n, vector<int>(sum/2+1, 0));

            if (sum % 2 != 0) {
                return false;
            }
            // * initiate dp array
            for (int j = nums[0]; j <= sum/2; ++j) {
                dp[0][j] = nums[0];
            }
            for (int i = 0; i < n; ++i) {
                dp[i][0] = 0;
            }

            // * determine the recurrence formula
            for (int i = 1; i < n; ++i) {
                for (int j = 1; j <= sum/2; ++j) {
                    if (j < nums[i]) {
                        dp[i][j] = dp [i-1][j];
                    } else {
                        dp[i][j] = max(dp[i-1][j], dp[i-1][j-nums[i]] + nums[i]);
                    }
                    if (dp[i][j] == sum/2) {
                        return true;
                    }
                }
            }
            return false;
        }
};
```

### 1049.[最后一块石头的重量 II](https://leetcode.cn/problems/last-stone-weight-ii/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (70.15%) |  839  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/greedy" title="https://leetcode.com/tag/greedy" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

有一堆石头，用整数数组 `stones` 表示。其中 `stones[i]` 表示第 `i` 块石头的重量。

每一回合，从中选出**任意两块石头**，然后将它们一起粉碎。假设石头的重量分别为 `x` 和 `y`，且 `x <= y`。那么粉碎的可能结果如下：

- 如果 `x == y`，那么两块石头都会被完全粉碎；
- 如果 `x != y`，那么重量为 `x` 的石头将会完全粉碎，而重量为 `y` 的石头新重量为 `y-x`。

最后，**最多只会剩下一块** 石头。返回此石头 **最小的可能重量** 。如果没有石头剩下，就返回 `0`。

 

**示例 1：**

```
输入：stones = [2,7,4,1,8,1]
输出：1
解释：
组合 2 和 4，得到 2，所以数组转化为 [2,7,1,8,1]，
组合 7 和 8，得到 1，所以数组转化为 [2,1,1,1]，
组合 2 和 1，得到 1，所以数组转化为 [1,1,1]，
组合 1 和 1，得到 0，所以数组转化为 [1]，这就是最优值。
```

**示例 2：**

```
输入：stones = [31,26,33,21,40]
输出：5
```

**My solution**

乍一看这个题目很复杂，需要对问题转化。

题目所表达的意思就是将石头进行撞击消融，转换为*0-1背包*问题就是：

当背包容量为`石头重量总和/2`时，找到背包所能装入的最大石头重量总和，此时*背包中的石头重量和*与*剩余石头重量和*之差一定最小。

*动态规划五步曲*

* 确定dp数组含义

  `dp[i][j]`表示背包容量为j时，从0-i个石头中选择，能够达到最大的石头重量和

* 初始化dp数组

  主要是初始化dp数组的边界

  ```cpp
  // * initiate dp array
          // ? dp[i][j] = 0, when j == 0; dp[i][j] = stones[0], when j > stones[0]
          for (int i = 0;i < n;++i) {
              dp[i][0] = 0;
          }
          for (int j = stones[0]; j <= sum/2; ++j) {
              dp[0][j] = stones[0];
          }
  ```

* 确定递推公式

  ```cpp
  // * determine recurrence formula
          // ? dp[i][j] = dp[i-1][j], if stones[i] > j
          // ? dp[i][j] = dp[i-1][j-1] + stones[i], else
          for (int i = 1;i < n;++i) {
              for (int j = 1;j <= sum/2;++j) {
                  // if (stones[i] > j - dp[i-1][j-1]) {
                  //     dp[i][j] = dp[i-1][j];
                  // } else {
                  //     dp[i][j] = dp[i-1][j-1] + stones[i];
                  // }
                  if (j < stones[i]) {
                      dp[i][j] = dp[i-1][j];
                  } else {
                      dp[i][j] = max(dp[i-1][j], dp[i-1][j-stones[i]] + stones[i]);
                  }
              }
          }
  ```

完整代码

```cpp
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        // * 本题需要理解转化
        // * 将问题转换为：当背包容量为sum/2时，找到最大的数字和，这样和数组剩余的石头相撞，剩下的就是最小的

        // * determine meaning of dp array
        // ? dp[i][j]: i represents range of goods, j represents container of bag
        int n = stones.size();
        int sum = accumulate(stones.begin(), stones.end(), 0);
        vector<vector<int>> dp(n, vector<int>(sum/2 + 1, 0));

        // * initiate dp array
        // ? dp[i][j] = 0, when j == 0; dp[i][j] = stones[0], when j > stones[0]
        for (int i = 0;i < n;++i) {
            dp[i][0] = 0;
        }
        for (int j = stones[0]; j <= sum/2; ++j) {
            dp[0][j] = stones[0];
        }

        // * determine recurrence formula
        // ? dp[i][j] = dp[i-1][j], if stones[i] > j
        // ? dp[i][j] = dp[i-1][j-1] + stones[i], else
        for (int i = 1;i < n;++i) {
            for (int j = 1;j <= sum/2;++j) {
                // if (stones[i] > j - dp[i-1][j-1]) {
                //     dp[i][j] = dp[i-1][j];
                // } else {
                //     dp[i][j] = dp[i-1][j-1] + stones[i];
                // }
                if (j < stones[i]) {
                    dp[i][j] = dp[i-1][j];
                } else {
                    dp[i][j] = max(dp[i-1][j], dp[i-1][j-stones[i]] + stones[i]);
                }
            }
        }
        // * 这里的返回值需要注意
        // * 不能直接用sum/2-dp[n-1][sum/2]，而是应该用剩余的石头重量和`sum-dp[n-1][sum/2]`减去所求最大的石头重量和`dp[n-1][sum/2]`
        return sum - dp[n-1][sum/2] * 2;
    }
};
```

### 494.[目标和](https://leetcode.cn/problems/target-sum/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (48.34%) | 1894  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个非负整数数组 `nums` 和一个整数 `target` 。

向数组中的每个整数前添加 `'+'` 或 `'-'` ，然后串联起所有整数，可以构造一个 **表达式** ：

- 例如，`nums = [2, 1]` ，可以在 `2` 之前添加 `'+'` ，在 `1` 之前添加 `'-'` ，然后串联起来得到表达式 `"+2-1"` 。

返回可以通过上述方法构造的、运算结果等于 `target` 的不同 **表达式** 的数目。

 

**示例 1：**

```
输入：nums = [1,1,1,1,1], target = 3
输出：5
解释：一共有 5 种方法让最终目标和为 3 。
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3
```

**示例 2：**

```
输入：nums = [1], target = 1
输出：1
```

同样，这题的关键是要转化问题。

如何转化为01背包问题呢。

假设加法的总和为x，那么减法对应的总和就是sum - x。

所以我们要求的是 x - (sum - x) = target

x = (target + sum) / 2

**此时问题就转化为，装满容量为x的背包，有几种方法**。

*动态规划五步曲*

* dp数组含义

  现在越来越发觉**dp数组含义很重要**。对dp数组含义一定要清晰！

  `dp[i][j]`表示的是*在前i个数中选取，最终和为j的方法数量*，i从1开始

* dp数组初始化

  `dp[0][0]=1`

* 递推公式

  `dp[i][j]=dp[i-1][j]`, if `j < nums[i-1]`

  `dp[i][j]=dp[i][j] + d[i-1][j-nums[i-1]]`, else

  可以这么理解，数组中的*要么选，要么不选*。如果不选的话，方法数就和*在前i-1个数中选择，和为j*的方法数相同；如果选的话，方法数和就是*在前i-1个数中选择，和为j*以及*在前i-1个数中选择，和为j-nums[i-1]*的方法数相加。

```cpp
class Solution {
public:
    public:
    int findTargetSumWays(vector<int>& nums, int target) {
        // int n = nums.size();
        // // * left - right = target, left + right = sum, right = sum - left
        // // * left - (sum - left) = target, left = (target + sum)/2
        // int sum = accumulate(nums.begin(), nums.end(), 0);
        // if ((sum + target) % 2) return 0;
        // if (abs(target) > sum) return 0;
        // int bagsize = (sum + target) / 2;      

        // // * determine meaning of dp array
        // // ? dp[i] represents ways for target == i
        // vector<int> dp(bagsize + 1, 0);
        // dp[0] = 1;

        // // * determine formula recurrence 
        // // ? dp[i] = dp[i] + dp[i-1]
        // sort(nums.begin(), nums.end());
        // for (int i = 0;i < n;++i) {
        //     // for (int j = 1;j < bagsize;++j) {
        //     //     dp[j] = dp[j] + dp[j-1];
        //     // }
        //     for (int j = bagsize; j >= nums[i];--j) {
        //         dp[j] = dp[j] + dp[j-nums[i]];
        //     }
        // }
        // return dp[bagsize];

        // * use 0-1 bag model
        int n = nums.size();

        // * determine meaning of dp array
        // ? dp[i][j] represents 
        int sum = accumulate(nums.begin(), nums.end(), 0);
        if ((sum + target) % 2) return 0;
        if (abs(target) > sum) return 0;
        int bagsize = (sum + target) / 2; 
        vector<vector<int>> dp(n+1, vector<int>(bagsize+1, 0));
        // * initiate dp array
        dp[0][0] = 1;

        // * determine recurrence formula
        for (int i = 1;i <= n;++i) {
            for (int j = 0; j <= bagsize; ++j) {
                dp[i][j] = dp[i-1][j];
                if (j >= nums[i-1]) {
                    dp[i][j] = dp[i][j] + dp[i-1][j-nums[i-1]];
                }
            }
        }

        return dp[n][bagsize];

    }
```

### 474.[一和零](https://leetcode.cn/problems/ones-and-zeroes/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (66.42%) | 1165  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: var(--text-link-decoration);"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个二进制字符串数组 `strs` 和两个整数 `m` 和 `n` 。

请你找出并返回 `strs` 的最大子集的长度，该子集中 **最多** 有 `m` 个 `0` 和 `n` 个 `1` 。

如果 `x` 的所有元素也是 `y` 的元素，集合 `x` 是集合 `y` 的 **子集** 。

 

**示例 1：**

```
输入：strs = ["10", "0001", "111001", "1", "0"], m = 5, n = 3
输出：4
解释：最多有 5 个 0 和 3 个 1 的最大子集是 {"10","0001","1","0"} ，因此答案是 4 。
其他满足题意但较小的子集包括 {"0001","1"} 和 {"10","1","0"} 。{"111001"} 不满足题意，因为它含 4 个 1 ，大于 n 的值 3 。
```

**示例 2：**

```
输入：strs = ["10", "0", "1"], m = 1, n = 1
输出：2
解释：最大的子集是 {"0", "1"} ，所以答案是 2 。
```

**My solution**

这题本质上是0-1背包问题，只不过背包容量变成了*二维数组*。

一开始的递推公式写得不对，注意需要统计的是子集中字符串的数量，而不是有多少个满足条件的子集。

```cpp
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        // * 24.Sep.4

        // * count number of satisfying the petition of 0
        // vector<vector<vector<int>>> dp(n, vector<vector<int>>(m+1, vector<int>(n+1, 0))));
        vector<vector<int>> dp(m + 1, vector<int>(n+1, 0));
        // dp[0][0] = 1;
        for (int i = 0;i < strs.size();++i) {
            for (int j = m;j >= 0;--j) {
                for (int k = n;k >= 0;--k) {
                    if (j >= count0(strs[i]) && k >= (count1(strs[i]))) {
                        // dp[j][k] = dp[j][k] + dp[j-count0(strs[i])][k-count1(strs[i])];
                        dp[j][k] = max(dp[j][k], dp[j-count0(strs[i])][k-count1(strs[i])] + 1);
                    }
                }
            }
        }
        return dp[m][n];
    }

    int count1(string s) {
        int count = 0;
        for (auto&x : s) {
            if (x == '1') {
                ++count;
            }
        }
        return count;
    }

    int count0(string s) {
        int count = 0;
        for (auto&x : s) {
            if (x == '0') {
                ++count;
            }
        }
        return count;
    }
};
```





### 518.[零钱兑换 II](https://leetcode.cn/problems/coin-change-ii/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (71.19%) | 1284  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/Unknown" title="https://leetcode.com/tag/Unknown" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数数组 `coins` 表示不同面额的硬币，另给一个整数 `amount` 表示总金额。

请你计算并返回可以凑成总金额的硬币组合数。如果任何硬币组合都无法凑出总金额，返回 `0` 。

假设每一种面额的硬币有无限个。 

题目数据保证结果符合 32 位带符号整数。

**示例 1：**

```
输入：amount = 5, coins = [1, 2, 5]
输出：4
解释：有四种方式可以凑成总金额：
5=5
5=2+2+1
5=2+1+1+1
5=1+1+1+1+1
```

**示例 2：**

```
输入：amount = 3, coins = [2]
输出：0
解释：只用面额 2 的硬币不能凑成总金额 3 。
```

**示例 3：**

```
输入：amount = 10, coins = [10] 
输出：1
```

**My solution**

*动态规划五步曲*

* 确定数组含义

  使用二维数组

  `dp[i][j]`表示从前i个数中选择，和为j的方法数

* 初始化二维数组

  `dp[0][0]=1`

* 确定递推公式

  确定递推公式时要考虑到*硬币可以无限使用*

  所以在递推时，要把`dp[i][j] += dp[i-1][j-coins[i-1]]`改为`dp[i][j] += dp[i][j-coins[i-1]]`

```cpp
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        // int res = 0, sum = 0;
        // backtrace(amount, coins, sum, res);
        // return res;

        // * 完全背包问题：石头可以无限使用
        // * 确定dp数组含义
        // ? dp[i][j]表示从前i个数中选择，和为j的组合数
        int n = coins.size();
        vector<vector<int>> dp (n+1, vector<int>(amount+1, 0));

        // * initiate dp array
        // ? dp[0][0] = 1
        dp[0][0] = 1;

        // * determine recurrence formula
        for (int i = 1;i <= n;++i) {
            for (int j = 0;j <= amount;++j) {
                dp[i][j] = dp[i-1][j];
                if(j >= coins[i-1]) {
                    // ! since elements can be used infinitely, must add `dp[i][j-coins[i-1]]`, rather than `dp[i-1][j-coins[i-1]]`
                    dp[i][j] += dp[i][j-coins[i-1]];
                }
            }
        }

        return dp[n][amount];
    }
```

元素有限使用组合

|      |  0   |  1   |  2   | 3    | 4    |      |
| :--: | :--: | :--: | :--: | ---- | ---- | ---- |
|  0   |  1   |  0   |  0   | 0    | 0    |      |
|  1   |  1   |  1   |  0   | 0    | 0    |      |
|  2   |  1   |  1   |  1   | 1    | 0    |      |
|  3   |  1   |  1   |  1   | 2    | 1    |      |

元素无限使用，组合

|      | 0    | 1    | 2    | 3    | 4    |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 0    | 1    | 0    | 0    | 0    | 0    |
| 1    | 1    | 1    | 1    | 1    | 1    |
| 2    | 1    | 1    | 2    | 2    | 3    |
| 3    | 1    | 1    | 2    | 3    | 4    |

元素无限使用，排列

|      | 0    | 1    | 2    | 3    | 4    |
| ---- | ---- | ---- | ---- | ---- | ---- |
| 0    | 1    | 0    | 0    | 0    | 0    |
| 1    | 1    | 1    | 1    | 2    | 4    |
| 2    | 1    | 1    | 2    | 3    | 6    |
| 3    | 1    | 1    | 2    | 4    | 7    |

### 377.[组合总和 Ⅳ](https://leetcode.cn/problems/combination-sum-iv/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (52.59%) |  941  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个由 **不同** 整数组成的数组 `nums` ，和一个目标整数 `target` 。请你从 `nums` 中找出并返回总和为 `target` 的元素组合的个数。

题目数据保证答案符合 32 位整数范围。

 

**示例 1：**

```
输入：nums = [1,2,3], target = 4
输出：7
解释：
所有可能的组合为：
(1, 1, 1, 1)
(1, 1, 2)
(1, 2, 1)
(1, 3)
(2, 1, 1)
(2, 2)
(3, 1)
请注意，顺序不同的序列被视作不同的组合。
```

**示例 2：**

```
输入：nums = [9], target = 3
输出：0
```

**My solution**

分为两种方法：*一维数组*和*二维数组*

* 二维数组

  *动态规划五步曲*

  1. 二维数组含义

     `dp[i][j]`表示从前i个数里选择，总和为j的排列数

  2. 递推公式

     这里注意到要求的是排列数，且元素**无限使用**，所以递推代码如下：

     ```cpp
     for (int j = 0;j <= target;++j) {
                 for (int i = 1;i <= n;++i) {
                     dp[i][j] = dp[i-1][j];
                     if (j >= nums[i-1]) {
                         dp[i][j] += dp[n][j-nums[i-1]];
                     }
                 }
             }
     ```

     ⚠️循环最内层相加的是`dp[n][]`而不是`dp[i][]`或`dp[i-1][]`

     如果求的是组合数，那么就要**先遍历物品**，上面代码是**先遍历背包**

     *完整代码*

     ```cpp
     class Solution {
     public:
         int combinationSum4(vector<int>& nums, int target) {
             int n = nums.size();
             // * determine meaning of dp array
             vector<vector<int>> dp(n+1, vector<int>(target+1, 0));
           	// * initiate dp array
             dp[0][0] = 1;
           	for (int j = 0;j <= target;++j) {
                 for (int i = 1;i <= n;++i) {
                     dp[i][j] = dp[i-1][j];
                     if (j >= nums[i-1]) {
                         dp[i][j] += dp[n][j-nums[i-1]];
                     }
                 }
             }
           	return dp[n][target];
     ```

* *一维数组*

  *动态规划五步曲*

  1. 一维数组含义

     `dp[j]`表示和为j的排列数为`dp[j]`

  2. 递推公式

     需要注意到这里**元素无限使用**，所以**从小到大**遍历`j`；

     所求结果为**排列数**，所以**先遍历背包容量**

     ```cpp
     for (int j = 1;j <= target;++j) {
                 for (int i = 0;i < n;++i){
                     // dp2[j] = dp2[j-1];
                     if (j >= nums[i]) {
                         dp2[j] = dp2[j] + dp2[j-nums[i]];
                     }
                 }
             }
     ```

     ⚠️只有当`j>=nums[i]`时才更新`dp[j]`

  完整代码

  ```cpp
  class Solution {
  public:
      int combinationSum4(vector<int>& nums, int target) {
          int n = nums.size();
          // * determine meaning of dp array
        	vector<int> dp2(target+1, 0);
        	dp2[0] = 1;
        	for (int j = 1;j <= target;++j) {
              for (int i = 0;i < n;++i){
                  // dp2[j] = dp2[j-1];
                  if (j >= nums[i]) {
                      dp2[j] = dp2[j] + dp2[j-nums[i]];
                  }
              }
          }
        	return dp2[target];
  ```



### 198.[打家劫舍](https://leetcode.cn/problems/house-robber/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (54.99%) | 2975  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警**。

给定一个代表每个房屋存放金额的非负整数数组，计算你 **不触动警报装置的情况下** ，一夜之内能够偷窃到的最高金额。

 

**示例 1：**

```
输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

**示例 2：**

```
输入：[2,7,9,3,1]
输出：12
解释：偷窃 1 号房屋 (金额 = 2), 偷窃 3 号房屋 (金额 = 9)，接着偷窃 5 号房屋 (金额 = 1)。
     偷窃到的最高金额 = 2 + 9 + 1 = 12 。
```

**My solution**

本题使用*动态规划*

参考了之前做过的*买卖股票*的思想，定义一个二维数组，`0`表示未偷窃，`1`表示偷窃

*动态规划五步曲*

* 二维数组含义

  `dp[n][2]`

  `dp[i][0]`表示当晚未偷窃时，手中的最高金额；`dp[i][1]`表示当晚偷窃时，手中的最高金额

* 递推公式

  ```cpp
  // * decide recurrence formula
          for (int i = 1;i < n;++i) {
              dp[i][0] = max(dp[i-1][0], dp[i-1][1]);
              dp[i][1] = dp[i-1][0] + nums[i];
          }
  ```

* 动态数组初始化

  ```cpp
  dp[0][0] = 0;
  dp[0][1] = nums[0];
  ```

*完整代码*

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        // * determine meaning of dp array
        int n = nums.size();
        // ? dp[i][1] denotes having stolen, while dp[i][0] represents not stolen
        vector<vector<int>> dp(n, vector<int>(2, 0));
        dp[0][0] = 0;
        dp[0][1] = nums[0];

        // * decide recurrence formula
        for (int i = 1;i < n;++i) {
            dp[i][0] = max(dp[i-1][0], dp[i-1][1]);
            dp[i][1] = dp[i-1][0] + nums[i];
        }
        return max(dp[n-1][0], dp[n-1][1]);
    }
};
```

*代码随想录*方法

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        if (nums.size() == 1) return nums[0];
        vector<int> dp(nums.size());
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);
        for (int i = 2; i < nums.size(); i++) {
            dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]);
        }
        return dp[nums.size() - 1];
    }
};
```

### 213.[打家劫舍 II](https://leetcode.cn/problems/house-robber-ii/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (44.95%) | 1583  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

你是一个专业的小偷，计划偷窃沿街的房屋，每间房内都藏有一定的现金。这个地方所有的房屋都 **围成一圈** ，这意味着第一个房屋和最后一个房屋是紧挨着的。同时，相邻的房屋装有相互连通的防盗系统，**如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警** 。

给定一个代表每个房屋存放金额的非负整数数组，计算你 **在不触动警报装置的情况下** ，今晚能够偷窃到的最高金额。

 

**示例 1：**

```
输入：nums = [2,3,2]
输出：3
解释：你不能先偷窃 1 号房屋（金额 = 2），然后偷窃 3 号房屋（金额 = 2）, 因为他们是相邻的。
```

**示例 2：**

```
输入：nums = [1,2,3,1]
输出：4
解释：你可以先偷窃 1 号房屋（金额 = 1），然后偷窃 3 号房屋（金额 = 3）。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

**示例 3：**

```
输入：nums = [1,2,3]
输出：3
```

**My solution**

这里的难点是如何处理首尾相连的情况。

想到了要分情况讨论，但没有具体的思路。这里*代码随想录*给出的两种情况分别是：

1. 不考虑第一个房间
2. 不考虑最后一个房间

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        vector<vector<int>> dp(n, vector<int>(2, 0));
        int res = 0;
        if (n < 2) {
            return nums[0];
        }

        dp[0][0] = 0;
        dp[0][1] = nums[0];
        for (int i = 1;i < n-1;++i) {
            dp[i][0] = max(dp[i-1][0], dp[i-1][1]);
            dp[i][1] = dp[i-1][0] + nums[i];
        }
        res = max(res, max(dp[n-2][0], dp[n-2][1]));
        for (int i = 0;i < n;++i) {
            dp[i][0] = 0;
            dp[i][1] = 0;
        }
        dp[1][0] = 0;
        dp[1][1] = nums[1];
        for (int i = 2;i < n;++i) {
            dp[i][0] = max(dp[i-1][0], dp[i-1][1]);
            dp[i][1] = dp[i-1][0] + nums[i];
        }
        res = max(res, max(dp[n-1][0], dp[n-1][1]));
        return res;
    }
};
```

*代码随想录代码*

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.size() == 0) return 0;
        if (nums.size() == 1) return nums[0];
        int result1 = robRange(nums, 0, nums.size() - 2); // 情况二
        int result2 = robRange(nums, 1, nums.size() - 1); // 情况三
        return max(result1, result2);
    }
    // 198.打家劫舍的逻辑
    int robRange(vector<int>& nums, int start, int end) {
        if (end == start) return nums[start];
        vector<int> dp(nums.size());
        dp[start] = nums[start];
        dp[start + 1] = max(nums[start], nums[start + 1]);
        for (int i = start + 2; i <= end; i++) {
            dp[i] = max(dp[i - 2] + nums[i], dp[i - 1]);
        }
        return dp[end];
    }
};
```

### 337.[打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (61.57%) | 1972  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

小偷又发现了一个新的可行窃的地区。这个地区只有一个入口，我们称之为 `root` 。

除了 `root` 之外，每栋房子有且只有一个“父“房子与之相连。一番侦察之后，聪明的小偷意识到“这个地方的所有房屋的排列类似于一棵二叉树”。 如果 **两个直接相连的房子在同一天晚上被打劫** ，房屋将自动报警。

给定二叉树的 `root` 。返回 ***在不触动警报的情况下** ，小偷能够盗取的最高金额* 。

 

**示例 1:**

![img](https://assets.leetcode.com/uploads/2021/03/10/rob1-tree.jpg)

```
输入: root = [3,2,3,null,3,null,1]
输出: 7 
解释: 小偷一晚能够盗取的最高金额 3 + 3 + 1 = 7
```

**示例 2:**

![img](https://assets.leetcode.com/uploads/2021/03/10/rob2-tree.jpg)

```
输入: root = [3,4,5,1,3,null,1]
输出: 9
解释: 小偷一晚能够盗取的最高金额 4 + 5 = 9
```

**My solution**

*回溯法*

⚠️需要**记忆化递推**

```cpp
unordered_map<TreeNode*, int> computed;
    int rob(TreeNode* root) {
        if (root == nullptr) return 0;
        if (root->left == nullptr && nullptr == root->right) return root->val;

        if(computed[root]) return computed[root];
        // * steal father
        int val1 = root->val;
        // ! judge if left or right child tree is void respectively
        if (root->left) {
            val1 += rob(root->left->left) + rob(root->left->right);
        }
        if (root->right) {
            val1 += rob(root->right->left) + rob(root->right->right);
        }
        int val2 = 0;
        val2 += rob(root->left) + rob(root->right);
        computed[root] = max(val1, val2);
        return max(val1, val2);
    }
```

*动态规划法*

这里的动态规划数组，只用到了大小为二的`dp`数组。

*下标0*表示**不偷**当前节点的最大金额；*下标1*表示**偷**当前节点的最大金额

```cpp
int rob(TreeNode* root) {
        vector<int> res = suffOrder(root);
        return max(res[0], res[1]);

    }
    vector<int> suffOrder(TreeNode* root) {
        if (nullptr == root) return vector<int>{0, 0};
        
        vector<int> left = suffOrder(root->left);
        vector<int> right = suffOrder(root->right);
        // int val1 = max(left[1], right[1]);
        // int val2 = max(left[0] + root->val, right[0] + root->val);
      	// not steal cur node
        int val1 = max(left[0], left[1]) + max(right[0], right[1]);
      	// steal cur node
        int val2 = root->val + left[0] + right[0];
        return vector<int>{val1, val2};
    }
```

### 122.[买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (73.46%) | 2471  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/greedy" title="https://leetcode.com/tag/greedy" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数数组 `prices` ，其中 `prices[i]` 表示某支股票第 `i` 天的价格。

在每一天，你可以决定是否购买和/或出售股票。你在任何时候 **最多** 只能持有 **一股** 股票。你也可以先购买，然后在 **同一天** 出售。

返回 *你能获得的 **最大** 利润* 。

 

**示例 1：**

```
输入：prices = [7,1,5,3,6,4]
输出：7
解释：在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6 - 3 = 3 。
     总利润为 4 + 3 = 7 。
```

**示例 2：**

```
输入：prices = [1,2,3,4,5]
输出：4
解释：在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4 。
     总利润为 4 。
```

**示例 3：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 交易无法获得正利润，所以不参与交易可以获得最大利润，最大利润为 0 。
```

**My solution**

这题使用贪心算法很容易解决。下面是*动态规划*的理解

*动态规划五步曲*

* dp数组含义

  `dp[n][2]`

  `dp[i][0]`表示当前不持有股票时，手中最大现金

  `dp[i][1]`表示当前持有股票时，手中最大现金

* 递推公式

  难点就在于递推公式，因为本题中的股票可以*买卖多次*，所以第i天不持股可能是将第i-1天买入的股票出售

  ```cpp
  dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i]);
              dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i]);
  ```

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        // int profit = 0;
        // int n = prices.size();
        // int sufdiff = 0;
        // for (int i = 0;i < n-1;++i) {
        //     sufdiff = prices[i+1] - prices[i];
        //     if (sufdiff > 0) {
        //         profit += sufdiff;
        //     }
        // }
        // return profit;

        // * dynamic programming
        // * determine meaning of dp array
        int n = prices.size();
        vector<vector<int>> dp(n, vector<int>(2, 0));
        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        
        for (int i = 1;i < n;++i) {
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i]);
            dp[i][1] = max(dp[i-1][1], dp[i-1][0] - prices[i]); 
        }

        return dp[n-1][0];
    }
};
```

### 123.[买卖股票的最佳时机 III](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iii/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Hard (60.52%) | 1690  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个数组，它的第 `i` 个元素是一支给定的股票在第 `i` 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 **两笔** 交易。

**注意：**你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

 

**示例 1:**

```
输入：prices = [3,3,5,0,0,3,1,4]
输出：6
解释：在第 4 天（股票价格 = 0）的时候买入，在第 6 天（股票价格 = 3）的时候卖出，这笔交易所能获得利润 = 3-0 = 3 。
     随后，在第 7 天（股票价格 = 1）的时候买入，在第 8 天 （股票价格 = 4）的时候卖出，这笔交易所能获得利润 = 4-1 = 3 。
```

**示例 2：**

```
输入：prices = [1,2,3,4,5]
输出：4
解释：在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5-1 = 4 。   
     注意你不能在第 1 天和第 2 天接连购买股票，之后再将它们卖出。   
     因为这样属于同时参与了多笔交易，你必须在再次购买前出售掉之前的股票。
```

**示例 3：**

```
输入：prices = [7,6,4,3,1] 
输出：0 
解释：在这个情况下, 没有交易完成, 所以最大利润为 0。
```

**示例 4：**

```
输入：prices = [1]
输出：0
```

**My solution**

本题使用动态规划，要注意本题共定义了四个状态，分别是*第一次不持有*，*第一次持有*，*第二次不持有*，*第二次持有*。分别为`dp[i][0]`, `dp[i][1]`, `dp[i][2]`, `dp[i][3]`

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<vector<int>> dp(n, vector<int>(4, 0));

        dp[0][0] = 0;
        dp[0][1] = -prices[0];
        dp[0][2] = 0;
        dp[0][3] = -prices[0];

        for (int i = 1;i < n;++i) {
            dp[i][0] = max(dp[i-1][0], dp[i-1][1] + prices[i]);
            dp[i][1] = max(dp[i-1][1], - prices[i]);
            dp[i][2] = max(dp[i-1][2], dp[i-1][3] + prices[i]);
            dp[i][3] = max(dp[i-1][3], dp[i-1][0] - prices[i]);
        }
        return dp[n-1][2];
    }
};
```

说实话，这里状态定义有点复杂了，应该按照*第一次持有*，*第一次不持有*，*第二次持有*，*第二次不持有*这样的顺序来定义。

### 188.[买卖股票的最佳时机 IV](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-iv/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Hard (49.61%) | 1158  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数数组 `prices` 和一个整数 `k` ，其中 `prices[i]` 是某支给定的股票在第 `i` 天的价格。

设计一个算法来计算你所能获取的最大利润。你最多可以完成 `k` 笔交易。也就是说，你最多可以买 `k` 次，卖 `k` 次。

**注意：**你不能同时参与多笔交易（你必须在再次购买前出售掉之前的股票）。

 

**示例 1：**

```
输入：k = 2, prices = [2,4,1]
输出：2
解释：在第 1 天 (股票价格 = 2) 的时候买入，在第 2 天 (股票价格 = 4) 的时候卖出，这笔交易所能获得利润 = 4-2 = 2 。
```

**示例 2：**

```
输入：k = 2, prices = [3,2,6,5,0,3]
输出：7
解释：在第 2 天 (股票价格 = 2) 的时候买入，在第 3 天 (股票价格 = 6) 的时候卖出, 这笔交易所能获得利润 = 6-2 = 4 。
     随后，在第 5 天 (股票价格 = 0) 的时候买入，在第 6 天 (股票价格 = 3) 的时候卖出, 这笔交易所能获得利润 = 3-0 = 3 。
```

**My solution**

这题和`123.买卖股票的最佳时机III`类型，找到递推公式的规律即可

```cpp
class Solution {
public:
    int maxProfit(int k, vector<int>& prices) {
        int n = prices.size();
        // * `+1` is to facilitate the coming procession in recycle
        vector<vector<int>> dp(n, vector<int>(2*k+1, 0));

        dp[0][0] = 0;
        for (int i = 1;i < 2*k + 1;++i) {
            if (i % 2 == 0) {
                dp[0][i] = 0;
            } else {
                dp[0][i] = -prices[0];
            }
        }

        for (int i = 1;i < n;++i) {
            dp[i][0] = 0;
            for (int j = 1;j < 2*k+1;++j) {
                if (j % 2 == 0) {
                    dp[i][j] = max(dp[i-1][j], dp[i-1][j-1] + prices[i]);
                } else {
                    dp[i][j] = max(dp[i-1][j], dp[i-1][j-1] - prices[i]);
                }
            }
        }
        return dp[n-1][2*k];

    }
};
```



### 279.[完全平方数](https://leetcode.cn/problems/perfect-squares/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (66.69%) | 1950  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/math" title="https://leetcode.com/tag/math" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/breadth-first-search" title="https://leetcode.com/tag/breadth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数 `n` ，返回 *和为 `n` 的完全平方数的最少数量* 。

**完全平方数** 是一个整数，其值等于另一个整数的平方；换句话说，其值等于一个整数自乘的积。例如，`1`、`4`、`9` 和 `16` 都是完全平方数，而 `3` 和 `11` 不是。

 

**示例 1：**

```
输入：n = 12
输出：3 
解释：12 = 4 + 4 + 4
```

**示例 2：**

```
输入：n = 13
输出：2
解释：13 = 4 + 9
```

**My solution**

*动态规划五步曲*

- 动态数组含义

  `dp[j]`表示完全平方数和为j的方案最小数

- 动态数组初始化

  我一开始初始化`dp[0]=1`，但是不对。因为题目中只提到了1为完全平方数，没有提到0

- 递推公式

  因为本题求的是最值，所以按照默认**先遍历物品**即可。

  ```cpp
  for (int i = 0;i <= size;++i) {
              for (int j = 1;j <= n;++j) {
                  if (j >= i * i) {
                      dp[j] = min(dp[j], dp[j-i*i] + 1);
                  }
              }
          }
  ```

*完整代码*

```cpp
class Solution {
public:
    int numSquares(int n) {
        // * determine meaning of dp array
        int size = sqrt(n);
        vector<int> dp(n+1, n);

        dp[0] = 0;
        int res  = 0;

        for (int i = 0;i <= size;++i) {
            for (int j = 1;j <= n;++j) {
                if (j >= i * i) {
                    dp[j] = min(dp[j], dp[j-i*i] + 1);
                }
            }
        }
        return dp[n];
    }
};
```













### 72.[编辑距离](https://leetcode.cn/problems/edit-distance/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (62.82%) | 3364  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/string" title="https://leetcode.com/tag/string" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你两个单词 `word1` 和 `word2`， *请返回将 `word1` 转换成 `word2` 所使用的最少操作数* 。

你可以对一个单词进行如下三种操作：

- 插入一个字符
- 删除一个字符
- 替换一个字符

 

**示例 1：**

```
输入：word1 = "horse", word2 = "ros"
输出：3
解释：
horse -> rorse (将 'h' 替换为 'r')
rorse -> rose (删除 'r')
rose -> ros (删除 'e')
```

**示例 2：**

```
输入：word1 = "intention", word2 = "execution"
输出：5
解释：
intention -> inention (删除 't')
inention -> enention (将 'i' 替换为 'e')
enention -> exention (将 'n' 替换为 'x')
exention -> exection (将 'n' 替换为 'c')
exection -> execution (插入 'u')
```

**My solution**

首先要弄清楚本题是动态规划的*二维数组*

感觉动态规划这类题目最主要的就是想明白

- 动态数组的含义
- 递推公式

另外，递推顺序也需要注意。递推顺序就是根据*递推公式*观察`dp`需要先前哪些位置的值，要保证这些位置的值都已经被计算。

*动态规划五步曲*

1. 确定动态数组的含义

   首先这是一个二维数组

   `dp[i][j]`表示以下标i-1结尾的word1和以下标j-1结尾的word2，使它们相同的最小操作次数

2. 初始化

   当`i=0`或`j=0`，word1 和 word2 都是空字符串，此时要使得相同，操作次数就是另一个字符串的长度

3. 确定递推公式

   ```cpp
   for (int i = 1;i <= n1;++i) {
               for (int j = 1;j <= n2;++j) {
                   if (word1[i-1] == word2[j-1]) {
                       dp[i][j] = dp[i-1][j-1];
                   } else {
                       dp[i][j] = min(dp[i-1][j], min(dp[i][j-1], dp[i-1][j-1])) + 1;
                   }
               }
           }
   ```

   当`word1[i-1]!=word2[j-1]`时，`dp[i-1][j]`代表的意思是删除word1[i-1]，`dp[i][j-1]`的意思是删除word2[j-1]，`dp[i-1][j-1]`表示对word1[i-1]或word2[j-1]进行替换。

*entire code*

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        // * determine meaning of dynamic programming
        // ? dp[i][j] means the shortest distance between str1 and str2 which end with index `i-1` and `j-1` respectively
        int n1 = word1.length();
        int n2 = word2.length();
        vector<vector<int>> dp(n1+1, vector<int>(n2+1, 0));

        // * initiate dp array
        for (int i = 0;i <= n1;++i) {
            dp[i][0] = i;
        }
        for (int i = 0;i <= n2;++i) {
            dp[0][i] = i;
        }

        // * determine recurrence formula
        for (int i = 1;i <= n1;++i) {
            for (int j = 1;j <= n2;++j) {
                if (word1[i-1] == word2[j-1]) {
                    dp[i][j] = dp[i-1][j-1];
                } else {
                    dp[i][j] = min(dp[i-1][j], min(dp[i][j-1], dp[i-1][j-1])) + 1;
                }
            }
        }
        return dp[n1][n2];
    }
};
```

## 二叉树

### 144.前序遍历

```cpp
class Solution {
public:
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> values;

        preorder(values, root);
        return values;

    }

    void preorder(vector<int>& values, TreeNode* cur) {
        if (cur == nullptr) {
            return;
        } 
        values.push_back(cur->val);
        preorder(values, cur->left);
        preorder(values, cur->right);
    }
};
```



### 145.[二叉树的后序遍历](https://leetcode.cn/problems/binary-tree-postorder-traversal/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (76.58%) | 1166  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/stack" title="https://leetcode.com/tag/stack" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一棵二叉树的根节点 `root` ，返回其节点值的 **后序遍历** 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/08/28/pre1.jpg)

```
输入：root = [1,null,2,3]
输出：[3,2,1]
```

**示例 2：**

```
输入：root = []
输出：[]
```

**示例 3：**

```
输入：root = [1]
输出：[1]
```

**My solution**

* 递归法，很简单，不给代码了

* 迭代法

  **一般写法**

  *后序遍历*的顺序是**左右中**。与前序遍历正好相反。可以参考*前序遍历*迭代法的一般写法。

  因为迭代法需要用到**栈**。在最后一步**翻转**之前，我们希望得到的顺序是**中右左**，所以在*入栈*操作时，顺序就应该是**中、左、右**

  ```cpp
  void itirator1(TreeNode* root, stack<TreeNode*> st, vector<int>& res) {
          if (root != nullptr) st.push(root);
          while(!st.empty()) {
              TreeNode* node = st.top();
              st.pop();
              res.push_back(node->val);
  
              if(node->left) st.push(node->left);
              if(node->right) st.push(node->right);
          }
          reverse(res.begin(), res.end()); 
      }
  ```

  **统一写法**

  关键思想是：每个需要输出的节点后插入一个`nullptr`指针，如果栈弹出的结点是`nullptr`就输出值。

  另外，在对结点操作时，*先从栈将结点弹出，重新入栈操作，避免**重复操作***。

  ```cpp
  void itirator2(TreeNode* root, stack<TreeNode*> st, vector<int> res) {
          if (root != nullptr) st.push(root);
          while(!st.empty()) {
              TreeNode* node = st.top();
              if (node != nullptr) {
                  st.pop();
                  st.push(node);
                  st.push(nullptr);
  
                  if(node->right) st.push(node->right);
                  if(node->left) st.push(node->left);
              }
              else {
                  st.pop();
                  node = st.top();
                  res.push_back(node->val);
                  st.pop();
              }
          }
      }
  ```



### 94.[二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (76.66%) | 2056  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/hash-table" title="https://leetcode.com/tag/hash-table" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/stack" title="https://leetcode.com/tag/stack" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个二叉树的根节点 `root` ，返回 *它的 **中序** 遍历* 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/09/15/inorder_1.jpg)

```
输入：root = [1,null,2,3]
输出：[1,3,2]
```

**示例 2：**

```
输入：root = []
输出：[]
```

**示例 3：**

```
输入：root = [1]
输出：[1]
```

**My solution**

* 迭代法

  **一般写法**

  由于需要处理的结点位于中间，所以处理起来会比较麻烦。

  ```cpp
  void itirator2(TreeNode* root, stack<TreeNode*> st, vector<int>& res) {
          // if (root != nullptr) st.push(root);
          // while(!st.empty()) {
          //     TreeNode* cur = st.top();
          //     // st.pop();
          //     if(cur != nullptr) {
          //         st.push(cur->left);
          //     }
          //     else {
          //         cur = st.top();
          //         res.push_back(cur->val);
          //         st.pop();
          //         st.push(cur->right);
          //     }
          // }
          TreeNode* cur = root;
          while (cur != nullptr || !st.empty()) {
              if (cur != nullptr) {
                  st.push(cur);
                  cur = cur->left;			// left
              }
              else {
                  cur = st.top();
                  st.pop();
                  res.push_back(cur->val);			// middle
                  cur = cur->right;							// right
              }
          }
      }
  ```

  **统一写法**

  ```cpp
  void itirator1(TreeNode* root, stack<TreeNode*> st, vector<int>& res) {
          if (root != nullptr) st.push(root);
          while(!st.empty()) {
              TreeNode* cur = st.top();
              st.pop();
              if (cur != nullptr) {
                  if (cur->right) st.push(cur->right);
                  st.push(cur);
                  st.push(nullptr);
                  if (cur->left) st.push(cur->left);
              }
              else {
                  cur = st.top();
                  res.push_back(cur->val);
                  st.pop();
              }
          }
      }
  ```

  ### 102.[二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/description/)

  |  Category  |   Difficulty    | Likes | Dislikes |
  | :--------: | :-------------: | :---: | :------: |
  | algorithms | Medium (66.94%) | 1929  |    -     |

  <details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/breadth-first-search" title="https://leetcode.com/tag/breadth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

  <details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

  ------

  给你二叉树的根节点 `root` ，返回其节点值的 **层序遍历** 。 （即逐层地，从左到右访问所有节点）。

   

  **示例 1：**

  ![img](https://assets.leetcode.com/uploads/2021/02/19/tree1.jpg)

  ```
  输入：root = [3,9,20,null,null,15,7]
  输出：[[3],[9,20],[15,7]]
  ```

  **示例 2：**

  ```
  输入：root = [1]
  输出：[[1]]
  ```

  **示例 3：**

  ```
  输入：root = []
  输出：[]
  ```

**My solution**

层序遍历也有**两种**写法

* 迭代法

  使用*队列*结构，在处理每一层的结点时，将其子结点都放入队列。

  这样一来，每次处理的就是一层的结点；所以，**队列的size要使用变量固定住**

  ```cpp
  vector<vector<int>> levelOrder(TreeNode* root) {
          queue<TreeNode*> que;
          if(root != nullptr) que.push(root);
          vector<vector<int>> res;
          
          while(!que.empty()) {
              vector<int> vec;
              int size = que.size();
              for (int i = 0;i < size;++i) {
                  TreeNode* cur = que.front();
                  vec.push_back(cur->val);
                  que.pop();
                  if(cur->left) que.push(cur->left);
                  if(cur->right) que.push(cur->right);
              }
              res.push_back(vec);
          }
  
          return res;
          // vector<vector<int>> res = recurveOrder(root);
          // return res;
      }
  ```

* 递归法

  关键思想：用`depth`来表示当前处理结点的层数，并把结点的值放入对应*层数*的数组中

  ```cpp
      vector<vector<int>> res;
          int depth = 0;
          order(root, res, depth);
          return res;
  
      }
  
      void order(TreeNode* root, vector<vector<int>>& res, int depth) {
          if (root == nullptr) return;
          if (res.size() == depth) res.push_back(vector<int>{});
          res[depth].push_back(root->val);
          order(root->left, res, depth + 1);
          order(root->right, res, depth + 1);
      }
  ```

### 101.[对称二叉树](https://leetcode.cn/problems/symmetric-tree/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (59.95%) | 2684  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/breadth-first-search" title="https://leetcode.com/tag/breadth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个二叉树的根节点 `root` ， 检查它是否轴对称。

 

**示例 1：**

![img](https://pic.leetcode.cn/1698026966-JDYPDU-image.png)

```
输入：root = [1,2,2,3,4,4,3]
输出：true
```

**示例 2：**

![img](https://pic.leetcode.cn/1698027008-nPFLbM-image.png)

```
输入：root = [1,2,2,null,3,null,3]
输出：false
```

**My solution**

这题的关键点在于，如何对两个结点进行比较：**左结点的左子树和右结点的右子树**；**左结点的右子树和右结点的左子树**

`compare()`

```cpp
bool compare(TreeNode* left, TreeNode* right) {
        if (!left && right) {
            return false;
        } else if (left && !right) {
            return false;
        } else if (!left && !right) {
            return true;
        } else if (left->val != right->val) {
            return false;
        }

        bool inside = compare(left->right, right->left);
        bool outside = compare(left->left, right->right);
        if (inside && outside) {
            return true;
        } else {
            return false;
        }

    }
```

```cpp
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if (nullptr == root) return true;
        return compare(root->left, root->right);
    }

    bool compare(TreeNode* left, TreeNode* right) {
        if (!left && right) {
            return false;
        } else if (left && !right) {
            return false;
        } else if (!left && !right) {
            return true;
        } else if (left->val != right->val) {
            return false;
        }

        bool inside = compare(left->right, right->left);
        bool outside = compare(left->left, right->right);
        if (inside && outside) {
            return true;
        } else {
            return false;
        }

    }
};
```

### 236.[二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (70.87%) | 2644  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个二叉树, 找到该树中两个指定节点的最近公共祖先。

[百度百科](https://baike.baidu.com/item/最近公共祖先/8918834?fr=aladdin)中最近公共祖先的定义为：“对于有根树 T 的两个节点 p、q，最近公共祖先表示为一个节点 x，满足 x 是 p、q 的祖先且 x 的深度尽可能大（**一个节点也可以是它自己的祖先**）。”

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 1
输出：3
解释：节点 5 和节点 1 的最近公共祖先是节点 3 。
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2018/12/14/binarytree.png)

```
输入：root = [3,5,1,6,2,0,8,null,null,7,4], p = 5, q = 4
输出：5
解释：节点 5 和节点 4 的最近公共祖先是节点 5 。因为根据定义最近公共祖先节点可以为节点本身。
```

**示例 3：**

```
输入：root = [1,2], p = 1, q = 2
输出：1
```

**My solution**

这题的关键是需要**自底向上**遍历二叉树，满足这一需求的遍历方式就是`后序遍历`

递归解法的几个步骤

1. 确定返回值和参数

   由于题目要求返回最近的结点，故返回类型为`TreeNode*`，参数为`root, p, q`

2. 确定终止条件

   首先是要查找树中结点*p*和*q*的位置，如果找到即返回。则`root==p || root== q || root == null`就返回`root`

3. 确定处理逻辑

   由于需要对回溯后的结点做判断，所以要使用`TreeNode* left = backtrace()`来接住回溯结果

   如果左右结点都不为空，则说明该`root`就是最近的公共结点；

   如果左右结点有一个为空，则另一个就是我们需要返回的（说明右边有一个结果）

   如果左右结点都为空，则没找到，返回`null`

```cpp
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == p || root == q || nullptr == root) return root;
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        if (nullptr == left && nullptr != right) {
            return right;
        } else if (nullptr != left && nullptr == right) {
            return left;
        } else if (nullptr != left && nullptr != right) {
            return root;
        } else {
            return nullptr;
        }
    }
};
```

### 617.[合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (79.32%) | 1388  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你两棵二叉树： `root1` 和 `root2` 。

想象一下，当你将其中一棵覆盖到另一棵之上时，两棵树上的一些节点将会重叠（而另一些不会）。你需要将这两棵树合并成一棵新二叉树。合并的规则是：如果两个节点重叠，那么将这两个节点的值相加作为合并后节点的新值；否则，**不为** null 的节点将直接作为新二叉树的节点。

返回合并后的二叉树。

**注意:** 合并过程必须从两个树的根节点开始。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/02/05/merge.jpg)

```
输入：root1 = [1,3,2,5], root2 = [2,1,3,null,4,null,7]
输出：[3,4,5,5,4,null,7]
```

**示例 2：**

```
输入：root1 = [1], root2 = [1,2]
输出：[2,2]
```

**My solution**

首先弄清楚本题合并二叉树的意思是*对应结点相加或补充*

各种遍历顺序都可以，写的时候要⚠️**遍历结果要用来更新本层递归的左右结点**

```cpp
class Solution {
public:
    TreeNode* mergeTrees(TreeNode* root1, TreeNode* root2) {
        if (nullptr == root1) return root2;
        if (nullptr == root2) return root1;
        root1->val += root2->val;
        // ! mergeTrees的返回值要对root1->left和root1->right更新
        root1->left = mergeTrees(root1->left, root2->left);
        root1->right = mergeTrees(root1->right, root2->right);
        return root1;
    }
};
```

### 110.[平衡二叉树](https://leetcode.cn/problems/balanced-binary-tree/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (58.25%) | 1499  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个二叉树，判断它是否是 平衡二叉树 

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/06/balance_1.jpg)

```
输入：root = [3,9,20,null,null,15,7]
输出：true
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/10/06/balance_2.jpg)

```
输入：root = [1,2,2,3,3,null,null,4,4]
输出：false
```

**示例 3：**

```
输入：root = []
输出：true
```

**My solution**

本题做的时候要想明白，每个传入下一层的节点，就是一颗*子树*，我们需要去判断该“子树”是否为*平衡二叉树*。如果不是，就直接返回（使用-1标记）。

如果“子树”是平衡二叉树，返回的是“子树”的**最大深度**

```cpp
class Solution {
public:
    bool isBalanced(TreeNode* root) {
        if (-1 == depth(root)) {
            return false;
        } else {
            return true;
        }
    }
    int depth(TreeNode* root) {
        if (root == nullptr) return 0;
        int res = 1;
        int leftdepth = depth(root->left);
        int rightdepth = depth(root->right);
        if (leftdepth == -1 || rightdepth == -1) {
            return -1;
        }
        if (abs(leftdepth - rightdepth) > 1) return -1;
        return max(res + leftdepth, res + rightdepth);
    }
  /*
    int mindepth(TreeNode* root) {
        if (nullptr == root) return 0;
        int res = 1;
        if (nullptr == root->left || nullptr == root->right) {
            return res;
        }
        int leftdepth = mindepth(root->left);
        int rightdepth = mindepth(root->right);
        return min(res + leftdepth, res + rightdepth);
    }
    int maxdepth(TreeNode* root) {
        if (nullptr == root) return 0;
        int res = 1;
        if (nullptr == root->left && nullptr == root->right) {
            return res;
        }
        int leftdepth = maxdepth(root->left);
        int rightdepth = maxdepth(root->right);
        return max(res + leftdepth, res + rightdepth);
    }
    */
    // pair<int, int> preorder(TreeNode* root) {
    //     if (nullptr == root) return 0;
    //     int res = 1;
    //     int leftdepth = res + preorder(root->left);
    //     int rightdepth = res + preorder(root->right);
    //     if (leftdepth <= rightdepth) {
    //         return {leftdepth, rightdepth};
    //     } else {
    //         return {rightdepth, leftdepth};
    //     }
    // }
};
```

### 257.[二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (70.73%) | 1122  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个二叉树的根节点 `root` ，按 **任意顺序** ，返回所有从根节点到叶子节点的路径。

**叶子节点** 是指没有子节点的节点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/03/12/paths-tree.jpg)

```
输入：root = [1,2,3,null,5]
输出：["1->2->5","1->3"]
```

**示例 2：**

```
输入：root = [1]
输出：["1"]
```

**My solution**

使用*回溯法*

需要⚠️`path.push_back()`放的位置

```cpp
class Solution {
public:
    vector<string> binaryTreePaths(TreeNode* root) {
        vector<int> path;
        vector<string> res;
        findPaths(root, res, path);
        return res;

    }
    void findPaths(TreeNode* root, vector<string>& res, vector<int>& path) {
        if (nullptr == root) return;
        path.push_back(root->val);			// ! 不能放在findPaths() 上一行
        if (nullptr == root->left && nullptr == root->right) {
            string connect = "->";
            string s = "";
            for (int i = 0;i < path.size()-1;++i) {
                string num = to_string(path[i]);
                s += num + connect;
            }
            s += to_string(path[path.size()-1]);
            res.push_back(s);
        }
        findPaths(root->left, res, path);
        findPaths(root->right, res, path);
        path.pop_back();
    }
};
```





## 图论

### 200.[岛屿数量](https://leetcode.cn/problems/number-of-islands/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (60.26%) | 2463  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/breadth-first-search" title="https://leetcode.com/tag/breadth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/union-find" title="https://leetcode.com/tag/union-find" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个由 `'1'`（陆地）和 `'0'`（水）组成的的二维网格，请你计算网格中岛屿的数量。

岛屿总是被水包围，并且每座岛屿只能由水平方向和/或竖直方向上相邻的陆地连接形成。

此外，你可以假设该网格的四条边均被水包围。

 

**示例 1：**

```
输入：grid = [
  ["1","1","1","1","0"],
  ["1","1","0","1","0"],
  ["1","1","0","0","0"],
  ["0","0","0","0","0"]
]
输出：1
```

**示例 2：**

```
输入：grid = [
  ["1","1","0","0","0"],
  ["1","1","0","0","0"],
  ["0","0","1","0","0"],
  ["0","0","0","1","1"]
]
输出：3
```

* 方法一

  广度遍历

  ```cpp
  class Solution {
  public:
      int numIslands(vector<vector<char>>& grid) {
          int dir[4][2] = {0, 1, 1, 0, -1, 0, 0, -1};
          int m = grid.size(), n = grid[0].size();
  
          vector<vector<bool>> visited(m, vector<bool>(n, false));
          int res = 0;
  
          for (int i = 0;i < m;++i) {
              for (int j = 0;j < n;++j) {
                  if (!visited[i][j] && grid[i][j] == '1') {
                      ++res;
                      bfs(grid, visited, dir, i, j);
                  }
              }
          }
          return res;
  
          
      }
      void bfs(vector<vector<char>>& grid, vector<vector<bool>>& visited, int dir[][2], int x, int y) {
          queue<pair<int, int>> que;
          que.push({x, y});
          visited[x][y] = true;
          while(!que.empty()) {
              int curx = que.front().first;
              int cury = que.front().second;
              que.pop();
              for (int i = 0;i < 4;++i) {
                  int nextx = curx + dir[i][0];
                  int nexty = cury + dir[i][1];
                  if (nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
                  if (grid[nextx][nexty] == '1' && !visited[nextx][nexty]) {
                      que.push({nextx, nexty});
                      visited[nextx][nexty] = true;
                  }
              }
          }
      }
  };
  ```

* 方法二：**深度遍历**

  ```cpp
  int numIslands(vector<vector<char>>& grid) {
          int dir[4][2] = {0, 1, 1, 0, -1, 0, 0, -1};
          int m = grid.size(), n = grid[0].size();
  
          vector<vector<bool>> visited(m, vector<bool>(n, false));
          int res = 0;
  
          for (int i = 0;i < m;++i) {
              for (int j = 0;j < n;++j) {
                  if (!visited[i][j] && grid[i][j] == '1') {
                      //visited[i][j] = true;
                      ++res;
                      //bfs(grid, visited, dir, i, j);
                      dfs(grid, visited, dir, i, j);
                  }
              }
          }
          return res;
      }
  
      void dfs(vector<vector<char>>& grid, vector<vector<bool>>& visited, int dir[][2], int x, int y) {
          if (visited[x][y] || grid[x][y] == '0') {
              return;
          }
          visited[x][y] = true;
          for (int i = 0;i < 4;++i) {
              int nextx = x + dir[i][0];
              int nexty = y + dir[i][1];
              if (nextx < 0 || nextx > grid.size()-1 || nexty < 0 || nexty > grid[0].size()-1) continue;
              dfs(grid, visited, dir, nextx, nexty);
  
              // if (!visited[nextx][nexty] && grid[nextx][nexty] == '1') {
              //     visited[nextx][nexty] = true;
              //     dfs(grid, visited, dir, nextx, nexty);
              // }
              
          }
      }
  ```

### 695.[岛屿的最大面积](https://leetcode.cn/problems/max-area-of-island/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (68.10%) | 1064  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个大小为 `m x n` 的二进制矩阵 `grid` 。

**岛屿** 是由一些相邻的 `1` (代表土地) 构成的组合，这里的「相邻」要求两个 `1` 必须在 **水平或者竖直的四个方向上** 相邻。你可以假设 `grid` 的四个边缘都被 `0`（代表水）包围着。

岛屿的面积是岛上值为 `1` 的单元格的数目。

计算并返回 `grid` 中最大的岛屿面积。如果没有岛屿，则返回面积为 `0` 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/05/01/maxarea1-grid.jpg)

```
输入：grid = [[0,0,1,0,0,0,0,1,0,0,0,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,1,1,0,1,0,0,0,0,0,0,0,0],[0,1,0,0,1,1,0,0,1,0,1,0,0],[0,1,0,0,1,1,0,0,1,1,1,0,0],[0,0,0,0,0,0,0,0,0,0,1,0,0],[0,0,0,0,0,0,0,1,1,1,0,0,0],[0,0,0,0,0,0,0,1,1,0,0,0,0]]
输出：6
解释：答案不应该是 11 ，因为岛屿只能包含水平或垂直这四个方向上的 1 。
```

**示例 2：**

```
输入：grid = [[0,0,0,0,0,0,0,0]]
输出：0
```



**My solution**

该类题目套路基本相同，无非是先遍历图，找到图中的“*岛屿*”，再使用**深度遍历**或**广度遍历**找到与其相连的岛屿并标记为*已访问*。在这之中再做一些操作

*深度遍历*or*广度遍历*

```cpp
class Solution {
public:
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        int dir[4][2] = {0, 1, 1, 0, 0, -1, -1, 0};
        int m = grid.size(), n = grid[0].size();
        vector<vector<bool>> visited(m, vector<bool>(n, false));
        int res = 0;
        int result = 0;
        for (int i = 0;i < grid.size();++i) {
            for (int j = 0;j < grid[0].size();++j) {
                if (!visited[i][j] && grid[i][j] == 1) {
                    res = 0;
                    dfs(grid, visited, i, j, dir, res);
                    result = max(result, res);
                }
            }
        }
        for (int i = 0;i < grid.size();++i) {
            for (int j = 0;j < grid[0].size();++j) {
                if (!visited[i][j] && grid[i][j] == 1) {
                    res = 1;
                    bfs(grid, visited, i, j, dir, res);
                    result = max(result, res);
                }
            }
        }
        return result;
    }

    void dfs(vector<vector<int>>& grid, vector<vector<bool>>& visited, int x, int y, int (*dir)[2], int& res) {
        if (grid[x][y] == 1 && !visited[x][y]) {
            res++;
        }
        if (grid[x][y] == 0 || visited[x][y]) {
            return;
        }
        visited[x][y] = true;
        for (int i = 0;i < 4;++i) {
            int nextx = x + dir[i][0];
            int nexty = y + dir[i][1];
            if (nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) {
                continue;
            }
            dfs(grid, visited, nextx, nexty, dir, res);
        }
    }

    void bfs(vector<vector<int>>& grid, vector<vector<bool>>& visited, int x, int y, int (*dir)[2], int& res) {
        queue<pair<int, int>> que;
        que.push({x, y});
        // * mark as visited, as soon as put in the queue
        visited[x][y] = true;
        while(!que.empty()) {
            int curx = que.front().first;
            int cury = que.front().second;
            que.pop();
            for (int i = 0;i < 4;++i) {
                int nextx = curx + dir[i][0];
                int nexty = cury + dir[i][1];
                if (nextx < 0 || nextx >= grid.size() || nexty < 0 || nexty >= grid[0].size()) continue;
                if (grid[nextx][nexty] == 1 && !visited[nextx][nexty]) {
                    que.push({nextx, nexty});
                    visited[nextx][nexty] = true;
                    res++;
                }
            }
        }
    }
};
```









### 1971.[寻找图中是否存在路径](https://leetcode.cn/problems/find-if-path-exists-in-graph/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (53.84%) |  184  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/Unknown" title="https://leetcode.com/tag/Unknown" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

有一个具有 `n` 个顶点的 **双向** 图，其中每个顶点标记从 `0` 到 `n - 1`（包含 `0` 和 `n - 1`）。图中的边用一个二维整数数组 `edges` 表示，其中 `edges[i] = [ui, vi]` 表示顶点 `ui` 和顶点 `vi` 之间的双向边。 每个顶点对由 **最多一条** 边连接，并且没有顶点存在与自身相连的边。

请你确定是否存在从顶点 `source` 开始，到顶点 `destination` 结束的 **有效路径** 。

给你数组 `edges` 和整数 `n`、`source` 和 `destination`，如果从 `source` 到 `destination` 存在 **有效路径** ，则返回 `true`，否则返回 `false` 。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/08/14/validpath-ex1.png)

```
输入：n = 3, edges = [[0,1],[1,2],[2,0]], source = 0, destination = 2
输出：true
解释：存在由顶点 0 到顶点 2 的路径:
- 0 → 1 → 2 
- 0 → 2
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/08/14/validpath-ex2.png)

```
输入：n = 6, edges = [[0,1],[0,2],[3,5],[5,4],[4,3]], source = 0, destination = 5
输出：false
解释：不存在由顶点 0 到顶点 5 的路径.
```

<mark>**并查集**</mark>

并查集有如下几个功能

1. 查找根节点

   ```cpp
   int find(vector<int>& father, int u) {
     if (u == father[u]) {
       return u;
     }
     // compress path length
     father[u] = find(father, father[u]);
     return father[u];
   }
   ```

2. 将两节点加入到一个集合（**这两个节点有边相连**）

   ```cpp
   void join(vector<int>& father, int u, int v) {
     u = find(father, u);
     v = find(father, v);
     if (u == v) {
       return;
     }
     father[v] = u;
   }
   ```

3. 判断两节点是否在同一集合

   ```cpp
   bool isSame(vector<int>& father, int u, int v) {
     u = find(father, u);
     v = find(father, v);
     return u==v;
   }
   ```

首先第一步是对`vector<int> father`数组初始化

```cpp
void init(vector<int>& father) {
  for (int i = 0;i < father.size();++i) {
    father[i] = i;
  }
}
```

*完整代码*

```cpp
class Solution {
public:
    bool validPath(int n, vector<vector<int>>& edges, int source, int destination) {
       // * 并查集
        //int n = edges.size();
        vector<int> father(n+2, 0);
        init(father, edges);

        for (int i = 0;i < edges.size();++i) {
            join(father, edges[i][0], edges[i][1]);
        }
        return isSame(father, source, destination);

    }

    void init(vector<int>& father, vector<vector<int>>& edges) {
        for (int i = 0;i < father.size();i++) {
            father[i] = i;
        }
    }

    int find(vector<int>& father, int u) {
        int v = father[u];
        if (v == u) return u;
        // * compress path length
        father[u] = find(father, v);
        return father[u];   
    }

    void join(vector<int>& father, int u, int v) {
        u = find(father, u);
        v = find(father, v);
        if (u == v) return;
        father[v] = u;
    }

    bool isSame(vector<int>& father, int u, int v) {
        u = find(father, u);
        v = find(father, v);
        return u==v;
    }
};
```

### 685.[冗余连接 II](https://leetcode.cn/problems/redundant-connection-ii/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Hard (42.17%) |  404  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/tree" title="https://leetcode.com/tag/tree" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/depth-first-search" title="https://leetcode.com/tag/depth-first-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/union-find" title="https://leetcode.com/tag/union-find" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/graph" title="https://leetcode.com/tag/graph" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

在本问题中，有根树指满足以下条件的 **有向** 图。该树只有一个根节点，所有其他节点都是该根节点的后继。该树除了根节点之外的每一个节点都有且只有一个父节点，而根节点没有父节点。

输入一个有向图，该图由一个有着 `n` 个节点（节点值不重复，从 `1` 到 `n`）的树及一条附加的有向边构成。附加的边包含在 `1` 到 `n` 中的两个不同顶点间，这条附加的边不属于树中已存在的边。

结果图是一个以边组成的二维数组 `edges` 。 每个元素是一对 `[ui, vi]`，用以表示 **有向** 图中连接顶点 `ui` 和顶点 `vi` 的边，其中 `ui` 是 `vi` 的一个父节点。

返回一条能删除的边，使得剩下的图是有 `n` 个节点的有根树。若有多个答案，返回最后出现在给定二维数组的答案。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/12/20/graph1.jpg)

```
输入：edges = [[1,2],[1,3],[2,3]]
输出：[2,3]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/12/20/graph2.jpg)

```
输入：edges = [[1,2],[2,3],[3,4],[4,1],[1,5]]
输出：[4,1]
```

**My solution**

这题是一个*有向图*。需要对题目分析，要删除的节点无非有两种情况：

1. 结点的*入度*为2

   下面是寻找节点*入度*的代码

   ```cpp
   // * record indegree
   vector<int> inDegree(n + 5, 0);
   for (int i = 0;i < edges.size();++i) {
     inDegree[edges[i][1]]++;
   }
   ```

   然后判断删去*入度为2*的边（入度为2的节点边不加入图中）之后，能不能构成一个树（会不会出现环---使用**并查集**）

   ```cpp
   bool isTreeAfterRemove(vector<vector<int>>& edges, vector<int>& father, int node) {
           init(father);
           for (int i = 0;i < edges.size();++i) {
               // * 在构建并查集时，跳过所要删除的结点, 然后判断剩下的节点会不会构成有向环
               if(i == node) continue;
               if (isSame(father, edges[i][0], edges[i][1])) {
                   return false;
               }
               join(father, edges[i][0], edges[i][1]);
           }
           return true;
       }
   ```

2. 没有入度为2的节点，*一定是出现了有向环*

   有向环的判断就是用**并查集**，如果遍历过程中出现了*两节点的父节点相同*就说明这两个节点已经出现在同一个集合，*加入该边之后就会出现环*。

   ```cpp
   vector<int> getRemoveEdge(vector<int>& father, vector<vector<int>>& edges) {
           init(father);
           for(int i = 0;i < edges.size();++i) {
               if (isSame(father, edges[i][0], edges[i][1])) {
                   return edges[i];
               }
               join(father, edges[i][0], edges[i][1]);
           }
           return vector<int>{};
       }
   ```

   因为要返回数组中出现靠后的边，所以要从前往后遍历。

### 53. [寻宝（第七期模拟笔试）](https://kamacoder.com/problempage.php?pid=1053)

时间限制：1.000S 空间限制：128MB

题目描述

在世界的某个区域，有一些分散的神秘岛屿，每个岛屿上都有一种珍稀的资源或者宝藏。国王打算在这些岛屿上建公路，方便运输。

不同岛屿之间，路途距离不同，国王希望你可以规划建公路的方案，如何可以以最短的总公路距离将 所有岛屿联通起来（注意：这是一个无向图）。 

给定一张地图，其中包括了所有的岛屿，以及它们之间的距离。以最小化公路建设长度，确保可以链接到所有岛屿。

输入描述

第一行包含两个整数V 和 E，V代表顶点数，E代表边数 。顶点编号是从1到V。例如：V=2，一个有两个顶点，分别是1和2。

接下来共有 E 行，每行三个整数 v1，v2 和 val，v1 和 v2 为边的起点和终点，val代表边的权值。

输出描述

输出联通所有岛屿的最小路径总距离

输入示例

```
7 11
1 2 1
1 3 1
1 5 2
2 6 1
2 4 2
2 3 2
3 4 1
4 5 1
5 6 2
5 7 1
6 7 1
```

输出示例

```
6
```

> **My solution**

使用`prime`算法

```cpp
#include<iostream>
#include<vector>
#include <climits>
#include <numeric>

using namespace std;
int main() {
    int v, e;
    int x, y, k;
    cin >> v >> e;
    // 填一个默认最大值，题目描述val最大为10000
    vector<vector<int>> grid(v + 1, vector<int>(v + 1, 10001));
    while (e--) {
        cin >> x >> y >> k;
        // 因为是双向图，所以两个方向都要填上
        grid[x][y] = k;
        grid[y][x] = k;

    }
    
    vector<bool> inTree(v+1, false);
    vector<int> minDist(v+1, 10001);
    
    // * begin the recurrence 
    for (int j = 1;j < v;++j) {
        int cur = -1;
        int minVal = INT_MAX;
        // * choose outer node with minimal dist
        for (int i = 1;i <= v;++i) {
            if (!inTree[i] && minDist[i] < minVal) {
                minVal = minDist[i];
                cur = i;
            }
        }
        // * add the node into tree
        inTree[cur] = true;
        // * update the minDist
        for (int i = 1;i <= v;++i) {
            if (!inTree[i] && grid[cur][i] < minDist[i]) {
                minDist[i] = grid[cur][i];
            }
        }
    }
    // int result = 0;
    // for (int i = 2; i <= v; i++) { // 不计第一个顶点，因为统计的是边的权值，v个节点有 v-1条边
    //     result += minDist[i];
    // }
    // cout << endl;
   	cout << accumulate(minDist.begin()+2, minDist.end(), 0) << endl;
    // return result;
}
```

比较坑的是*卡码网*最后的结果是要打印出来的。

> **Kruskal**算法

相对于`prime`算法针对*结点*，这个算法是针对于*边*

结点多，边少的情况可以使用`Kruskal`算法

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
#include <numeric>

using namespace std;

void init(vector<int>& father) {
    for (int i = 0;i < father.size();++i) {
        father[i] = i;
    }
}

int find(vector<int>& father, int u) {
    // if (u == father[u]) {
    //     return u;
    // } else {
    //     father[u] = find(father, u);
    //     return father[u];
    // }
    int v = father[u];
    if (u == v) return u;
    father[u] = find(father, v);
    return father[u];
}

void join(vector<int>& father, int u, int v) {
    u = find(father, u);
    v = find(father, v);
    if (u == v) return;
    father[v] = u;
}

bool isSame(vector<int>& father, int u, int v) {
    if(find(father, u) == find(father, v)) {
        return true;
    } else {
        return false;
    }
}

struct Edge {
    int v1;
    int v2;
    int val;
};


int main() {

    int v, e;
    int v1, v2, val;
    vector<Edge> edges;
    int result_val = 0;
    cin >> v >> e;
    while (e--) {
        cin >> v1 >> v2 >> val;
        edges.push_back({v1, v2, val});
    }
    sort(edges.begin(), edges.end(), [](Edge a, Edge b){
                                        return a.val < b.val;
    });
    
    vector<int> res;
    vector<bool> inTree(10001, false);
    vector<int> father(10001, 0);
    init(father);
    
    for (int i = 0;i < edges.size();++i) {
        int node1 = edges[i].v1, node2 = edges[i].v2;
        if (!isSame(father, node1, node2)) {
            inTree[node1] = true;
            inTree[node2] = true;
            res.push_back(edges[i].val);
            join(father, node1, node2);
        }

    }
    
    int result = accumulate(res.begin(), res.end(), 0);
    
    cout << result << endl;
    return 0;  
}
```

### [117. 软件构建](https://kamacoder.com/problempage.php?pid=1191)

时间限制：1.000S 空间限制：256MB

题目描述

某个大型软件项目的构建系统拥有 N 个文件，文件编号从 0 到 N - 1，在这些文件中，某些文件依赖于其他文件的内容，这意味着如果文件 A 依赖于文件 B，则必须在处理文件 A 之前处理文件 B （0 <= A, B <= N - 1）。请编写一个算法，用于确定文件处理的顺序。

输入描述

第一行输入两个正整数 N, M。表示 N 个文件之间拥有 M 条依赖关系。

后续 M 行，每行两个正整数 S 和 T，表示 T 文件依赖于 S 文件。

输出描述

输出共一行，如果能处理成功，则输出文件顺序，用空格隔开。 

如果不能成功处理（相互依赖），则输出 -1。

输入示例

```
5 4
0 1
0 2
1 3
2 4
```

输出示例

```
0 1 2 3 4
```

提示信息

文件依赖关系如下：



![img](https://kamacoder.com/upload/kamacoder.com/image/20240430/20240430150745_18459.png)



所以，文件处理的顺序除了示例中的顺序，还存在

0 2 4 1 3

0 2 1 3 4

等等合法的顺序。

数据范围：

0 <= N <= 10 ^ 5

1 <= M <= 10 ^ 9

每行末尾无空格。

> **My solution**

拓扑排序的思想就是：先找到入度为0的结点，不断更新选择的结点之后结点的入度信息。总是选择入度为零的结点，如果没有入度为0的结点，就说明出现了环。

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <unordered_map>
using namespace std;
int main() {
    int m, n, s, t;
    cin >> n >> m;
    vector<int> inDegree(n, 0); // 记录每个文件的入度

    unordered_map<int, vector<int>> umap;// 记录文件依赖关系
    vector<int> result; // 记录结果

    while (m--) {
        // s->t，先有s才能有t
        cin >> s >> t;
        inDegree[t]++; // t的入度加一
        umap[s].push_back(t); // 记录s指向哪些文件
    }
    
    queue<int> que;
    
    for (int i = 0;i < inDegree.size();++i) {
        if (inDegree[i] == 0) {
            que.push(i);
        }
    }
    while(!que.empty()) {
        int cur = que.front();
        que.pop();
        result.push_back(cur);
        
        vector<int> files = umap[cur];
        for (int i = 0;i < files.size();++i) {
            inDegree[files[i]]--;
            if (inDegree[files[i]] == 0) {
                que.push(files[i]);
            }
        }
    }
    
    if (result.size() == n) {
        for (int i = 0; i < n - 1; i++) cout << result[i] << " ";
        cout << result[n - 1];
    } else cout << -1 << endl;
}  
```



> **My solution**

### 47. 参加科学大会（第六期模拟笔试）

时间限制：1.500S 空间限制：128MB

题目描述

小明是一位科学家，他需要参加一场重要的国际科学大会，以展示自己的最新研究成果。

小明的起点是第一个车站，终点是最后一个车站。然而，途中的各个车站之间的道路状况、交通拥堵程度以及可能的自然因素（如天气变化）等不同，这些因素都会影响每条路径的通行时间。

小明希望能选择一条花费时间最少的路线，以确保他能够尽快到达目的地。

输入描述

第一行包含两个正整数，第一个正整数 N 表示一共有 N 个公共汽车站，第二个正整数 M 表示有 M 条公路。 

接下来为 M 行，每行包括三个整数，S、E 和 V，代表了从 S 车站可以单向直达 E 车站，并且需要花费 V 单位的时间。

输出描述

输出一个整数，代表小明从起点到终点所花费的最小时间。

输入示例

```
7 9
1 2 1
1 3 4
2 3 2
2 4 5
3 4 2
4 5 3
2 6 4
5 7 4
6 7 9
```

输出示例

```
12
```

提示信息

**能够到达的情况：**

如下图所示，起始车站为 1 号车站，终点车站为 7 号车站，绿色路线为最短的路线，路线总长度为 12，则输出 12。



![img](https://kamacoder.com/upload/kamacoder.com/image/20240122/20240122163716_71030.png)

**
**

**不能到达的情况：**

如下图所示，当从起始车站不能到达终点车站时，则输出 -1。



![img](https://kamacoder.com/upload/kamacoder.com/image/20240125/20240125154052_26956.png)

> **My solution**

有几个细节需要注意：

* 选择的是结点，所以要**循环n次**

* `cur`要初始化为1，因为最后一遍遍历全都访问过，`cur=-1`会导致访问出错
* 要先判断`grid[cur][j]`是否为`INT_MAX`，否则`minDist[cur] + grid[cur][j]`会导致整形溢出

需要理解这些数组的含义：

* `minDist`

  `minDist[i]`表示上一个选中的结点到结点`i`的最短距离

* `minDist[cur]+grid[cur][j]`

  表示如果选择结点`j`，总共距离是多少

```cpp
#include <iostream>
#include <vector>
#include <climits>
using namespace std;
int main() {
    int n, m, p1, p2, val;
    cin >> n >> m;

    vector<vector<int>> grid(n + 1, vector<int>(n + 1, INT_MAX));
    for(int i = 0; i < m; i++){
        cin >> p1 >> p2 >> val;
        grid[p1][p2] = val;
    }

    int start = 1;
    int end = n;
    
    vector<int> minDist(n+1, INT_MAX);
    vector<bool> visited(n+1, false);
    
    minDist[start] = 0;
    
    for (int i = 1;i <= n;++i) {
        int cur = 1;
        int minVal = INT_MAX;
        for (int j = 1;j <= n;++j) {
            if (!visited[j] && minDist[j] < minVal) {
                minVal = minDist[j];
                cur = j;
            }
        }
        visited[cur] = true;
        for (int j = 1;j <= n;++j) {
            if (!visited[j] && grid[cur][j] != INT_MAX && minDist[cur] + grid[cur][j] < minDist[j]) {
                minDist[j] = minDist[cur] + grid[cur][j];
            }
        }
        // // 打印日志：
        // cout << "select:" << cur << endl;
        // for (int v = 1; v <= n; v++) cout <<  v << ":" << minDist[v] << " ";
        // cout << endl << endl;
    }
    if (minDist[end] == INT_MAX) cout << -1 << endl; // 不能到达终点
    else cout << minDist[end] << endl; // 到达终点最短路径
}
```











## 单调栈

### 739.[每日温度](https://leetcode.cn/problems/daily-temperatures/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (68.76%) | 1747  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/hash-table" title="https://leetcode.com/tag/hash-table" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/stack" title="https://leetcode.com/tag/stack" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个整数数组 `temperatures` ，表示每天的温度，返回一个数组 `answer` ，其中 `answer[i]` 是指对于第 `i` 天，下一个更高温度出现在几天后。如果气温在这之后都不会升高，请在该位置用 `0` 来代替。

 

**示例 1:**

```
输入: temperatures = [73,74,75,71,69,72,76,73]
输出: [1,1,4,2,1,1,0,0]
```

**示例 2:**

```
输入: temperatures = [30,40,50,60]
输出: [1,1,1,0]
```

**示例 3:**

```
输入: temperatures = [30,60,90]
输出: [1,1,0]
```

**My solution**

这题主要就是对*单调栈*的应用

```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        int n = temperatures.size();
        vector<int> res(n, 0);
        // vector<int> res;
        stack<int> st;
        st.push(0);
        for (int i = 1;i < n;++i) {
            if (temperatures[i] <= temperatures[st.top()]) {
                st.push(i);
            } else {
                while(!st.empty() && temperatures[i] > temperatures[st.top()]) {
                    // res.push_back(i-st.top());
                    res[st.top()] = i - st.top();
                    st.pop();
                }
                st.push(i);
            } 
        }
        return res;
    }
};
```





### 496.[下一个更大元素 I](https://leetcode.cn/problems/next-greater-element-i/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (71.91%) | 1164  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/stack" title="https://leetcode.com/tag/stack" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

`nums1` 中数字 `x` 的 **下一个更大元素** 是指 `x` 在 `nums2` 中对应位置 **右侧** 的 **第一个** 比 `x` 大的元素。

给你两个 **没有重复元素** 的数组 `nums1` 和 `nums2` ，下标从 **0** 开始计数，其中`nums1` 是 `nums2` 的子集。

对于每个 `0 <= i < nums1.length` ，找出满足 `nums1[i] == nums2[j]` 的下标 `j` ，并且在 `nums2` 确定 `nums2[j]` 的 **下一个更大元素** 。如果不存在下一个更大元素，那么本次查询的答案是 `-1` 。

返回一个长度为 `nums1.length` 的数组 `ans` 作为答案，满足 `ans[i]` 是如上所述的 **下一个更大元素** 。

 

**示例 1：**

```
输入：nums1 = [4,1,2], nums2 = [1,3,4,2].
输出：[-1,3,-1]
解释：nums1 中每个值的下一个更大元素如下所述：
- 4 ，用加粗斜体标识，nums2 = [1,3,4,2]。不存在下一个更大元素，所以答案是 -1 。
- 1 ，用加粗斜体标识，nums2 = [1,3,4,2]。下一个更大元素是 3 。
- 2 ，用加粗斜体标识，nums2 = [1,3,4,2]。不存在下一个更大元素，所以答案是 -1 。
```

**示例 2：**

```
输入：nums1 = [2,4], nums2 = [1,2,3,4].
输出：[3,-1]
解释：nums1 中每个值的下一个更大元素如下所述：
- 2 ，用加粗斜体标识，nums2 = [1,2,3,4]。下一个更大元素是 3 。
- 4 ，用加粗斜体标识，nums2 = [1,2,3,4]。不存在下一个更大元素，所以答案是 -1 。
```

**My solution**

本题我用到了`map`数据结构

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        stack<int> st;
        vector<int> res;
        map<int, int> tag;

        int n1 = nums1.size();
        int n2 = nums2.size();

        st.push(0);
        for (int i = 1;i < n2;++i) {
            if (nums2[i] <= nums2[st.top()]) {
                st.push(i);
            } else {
                // ! `st` can't be empty
                //while(nums2[i] > nums2[st.top()] && !st.empty()) {
                while(!st.empty() && nums2[i] > nums2[st.top()]) {
                    tag[nums2[st.top()]] = nums2[i];
                    st.pop();
                }
                st.push(i);
            }
        }
        for (int i = 0;i < n1;++i) {
            if (tag.count(nums1[i]) != 0) {
                res.push_back(tag[nums1[i]]);
            } else {
                res.push_back(-1);
            }
        }

        return res;
    }
};
```





### 503.[下一个更大元素 II](https://leetcode.cn/problems/next-greater-element-ii/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (67.33%) |  927  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/stack" title="https://leetcode.com/tag/stack" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个循环数组 `nums` （ `nums[nums.length - 1]` 的下一个元素是 `nums[0]` ），返回 *`nums` 中每个元素的 **下一个更大元素*** 。

数字 `x` 的 **下一个更大的元素** 是按数组遍历顺序，这个数字之后的第一个比它更大的数，这意味着你应该循环地搜索它的下一个更大的数。如果不存在，则输出 `-1` 。

 

**示例 1:**

```
输入: nums = [1,2,1]
输出: [2,-1,2]
解释: 第一个 1 的下一个更大的数是 2；
数字 2 找不到下一个更大的数； 
第二个 1 的下一个最大的数需要循环搜索，结果也是 2。
```

**示例 2:**

```
输入: nums = [1,2,3,4,3]
输出: [2,3,4,-1,4]
```

**My solution**

本题首先要用到单调栈是肯定，其次，要注意如何去遍历数组，**循环数组的遍历方法**

```cpp
for (int i = 0;i < 2*n;++i) {
  int index = i % n;
}
```

*完整代码*

```cpp
class Solution {
public:
    vector<int> nextGreaterElements(vector<int>& nums) {
        int n = nums.size();
        vector<int> res(n, -1);

        stack<int> st;
        st.push(0);
        for (int i = 1;i < 2*n;++i) {
            int index = i % n;
            //st.push(index);
            if (nums[index] <= nums[st.top()]) {
                st.push(index);
            } else {
                while (!st.empty() && nums[index] > nums[st.top()]) {
                    if (res[st.top()] == -1) {
                        res[st.top()] = nums[index];
                    }
                    st.pop();
                }
                st.push(index);
            }
        }
        return res;
    }
};
```

## 贪心算法

### 455.[分发饼干](https://leetcode.cn/problems/assign-cookies/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (56.16%) |  849  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/greedy" title="https://leetcode.com/tag/greedy" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

假设你是一位很棒的家长，想要给你的孩子们一些小饼干。但是，每个孩子最多只能给一块饼干。

对每个孩子 `i`，都有一个胃口值 `g[i]`，这是能让孩子们满足胃口的饼干的最小尺寸；并且每块饼干 `j`，都有一个尺寸 `s[j]` 。如果 `s[j] >= g[i]`，我们可以将这个饼干 `j` 分配给孩子 `i` ，这个孩子会得到满足。你的目标是尽可能满足越多数量的孩子，并输出这个最大数值。

 

**示例 1:**

```
输入: g = [1,2,3], s = [1,1]
输出: 1
解释: 
你有三个孩子和两块小饼干，3个孩子的胃口值分别是：1,2,3。
虽然你有两块小饼干，由于他们的尺寸都是1，你只能让胃口值是1的孩子满足。
所以你应该输出1。
```

**示例 2:**

```
输入: g = [1,2], s = [1,2,3]
输出: 2
解释: 
你有两个孩子和三块小饼干，2个孩子的胃口值分别是1,2。
你拥有的饼干数量和尺寸都足以让所有孩子满足。
所以你应该输出2.
```

**My solution**

本题是简单的贪心算法应用。

我在解决的时候首先将两数组排序，使得最大的饼干满足胃口最大的孩子，以此类推。

```cpp
class Solution {
public:
    int findContentChildren(vector<int>& g, vector<int>& s) {
        sort(g.begin(), g.end());
        sort(s.begin(), s.end());
        
        int sn = s.size();
        int gn = g.size();
        int res = 0;

        int i = gn-1;
        int j = sn-1;
        while(i >= 0 && j >= 0) {
            if (s[j] >= g[i]) {
                ++res;
                --j;
                --i;
            } else {
                --i;
            }
        }
        return res;
    }
};
```

### 376.[摆动序列](https://leetcode.cn/problems/wiggle-subsequence/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (46.42%) | 1115  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/greedy" title="https://leetcode.com/tag/greedy" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

如果连续数字之间的差严格地在正数和负数之间交替，则数字序列称为 **摆动序列 。**第一个差（如果存在的话）可能是正数或负数。仅有一个元素或者含两个不等元素的序列也视作摆动序列。

- 例如， `[1, 7, 4, 9, 2, 5]` 是一个 **摆动序列** ，因为差值 `(6, -3, 5, -7, 3)` 是正负交替出现的。
- 相反，`[1, 4, 7, 2, 5]` 和 `[1, 7, 4, 5, 5]` 不是摆动序列，第一个序列是因为它的前两个差值都是正数，第二个序列是因为它的最后一个差值为零。

**子序列** 可以通过从原始序列中删除一些（也可以不删除）元素来获得，剩下的元素保持其原始顺序。

给你一个整数数组 `nums` ，返回 `nums` 中作为 **摆动序列** 的 **最长子序列的长度** 。

 

**示例 1：**

```
输入：nums = [1,7,4,9,2,5]
输出：6
解释：整个序列均为摆动序列，各元素之间的差值为 (6, -3, 5, -7, 3) 。
```

**示例 2：**

```
输入：nums = [1,17,5,10,13,15,10,5,16,8]
输出：7
解释：这个序列包含几个长度为 7 摆动序列。
其中一个是 [1, 17, 10, 13, 10, 16, 8] ，各元素之间的差值为 (16, -7, 3, -3, 6, -8) 。
```

**示例 3：**

```
输入：nums = [1,2,3,4,5,6,7,8,9]
输出：2
```

**My solution**

这题要想到用波峰，波谷去理解。

`sufdiff = nums[i+1] - nums[i]`，`prediff`初始化为0，使用`prediff = sufdiff`进行迭代。

```cpp
class Solution {
public:
    int wiggleMaxLength(vector<int>& nums) {
        int n = nums.size();
        if (n <= 1) {
            return n;
        }
        int length = 1;
        int prediff = 0;
        int sufdiff;
        for (int i = 0;i < n-1;++i) {
            sufdiff = nums[i+1] - nums[i];
            if ((prediff <= 0 && sufdiff > 0) || (prediff >= 0 && sufdiff < 0)) {
                ++length;
                prediff = sufdiff;
            }
        }
        return length;
    }
}
```





### 53.[最大子数组和](https://leetcode.cn/problems/maximum-subarray/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (55.23%) | 6632  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/divide-and-conquer" title="https://leetcode.com/tag/divide-and-conquer" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/dynamic-programming" title="https://leetcode.com/tag/dynamic-programming" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数数组 `nums` ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

**子数组** 是数组中的一个连续部分。

 

**示例 1：**

```
输入：nums = [-2,1,-3,4,-1,2,1,-5,4]
输出：6
解释：连续子数组 [4,-1,2,1] 的和最大，为 6 。
```

**示例 2：**

```
输入：nums = [1]
输出：1
```

**示例 3：**

```
输入：nums = [5,4,-1,7,8]
输出：23
```

**My solution**

本题的思路是计算*以正整数开头的子序列和*

可以想象，如果子序列的第一个数字是负数，肯定没有第一个数字是正整数的子序列和大。

另外当子序列的和小于0时，就要重新计算下一个子序列和。

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int res = INT32_MIN;
        int n = nums.size();

        int i = 0;
        while (i < n) {
            int count = 0;
            int j = i;
            for (;j < n;++j) {
                count += nums[j];
                res = max(res, count);
                if (count < 0) {
                    i = j+1;
                    break;
                }
            }
            i = j + 1;
        }
        return res;
    }
};
```

### 55.[跳跃游戏](https://leetcode.cn/problems/jump-game/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (43.30%) | 2727  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/greedy" title="https://leetcode.com/tag/greedy" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个非负整数数组 `nums` ，你最初位于数组的 **第一个下标** 。数组中的每个元素代表你在该位置可以跳跃的最大长度。

判断你是否能够到达最后一个下标，如果可以，返回 `true` ；否则，返回 `false` 。

 

**示例 1：**

```
输入：nums = [2,3,1,1,4]
输出：true
解释：可以先跳 1 步，从下标 0 到达下标 1, 然后再从下标 1 跳 3 步到达最后一个下标。
```

**示例 2：**

```
输入：nums = [3,2,1,0,4]
输出：false
解释：无论怎样，总会到达下标为 3 的位置。但该下标的最大跳跃长度是 0 ， 所以永远不可能到达最后一个下标。
```

**My solution**

这题我的想法是用变量`int range`来记录可以跳的最大范围，并不断更新遍历，如果遍历过程中`range`超过n-1就返回`true`

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int n = nums.size();
        int range = 0;
        int i = 0;
        while(i <= range) {
            range = max(range, i + nums[i]);
            if (range >= n-1) return true;
            i++;
        }
        return false;
    }
};
```

### 122.[买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (73.29%) | 2466  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/greedy" title="https://leetcode.com/tag/greedy" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个整数数组 `prices` ，其中 `prices[i]` 表示某支股票第 `i` 天的价格。

在每一天，你可以决定是否购买和/或出售股票。你在任何时候 **最多** 只能持有 **一股** 股票。你也可以先购买，然后在 **同一天** 出售。

返回 *你能获得的 **最大** 利润* 。

 

**示例 1：**

```
输入：prices = [7,1,5,3,6,4]
输出：7
解释：在第 2 天（股票价格 = 1）的时候买入，在第 3 天（股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4 。
     随后，在第 4 天（股票价格 = 3）的时候买入，在第 5 天（股票价格 = 6）的时候卖出, 这笔交易所能获得利润 = 6 - 3 = 3 。
     总利润为 4 + 3 = 7 。
```

**示例 2：**

```
输入：prices = [1,2,3,4,5]
输出：4
解释：在第 1 天（股票价格 = 1）的时候买入，在第 5 天 （股票价格 = 5）的时候卖出, 这笔交易所能获得利润 = 5 - 1 = 4 。
     总利润为 4 。
```

**示例 3：**

```
输入：prices = [7,6,4,3,1]
输出：0
解释：在这种情况下, 交易无法获得正利润，所以不参与交易可以获得最大利润，最大利润为 0 。
```

**My solution**

这题和`376.摆动序列`类似，但比*摆动序列*要简单，只需要判断`sufdiff`是否大于0即可，若其大于0，说明利润是正的，累加即可。

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int profit = 0;
        int n = prices.size();
        int sufdiff = 0;
        for (int i = 0;i < n-1;++i) {
            sufdiff = prices[i+1] - prices[i];
            if (sufdiff > 0) {
                profit += sufdiff;
            }
        }
        return profit;
    }
};
```

## 双指针法

### 27. [移除元素](https://leetcode.cn/problems/remove-element/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (59.40%) | 2199  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/two-pointers" title="https://leetcode.com/tag/two-pointers" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个数组 `nums` 和一个值 `val`，你需要 **[原地](https://baike.baidu.com/item/原地算法)** 移除所有数值等于 `val` 的元素，并返回移除后数组的新长度。

不要使用额外的数组空间，你必须仅使用 `O(1)` 额外空间并 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组**。

元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。

 

**说明:**

为什么返回数值是整数，但输出的答案是数组呢?

请注意，输入数组是以**「引用」**方式传递的，这意味着在函数里修改输入数组对于调用者是可见的。

你可以想象内部操作如下:

```
// nums 是以“引用”方式传递的。也就是说，不对实参作任何拷贝
int len = removeElement(nums, val);

// 在函数里修改输入数组对于调用者是可见的。
// 根据你的函数返回的长度, 它会打印出数组中 该长度范围内 的所有元素。
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```

 

**示例 1：**

```
输入：nums = [3,2,2,3], val = 3
输出：2, nums = [2,2]
解释：函数应该返回新的长度 2, 并且 nums 中的前两个元素均为 2。你不需要考虑数组中超出新长度后面的元素。例如，函数返回的新长度为 2 ，而 nums = [2,2,3,3] 或 nums = [2,2,0,0]，也会被视作正确答案。
```

**示例 2：**

```
输入：nums = [0,1,2,2,3,0,4,2], val = 2
输出：5, nums = [0,1,3,0,4]
解释：函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。注意这五个元素可为任意顺序。你不需要考虑数组中超出新长度后面的元素。
```

**My solution**

该说不说，第一题双指针法整了我挺久，因为之前没写过双指针法，还是有些细节不知道。

这是我写的代码

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int n = nums.size();
        int i = 0, j = 0;
        while (i < n && j < n) {
            if (nums[j] == val) {
                ++j;
            } else if (i != j) {
                nums[i] = nums[j];
                ++i, ++j;
            } else {
                ++i, ++j;
            }
        }
        return i;
    }
};
```

这是*代码随想录*的代码

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slowIndex = 0;
        for (int fastIndex = 0; fastIndex < nums.size(); fastIndex++) {
            if (val != nums[fastIndex]) {
                nums[slowIndex++] = nums[fastIndex];
            }
        }
        return slowIndex;
    }
};
```

**总结**

其实可以不用考虑原始数组，直接用*快指针*遍历原始数组；由于*快指针*已经遍历过，*慢指针*遍历的数组可以直接当成新数组，直接赋值。

### 541.[反转字符串 II](https://leetcode.cn/problems/reverse-string-ii/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (57.47%) |  591  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/string" title="https://leetcode.com/tag/string" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个字符串 `s` 和一个整数 `k`，从字符串开头算起，每计数至 `2k` 个字符，就反转这 `2k` 字符中的前 `k` 个字符。

- 如果剩余字符少于 `k` 个，则将剩余字符全部反转。
- 如果剩余字符小于 `2k` 但大于或等于 `k` 个，则反转前 `k` 个字符，其余字符保持原样。

 

**示例 1：**

```
输入：s = "abcdefg", k = 2
输出："bacdfeg"
```

**示例 2：**

```
输入：s = "abcd", k = 2
输出："bacd"
```

**My solution**

```cpp
class Solution {
public:
    string reverseStr(string s, int k) {
        int n = s.length();
        int i = 0;
        while (i < n) {
            int j = i + 2*k-1;
            if (j <= n) {
                reverse(s, i, i + k - 1);
                i = j + 1;
            } else if (n-1-i >= k) {
                reverse(s, i, i + k - 1);
                return s;
            } else if (n-1-i < k) {
                reverse(s, i, n-1);
                return s;
            }
        }
        return s;

    }

    void reverse(string& s, int i, int j) {
        while(i < j) {
            char tem = s[i];
            s[i] = s[j];
            s[j] = tem;
            ++i, --j;
        }
    }
};
```

### 142.[环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (58.61%) | 2518  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/linked-list" title="https://leetcode.com/tag/linked-list" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/two-pointers" title="https://leetcode.com/tag/two-pointers" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个链表的头节点  `head` ，返回链表开始入环的第一个节点。 *如果链表无环，则返回 `null`。*

如果链表中有某个节点，可以通过连续跟踪 `next` 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 `pos` 来表示链表尾连接到链表中的位置（**索引从 0 开始**）。如果 `pos` 是 `-1`，则在该链表中没有环。**注意：`pos` 不作为参数进行传递**，仅仅是为了标识链表的实际情况。

**不允许修改** 链表。



 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2018/12/07/circularlinkedlist.png)

```
输入：head = [3,2,0,-4], pos = 1
输出：返回索引为 1 的链表节点
解释：链表中有一个环，其尾部连接到第二个节点。
```

**示例 2：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test2.png)

```
输入：head = [1,2], pos = 0
输出：返回索引为 0 的链表节点
解释：链表中有一个环，其尾部连接到第一个节点。
```

**示例 3：**

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2018/12/07/circularlinkedlist_test3.png)

```
输入：head = [1], pos = -1
输出：返回 null
解释：链表中没有环。
```

**My solution**

本题使用*快慢指针*

>  首先判断链表中是否有环

让*快指针*每次移动两个单位，*慢指针*每次移动一个单位。如果两指针相遇则说明这个链表一定有环。

> 判断“环”的入口节点

用两个指针分别从*头节点*和*相遇节点*出发，每次移动一个单位，两者相遇节点就是**环的入口节点**（有[数学证明](https://programmercarl.com/0142.%E7%8E%AF%E5%BD%A2%E9%93%BE%E8%A1%A8II.html#%E6%80%9D%E8%B7%AF)）

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while (slow != nullptr && fast != nullptr) {
            slow = slow->next;
            if (fast->next == nullptr) {
                return nullptr;
            }
            fast = fast->next->next;
            if (slow == fast) {
                ListNode* index1 = head;
                ListNode* index2 = slow;
                while (index1 != index2) {
                    index1 = index1->next;
                    index2 = index2->next;
                }
                return index1;
            }
        }
        return nullptr;
        
    }
};
```



```
[-1,0,1,2,-1,-4]
[-4, -1, -1, 0, 1, 2]
[-1, -1, 0, 1]
```





























## 二分法

### 35.[搜索插入位置](https://leetcode.cn/problems/search-insert-position/description/)

|  Category  |  Difficulty   | Likes | Dislikes |
| :--------: | :-----------: | :---: | :------: |
| algorithms | Easy (45.72%) | 2264  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/binary-search" title="https://leetcode.com/tag/binary-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 `O(log n)` 的算法。

 

**示例 1:**

```
输入: nums = [1,3,5,6], target = 5
输出: 2
```

**示例 2:**

```
输入: nums = [1,3,5,6], target = 2
输出: 1
```

**示例 3:**

```
输入: nums = [1,3,5,6], target = 7
输出: 4
```

 

**提示:**

- `1 <= nums.length <= 104`
- `-104 <= nums[i] <= 104`
- `nums` 为 **无重复元素** 的 **升序** 排列数组
- `-104 <= target <= 104`

#### My solution

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size();
        int middle;
        if (nums[left] >= target) {
            return left;
        }
        while (left < right) {
            middle = left + ((right - left) >> 1);
            if (nums[middle] >= target && nums[middle - 1] < target) {
                return middle;
            } else if (nums[middle] > target) {
                right = middle;
            } else if (nums[middle] < target) {
                left = middle + 1;
            }
        }
        if (nums[right - 1] <= target) {
            return right;
        }
        return -1;

    }
};
```

写的有点过于复杂了，原因在于没有理解**二分法**的内核。

**二分法**终归是搜索算法，最终目的是找到数组中与*目标值*相同的值，固有以下几种情况（搜索范围**左闭右开**）：

* 数组中所有元素均**小于**目标值，则`right`会减小到与`left`相同，此时返回`left`或`right`均可
* 数组中存在元素**等于**目标值，则返回`middle`
* 数组中所有元素均**大于**目标值，则`left`会增大到`right - 1`（搜索范围左闭右开），此时返回`right`

综上所述，如果数组中**不存在**元素和目标值相等，则返回`right`。

修改后的代码：

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size();
        int middle;
        while (left < right) {
            middle = left + ((right - left) >> 1);
            if (nums[middle] > target) {
                right = middle;
            } else if (nums[middle] < target) {
                left = middle + 1;
            } else {
                return middle;
            }
        }
        return right;

    }
};
```

### 34.[在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/description/)

|  Category  |   Difficulty    | Likes | Dislikes |
| :--------: | :-------------: | :---: | :------: |
| algorithms | Medium (43.06%) | 2627  |    -     |

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Tags</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><a href="https://leetcode.com/tag/array" title="https://leetcode.com/tag/array" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a><a href="https://leetcode.com/tag/binary-search" title="https://leetcode.com/tag/binary-search" style="color: var(--vscode-textLink-foreground); text-decoration: none;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textLink-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></a></p></details>

<details style="color: rgb(36, 41, 47); font-family: -apple-system, &quot;system-ui&quot;, &quot;Segoe WPC&quot;, &quot;Segoe UI&quot;, system-ui, Ubuntu, &quot;Droid Sans&quot;, sans-serif; font-size: 14px; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; white-space: normal; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary><strong>Companies</strong></summary><p style="margin-top: 0px; margin-bottom: 16px;"><code style="font-family: var(--vscode-editor-font-family, &quot;SF Mono&quot;, Monaco, Menlo, Consolas, &quot;Ubuntu Mono&quot;, &quot;Liberation Mono&quot;, &quot;DejaVu Sans Mono&quot;, &quot;Courier New&quot;, monospace); color: var(--vscode-textPreformat-foreground); background-color: var(--vscode-textPreformat-background); padding: 1px 3px; border-radius: 4px; font-size: 1em; line-height: 1.357em; white-space: pre;"></code></p></details>

------

给你一个按照非递减顺序排列的整数数组 `nums`，和一个目标值 `target`。请你找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 `target`，返回 `[-1, -1]`。

你必须设计并实现时间复杂度为 `O(log n)` 的算法解决此问题。

 

**示例 1：**

```
输入：nums = [5,7,7,8,8,10], target = 8
输出：[3,4]
```

**示例 2：**

```
输入：nums = [5,7,7,8,8,10], target = 6
输出：[-1,-1]
```

**示例 3：**

```
输入：nums = [], target = 0
输出：[-1,-1]
```

## 其他题目

### LCR143.子结构判断

给定两棵二叉树 `tree1` 和 `tree2`，判断 `tree2` 是否以 `tree1` 的某个节点为根的子树具有 **相同的结构和节点值** 。
注意，**空树** 不会是以 `tree1` 的某个节点为根的子树具有 **相同的结构和节点值** 。

 

**示例 1：**

 

![img](https://pic.leetcode.cn/1694684670-vwyIgY-two_tree.png)

 

```
输入：tree1 = [1,7,5], tree2 = [6,1]
输出：false
解释：tree2 与 tree1 的一个子树没有相同的结构和节点值。
```

**示例 2：**

![img](https://pic.leetcode.cn/1694685602-myWXCv-two_tree_2.png)

```
输入：tree1 = [3,6,7,1,8], tree2 = [6,1]
输出：true
解释：tree2 与 tree1 的一个子树拥有相同的结构和节点值。即 6 - > 1。
```

 

**提示：**

```
0 <= 节点个数 <= 10000
```

**My solution**

本来我想用先序遍历，把两个树都存储到一个数组中，接着判断。但有些用例无法通过。

> **official solution**

官解的做法是这样的：

先序遍历中对节点的处理逻辑：判断以该节点为根节点的树是否和B树相同。

```cpp
class Solution {
public:
    bool isSubStructure(TreeNode* A, TreeNode* B) {
        if (NULL == A || NULL == B) return false;
        if (isSubTree(A, B) || isSubStructure(A->left, B) || isSubStructure(A->right, B)) {
            return true;
        } else {
            return false;
        }

    }

    bool isSubTree(TreeNode* root, TreeNode* compare) {
        if (NULL == compare) return true;
        if (NULL == root) return false;
        if (root->val == compare->val) {
            return isSubTree(root->left, compare->left) && isSubTree(root->right, compare->right);
        } else {
            return false;
        }
    }
};
```

