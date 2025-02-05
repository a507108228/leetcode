# [676. 实现一个魔法字典](https://leetcode.cn/problems/implement-magic-dictionary)

[English Version](/solution/0600-0699/0676.Implement%20Magic%20Dictionary/README_EN.md)

## 题目描述

<!-- 这里写题目描述 -->

<p>设计一个使用单词列表进行初始化的数据结构，单词列表中的单词 <strong>互不相同</strong> 。 如果给出一个单词，请判定能否只将这个单词中<strong>一个</strong>字母换成另一个字母，使得所形成的新单词存在于你构建的字典中。</p>

<p>实现 <code>MagicDictionary</code> 类：</p>

<ul>
	<li><code>MagicDictionary()</code> 初始化对象</li>
	<li><code>void buildDict(String[] dictionary)</code> 使用字符串数组 <code>dictionary</code> 设定该数据结构，<code>dictionary</code> 中的字符串互不相同</li>
	<li><code>bool search(String searchWord)</code> 给定一个字符串 <code>searchWord</code> ，判定能否只将字符串中<strong> 一个 </strong>字母换成另一个字母，使得所形成的新字符串能够与字典中的任一字符串匹配。如果可以，返回 <code>true</code> ；否则，返回 <code>false</code> 。</li>
</ul>

<p> </p>

<div class="top-view__1vxA">
<div class="original__bRMd">
<div>
<p><strong>示例：</strong></p>

<pre>
<strong>输入</strong>
["MagicDictionary", "buildDict", "search", "search", "search", "search"]
[[], [["hello", "leetcode"]], ["hello"], ["hhllo"], ["hell"], ["leetcoded"]]
<strong>输出</strong>
[null, null, false, true, false, false]

<strong>解释</strong>
MagicDictionary magicDictionary = new MagicDictionary();
magicDictionary.buildDict(["hello", "leetcode"]);
magicDictionary.search("hello"); // 返回 False
magicDictionary.search("hhllo"); // 将第二个 'h' 替换为 'e' 可以匹配 "hello" ，所以返回 True
magicDictionary.search("hell"); // 返回 False
magicDictionary.search("leetcoded"); // 返回 False
</pre>

<p> </p>

<p><strong>提示：</strong></p>

<ul>
	<li><code>1 <= dictionary.length <= 100</code></li>
	<li><code>1 <= dictionary[i].length <= 100</code></li>
	<li><code>dictionary[i]</code> 仅由小写英文字母组成</li>
	<li><code>dictionary</code> 中的所有字符串 <strong>互不相同</strong></li>
	<li><code>1 <= searchWord.length <= 100</code></li>
	<li><code>searchWord</code> 仅由小写英文字母组成</li>
	<li><code>buildDict</code> 仅在 <code>search</code> 之前调用一次</li>
	<li>最多调用 <code>100</code> 次 <code>search</code></li>
</ul>
</div>
</div>
</div>

## 解法

<!-- 这里可写通用的实现逻辑 -->

哈希表实现。

<!-- tabs:start -->

### **Python3**

<!-- 这里可写当前语言的特殊实现逻辑 -->

```python
class MagicDictionary:

    def __init__(self):
        """
        Initialize your data structure here.
        """

    def _patterns(self, word):
        return [word[:i] + '*' + word[i + 1:] for i in range(len(word))]

    def buildDict(self, dictionary: List[str]) -> None:
        self.words = set(dictionary)
        self.counter = Counter(
            p for word in dictionary for p in self._patterns(word))

    def search(self, searchWord: str) -> bool:
        for p in self._patterns(searchWord):
            if self.counter[p] > 1 or (self.counter[p] == 1 and searchWord not in self.words):
                return True
        return False


# Your MagicDictionary object will be instantiated and called as such:
# obj = MagicDictionary()
# obj.buildDict(dictionary)
# param_2 = obj.search(searchWord)
```

### **Java**

<!-- 这里可写当前语言的特殊实现逻辑 -->

暴力法。直接遍历字典，判断是否存在一个单词与所要查找的单词之间只有一个字符不同，若是返回 true，否则返回 false。

