# LeetCode 271. Encode and Decode Strings

## 🧩 Problem Description
Design an algorithm to encode a list of strings to a single string and decode it back to the original list of strings.

You need to implement two methods:
- `encode(List<String> strs) -> str`
- `decode(String s) -> List<String>`

The encoded string is guaranteed to be decodable. That means no extra checks are needed to ensure a valid decoding.

---

## 🔐 Encode/Decode Strategy
We need a way to encode each string so that we know:
1. Where it starts
2. Where it ends

### 🔑 Common Approach:
Use a **delimiter** that doesn't appear in the input strings, or better:

✅ **Prefix each string with its length followed by a special character** (e.g. `#`).

---

## 🔍 Step-by-Step Walkthrough

### Input Example:
```text
Input: ["hello", "world"]
```

### Encode Process:
1. For each string, compute its length.
2. Concatenate: `<length>#<string>`

```text
"hello" -> "5#hello"
"world" -> "5#world"
Final Encoded: "5#hello5#world"
```

### Decode Process:
1. Read the length until you hit `#`.
2. Read the next `length` characters as the original string.
3. Repeat until the end of the encoded string.

Visual:
```text
5#hello5#world
^   ^    ^
|   |    |
|   |    Next length: 5, so read 5 chars => "world"
|   End of "hello"
Start at index 0
```

---

## 🧠 Algorithm Details

### Python Implementation
```python
class Codec:
    def encode(self, strs):
        """Encodes a list of strings to a single string."""
        return ''.join(f"{len(s)}#{s}" for s in strs)

    def decode(self, s):
        """Decodes a single string to a list of strings."""
        res = []
        i = 0
        while i < len(s):
            j = i
            while s[j] != '#':
                j += 1
            length = int(s[i:j])
            res.append(s[j+1:j+1+length])
            i = j + 1 + length
        return res
```

### Java Implementation
```java
public class Codec {
    // Encodes a list of strings to a single string.
    public String encode(List<String> strs) {
        StringBuilder sb = new StringBuilder();
        for (String str : strs) {
            sb.append(str.length()).append('#').append(str);
        }
        return sb.toString();
    }

    // Decodes a single string to a list of strings.
    public List<String> decode(String s) {
        List<String> result = new ArrayList<>();
        int i = 0;
        while (i < s.length()) {
            int j = i;
            while (s.charAt(j) != '#') {
                j++;
            }
            int len = Integer.parseInt(s.substring(i, j));
            result.add(s.substring(j + 1, j + 1 + len));
            i = j + 1 + len;
        }
        return result;
    }
}
```

---

## 🧪 Edge Cases & Testing
- Empty list → `[]`
- List with empty string → `["", "abc"]`
- List with special characters → `["#hello#", "world"]`
- List with mixed lengths → `["a", "bc", "def"]`

### Example:
```text
encode(["", "abc"]) → "0#3#abc"
decode("0#3#abc") → ["", "abc"]
```

---

## ⏱ Time and Space Complexity

### Encode:
- **Time:** O(N) — where N is the total length of all strings
- **Space:** O(N) — for building the encoded string

### Decode:
- **Time:** O(N)
- **Space:** O(N)

---

## ✅ Final Notes
- Using length-prefixing with `#` avoids delimiter collision issues.
- Works for any printable string — even those containing `#`.
- Deterministic and fast, ideal for system design and string serialization problems.

