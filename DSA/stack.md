# Stack Data Structure
A comprehensive guide to understanding and working with stacks in C++.

Table of Contents
1. Introduction
2. Getting Started
3. Basic Operations
4. Stack Implementation
5. Common Stack Patterns
6. Advanced Topics

## Introduction
A stack is a Last-In-First-Out (LIFO) data structure where elements are added and removed from the same end, called the top. Think of it like a stack of plates - you can only add or remove plates from the top.

Key Features:
- LIFO ordering
- O(1) operations for push and pop
- Tracks only the top element
- Essential for function calls, backtracking, and expression evaluation

## Getting Started

### Including Stack Library
```cpp
#include <stack>
using std::stack;  // Optional: if you want to use stack directly
```

### Initialization and Declaration
```cpp
// Different ways to initialize stacks
stack<int> st;                // Empty stack of integers
stack<string> strStack;       // Stack of strings
stack<pair<int, int>> pairSt; // Stack of pairs

// Initialize stack from another container
vector<int> vec = {1, 2, 3, 4, 5};
stack<int> st(deque<int>(vec.begin(), vec.end()));
```

### Basic Input/Output
```cpp
// Output (Note: this will empty the stack)
while (!st.empty()) {
    cout << st.top() << ' ';
    st.pop();
}

// Input
int n = 5;
for(int i = 0; i < n; i++) {
    int x;
    cin >> x;
    st.push(x);
}
```

## Basic Operations

### Core Stack Operations
```cpp
stack<int> st;

// Push: Add element to top
st.push(10);

// Top: View top element
int topElement = st.top();

// Pop: Remove top element
st.pop();

// Size: Get number of elements
int size = st.size();

// Empty: Check if stack is empty
bool isEmpty = st.empty();
```

### Custom Stack Implementation
```cpp
template <typename T>
class Stack {
private:
    vector<T> data;
    
public:
    void push(T value) {
        data.push_back(value);
    }
    
    void pop() {
        if (!empty()) {
            data.pop_back();
        }
    }
    
    T top() {
        if (!empty()) {
            return data.back();
        }
        throw runtime_error("Stack is empty");
    }
    
    bool empty() {
        return data.empty();
    }
    
    size_t size() {
        return data.size();
    }
};
```

## Common Stack Patterns

### 1. Parentheses Matching
```cpp
bool isValidParentheses(string s) {
    stack<char> st;
    unordered_map<char, char> pairs = {
        {')', '('}, {']', '['}, {'}', '{'}
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
```

### 2. Infix to Postfix Conversion
```cpp
int precedence(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/') return 2;
    return 0;
}

string infixToPostfix(string infix) {
    stack<char> st;
    string postfix;
    
    for (char c : infix) {
        if (isalnum(c)) {
            postfix += c;
        }
        else if (c == '(') {
            st.push(c);
        }
        else if (c == ')') {
            while (!st.empty() && st.top() != '(') {
                postfix += st.top();
                st.pop();
            }
            if (!st.empty()) st.pop(); // Remove '('
        }
        else {
            while (!st.empty() && st.top() != '(' && 
                   precedence(st.top()) >= precedence(c)) {
                postfix += st.top();
                st.pop();
            }
            st.push(c);
        }
    }
    
    while (!st.empty()) {
        postfix += st.top();
        st.pop();
    }
    
    return postfix;
}
```

### 3. Next Greater Element
```cpp
vector<int> nextGreaterElement(vector<int>& nums) {
    int n = nums.size();
    vector<int> result(n, -1);
    stack<int> st;  // store indices
    
    for (int i = 0; i < n; i++) {
        while (!st.empty() && nums[st.top()] < nums[i]) {
            result[st.top()] = nums[i];
            st.pop();
        }
        st.push(i);
    }
    
    return result;
}
```

## Advanced Topics

### 1. Min Stack Implementation
```cpp
class MinStack {
private:
    stack<int> mainStack;
    stack<int> minStack;
    
public:
    void push(int val) {
        mainStack.push(val);
        if (minStack.empty() || val <= minStack.top()) {
            minStack.push(val);
        }
    }
    
    void pop() {
        if (!mainStack.empty()) {
            if (mainStack.top() == minStack.top()) {
                minStack.pop();
            }
            mainStack.pop();
        }
    }
    
    int top() {
        return mainStack.top();
    }
    
    int getMin() {
        return minStack.top();
    }
};
```

### 2. Stack Using Two Queues
```cpp
class StackUsingQueues {
private:
    queue<int> q1, q2;
    
public:
    void push(int val) {
        q2.push(val);
        while (!q1.empty()) {
            q2.push(q1.front());
            q1.pop();
        }
        swap(q1, q2);
    }
    
    int pop() {
        if (q1.empty()) return -1;
        int val = q1.front();
        q1.pop();
        return val;
    }
    
    int top() {
        if (q1.empty()) return -1;
        return q1.front();
    }
    
    bool empty() {
        return q1.empty();
    }
};
```

### Common Stack Problems and Solutions:
1. Evaluate Postfix Expression
2. Sort a Stack
3. Remove Middle Element
4. Reverse a Stack
5. Stock Span Problem
6. Largest Rectangle in Histogram

Time Complexity Analysis:
- Push: O(1)
- Pop: O(1)
- Top: O(1)
- Empty: O(1)
- Size: O(1)

Space Complexity:
- O(n) where n is the number of elements in the stack