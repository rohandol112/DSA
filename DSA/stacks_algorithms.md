# Stack Algorithms in C++

A **stack** is a linear data structure that follows the **Last In, First Out (LIFO)** principle. This means that the last element added to the stack is the first one to be removed. Stacks are widely used in programming for tasks like parsing expressions, backtracking, and managing function calls.

## Basic Operations of a Stack

### 1. Push
The `push` operation adds an element to the top of the stack.

**Explanation**:
- Check if the stack is full.
- If not, add the element to the top of the stack.

### 2. Pop
The `pop` operation removes the top element from the stack.

**Explanation**:
- Check if the stack is empty.
- If not, remove and return the top element.

### 3. Peek
The `peek` operation retrieves the top element without removing it.

**Explanation**:
- Check if the stack is empty.
- If not, return the top element.

### 4. IsEmpty
The `isEmpty` operation checks whether the stack is empty.

**Explanation**:
- Return `true` if the stack has no elements; otherwise, return `false`.

### 5. IsFull
The `isFull` operation checks whether the stack is full.

**Explanation**:
- Return `true` if the stack has reached its maximum size; otherwise, return `false`.

## Applications of Stacks

### 1. Expression Evaluation
Stacks are used to evaluate arithmetic expressions, especially in postfix (Reverse Polish Notation) form.

### 2. Balanced Parentheses
Stacks are used to check whether parentheses in an expression are balanced.

### 3. Backtracking
Stacks are used in algorithms like depth-first search (DFS) to keep track of visited nodes and backtrack when necessary.

### 4. Function Call Management
Stacks are used by programming languages to manage function calls, storing local variables and return addresses.

## Example Implementation in C++

```cpp
#include <iostream>
#include <vector>
using namespace std;

class Stack {
private:
    vector<int> stack;  // Dynamic container to hold stack elements
    int maxSize;        // Maximum size of the stack

public:
    // Constructor to initialize the stack with a maximum size
    Stack(int size) {
        maxSize = size;
    }

    // Push operation to add an element to the stack
    void push(int element) {
        if (stack.size() >= maxSize) {
            cout << "Stack Overflow" << endl;
            return;
        }
        stack.push_back(element);
    }

    // Pop operation to remove the top element from the stack
    int pop() {
        if (stack.empty()) {
            cout << "Stack Underflow" << endl;
            return -1; // Return -1 to indicate an error
        }
        int topElement = stack.back();
        stack.pop_back();
        return topElement;
    }

    // Peek operation to view the top element without removing it
    int peek() {
        if (stack.empty()) {
            cout << "Stack is Empty" << endl;
            return -1; // Return -1 to indicate an error
        }
        return stack.back();
    }

    // Check if the stack is empty
    bool isEmpty() {
        return stack.empty();
    }

    // Check if the stack is full
    bool isFull() {
        return stack.size() == maxSize;
    }
};

int main() {
    // Create a stack with a maximum size of 5
    Stack stack(5);

    // Push elements onto the stack
    stack.push(10);
    stack.push(20);

    // Pop an element and display it
    cout << stack.pop() << endl;  // Output: 20

    // Peek the top element and display it
    cout << stack.peek() << endl; // Output: 10

    return 0;
}
```

## Explanation of the Code

1. **Class Definition**:
   - The `Stack` class encapsulates the stack operations and data.
   - It uses a `vector` to store the stack elements dynamically.

2. **Constructor**:
   - Initializes the stack with a specified maximum size.

3. **Push Operation**:
   - Adds an element to the stack if it is not full.
   - Prints "Stack Overflow" if the stack has reached its maximum capacity.

4. **Pop Operation**:
   - Removes the top element and returns it.
   - Prints "Stack Underflow" if the stack is empty.

5. **Peek Operation**:
   - Returns the top element without removing it.
   - Prints "Stack is Empty" if the stack is empty.

6. **IsEmpty and IsFull Operations**:
   - Check if the stack is empty or full, respectively.

7. **Main Function**:
   - Demonstrates the usage of the stack by performing `push`, `pop`, and `peek` operations.

Stacks are essential for solving problems requiring reverse order processing and are a key concept in computer science.


# Stack Algorithms and LeetCode Problems
A comprehensive collection of stack-based algorithms and common LeetCode problems.

## 1. Easy Problems

