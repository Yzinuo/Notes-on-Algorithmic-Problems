
![](attachments/哈希表加有序集合%20multiset_image_0.png)
 题目中明确要使用哈希表，但是对于最大价格和最小价格，哈希表是无能为力的，因为哈希表是无序的。
 所以我们要搭配一个有序表。 有序表是
```
multiset<int> prices;
```

```
multiset<int> 是 C++ 标准模板库 (STL) 中的一个容器，它可以存储多个 int 类型的元素。multiset 是一种关联容器，它允许存储重复的元素，并且元素是自动排序的。
在你的代码中，multiset<int> prices; 创建了一个名为 prices 的 multiset 容器，用于存储 int 类型的元素。
以下是 multiset 的一些主要特性和操作：
自动排序
：multiset 中的元素在插入时会自动进行排序。默认情况下，排序规则是 <，因此元素会按照升序排列(从小到大)。你也可以在创建 multiset 时提供自定义的比较函数。
允许重复
：与 set 不同，multiset 允许插入重复的元素。
插入元素
：你可以使用 insert() 函数来向 multiset 中插入元素。例如：prices.insert(5);
删除元素
：你可以使用 erase() 函数来删除 multiset 中的元素。例如：prices.erase(5);
查找元素
：你可以使用 find() 函数来查找 multiset 中是否存在某个元素。例如：如果 prices.find(5) != prices.end()，那么说明 prices 中存在元素 5。
遍历元素
：你可以使用迭代器（例如：multiset<int>::iterator it = prices.begin(); it != prices.end(); ++it）来遍历 multiset 中的所有元素。
在 C++ 中，multiset::find() 函数会返回一个指向找到的元素的迭代器。如果 multiset 中不存在这个元素，那么 find() 函数会返回 multiset::end()。
因此，if (it != prices.end()) 这个条件判断就是在检查我们是否真的找到了要删除的元素。如果 it 等于 prices.end()，那就意味着我们没有找到要删除的元素，所以就不应该尝试去删除它。
如果我们不做这个检查，而直接调用 prices.erase(it);，那么当 it 等于 prices.end() 时，我们就会尝试删除一个并不存在的元素，这将导致未定义的行为。所以，为了保证代码的正确性和稳定性，我们需要进行这样的检查。
在 C++ 中，auto 关键字用于自动类型推断。当你不确定变量的确切类型，或者类型名称过长难以书写时，可以使用 auto 让编译器自动为你推断类型。
在你的代码 auto it = prices.find(prevPrice); 中，prices.find(prevPrice) 返回的是一个迭代器（iterator）。这个迭代器指向 multiset 中等于 prevPrice 的元素。如果 multiset 中没有找到等于 prevPrice 的元素，那么 find() 函数会返回 end()，表示范围的末尾。
因此，auto it = prices.find(prevPrice); 这行代码中的 it 是一个迭代器，具体来说，是 multiset<int>::iterator 类型。使用 auto 可以避免手动写出这个长类型名称。
```
  *prices.rbegin(); 和 *prices.begin(); 分别返回 multiset 中的最大值和最小值。
- prices.rbegin()
 返回一个反向迭代器，它指向 multiset 中的最后一个元素，也就是这个集合中的最大值。然后，使用 * 运算符可以获取这个迭代器所指向的元素的值。
- 同样，
prices.begin() 返回一个正向迭代器，它指向 multiset 中的第一个元素，也就是这个集合中的最小值。使用 * 运算符可以获取这个迭代器所指向的元素的值。
因此，如果你想在函数中返回 multiset 中的最大值和最小值，你可以这样写：
```cpp
int max_value = *prices.rbegin();
int min_value = *prices.begin();
```

这一题的代码：
```
class StockPrice {
public:
    unordered_map<int,int> hash;
    int time;
    multiset<int> prices;
    StockPrice() {
        time = 0;
    }
    
    void update(int timestamp, int price) {
        time = max(time,timestamp);
        int prevprice = !hash.count(timestamp)?0:hash[timestamp];
        hash[timestamp] = price;
        if(prevprice > 0)
        {
            auto it = prices.find(prevprice);
            if(it != prices.end())
            {
                prices.erase(it);
            }
        }
        prices.emplace(price);
    }
    
    int current() {
        return hash[time];
    }
    
    int maximum() {
         return *prices.rbegin();
    }
    
    int minimum() {
        return *prices.begin();
    }
};
/**
 * Your StockPrice object will be instantiated and called as such:
 * StockPrice* obj = new StockPrice();
 * obj->update(timestamp,price);
 * int param_2 = obj->current();
 * int param_3 = obj->maximum();
 * int param_4 = obj->minimum();
 */
```
