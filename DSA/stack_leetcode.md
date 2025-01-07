# Stack LeetCode Solutions (Part 1)

## 1. Implement Queue using Stacks (Easy)
```cpp
class MyQueue {
private:
    stack<int> s1; // for push
    stack<int> s2; // for pop
    
public:
    void push(int x) {
        s1.push(x);
    }
    
    int pop() {
        if (s2.empty()) {
            while (!s1.empty()) {
                s2.push(s1.top());
                s1.pop();
            }
        }
        int front = s2.top();
        s2.pop();
        return front;
    }
    
    int peek() {
        if (s2.empty()) {
            while (!s1.empty()) {
                s2.push(s1.top());
                s1.pop();
            }
        }
        return s2.top();
    }
    
    bool empty() {
        return s1.empty() && s2.empty();
    }
};
```

## 2. Decode String (Medium)
```cpp
class Solution {
public:
    string decodeString(string s) {
        stack<string> strStack;
        stack<int> countStack;
        string currentString;
        int currentNumber = 0;
        
        for (char c : s) {
            if (isdigit(c)) {
                currentNumber = currentNumber * 10 + (c - '0');
            }
            else if (c == '[') {
                countStack.push(currentNumber);
                strStack.push(currentString);
                currentNumber = 0;
                currentString = "";
            }
            else if (c == ']') {
                string decodedString = strStack.top();
                strStack.pop();
                int count = countStack.top();
                countStack.pop();
                
                for (int i = 0; i < count; i++) {
                    decodedString += currentString;
                }
                currentString = decodedString;
            }
            else {
                currentString += c;
            }
        }
        
        return currentString;
    }
};
```

## 3. Car Fleet (Medium)
```cpp
class Solution {
public:
    int carFleet(int target, vector<int>& position, vector<int>& speed) {
        int n = position.size();
        vector<pair<int, double>> cars;
        
        // Store position and time to reach target
        for (int i = 0; i < n; i++) {
            double time = (double)(target - position[i]) / speed[i];
            cars.push_back({position[i], time});
        }
        
        // Sort by position
        sort(cars.begin(), cars.end());
        
        // Use stack to track fleets
        stack<double> fleet;
        for (int i = n - 1; i >= 0; i--) {
            double time = cars[i].second;
            if (fleet.empty() || time > fleet.top()) {
                fleet.push(time);
            }
        }
        
        return fleet.size();
    }
};
```

## 4. Backspace String Compare (Easy)
```cpp
class Solution {
public:
    bool backspaceCompare(string s, string t) {
        return buildString(s) == buildString(t);
    }
    
private:
    string buildString(string s) {
        stack<char> st;
        
        for (char c : s) {
            if (c != '#') {
                st.push(c);
            } else if (!st.empty()) {
                st.pop();
            }
        }
        
        string result;
        while (!st.empty()) {
            result = st.top() + result;
            st.pop();
        }
        return result;
    }
};
```

## 5. Asteroid Collision (Medium)
```cpp
class Solution {
public:
    vector<int> asteroidCollision(vector<int>& asteroids) {
        stack<int> st;
        
        for (int asteroid : asteroids) {
            bool survive = true;
            
            while (!st.empty() && asteroid < 0 && st.top() > 0) {
                if (st.top() < -asteroid) {
                    st.pop();
                    continue;
                } else if (st.top() == -asteroid) {
                    st.pop();
                }
                survive = false;
                break;
            }
            
            if (survive) {
                st.push(asteroid);
            }
        }
        
        vector<int> result(st.size());
        for (int i = st.size() - 1; i >= 0; i--) {
            result[i] = st.top();
            st.pop();
        }
        
        return result;
    }
};
```

## 6. Exclusive Time of Functions (Medium)
```cpp
class Solution {
public:
    vector<int> exclusiveTime(int n, vector<string>& logs) {
        vector<int> result(n, 0);
        stack<pair<int, int>> st; // {id, startTime}
        
        for (string& log : logs) {
            stringstream ss(log);
            string idStr, type, timeStr;
            getline(ss, idStr, ':');
            getline(ss, type, ':');
            getline(ss, timeStr, ':');
            
            int id = stoi(idStr);
            int timestamp = stoi(timeStr);
            
            if (type == "start") {
                if (!st.empty()) {
                    result[st.top().first] += timestamp - st.top().second;
                }
                st.push({id, timestamp});
            } else {
                auto [startId, startTime] = st.top();
                st.pop();
                result[id] += timestamp - startTime + 1;
                if (!st.empty()) {
                    st.top().second = timestamp + 1;
                }
            }
        }
        
        return result;
    }
};
```