### Valid Parentheses (LeetCode 20)
Problem: Check if string has valid parentheses.
```cpp
bool isValid(string s) {
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

### Remove All Adjacent Duplicates (LeetCode 1047)
Problem: Remove all adjacent duplicate characters.
```cpp
string removeDuplicates(string s) {
    stack<char> st;
    
    for (char c : s) {
        if (!st.empty() && st.top() == c) {
            st.pop();
        } else {
            st.push(c);
        }
    }
    
    string result;
    while (!st.empty()) {
        result = st.top() + result;
        st.pop();
    }
    return result;
}
```

## 2. Medium Problems

### Daily Temperatures (LeetCode 739)
Problem: Find the next warmer temperature for each day.
```cpp
vector<int> dailyTemperatures(vector<int>& temperatures) {
    int n = temperatures.size();
    vector<int> result(n);
    stack<int> st;  // stores indices
    
    for (int i = 0; i < n; i++) {
        while (!st.empty() && temperatures[st.top()] < temperatures[i]) {
            int prevDay = st.top();
            st.pop();
            result[prevDay] = i - prevDay;
        }
        st.push(i);
    }
    return result;
}
```

### Evaluate Reverse Polish Notation (LeetCode 150)
Problem: Evaluate postfix expression.
```cpp
int evalRPN(vector<string>& tokens) {
    stack<int> st;
    
    for (const string& token : tokens) {
        if (token == "+" || token == "-" || token == "*" || token == "/") {
            int b = st.top(); st.pop();
            int a = st.top(); st.pop();
            
            if (token == "+") st.push(a + b);
            else if (token == "-") st.push(a - b);
            else if (token == "*") st.push(a * b);
            else st.push(a / b);
        } else {
            st.push(stoi(token));
        }
    }
    return st.top();
}
```

## 3. Hard Problems

### Largest Rectangle in Histogram (LeetCode 84)
Problem: Find the largest rectangular area possible in a histogram.
```cpp
int largestRectangleArea(vector<int>& heights) {
    stack<int> st;
    int maxArea = 0;
    int n = heights.size();
    
    for (int i = 0; i <= n; i++) {
        int currHeight = (i == n) ? 0 : heights[i];
        
        while (!st.empty() && heights[st.top()] > currHeight) {
            int height = heights[st.top()];
            st.pop();
            int width = st.empty() ? i : i - st.top() - 1;
            maxArea = max(maxArea, height * width);
        }
        st.push(i);
    }
    return maxArea;
}
```

### Basic Calculator (LeetCode 224)
Problem: Implement basic calculator with parentheses.
```cpp
int calculate(string s) {
    stack<int> st;
    int result = 0;
    int number = 0;
    int sign = 1;
    
    for (char c : s) {
        if (isdigit(c)) {
            number = number * 10 + (c - '0');
        }
        else if (c == '+' || c == '-') {
            result += sign * number;
            number = 0;
            sign = (c == '+') ? 1 : -1;
        }
        else if (c == '(') {
            st.push(result);
            st.push(sign);
            result = 0;
            sign = 1;
        }
        else if (c == ')') {
            result += sign * number;
            number = 0;
            result *= st.top(); st.pop();
            result += st.top(); st.pop();
        }
    }
    return result + sign * number;
}
```

## 4. Common Stack Patterns and Techniques

### 1. Monotonic Stack Pattern
Used when you need to find the next/previous greater/smaller element.
```cpp
vector<int> nextGreaterElements(vector<int>& nums) {
    int n = nums.size();
    vector<int> result(n, -1);
    stack<int> st;
    
    for (int i = 0; i < n * 2; i++) {
        int num = nums[i % n];
        while (!st.empty() && nums[st.top()] < num) {
            result[st.top()] = num;
            st.pop();
        }
        if (i < n) st.push(i);
    }
    return result;
}
```

### 2. Two Stacks Pattern
Used for parsing expressions or implementing special data structures.
```cpp
class MinStack {
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
        if (mainStack.top() == minStack.top()) {
            minStack.pop();
        }
        mainStack.pop();
    }
    
    int top() {
        return mainStack.top();
    }
    
    int getMin() {
        return minStack.top();
    }
};
```

### 3. Stack with Extra Information
Used when you need to maintain additional information along with stack elements.
```cpp
struct StackNode {
    int value;
    int min;
    StackNode(int v, int m) : value(v), min(m) {}
};

class MinStack {
    stack<StackNode> st;
public:
    void push(int val) {
        int minVal = st.empty() ? val : min(val, st.top().min);
        st.push(StackNode(val, minVal));
    }
    
    int getMin() {
        return st.top().min;
    }
};
```

## 5. Problem-Solving Tips

1. When to Use Stack:
   - Dealing with nested structures (parentheses, brackets)
   - Need to track historical information
   - Processing strings or arrays in reverse order
   - Finding next/previous greater/smaller elements

2. Common Tricks:
   - Using dummy values/sentinels
   - Two-pass approach
   - Combining with other data structures
   - Using multiple stacks

3. Time/Space Complexity:
   - Most stack operations: O(1)
   - Processing array once: O(n)
   - Space complexity usually O(n)

4. Edge Cases to Consider:
   - Empty stack
   - Stack with one element
   - Duplicate elements
   - Negative numbers
   - Overflow/underflow situations



# Stack-based Algorithms in C++

## 1. Reverse a String Using Stack

```cpp
#include <iostream>
#include <stack>
#include <string>

std::string reverse_string_using_stack(const std::string& input_string) {
    std::stack<char> stack;
    for (char ch : input_string) {
        stack.push(ch);
    }

    std::string reversed_string;
    while (!stack.empty()) {
        reversed_string += stack.top();
        stack.pop();
    }
    return reversed_string;
}

