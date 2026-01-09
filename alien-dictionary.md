# 📋 LeetCode: Alien Dictionary

**Difficulty:** Hard
**Topic:** Topological Sort, Graph, BFS/DFS

---

## 🧠 Problem Summary

You're given a list of words sorted according to the rules of an unknown language. The goal is to determine the order of characters in that language.

---

## 📥 Input

A list of words sorted lexicographically in the alien language.

```java
["wrt", "wrf", "er", "ett", "rftt"]
```

---

## 📤 Output

A string representing the correct character order.

```java
"wertf"
```

---

## ❟Constraints

* Words are non-empty and consist of lowercase English letters.
* The input may contain invalid cases like prefix violations or cycles.

---

## ✅ Approach 1: Kahn’s Algorithm (BFS-Based Topological Sort)

### 🔗 Intuition

We must determine the **relative ordering of characters** — this is a classic use-case for **topological sorting**. Since we know the list of words is sorted in an alien language, we can infer order between adjacent words.

---

### 🔄 Algorithm

1. **Initialize:**

   * Build an adjacency list (`adj`) and an in-degree map (`inDegree`) for all characters.

2. **Build Graph:**

   * Compare adjacent words.
   * First differing characters `c1` and `c2` imply an edge `c1 → c2`.
   * Only add new edges to prevent over-counting in-degrees.

3. **Topological Sort via BFS:**

   * Add all characters with in-degree 0 to a queue.
   * Process each node, appending it to result and updating neighbors' in-degrees.

4. **Edge Case:**

   * If a longer word is a prefix of a shorter one (e.g., `"abc"` vs `"ab"`), return `""`.

---

### 🧪 Dry Run Example

Input:
`["wrt", "wrf", "er", "ett", "rftt"]`

Inferred Edges:

```
t → f  
w → e  
e → r  
r → t
```

Topological order: `wertf`

---

### 🗒️ Java Code: Kahn's Algorithm

```java
class Solution {
    public String alienOrder(String[] words) {
        Map<Character, Set<Character>> adj = new HashMap<>();
        Map<Character, Integer> inDegree = new HashMap<>();

        // Initialize
        for (String word : words) {
            for (char c : word.toCharArray()) {
                adj.putIfAbsent(c, new HashSet<>());
                inDegree.putIfAbsent(c, 0);
            }
        }

        // Build edges
        for (int i = 0; i < words.length - 1; i++) {
            String w1 = words[i], w2 = words[i + 1];
            int len = Math.min(w1.length(), w2.length());

            if (w1.length() > w2.length() && w1.substring(0, len).equals(w2)) {
                return "";
            }

            for (int j = 0; j < len; j++) {
                char c1 = w1.charAt(j), c2 = w2.charAt(j);
                if (c1 != c2) {
                    if (adj.get(c1).add(c2)) {
                        inDegree.put(c2, inDegree.get(c2) + 1);
                    }
                    break;
                }
            }
        }

        // Topological Sort (BFS)
        Queue<Character> q = new LinkedList<>();
        for (char c : inDegree.keySet()) {
            if (inDegree.get(c) == 0) q.offer(c);
        }

        StringBuilder sb = new StringBuilder();
        while (!q.isEmpty()) {
            char cur = q.poll();
            sb.append(cur);
            for (char nei : adj.get(cur)) {
                inDegree.put(nei, inDegree.get(nei) - 1);
                if (inDegree.get(nei) == 0) {
                    q.offer(nei);
                }
            }
        }

        return sb.length() == inDegree.size() ? sb.toString() : "";
    }
}
```

---

## ✅ Approach 2: DFS-Based Topological Sort

### 🔗 Intuition

Instead of using in-degree and a queue, we use a DFS approach:

* Visit all nodes recursively.
* Track a visited map and a recursion path to detect cycles.
* Append characters to a result list in **post-order** (after all dependencies are visited).

---

### 🗒️ Java Code: DFS Implementation

```java
class Solution {

    private boolean dfs(char ch, Map<Character, Boolean> visit, List<Character> res, Map<Character, Set<Character>> adj) {
        if (visit.containsKey(ch)) {
            return visit.get(ch); // return true if cycle is detected
        }

        visit.put(ch, true); // mark in current path

        for (char nei : adj.get(ch)) {
            if (dfs(nei, visit, res, adj)) {
                return true; // cycle detected
            }
        }

        visit.put(ch, false); // backtrack
        res.add(ch); // post-order

        return false;
    }

    public String foreignDictionary(String[] words) {
        Map<Character, Set<Character>> adj = new HashMap<>();

        // Initialize
        for (String word : words) {
            for (char ch : word.toCharArray()) {
                adj.put(ch, new HashSet<>());
            }
        }

        // Build graph
        for (int i = 0; i < words.length - 1; i++) {
            String w1 = words[i], w2 = words[i + 1];
            int minLength = Math.min(w1.length(), w2.length());

            if (w1.length() > w2.length() && w1.substring(0, minLength).equals(w2)) {
                return "";
            }

            for (int j = 0; j < minLength; j++) {
                if (w1.charAt(j) != w2.charAt(j)) {
                    adj.get(w1.charAt(j)).add(w2.charAt(j));
                    break;
                }
            }
        }

        Map<Character, Boolean> visit = new HashMap<>();
        List<Character> res = new ArrayList<>();

        for (char c : adj.keySet()) {
            if (dfs(c, visit, res, adj)) {
                return ""; // cycle detected
            }
        }

        Collections.reverse(res);
        StringBuilder sb = new StringBuilder();

        for (char c : res) {
            sb.append(c);
        }

        return sb.toString();
    }
}
```

---

## 🧠 Key Takeaways

| Aspect          | BFS (Kahn's)            | DFS                         |
| --------------- | ----------------------- | --------------------------- |
| Uses            | In-degree map + queue   | Recursion + visited map     |
| Detect cycles   | Implicit (length check) | Explicit (path tracking)    |
| Order of output | Pre-order (queue-based) | Post-order (reverse result) |
| Performance     | O(C + E)                | O(C + E)                    |

---

## 📊 Complexity

* **Time:** O(C + E), where C is number of characters and E is number of edges
* **Space:** O(C + E)
