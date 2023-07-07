```java
/**设计一个使用单词列表进行初始化的数据结构，单词列表中的单词 互不相同 。 如果给出一个单词，请判定能否只将这个单词中一个字母换成另一个字母，使得所形成的新单词存在于已构建的神奇字典中。

实现 MagicDictionary 类：

MagicDictionary() 初始化对象
void buildDict(String[] dictionary) 使用字符串数组 dictionary 设定该数据结构，dictionary 中的字符串互不相同
bool search(String searchWord) 给定一个字符串 searchWord ，判定能否只将字符串中 一个 字母换成另一个字母，使得所形成的新字符串能够与字典中的任一字符串匹配。如果可以，返回 true ；否则，返回 false 。

样例：

输入
inputs = ["MagicDictionary", "buildDict", "search", "search", "search", "search"]
inputs = [[], [["hello", "leetcode"]], ["hello"], ["hhllo"], ["hell"], ["leetcoded"]]
输出
[null, null, false, true, false, false]

解释
MagicDictionary magicDictionary = new MagicDictionary();
magicDictionary.buildDict(["hello", "leetcode"]);
magicDictionary.search("hello"); // 返回 False
magicDictionary.search("hhllo"); // 将第二个 'h' 替换为 'e' 可以匹配 "hello" ，所以返回 True
magicDictionary.search("hell"); // 返回 False
magicDictionary.search("leetcoded"); // 返回 False
**
*/
class MagicDictionary {
    Tire root = new Tire();
    /** Initialize your data structure here. */
    public MagicDictionary() {

    }
    
    public void buildDict(String[] dictionary) {
        int n = dictionary.length;
        for(int i = 0; i < n; i++){
            insert(dictionary[i]);
        }
    }   
    public void insert(String dictionary){
        Tire node = root;
        char[] cs = dictionary.toCharArray();
        for(int i = 0; i < cs.length; i++){
            int pos = cs[i] - 'a';
            if(node.children[pos] == null) node.children[pos] = new Tire();
            node = node.children[pos];
        }
        node.isEnd = true;
    }
    
    public boolean search(String searchWord) {
        Tire node = root;
        return dfs(node, searchWord, 0, 0);
    }
    public boolean dfs(Tire node, String word, int index, int cnt){
        if(index == word.length()){
            if(cnt == 1 && node.isEnd) return true;
            else return false;
        }
        for(int x = 0; x < 26; x++){
            if(node.children[x] == null) continue;
            if('a' + x == word.charAt(index)){
                if(dfs(node.children[x], word, index + 1, cnt)) return true;
            }else{
                if(dfs(node.children[x], word, index + 1, cnt + 1)) return true;
            }
        }
        return false;
    }
    class Tire{
        public Tire[] children;
        boolean isEnd;
        public Tire(){
            children = new Tire[26];
            isEnd = false;
        }
    }
}

/**
 * Your MagicDictionary object will be instantiated and called as such:
 * MagicDictionary obj = new MagicDictionary();
 * obj.buildDict(dictionary);
 * boolean param_2 = obj.search(searchWord);
 */
```

