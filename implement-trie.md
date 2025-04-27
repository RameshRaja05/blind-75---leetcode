## 🔡 Trie (Prefix Tree) Implementation in Java

A **Trie** is a tree-like data structure used for efficient retrieval of strings, especially for prefix-based queries.

---

### ✅ Java Code

```java
class TrieNode{
    TrieNode[] children;
    boolean isLeaf;

    public TrieNode(){
        // every has 26 children that can capable of storing english 26 alphabets
        children = new TrieNode[26];
        // whether current children is a leaf node or not
        isLeaf = false;
    }
}
class Trie {
    public TrieNode root;

    public Trie() {
        root = new TrieNode();
    }
    
    public void insert(String word) {
        TrieNode current = root;
        for(char ch: word.toCharArray()){
            if(current.children[ch - 'a'] == null){
                current.children[ch - 'a'] = new TrieNode();
            }
            current = current.children[ch - 'a'];
        }
        current.isLeaf = true;
    }
    
    public boolean search(String word) {
        TrieNode current = root;

        for(char ch: word.toCharArray()){
            if(current.children[ch - 'a'] == null){
                return false;
            }

            current = current.children[ch - 'a'];
        }

        return current.isLeaf;
    }
    
    public boolean startsWith(String prefix) {
        TrieNode current = root;

        for(char ch: prefix.toCharArray()){
            if(current.children[ch - 'a'] == null){
                return false;
            }

            current = current.children[ch - 'a'];
        }

        return true;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```

---

### 📦 Time and Space Complexity

| Operation     | Time Complexity | Space Complexity |
|---------------|------------------|------------------|
| `insert()`     | O(m)             | O(m)             |
| `search()`     | O(m)             | O(1)             |
| `startsWith()` | O(m)             | O(1)             |

> *Where `m` is the length of the word or prefix.*

---

### 💡 When to Use Trie?

- Efficient prefix lookups
- Autocomplete systems
- Dictionary-like structures
