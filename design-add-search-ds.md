
---

## 📚 Word Dictionary with Wildcard Support (LeetCode)

Design a data structure that supports adding new words and searching strings that can contain a wildcard `.` to match any letter.

---

### 🧠 Problem Description

Implement the `WordDictionary` class:

- `WordDictionary()`: Initializes the object.
- `void addWord(word)`: Adds word to the data structure.
- `bool search(word)`: Returns true if any previously added word matches `word`. Dots `.` can be matched with any character.

---

### ✅ Java Code (Recursive DFS for . Character)

```java
class WordDictionary {

    private static class TrieNode {
        TrieNode[] children = new TrieNode[26];
        boolean isEndOfWord = false;
    }

    private TrieNode root;

    public WordDictionary() {
        root = new TrieNode();
    }

    public void addWord(String word) {
        TrieNode node = root;
        for (char c : word.toCharArray()) {
            int i = c - 'a';
            if (node.children[i] == null) {
                node.children[i] = new TrieNode();
            }
            node = node.children[i];
        }
        node.isEndOfWord = true;
    }

    public boolean search(String word) {
        return dfs(word, 0, root);
    }

    private boolean dfs(String word, int index, TrieNode node) {
        if (node == null) return false;
        if (index == word.length()) return node.isEndOfWord;

        char c = word.charAt(index);
        if (c == '.') {
            for (TrieNode child : node.children) {
                if (child != null && dfs(word, index + 1, child)) {
                    return true;
                }
            }
            return false;
        } else {
            return dfs(word, index + 1, node.children[c - 'a']);
        }
    }
}
```
### ✅ Java Code (Recursive DFS + Iterative)

```java
class TrieNode{
    TrieNode[] children;
    boolean isEndOftheWord;

    public TrieNode(){
        children = new TrieNode[26];
        isEndOftheWord = false;
    }
}
class WordDictionary {
    public TrieNode root;

    public WordDictionary() {
        root = new TrieNode();
    }
    
    public void addWord(String word) {
        TrieNode current = root;
        for(char ch:word.toCharArray()){
            if(current.children[ch - 'a'] == null){
                current.children[ch - 'a'] = new TrieNode();
            }
            current = current.children[ch - 'a'];
        }
        current.isEndOftheWord = true;
    }

    public boolean dfs(TrieNode root, int j, String word){
         TrieNode cur = root;

         for(int i = j;i < word.length();i++){
            char ch = word.charAt(i);
            if(ch == '.'){
                for(TrieNode child:cur.children){
                    if(child != null && dfs(child, i + 1, word)) return true;
                }
                return false;
            }else{
                if(cur.children[ch - 'a'] == null) return false;
                cur = cur.children[ch - 'a'];
            }
         }
         return cur.isEndOftheWord;
    }
    
    public boolean search(String word) {
        return dfs(root, 0, word);
    }
}

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * boolean param_2 = obj.search(word);
 */
```
---

### 🧪 Example

```java
WordDictionary dict = new WordDictionary();
dict.addWord("bad");
dict.addWord("dad");
dict.addWord("mad");

dict.search("pad"); // false
dict.search("bad"); // true
dict.search(".ad"); // true
dict.search("b.."); // true
```

---

### 📈 Time Complexity

- `addWord(word)` → O(L), L = length of word
- `search(word)`:
  - Best: O(L)
  - Worst with wildcards: O(26^k × L) where k = number of dots (max 2)

---

### 📌 Constraints

- `1 <= word.length <= 25`
- Words contain lowercase English letters and `.` (search only)
- Up to `10^4` calls to `addWord` and `search`

---
### ✅ Solution Overview

We can implement this data structure using a **Trie** (also known as a Prefix Tree). This is an extension of the standard Trie implementation with a modified search to handle the wildcard `.`.

---

### 🔍 Key Observations

- This is based on the classic **"Implement Trie"** problem.
- The major difference is that the `search()` method must handle the wildcard `.`:
  - `.` → can match **any lowercase English letter**.

---

### 🔄 Approach

To handle the wildcard `.`, we use **Depth-First Search (DFS)** over the Trie.

#### Case 1: Regular Character (`a` to `z`)
- Traverse down the tree normally by following the matching child node.

#### Case 2: Wildcard Character (`.`)
- Iterate through **all children** of the current node.
- Recursively call DFS for each non-null child to continue matching the rest of the word.

---

### 🌳 Visual Walkthrough

Suppose we added these words: `ate`, `but`, `cad`

```
                     root
                    / |  \
                  a  b   c
                  |  |    |
                  t  u    a
                  |  |    |
                  e  t    d
```

---

### 🧪 Walkthrough Examples

#### 🔸 Example 1: `search("ate")`

- Start from root → 'a' exists → move to 't' → then 'e'.
- Reached end of word and `isEndOfWord = true` → ✅ **Return `true`**

#### 🔹 Example 2: `search(".ad")`

- First character is `.` → try all children of root: `a`, `b`, `c`
  - `a` → no match for `"ad"` (no `d` after `t`)
  - `b` → no match for `"ad"` (no `a` path after `u`)
  - `c` → `a` exists → then `d` exists → ✅ **Return `true`**