```java
class MagicDictionary {
    List<char[]> dict;
    /** Initialize your data structure here. */
    public MagicDictionary() {
        dict = new ArrayList<>();
    }

    public void buildDict(String[] dictionary) {
        for (String item : dictionary) {
            dict.add(item.toCharArray());
        }
    }

    public boolean search(String searchWord) {
        char[] target = searchWord.toCharArray();
        for (char[] item : dict) {
            if (item.length != target.length) {
                continue;
            }
            int diff = 0;
            for (int i = 0; i < target.length; i++) {
                if (target[i] != item[i]) {
                    diff += 1;
                }
            }
            if (diff == 1) {
                return true;
            }
        }
        return false;
    }
}
```

哈希表。

```java
class MagicDictionary {
    private Set<String> words;
    private Map<String, Integer> counter;

    /** Initialize your data structure here. */
    public MagicDictionary() {
        words = new HashSet<>();
        counter = new HashMap<>();
    }

    public void buildDict(String[] dictionary) {
        for (String word : dictionary) {
            words.add(word);
            for (String p : patterns(word)) {
                counter.put(p, counter.getOrDefault(p, 0) + 1);
            }
        }
    }

    public boolean search(String searchWord) {
        for (String p : patterns(searchWord)) {
            int cnt = counter.getOrDefault(p, 0);
            if (cnt > 1 || (cnt == 1 && !words.contains(searchWord))) {
                return true;
            }
        }
        return false;
    }

    private List<String> patterns(String word) {
        List<String> res = new ArrayList<>();
        char[] chars = word.toCharArray();
        for (int i = 0; i < chars.length; ++i) {
            char c = chars[i];
            chars[i] = '*';
            res.add(new String(chars));
            chars[i] = c;
        }
        return res;
    }
}

/**
 * Your MagicDictionary object will be instantiated and called as such:
 * MagicDictionary obj = new MagicDictionary();
 * obj.buildDict(dictionary);
 * boolean param_2 = obj.search(searchWord);
 */
```

### **C++**

```cpp
class MagicDictionary {
public:
    /** Initialize your data structure here. */
    MagicDictionary() {

    }

    void buildDict(vector<string> dictionary) {
        for (string word : dictionary)
        {
            words.insert(word);
            for (string p : patterns(word)) ++counter[p];
        }
    }

    bool search(string searchWord) {
        for (string p : patterns(searchWord))
        {
            if (counter[p] > 1 || (counter[p] == 1 && !words.count(searchWord))) return true;
        }
        return false;
    }

private:
    unordered_set<string> words;
    unordered_map<string, int> counter;

    vector<string> patterns(string word) {
        vector<string> res;
        for (int i = 0; i < word.size(); ++i)
        {
            char c = word[i];
            word[i] = '*';
            res.push_back(word);
            word[i] = c;
        }
        return res;
    }
};

/**
 * Your MagicDictionary object will be instantiated and called as such:
 * MagicDictionary* obj = new MagicDictionary();
 * obj->buildDict(dictionary);
 * bool param_2 = obj->search(searchWord);
 */
```

### **Go**

```go
type MagicDictionary struct {
	words   map[string]bool
	counter map[string]int
}

/** Initialize your data structure here. */
func Constructor() MagicDictionary {
	return MagicDictionary{
		words:   make(map[string]bool),
		counter: make(map[string]int),
	}
}

func (this *MagicDictionary) BuildDict(dictionary []string) {
	for _, word := range dictionary {
		this.words[word] = true
		for _, p := range patterns(word) {
			this.counter[p]++
		}
	}
}

func (this *MagicDictionary) Search(searchWord string) bool {
	for _, p := range patterns(searchWord) {
		if this.counter[p] > 1 || (this.counter[p] == 1 && !this.words[searchWord]) {
			return true
		}
	}
	return false
}

func patterns(word string) []string {
	var res []string
	for i := 0; i < len(word); i++ {
		res = append(res, word[:i]+"."+word[i+1:])
	}
	return res
}

/**
 * Your MagicDictionary object will be instantiated and called as such:
 * obj := Constructor();
 * obj.BuildDict(dictionary);
 * param_2 := obj.Search(searchWord);
 */
```

### **...**

```

```

<!-- tabs:end -->
