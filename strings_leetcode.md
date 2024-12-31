# Company-Specific LeetCode String Problems

## Table of Contents
1. [Easy Problems](#easy-problems)
2. [Medium Problems](#medium-problems)
3. [Hard Problems](#hard-problems)

## Easy Problems

### 1. Valid Anagram (Netflix)
```cpp
/*
Time Complexity: O(n)
Company: Netflix
Difficulty: Easy

Problem: Given two strings s and t, return true if t is an anagram of s, and false otherwise.

Example:
Input: s = "anagram", t = "nagaram"
Output: true
*/

class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.length() != t.length()) return false;
        
        vector<int> freq(26, 0);
        for (int i = 0; i < s.length(); i++) {
            freq[s[i] - 'a']++;
            freq[t[i] - 'a']--;
        }
        
        return all_of(freq.begin(), freq.end(), [](int x) { return x == 0; });
    }
};
```

**Key Points:**
- Use frequency array for O(n) solution
- Could also use sorting approach (O(nlogn))
- Handle unequal length case first

### 2. Valid Parentheses (Amazon)
```cpp
/*
Time Complexity: O(n)
Company: Amazon
Difficulty: Easy

Problem: Given a string s containing just the characters '(', ')', '{', '}', '[' and ']',
determine if the input string is valid.

Example:
Input: s = "()[]{}"
Output: true
*/

class Solution {
public:
    bool isValid(string s) {
        stack<char> st;
        unordered_map<char, char> pairs = {
            {')', '('},
            {']', '['},
            {'}', '{'}
        };
        
        for (char c : s) {
            if (c == '(' || c == '[' || c == '{') {
                st.push(c);
            } else {
                if (st.empty() || st.top() != pairs[c]) return false;
                st.pop();
            }
        }
        
        return st.empty();
    }
};
```

**Key Points:**
- Use stack to track opening brackets
- Map closing brackets to corresponding opening brackets
- Check for empty stack at end

### 3. First Unique Character in a String (Apple)
```cpp
/*
Time Complexity: O(n)
Company: Apple
Difficulty: Medium

Problem: Given a string s, find the first non-repeating character and return its index.
If it does not exist, return -1.

Example:
Input: s = "leetcode"
Output: 0
*/

class Solution {
public:
    int firstUniqChar(string s) {
        vector<int> freq(26, 0);
        vector<int> firstPos(26, -1);
        
        for (int i = 0; i < s.length(); i++) {
            freq[s[i] - 'a']++;
            if (firstPos[s[i] - 'a'] == -1) {
                firstPos[s[i] - 'a'] = i;
            }
        }
        
        int result = INT_MAX;
        for (int i = 0; i < 26; i++) {
            if (freq[i] == 1) {
                result = min(result, firstPos[i]);
            }
        }
        
        return result == INT_MAX ? -1 : result;
    }
};
```

**Key Points:**
- Store both frequency and first position
- Two-pass solution for clarity
- Could be done in one pass with different data structure

### 4. License Key Formatting (Microsoft)
```cpp
/*
Time Complexity: O(n)
Company: Microsoft
Difficulty: Easy

Problem: Format a license key string with dashes every k characters, ignoring original dashes.

Example:
Input: s = "5F3Z-2e-9-w", k = 4
Output: "5F3Z-2E9W"
*/

class Solution {
public:
    string licenseKeyFormatting(string s, int k) {
        string result;
        int count = 0;
        
        // Process characters from right to left
        for (int i = s.length() - 1; i >= 0; i--) {
            if (s[i] != '-') {
                if (count == k) {
                    result += '-';
                    count = 0;
                }
                result += toupper(s[i]);
                count++;
            }
        }
        
        reverse(result.begin(), result.end());
        return result;
    }
};
```

## Medium Problems

### 1. Longest Repeating Character Replacement (Netflix)
```cpp
/*
Time Complexity: O(n)
Company: Netflix
Difficulty: Medium

Problem: Given a string s and an integer k, you can replace any character in the string
with another character at most k times. Return the length of the longest substring
containing the same letter you can get after performing the operations.

Example:
Input: s = "AABABBA", k = 1
Output: 4
*/

class Solution {
public:
    int characterReplacement(string s, int k) {
        vector<int> count(26, 0);
        int maxCount = 0, maxLen = 0;
        int start = 0;
        
        for (int end = 0; end < s.length(); end++) {
            count[s[end] - 'A']++;
            maxCount = max(maxCount, count[s[end] - 'A']);
            
            if (end - start + 1 - maxCount > k) {
                count[s[start] - 'A']--;
                start++;
            }
            
            maxLen = max(maxLen, end - start + 1);
        }
        
        return maxLen;
    }
};
```

### 2. Complex Number Multiplication (Google)
```cpp
/*
Time Complexity: O(1)
Company: Google
Difficulty: Medium

Problem: Given two complex numbers as strings, return their product as a string.

Example:
Input: num1 = "1+1i", num2 = "1+1i"
Output: "0+2i"
*/

class Solution {
public:
    pair<int, int> parseComplex(string num) {
        int plus = num.find('+');
        int real = stoi(num.substr(0, plus));
        int imag = stoi(num.substr(plus + 1, num.length() - plus - 2));
        return {real, imag};
    }
    
    string complexNumberMultiply(string num1, string num2) {
        auto [a, b] = parseComplex(num1);
        auto [c, d] = parseComplex(num2);
        
        int real = a * c - b * d;
        int imag = a * d + b * c;
        
        return to_string(real) + "+" + to_string(imag) + "i";
    }
};
```

### 3. Encode and Decode Strings (Amazon)
```cpp
/*
Time Complexity: O(n)
Company: Amazon
Difficulty: Medium

Problem: Design an algorithm to encode and decode a list of strings.

Example:
Input: ["Hello","World"]
Output: "5#Hello5#World"
*/

class Codec {
public:
    string encode(vector<string>& strs) {
        string encoded;
        for (const string& s : strs) {
            encoded += to_string(s.length()) + '#' + s;
        }
        return encoded;
    }
    
    vector<string> decode(string s) {
        vector<string> result;
        int i = 0;
        
        while (i < s.length()) {
            int j = s.find('#', i);
            int len = stoi(s.substr(i, j - i));
            result.push_back(s.substr(j + 1, len));
            i = j + 1 + len;
        }
        
        return result;
    }
};
```

## Hard Problems

### 1. Minimum Window Substring (Amazon)
```cpp
/*
Time Complexity: O(n+m)
Company: Amazon
Difficulty: Hard

Problem: Given strings s and t, return the minimum window substring of s that contains 
all characters of t. Return empty string if no such substring exists.

Example:
Input: s = "ADOBECODEBANC", t = "ABC"
Output: "BANC"
*/

class Solution {
public:
    string minWindow(string s, string t) {
        vector<int> freq(128, 0);
        for (char c : t) freq[c]++;
        
        int counter = t.length();
        int start = 0, minStart = 0;
        int minLen = INT_MAX;
        
        for (int end = 0; end < s.length(); end++) {
            if (freq[s[end]]-- > 0) {
                counter--;
            }
            
            while (counter == 0) {
                if (end - start + 1 < minLen) {
                    minLen = end - start + 1;
                    minStart = start;
                }
                
                if (freq[s[start]]++ >= 0) {
                    counter++;
                }
                start++;
            }
        }
        
        return minLen == INT_MAX ? "" : s.substr(minStart, minLen);
    }
};
```

**Key Points:**
- Use sliding window technique
- Maintain frequency count of characters
- Optimize window size when valid solution found
- Handle edge cases (empty strings, no solution)

### 2. Longest Common Substring (Facebook)
```cpp
/*
Time Complexity: O(n * m)
Company: Facebook
Difficulty: Hard

Problem: Given two strings, find the longest string which is substring of both strings.

Example:
Input: X = "ABCDGH", Y = "ACDGHR"
Output: 4 (length of "CDGH")
*/

class Solution {
public:
    int longestCommonSubstr(string S1, string S2, int n, int m) {
        vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
        int maxLength = 0;
        
        for (int i = 1; i <= n; i++) {
            for (int j = 1; j <= m; j++) {
                if (S1[i-1] == S2[j-1]) {
                    dp[i][j] = dp[i-1][j-1] + 1;
                    maxLength = max(maxLength, dp[i][j]);
                }
            }
        }
        
        return maxLength;
    }
};
```

**Key Points:**
- Use 2D DP table for length tracking
- Only increment when characters match
- Reset count when characters don't match
- Space can be optimized to O(min(n,m))

### 3. Longest Substring with At Least K Repeating Characters (Microsoft)
```cpp
/*
Time Complexity: O(n)
Company: Microsoft
Difficulty: Medium

Problem: Find the longest substring where each character appears at least k times.

Example:
Input: s = "aaabb", k = 3
Output: 3 (substring "aaa")
*/

class Solution {
public:
    int longestSubstring(string s, int k) {
        if (s.length() < k) return 0;
        if (k == 1) return s.length();
        
        unordered_map<char, int> count;
        for (char c : s) count[c]++;
        
        // If all characters appear at least k times
        bool valid = true;
        for (auto [c, freq] : count) {
            if (freq < k) {
                valid = false;
                break;
            }
        }
        if (valid) return s.length();
        
        // Divide and conquer
        int maxLen = 0;
        int start = 0;
        for (int i = 0; i < s.length(); i++) {
            if (count[s[i]] < k) {
                maxLen = max(maxLen, 
                           longestSubstring(s.substr(start, i - start), k));
                start = i + 1;
            }
        }
        
        // Check last partition
        if (start < s.length()) {
            maxLen = max(maxLen, 
                        longestSubstring(s.substr(start), k));
        }
        
        return maxLen;
    }
};
```

**Key Points:**
- Use divide and conquer approach
- Split at characters with frequency < k
- Recursive solution for subproblems
- Base cases for optimization

### 4. Count and Say (Amazon)
```cpp
/*
Time Complexity: O(n * m), where m is average length of string
Company: Amazon
Difficulty: Easy

Problem: Generate nth term of count-and-say sequence.
1 is read as "one 1" or 11
11 is read as "two 1s" or 21
21 is read as "one 2, one 1" or 1211

Example:
Input: n = 4
Output: "1211"
*/

class Solution {
public:
    string countAndSay(int n) {
        if (n == 1) return "1";
        
        string prev = countAndSay(n - 1);
        string result;
        
        int count = 1;
        char current = prev[0];
        
        for (int i = 1; i < prev.length(); i++) {
            if (prev[i] == current) {
                count++;
            } else {
                result += to_string(count) + current;
                current = prev[i];
                count = 1;
            }
        }
        
        // Add last group
        result += to_string(count) + current;
        return result;
    }
};
```

### 5. Add Strings (Google)
```cpp
/*
Time Complexity: O(n)
Company: Google
Difficulty: Medium

Problem: Add two numbers represented as strings without converting to integers.

Example:
Input: num1 = "456", num2 = "77"
Output: "533"
*/

class Solution {
public:
    string addStrings(string num1, string num2) {
        string result;
        int carry = 0;
        int i = num1.length() - 1;
        int j = num2.length() - 1;
        
        while (i >= 0 || j >= 0 || carry) {
            int sum = carry;
            if (i >= 0) sum += num1[i--] - '0';
            if (j >= 0) sum += num2[j--] - '0';
            
            carry = sum / 10;
            result += (sum % 10) + '0';
        }
        
        reverse(result.begin(), result.end());
        return result;
    }
};
```

**Common Patterns in String Problems:**

1. **Sliding Window Pattern**
   - Used in: Minimum Window Substring, Longest Repeating Character Replacement
   - Key: Maintain window bounds and validity conditions
   - Often uses frequency counting

2. **Two Pointers**
   - Used in: Valid Palindrome, Add Strings
   - Key: Process string from both ends
   - Useful for comparison and merging

3. **Dynamic Programming**
   - Used in: Longest Common Substring
   - Key: Build solution from smaller subproblems
   - Often uses 2D arrays for string comparison

4. **Hash Table/Counting**
   - Used in: Valid Anagram, First Unique Character
   - Key: Track character frequencies
   - O(n) space for O(1) lookups

5. **Stack-based**
   - Used in: Valid Parentheses
   - Key: Match opening/closing patterns
   - Good for nested structures

**Interview Tips:**
1. Always clarify input constraints:
   - Character set (ASCII/Unicode)
   - Case sensitivity
   - Special character handling
   - Empty string cases

2. Consider efficiency:
   - Can you solve in O(1) space?
   - Is string immutable in your language?
   - StringBuilder vs String concatenation

3. Common Edge Cases:
   - Empty strings
   - Single character strings
   - All same characters
   - No valid solution exists
   - Maximum/minimum input lengths

