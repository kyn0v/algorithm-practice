# LRUCache

```
struct CacheNode{
    int key;
    int value;
    CacheNode* prev;
    CacheNode* next;
    CacheNode(int k, int v):key(k), value(v), prev(NULL), next(NULL){}
};

class LRUCache {
private:
    int size, cnt;
    unordered_map<int, CacheNode*> m;
    CacheNode *head, *tail;

public:
    LRUCache(int capacity): size(capacity), cnt(0) {
        head = new CacheNode(-1, -1);
        tail = new CacheNode(-1, -1);
        head->next=tail;
        tail->prev=head;
    }
    
    void update(CacheNode *p){
        if(p->prev == head)
            return;
        
        p->prev->next = p->next;
		p->next->prev = p->prev;

        p->prev = head;
        p->next = head->next;
        head->next->prev = p;
        head->next = p;
    }

    void del(){
        CacheNode* p_del = tail->prev;
        tail->prev = p_del->prev;
        p_del->prev->next = tail;
        m.erase(m.find(p_del->key));
        delete p_del;
        cnt--;
    }

    int get(int key) {
        unordered_map<int, CacheNode*>::iterator it = m.find(key);
        if(it==m.end())
            return -1;
        CacheNode *p = it->second;
        update(p);
        return p->value;
    }
    
    void put(int key, int value) {
        unordered_map<int, CacheNode*>::iterator it = m.find(key);
        if(it==m.end()){
            CacheNode* p = new CacheNode(key, value);
            m[key] = p;
            p->next = head->next;
            p->prev = head;
            head->next->prev = p;
            head->next = p;
            cnt++;
            if(cnt>size){
                del();
            }
        } else{
            CacheNode *p = it->second;
            p->value = value;
            update(p);
            return;
        }
    }
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```