## 7. Baseball Game (Easy)
```cpp
class Solution {
public:
    int calPoints(vector<string>& operations) {
        stack<int> scores;
        
        for (string& op : operations) {
            if (op == "+") {
                int top = scores.top();
                scores.pop();
                int newTop = top + scores.top();
                scores.push(top);
                scores.push(newTop);
            }
            else if (op == "D") {
                scores.push(2 * scores.top());
            }
            else if (op == "C") {
                scores.pop();
            }
            else {
                scores.push(stoi(op));
            }
        }
        
        int sum = 0;
        while (!scores.empty()) {
            sum += scores.top();
            scores.pop();
        }
        return sum;
    }
};
```

# Stack LeetCode Solutions (Part 2)

## 8. Minimum Add to Make Parentheses Valid (Medium)
```cpp
class Solution {
public:
    int minAddToMakeValid(string s) {
        stack<char> st;
        int count = 0;
        
        for (char c : s) {
            if (c == '(') {
                st.push(c);
            } else {
                if (st.empty()) {
                    count++;
                } else {
                    st.pop();
                }
            }
        }
        
        return count + st.size();
    }
};
```

## 9. Next Greater Node In Linked List (Medium)
```cpp
class Solution {
public:
    vector<int> nextLargerNodes(ListNode* head) {
        vector<int> values;
        for (ListNode* curr = head; curr; curr = curr->next) {
            values.push_back(curr->val);
        }
        
        stack<int> st;
        vector<int> result(values.size());
        
        for (int i = values.size() - 1; i >= 0; i--) {
            while (!st.empty() && st.top() <= values[i]) {
                st.pop();
            }
            result[i] = st.empty() ? 0 : st.top();
            st.push(values[i]);
        }
        
        return result;
    }
};
```

## 10. Sum of Subarray Minimums (Medium)
```cpp
class Solution {
public:
    int sumSubarrayMins(vector<int>& arr) {
        const int MOD = 1e9 + 7;
        int n = arr.size();
        vector<long long> left(n), right(n);
        stack<pair<int, int>> st;
        
        // Find previous smaller element
        for (int i = 0; i < n; i++) {
            int count = 1;
            while (!st.empty() && st.top().first > arr[i]) {
                count += st.top().second;
                st.pop();
            }
            st.push({arr[i], count});
            left[i] = count;
        }
        
        // Clear stack for reuse
        while (!st.empty()) st.pop();
        
        // Find next smaller element
        for (int i = n - 1; i >= 0; i--) {
            int count = 1;
            while (!st.empty() && st.top().first >= arr[i]) {
                count += st.top().second;
                st.pop();
            }
            st.push({arr[i], count});
            right[i] = count;
        }
        
        // Calculate result
        long long result = 0;
        for (int i = 0; i < n; i++) {
            result = (result + (long long)arr[i] * left[i] * right[i]) % MOD;
        }
        
        return result;
    }
};
```

## 11. Minimum Cost Tree From Leaf Values (Medium)
```cpp
class Solution {
public:
    int mctFromLeafValues(vector<int>& arr) {
        stack<int> st;
        st.push(INT_MAX);
        int result = 0;
        
        for (int num : arr) {
            while (st.top() <= num) {
                int mid = st.top();
                st.pop();
                result += mid * min(st.top(), num);
            }
            st.push(num);
        }
        
        while (st.size() > 2) {
            int num = st.top();
            st.pop();
            result += num * st.top();
        }
        
        return result;
    }
};
```

## 12. Remove Outermost Parentheses (Easy)
```cpp
class Solution {
public:
    string removeOuterParentheses(string s) {
        string result;
        int opened = 0;
        
        for (char c : s) {
            if (c == '(' && opened++ > 0) result += c;
            if (c == ')' && --opened > 0) result += c;
        }
        
        return result;
    }
};
```

## 13. Check If Word Is Valid After Substitutions (Medium)
```cpp
class Solution {
public:
    bool isValid(string s) {
        stack<char> st;
        
        for (char c : s) {
            if (c == 'c') {
                if (st.size() < 2) return false;
                char b = st.top(); st.pop();
                char a = st.top(); st.pop();
                if (a != 'a' || b != 'b') return false;
            } else {
                st.push(c);
            }
        }
        
        return st.empty();
    }
};
```

## 14. Minimum Remove to Make Valid Parentheses (Medium)
```cpp
class Solution {
public:
    string minRemoveToMakeValid(string s) {
        stack<int> st;
        string result = s;
        
        for (int i = 0; i < s.length(); i++) {
            if (s[i] == '(') {
                st.push(i);
            } else if (s[i] == ')') {
                if (!st.empty()) {
                    st.pop();
                } else {
                    result[i] = '*';
                }
            }
        }
        
        while (!st.empty()) {
            result[st.top()] = '*';
            st.pop();
        }
        
        result.erase(remove(result.begin(), result.end(), '*'), result.end());
        return result;
    }
};
```