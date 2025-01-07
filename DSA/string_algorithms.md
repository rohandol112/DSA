# Common String Patterns Guide

## 1. Sliding Window Pattern

### Core Concept
The sliding window pattern maintains a window that can expand or contract based on certain conditions. It's particularly useful when you need to find a subarray or substring that meets specific criteria.

### How It Works
1. Create a window with two pointers (start, end)
2. Expand window by moving end pointer
3. Contract window by moving start pointer when needed
4. Track current window state (often using hash map/array)

### Example Implementation
```cpp
/*
Problem: Find longest substring with at most k distinct characters
LeetCode 340 - Longest Substring with At Most K Distinct Characters
*/

class Solution {
public:
    int lengthOfLongestSubstringKDistinct(string s, int k) {
        if (k == 0) return 0;
        
        unordered_map<char, int> freq;
        int start = 0, maxLen = 0;
        
        for (int end = 0; end < s.length(); end++) {
            // Expand window
            freq[s[end]]++;
            
            // Contract window if needed
            while (freq.size() > k) {
                if (--freq[s[start]] == 0) {
                    freq.erase(s[start]);
                }
                start++;
            }
            
            maxLen = max(maxLen, end - start + 1);
        }
        
        return maxLen;
    }
};
```

### Related LeetCode Problems
1. [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/)
2. [424. Longest Repeating Character Replacement](https://leetcode.com/problems/longest-repeating-character-replacement/)
3. [3. Longest Substring Without Repeating Characters](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

## 2. Two Pointers Pattern

### Core Concept
Two pointers technique uses two pointers to traverse the string, often from opposite ends, meeting in the middle.

### How It Works
1. Initialize two pointers (usually left and right)
2. Move pointers based on conditions
3. Process elements at pointer positions
4. Continue until pointers meet or cross

### Example Implementation
```cpp
/*
Problem: Check if string is palindrome ignoring non-alphanumeric characters
LeetCode 125 - Valid Palindrome
*/

class Solution {
public:
    bool isPalindrome(string s) {
        int left = 0, right = s.length() - 1;
        
        while (left < right) {
            // Skip non-alphanumeric characters
            while (left < right && !isalnum(s[left])) left++;
            while (left < right && !isalnum(s[right])) right--;
            
            // Compare characters
            if (tolower(s[left]) != tolower(s[right])) {
                return false;
            }
            
            left++;
            right--;
        }
        
        return true;
    }
};
```

### Related LeetCode Problems
1. [167. Two Sum II](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)
2. [344. Reverse String](https://leetcode.com/problems/reverse-string/)
3. [11. Container With Most Water](https://leetcode.com/problems/container-with-most-water/)

## 3. Dynamic Programming Pattern

### Core Concept
DP in strings involves building solutions using previously computed results, often using a table to store intermediate results.

### How It Works
1. Create DP table (often 2D for strings)
2. Define base cases
3. Fill table using recurrence relation
4. Extract final result

### Example Implementation
```cpp
/*
Problem: Find length of longest common substring
LeetCode (Similar to 1143. Longest Common Subsequence)
*/

class Solution {
public:
    int longestCommonSubstring(string text1, string text2) {
        int m = text1.length(), n = text2.length();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        int maxLength = 0;
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1[i-1] == text2[j-1]) {
                    dp[i][j] = dp[i-1][j-1] + 1;
                    maxLength = max(maxLength, dp[i][j]);
                }
            }
        }
        
        return maxLength;
    }
};
```

### Related LeetCode Problems
1. [1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/)
2. [516. Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/)
3. [5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring/)

## 4. Hash Table/Counting Pattern

### Core Concept
Use hash tables or arrays to count character frequencies or track character positions.

### How It Works
1. Create frequency map/array
2. Process string to update counts
3. Use counts to solve problem
4. Often O(n) time and space

### Example Implementation
```cpp
/*
Problem: First unique character in string
LeetCode 387 - First Unique Character in a String
*/

class Solution {
public:
    int firstUniqChar(string s) {
        vector<int> freq(26, 0);
        vector<int> firstPos(26, -1);
        
        // Count frequencies and first positions
        for (int i = 0; i < s.length(); i++) {
            freq[s[i] - 'a']++;
            if (firstPos[s[i] - 'a'] == -1) {
                firstPos[s[i] - 'a'] = i;
            }
        }
        
        // Find first unique character
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

### Related LeetCode Problems
1. [242. Valid Anagram](https://leetcode.com/problems/valid-anagram/)
2. [49. Group Anagrams](https://leetcode.com/problems/group-anagrams/)
3. [336. Palindrome Pairs](https://leetcode.com/problems/palindrome-pairs/)

## 5. Stack-based Pattern

### Core Concept
Use stack to track nested structures or maintain order of operations.

### How It Works
1. Initialize empty stack
2. Push opening characters
3. Pop and match with closing characters
4. Check stack state for validity

### Example Implementation
```cpp
/*
Problem: Valid parentheses checking
LeetCode 20 - Valid Parentheses
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
                if (st.empty() || st.top() != pairs[c]) {
                    return false;
                }
                st.pop();
            }
        }
        
        return st.empty();
    }
};
```

### Related LeetCode Problems
1. [32. Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/)
2. [856. Score of Parentheses](https://leetcode.com/problems/score-of-parentheses/)
3. [1249. Minimum Remove to Make Valid Parentheses](https://leetcode.com/problems/minimum-remove-to-make-valid-parentheses/)

## Pattern Selection Tips

1. **Choose Sliding Window when:**
   - Looking for a substring that meets certain conditions
   - Need to maintain a window of elements
   - Problem involves consecutive elements

2. **Choose Two Pointers when:**
   - Need to find pairs of elements
   - Working from both ends of string
   - Comparing elements at different positions

3. **Choose Dynamic Programming when:**
   - Problem has overlapping subproblems
   - Need to find optimal substring/subsequence
   - Problem involves pattern matching

4. **Choose Hash Table when:**
   - Need to count frequencies
   - Track character positions
   - Check for duplicates/uniqueness

5. **Choose Stack when:**
   - Working with nested structures
   - Need to match pairs of characters
   - Processing strings in LIFO order

