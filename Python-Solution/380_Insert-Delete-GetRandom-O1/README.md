# 380 - 常数时间插入、删除和获取随机元素

## 题目描述
![problem](images/380.png)

>关联题目： [381. O(1) 时间插入、删除和获取随机元素 - 允许重复](https://github.com/Rosevil1874/LeetCode/tree/master/Python-Solution/381_Insert-Delete-GetRandom-O(1)-Duplicates-allowed)  

## 题解
看到class就束手无策的蠢蛋本人，参考agave同学的思路：  
>We just keep track of the index of the added elements, so when we remove them, we copy the last one into it.  
1. list.append() takes O(1), both average and amortized.   
2. Dictionary get and set functions take O(1) average

list操作的时间复杂度如下：  

![list](images/list.png)

1. 插入：  
append时间复杂度为O(1)，只需在插入元素的同时使用字典pos记录其位置即可。

2. 删除：  
由于del的时间复杂度为O(N)，因此需要将末尾元素交换到待删除元素位置并更新其pos，再使用时间复杂度为O(1)的pop操作将其删除。

3. 随机元素：  
random模块：
```python
random.randint(1,10)         # 产生 1 到 10 的一个整数型随机数  
random.random()              # 产生 0 到 1 之间的随机浮点数
random.uniform(1.1,5.4)      # 产生  1.1 到 5.4 之间的随机浮点数，区间可以不是整数
random.choice('tomorrow')    # 从序列中随机选取一个元素
random.randrange(1,100,2)    # 生成从1到100的间隔为2的随机整数

a=[1,3,5,6,7]                # 将序列a中的元素顺序打乱
random.shuffle(a)
```

好了，AC代码如下：
```python
class RandomizedSet:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.cache = []     # 存储数据(列表的append和pop是O(1)复杂度，remove是O(N)复杂度)
        self.pos = {}       # 存储每个数据的位置(字典get和set都是O(1)复杂度)

        
    def insert(self, val: int) -> bool:
        """
        Inserts a value to the set. Returns true if the set did not already contain the specified element.
        """
        if val not in self.pos:
            self.cache.append(val)
            self.pos[val] = len(self.cache) - 1
            return True
        return False

    
    def remove(self, val: int) -> bool:
        """
        Removes a value from the set. Returns true if the set contained the specified element.
        """
        if val in self.pos:
            p = self.pos[val]
            tail = self.cache[-1]
            self.cache[p] = tail
            self.pos[tail] = p
            self.cache.pop()
            self.pos.pop(val)
            return True
        return False
        

    def getRandom(self) -> int:
        """
        Get a random element from the set.
        """
        return self.cache[random.randint(0, len(self.cache) - 1)]


# Your RandomizedSet object will be instantiated and called as such:
# obj = RandomizedSet()
# param_1 = obj.insert(val)
# param_2 = obj.remove(val)
# param_3 = obj.getRandom()
```