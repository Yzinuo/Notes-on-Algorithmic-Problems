
![](attachments/哈希链表%20LRF_image_0.png)

和前文中的LRU不同的是加了次数，考虑次数的话，我们要快速找到最小次数的最左边的节点，我们可以分几个维度的节点。由LRU的一维变成了二维。 也是哈希链表的运用。 运用两个哈希表，第一个用来查找节点，第二个用来记录不同次数的链表。



```
class Node
{
public:
    int key,value,freq = 1;
    Node *prev,*next;
    Node(int k = 0, int v = 0) :key(k),value(v){}
};    
class LFUCache {
private:
    int min;
    int capacity;
    unordered_map<int, Node*> key_to_node;
    unordered_map<int, Node*>  freq_to_dummy;
    Node* get_node(int key)
    {
        auto it = key_to_node.find(key);// 在哈希表中查找，返回一个迭代器it，it的second就是Node*
        if(it == key_to_node.end()) 没找到
            return nullptr;
        auto node = it->second;
        
        remove(node);
        auto dummy = freq_to_dummy[node->freq];
        if(dummy->prev == dummy)
        {
            freq_to_dummy.erase(node->freq);
            delete dummy;
            if(min == node->freq)
                min++;
        }
        push_front(++node->freq,node);
        return node;
    }
    Node* new_list()
    {
        auto dummy = new Node();
        dummy->next = dummy;
        dummy->prev = dummy;  单个哨兵，prev指向最底下的书
        return dummy;
    }
    void push_front(int freq, Node*x)
    {
        auto it = freq_to_dummy.find(freq);
        if(it == freq_to_dummy.end())
            it = freq_to_dummy.emplace(freq,new_list()).first; emplace会返回一个first迭代器和一个bool值，表示是否插入成功。
        
        auto dummy = it->second;
        x->prev = dummy;
        x->next = dummy->next;
        x->prev->next = x;
        x->next->prev = x;
    } 
    void remove(Node* x)
    {
        x->prev->next = x->next;
        x->next->prev = x->prev;
    }
public:
    LFUCache(int capacity)  : capacity(capacity){
    }
    
    int get(int key) {
        auto node = get_node(key);
        return node?node->value:-1;
    }
    
    void put(int key, int value) {
        auto node = get_node(key);
        if(node)
        {
            node->value = value;
            return;
        }
        if(key_to_node.size() == capacity)
        {
            auto dummy = freq_to_dummy[min];
            auto back_node = dummy->prev;
            key_to_node.erase(back_node->key);  从哈希表中去除
            remove(back_node);
            delete(back_node);
            if(dummy->prev == dummy)
            {
                freq_to_dummy.erase(min);
                delete(dummy);
            }
        }
            key_to_node[key] = node = new Node(key,value); 从哈希表中添加
            min = 1;
            push_front(1,node);
        
    }
};
/**
 * Your LFUCache object will be instantiated and called as such:
 * LFUCache* obj = new LFUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```
