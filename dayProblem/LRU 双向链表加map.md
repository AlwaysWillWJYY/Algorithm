```java

@SpringBootTest
public class AlgorithmTests {

    public static void main(String[] args) {
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
    }
}

/**
 * 运用所掌握的数据结构，设计和实现一个LRU (Least Recently Used，最近最少使用) 缓存机制 。
 *
 * 实现 LRUCache 类：
 *
 * LRUCache(int capacity) 以正整数作为容量capacity 初始化 LRU 缓存
 * int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
 * void put(int key, int value)如果关键字已经存在，则变更其数据值；如果关键字不存在，则插入该组「关键字-值」。当缓存容量达到上限时，它应该在写入新数据之前删除最久未使用的数据值，从而为新的数据值留出空间。
 *
 * 来源：力扣（LeetCode）
 * 链接：https://leetcode.cn/problems/OrIXps
 * 著作权归领扣网络所有。商业转载请联系官方授权，非商业转载请注明出处。
 */
class LRUCache {
    //双向链表加map
    class DlinkNode{
        int key;
        int value;
        DlinkNode prev;
        DlinkNode next;
        public DlinkNode(){};
        public DlinkNode(int key, int value){
            this.key = key;
            this.value = value;
        }
    }
    int size = 0;
    int cap;
    DlinkNode head;
    DlinkNode tail;
    Map<Integer, DlinkNode> map = new HashMap<>();
    public LRUCache(int capacity) {
        this.cap = capacity;
        head = new DlinkNode();
        tail = new DlinkNode();
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        DlinkNode t = map.get(key);
        if(t == null){
            return -1;
        }
        moveToHead(t);
        return t.value;
    }

    public void put(int key, int value) {
        DlinkNode node = map.get(key);
        if(node == null){
            //创建新节点，到头结点
            //创建map，移到头结点
            DlinkNode newNode = new DlinkNode(key, value);
            map.put(key, newNode);
            addToHead(newNode);
            ++size;
            if(size > this.cap){
                DlinkNode cu = removeTail();
                map.remove(cu.key);
                --size;
            }
        }else{
            node.value = value;
            moveToHead(node);
        }
    }
    public void moveToHead(DlinkNode node){
        removeNode(node);
        addToHead(node);
    }
    public void addToHead(DlinkNode node){
        node.prev = head;
        node.next = head.next;
        head.next.prev = node;
        head.next = node;
    }
    public void removeNode(DlinkNode node){
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }
    public DlinkNode removeTail(){
        DlinkNode cur = tail.prev;
        removeNode(cur);
        return cur;
    }
}
```

