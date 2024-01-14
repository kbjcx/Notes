---
aliases: []
date: 2024-01-09 22:53
---
KMP 利用已匹配部分中相同的「前缀」和「后缀」来加速下一次的匹配
```cpp
class KMP {
public:
    int kmp(std::string s, std::string pattern) {
        std::vector<int> next = getNext(pattern);
        for (int i = 0, j = 0; i < s.size(); ++i) {
            while (j > 0 && pattern[j] != s[i]) {
                j = next[j - 1];
            }
            if (s[i] == pattern[j]) {
                ++j;
            }

            if (j == pattern.size()) {
                return i - j + 1;
            }
        }

        return -1;
    }

    std::vector<int> getNext(std::string& pattern) {
        std::vector<int> next(pattern.size(), 0);
        int j = 0;
        for (int i = 1; i < pattern.size(); ++i) {
            while (j > 0 && pattern[i] != pattern[j]) {
                j = next[j - 1];
            }
            if (pattern[i] == pattern[j]) {
                next[i] = j + 1;
                ++j;
            }
        }

        return next;
    }
};
```