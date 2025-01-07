# String Data Structure

A comprehensive guide to understanding and working with strings in C++.

## Table of Contents
- [Introduction](#introduction)
- [Getting Started](#getting-started)
- [Basic Operations](#basic-operations)
- [String Manipulation](#string-manipulation)
- [Common String Algorithms](#common-string-algorithms)
- [Advanced Topics](#advanced-topics)

## Introduction

A string is a sequence of characters stored in a contiguous block of memory. In C++, strings are mutable (can be modified) and are implemented through the `std::string` class, which provides robust functionality for text manipulation and processing.

### Key Features
- Contiguous memory storage
- Character-by-character access
- Built-in methods for manipulation
- Efficient searching and pattern matching
- Essential for text processing and algorithms

## Getting Started

### Including the String Library
```cpp
#include <string>
using std::string;  // Optional: if you want to use string directly
```

### Initialization and Declaration
```cpp
// Different ways to initialize strings
string str;                      // Empty string
string str1 = "Hello";          // Using assignment
string str2("World");           // Using constructor
string str3 = str1 + " " + str2; // Using concatenation

// Note: Strings in memory end with a null character '\0'
```

### Basic Input/Output
```cpp
// Output
cout << str1 << endl;

// Input
string input;
cin >> input;         // Reads until whitespace
getline(cin, input);  // Reads entire line including spaces
```

## Basic Operations

### String Length
```cpp
string str = "Hello, world!";
int length = str.length();  // or str.size();
cout << "Length: " << length << endl;
```

### String Traversal
```cpp
string str = "Hello";
// Method 1: Using index
for (int i = 0; i < str.length(); i++) {
    cout << str[i] << ' ';
}

// Method 2: Using range-based for loop
for (char& c : str) {
    cout << c << ' ';
}
```

### String Comparison
```cpp
string str1 = "Hello";
string str2 = "hello";

if (str1 == str2) {
    cout << "Strings are equal" << endl;
} else {
    cout << "Strings are not equal" << endl;
}
```

## String Manipulation

### Case Conversion
```cpp
string str = "Hello World";

// To lowercase
for (char& c : str) {
    c = tolower(c);
}

// To uppercase
for (char& c : str) {
    c = toupper(c);
}

// Note: Case conversion formulas
// Uppercase to Lowercase: ('CHARACTER' - 'A') + 'a'
// Lowercase to Uppercase: ('character' - 'a') + 'A'
```

### String Modification
```cpp
string str = "Hello";
str.append(" World");     // Append at end
str.insert(0, "Say ");    // Insert at position
str.replace(0, 3, "Hi");  // Replace range
str.erase(0, 3);         // Erase characters
```

### String Rotation
```cpp
string str = "Hello, world!";
rotate(str.begin(), str.begin() + 7, str.end());
// Result: "world!Hello, "
```

## Common String Algorithms

### Palindrome Check (Simple)
```cpp
bool isPalindrome(string s) {
    int left = 0, right = s.length() - 1;
    while (left < right) {
        if (s[left] != s[right]) return false;
        left++;
        right--;
    }
    return true;
}
```

### Palindrome Check (Advanced)
```cpp
bool isAlphanumeric(char ch) {
    return isalpha(ch) || isdigit(ch);
}

bool isPalindrome(string s) {
    string cleanStr;
    // Remove non-alphanumeric and convert to lowercase
    for (char ch : s) {
        if (isAlphanumeric(ch)) {
            cleanStr += tolower(ch);
        }
    }
    
    int left = 0, right = cleanStr.length() - 1;
    while (left < right) {
        if (cleanStr[left] != cleanStr[right]) return false;
        left++;
        right--;
    }
    return true;
}
```

## Advanced Topics

### String Hashing
String hashing is an efficient technique for string comparison and pattern matching. It converts strings into numerical values for faster operations.

#### Polynomial Hashing Formula:
```cpp
hash(s) = (s[0]*A^(n-1) + s[1]*A^(n-2) + ... + s[n-1]*A^0) mod B

// Where:
// s[i]: ASCII value of character at position i
// A, B: Carefully chosen constants
// n: Length of string
```

Example implementation:
```cpp
long long computeHash(string const& s) {
    const int A = 911382323;
    const int B = 972663749;
    long long hash = 0;
    long long power = 1;
    
    for (char c : s) {
        hash = (hash + (c - 'a' + 1) * power) % B;
        power = (power * A) % B;
    }
    return hash;
}
```