int main() {
    std::string input_str = "Hello, World!";
    std::string reversed_str = reverse_string_using_stack(input_str);
    std::cout << "Original String: " << input_str << std::endl;
    std::cout << "Reversed String: " << reversed_str << std::endl;
    return 0;
}
## 2.\.Check Balanced Parentheses (Wellness Check)
cpp
Copy code
#include <iostream>
#include <stack>
#include <unordered_map>

bool is_balanced(const std::string& expression) {
    std::stack<char> stack;
    std::unordered_map<char, char> matching_brackets = { {')', '('}, {'}', '{'}, {']', '['} };
    
    for (char ch : expression) {
        if (matching_brackets.count(ch) == 0) {
            stack.push(ch);
        } else {
            if (stack.empty() || stack.top() != matching_brackets[ch]) {
                return false;
            }
            stack.pop();
        }
    }
    return stack.empty();
}

int main() {
    std::string expression = "{[()()]}";
    std::cout << "Balanced: " << (is_balanced(expression) ? "Yes" : "No") << std::endl;
    return 0;
}
3. Decimal to Binary Conversion Using Stack
cpp
Copy code
#include <iostream>
#include <stack>
#include <string>

std::string decimal_to_binary(int n) {
    std::stack<char> stack;
    while (n > 0) {
        stack.push((n % 2) + '0');
        n = n / 2;
    }

    std::string binary_number;
    while (!stack.empty()) {
        binary_number += stack.top();
        stack.pop();
    }
    return binary_number;
}

int main() {
    int number = 10;
    std::string binary_representation = decimal_to_binary(number);
    std::cout << "Decimal: " << number << " -> Binary: " << binary_representation << std::endl;
    return 0;
}
#4. Infix to Postfix Expression Conversion
cpp
Copy code
#include <iostream>
#include <stack>
#include <string>

int precedence(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/') return 2;
    return 0;
}

std::string infix_to_postfix(const std::string& expression) {
    std::stack<char> stack;
    std::string postfix;
    
    for (char ch : expression) {
        if (isalnum(ch)) {
            postfix += ch;
        } else if (ch == '(') {
            stack.push(ch);
        } else if (ch == ')') {
            while (!stack.empty() && stack.top() != '(') {
                postfix += stack.top();
                stack.pop();
            }
            stack.pop();
        } else {
            while (!stack.empty() && precedence(ch) <= precedence(stack.top())) {
                postfix += stack.top();
                stack.pop();
            }
            stack.push(ch);
        }
    }
    
    while (!stack.empty()) {
        postfix += stack.top();
        stack.pop();
    }
    
    return postfix;
}

int main() {
    std::string infix_expression = "(A+B)*(C-D)";
    std::string postfix_expression = infix_to_postfix(infix_expression);
    std::cout << "Infix: " << infix_expression << std::endl;
    std::cout << "Postfix: " << postfix_expression << std::endl;
    return 0;
}
5. Infix Expression Evaluation
cpp
Copy code
#include <iostream>
#include <stack>
#include <string>
#include <cctype>
#include <cstdlib>

int precedence(char op) {
    if (op == '+' || op == '-') return 1;
    if (op == '*' || op == '/') return 2;
    return 0;
}

int apply_operator(int a, int b, char op) {
    switch(op) {
        case '+': return a + b;
        case '-': return a - b;
        case '*': return a * b;
        case '/': return a / b;
        default: return 0;
    }
}

int evaluate_infix(const std::string& expression) {
    std::stack<int> values;
    std::stack<char> operators;
    
    for (size_t i = 0; i < expression.length(); i++) {
        if (isdigit(expression[i])) {
            int value = 0;
            while (i < expression.length() && isdigit(expression[i])) {
                value = value * 10 + (expression[i] - '0');
                i++;
            }
            values.push(value);
            i--;
        } else if (expression[i] == '(') {
            operators.push(expression[i]);
        } else if (expression[i] == ')') {
            while (!operators.empty() && operators.top() != '(') {
                int val2 = values.top();
                values.pop();
                int val1 = values.top();
                values.pop();
                char op = operators.top();
                operators.pop();
                values.push(apply_operator(val1, val2, op));
            }
            operators.pop();
        } else if (expression[i] == '+' || expression[i] == '-' || expression[i] == '*' || expression[i] == '/') {
            while (!operators.empty() && precedence(operators.top()) >= precedence(expression[i])) {
                int val2 = values.top();
                values.pop();
                int val1 = values.top();
                values.pop();
                char op = operators.top();
                operators.pop();
                values.push(apply_operator(val1, val2, op));
            }
            operators.push(expression[i]);
        }
    }
    
    while (!operators.empty()) {
        int val2 = values.top();
        values.pop();
        int val1 = values.top();
        values.pop();
        char op = operators.top();
        operators.pop();
        values.push(apply_operator(val1, val2, op));
    }
    
    return values.top();
}

int main() {
    std::string expression = "3 + (2 * 5) - 8";
    int result = evaluate_infix(expression);
    std::cout << "Result: " << result << std::endl;
    return 0;
}