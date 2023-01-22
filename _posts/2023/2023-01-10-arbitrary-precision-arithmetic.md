---
title: Arbitrary Precision Arithmetic
author: yanshi
date: 2023-01-10 21:37 +0800
categories: [Code, Algorithm]
tags: [string, simulation]
math: true
mermaid: true
pin: false
img_path: /assets/img/posts/202301/
---

## Definition

[Arbitrary-precision arithmetic - Wikipedia](https://en.wikipedia.org/wiki/Arbitrary-precision_arithmetic)

In computer science, **arbitrary-precision arithmetic**, also called **bignum arithmetic**, **multiple-precision arithmetic**, or sometimes **infinite-precision arithmetic**, indicates that calculations are performed on numbers whose digits of precisionare limited only by the available memory of the host system. This contrasts with the faster fixed-precision arithmetic found in most arithmetic logic unit (ALU) hardware, which typically offers between 8 and 64 bits of precision.

Several modern programming languages have built-in support for bignums, and others have libraries available for arbitrary-precision integer and floating-point math. Rather than storing values as a fixed number of bits related to the size of the processor register, these implementations typically use variable-length arrays of digits.

Arbitrary precision is used in applications where the speed of arithmetic is not a limiting factor, or where precise results with very large numbers are required. It should not be confused with the symbolic computation provided by many computer algebra systems, which represent numbers by expressions such as π·sin(2), and can thus represent any computable number with infinite precision.

(中) 高精度运算  (日) 任意精度演算

## Addition

### Addition Template

```c++
#include <iostream>
#include <vector>

using namespace std;

/* Non-negative addition */
vector<int> add(const vector<int> &num1_lst, const vector<int> &num2_lst) {
    vector<int> res;
    int tmp = 0;
    for (int i = 0; i < num1_lst.size() || i < num2_lst.size(); i++) {
        if (i < num1_lst.size()) { tmp += num1_lst[i]; }
        if (i < num2_lst.size()) { tmp += num2_lst[i]; }
        res.push_back(tmp % 10);
        tmp /= 10;
    }
    if (tmp) { res.push_back(1); }
    return res;
}

int main() {
    /* Input */
    string num1, num2;
    num1 = "1235423151238964", num2 = "215123"; // Or: cin >> num1 >> num2;
    vector<int> num1_lst, num2_lst; // Reverse Order. e.g. "123" -> [3, 2, 1]
    for (int i = num1.size() - 1; i >= 0; i--) { num1_lst.push_back(num1[i] - '0'); }
    for (int i = num2.size() - 1; i >= 0; i--) { num2_lst.push_back(num2[i] - '0'); }

    vector<int> result = add(num1_lst, num2_lst);

    /* Output */
    for (int i = result.size() - 1; i >= 0; i--) { printf("%d", result[i]); }
    cout << endl;
    return 0;
}
```

### LeetCode 415. Add Strings

[LeetCode 415. Add Strings](https://leetcode.cn/problems/add-strings/)

Given two non-negative integers, `num1` and `num2` represented as string, return *the sum of* `num1` *and* `num2` *as a string*.

You must solve the problem without using any built-in library for handling large integers (such as `BigInteger`). You must also not convert the inputs to integers directly.

 

**Example 1:**

```
Input: num1 = "11", num2 = "123"
Output: "134"
```

**Example 2:**

```
Input: num1 = "456", num2 = "77"
Output: "533"
```

**Example 3:**

```
Input: num1 = "0", num2 = "0"
Output: "0"
```

 

**Constraints:**

- `1 <= num1.length, num2.length <= 104`
- `num1` and `num2` consist of only digits.
- `num1` and `num2` don't have any leading zeros except for the zero itself.



```c++
class Solution {
public:
    string addStrings(string num1, string num2) {
        vector<int> num1_lst, num2_lst, res;
        for (int i = num1.size() - 1; i >= 0; i--) { num1_lst.push_back(num1[i] - '0'); }
        for (int i = num2.size() - 1; i >= 0; i--) { num2_lst.push_back(num2[i] - '0'); }
      
        int tmp = 0;
        for (int i = 0; i < num1.size() || i < num2.size(); i++) {
            if (i < num1.size()) { tmp += num1_lst[i]; }
            if (i < num2.size()) { tmp += num2_lst[i]; }
            res.push_back(tmp % 10);
            tmp /= 10;
        }
        if (tmp) { res.push_back(1); }
      
        string ans;
        for (int i = res.size() - 1; i >= 0; i--) { ans.push_back(res[i] + '0'); }
        return ans;
    }
};
```



### LeetCode 67. Add Binary

[LeetCode 67. Add Binary](https://leetcode.cn/problems/add-binary/)

Given two binary strings `a` and `b`, return *their sum as a binary string*.

 

**Example 1:**

```
Input: a = "11", b = "1"
Output: "100"
```

**Example 2:**

```
Input: a = "1010", b = "1011"
Output: "10101"
```

 

**Constraints:**

- `1 <= a.length, b.length <= 104`
- `a` and `b` consist only of `'0'` or `'1'` characters.
- Each string does not contain leading zeros except for the zero itself.



```c++
class Solution {
public:
    string addBinary(string a, string b) {
        vector<int> num1_lst, num2_lst, res;
        for (int i = a.size() - 1; i >= 0; i--) { num1_lst.push_back(a[i] - '0'); }
        for (int i = b.size() - 1; i >= 0; i--) { num2_lst.push_back(b[i] - '0'); }

        int tmp = 0;
        for (int i = 0; i < a.size() || i < b.size(); i++) {
            if (i < a.size()) { tmp += num1_lst[i]; }
            if (i < b.size()) { tmp += num2_lst[i]; }
            res.push_back(tmp % 2); // Binary
            tmp /= 2;
        }
        if (tmp) { res.push_back(1); }
        
        string ans;
        for (int i = res.size() - 1; i >= 0; i--) { ans.push_back(res[i] + '0'); }
        return ans;
    }
};
```

Other Solutions:

```java
import java.math.BigInteger;
class Solution {
    public String addBinary(String a, String b) {
        return new BigInteger(a, 2).add(new BigInteger(b, 2)).toString(2);
    }
}
```

```python
class Solution:
    def addBinary(self, a, b) -> str:
        return '{0:b}'.format(int(a, 2) + int(b, 2))
```

Bit Manipulation is also applied to this question.

## Subtraction

```c++
/* Non-negative Subtraction Template */
#include <iostream>
#include <vector>
using namespace std;

/* Whether num1 >= num2 or not. */
bool cmp(const vector<int> &num1_lst, const vector<int> &num2_lst) {
    if (num1_lst.size() > num2_lst.size()) { return true; }
    else if (num1_lst.size() < num2_lst.size()) { return false; }
    for (int i = num1_lst.size() - 1; i >= 0; i--) { // Equal length. From high to low digits.
        if (num1_lst[i] != num2_lst[i]) { return num1_lst[i] > num2_lst[i]; }
    }
    return true;
}

vector<int> sub(const vector<int> &a, const vector<int> &b) {
    vector<int> res;
    int tmp = 0; // Borrow
    for (int i = 0; i < a.size(); i++) { // a >= b, so i < b.size() is no need.
        tmp = a[i] - tmp;
        if (i < b.size()) { tmp -= b[i]; }
        res.push_back((tmp + 10) % 10);
        if (tmp < 0) { tmp = 1; }
        else { tmp = 0; }
    }
    /* Delete leading zeros. */
    while (res.size() > 1 && res.back() == 0) { res.pop_back(); }
    return res;
}

int main() {
    /* Input */
    string num1, num2;
    cin >> num1 >> num2;
    vector<int> num1_lst, num2_lst; // Reverse Order. e.g. "123" -> [3, 2, 1]
    for (int i = num1.size() - 1; i >= 0; i--) { num1_lst.push_back(num1[i] - '0'); }
    for (int i = num2.size() - 1; i >= 0; i--) { num2_lst.push_back(num2[i] - '0'); }

    /* Calculation */
    vector<int> result;
    bool negative = false;
    if (cmp(num1_lst, num2_lst)) { result = sub(num1_lst, num2_lst); }
    else {
        negative = true;
        result = sub(num2_lst, num1_lst);
    }

    /* Output */
    if (negative) { cout << "-"; }
    for (int i = result.size() - 1; i >= 0; i--) { printf("%d", result[i]); }
    cout << endl;
    return 0;
}
```



## Multiplication

### Multiplication Template

```c++
#include <iostream>
#include <vector>

using namespace std;

/* A non-negative big number(string) times a non-negative small number(int). */
vector<int> mul(const vector<int> &a, int b) {
    vector<int> res;
    int tmp = 0; // Carry
    for (int i = 0; i < a.size() || tmp; i++) {
        if (i < a.size()) { tmp += a[i] * b; }
        res.push_back(tmp % 10);
        tmp /= 10;
    }
    return res;
}

int main() {
    /* Input */
    string num1;
    int num2;
    cin >> num1 >> num2;
    vector<int> num1_lst; // Reverse Order. e.g. "123" -> [3, 2, 1]
    for (int i = num1.size() - 1; i >= 0; i--) { num1_lst.push_back(num1[i] - '0'); }

    /* Calculation */
    vector<int> result = mul(num1_lst, num2);

    /* Output */
    for (int i = result.size() - 1; i >= 0; i--) { printf("%d", result[i]); }
    cout << endl;
    return 0;
}
```

### LeetCode 43. Multiply Strings

[LeetCode 43. Multiply Strings](https://leetcode.cn/problems/multiply-strings/)

Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also represented as a string.

**Note:** You must not use any built-in BigInteger library or convert the inputs to integer directly.

 

**Example 1:**

```
Input: num1 = "2", num2 = "3"
Output: "6"
```

**Example 2:**

```
Input: num1 = "123", num2 = "456"
Output: "56088"
```

 

**Constraints:**

- `1 <= num1.length, num2.length <= 200`
- `num1` and `num2` consist of digits only.
- Both `num1` and `num2` do not contain any leading zero, except the number `0` itself.



```c++
class Solution {
public:
    string multiply(string num1, string num2) {
        if (num1 == "0" || num2 == "0") { return "0"; }
        vector<int> num1_lst, num2_lst;
        for (int i = num1.size() - 1; i >= 0; i--) { num1_lst.push_back(num1[i] - '0'); }
        for (int i = num2.size() - 1; i >= 0; i--) { num2_lst.push_back(num2[i] - '0'); }
        vector<int> res = {0};
        for (int i = 0; i < num2.size(); i++) {
            vector<int> num = mul(num1_lst, num2_lst[i]);
            reverse(num.begin(), num.end());
            for (int j = 0; j < i; j++) { num.push_back(0); }
            reverse(num.begin(), num.end());
            res = add(res, num);
        }
        string ans = "";
        for (int i = res.size() - 1; i >= 0; i--) { ans += res[i] + '0'; }
        return ans;
    }

    /* Multiplication Template */
    vector<int> mul(const vector<int> &a, int b) {
        vector<int> res;
        int tmp = 0; // Carry
        for (int i = 0; i < a.size() || tmp; i++) {
            if (i < a.size()) { tmp += a[i] * b; }
            res.push_back(tmp % 10);
            tmp /= 10;
        }
        return res;
    }

    /* Addition Template */
    vector<int> add(const vector<int> &num1_lst, const vector<int> &num2_lst) {
        vector<int> res;
        int tmp = 0;
        for (int i = 0; i < num1_lst.size() || i < num2_lst.size(); i++) {
            if (i < num1_lst.size()) { tmp += num1_lst[i]; }
            if (i < num2_lst.size()) { tmp += num2_lst[i]; }
            res.push_back(tmp % 10);
            tmp /= 10;
        }
        if (tmp) { res.push_back(1); }
        return res;
    }
};
```



## Division

### Division Template

```c++
#include <iostream>
#include <vector>
#include <algorithm> // For reverse().

using namespace std;

vector<int> div(const vector<int> &a, int b, int &r) { // r is the remainder.
    r = 0;
    vector<int> quotient;
    for (int i = a.size() - 1; i >= 0; i--) { // Division. From high to low digits.
        r = r * 10 + a[i];
        quotient.push_back(r / b);
        r %= b;
    }
    reverse(quotient.begin(), quotient.end());
    
    /* Delete leading zeros. */
    while (quotient.size() > 1 && quotient.back() == 0) { quotient.pop_back(); }
    return quotient;
}

int main() {
    /* Input */
    string num1;
    int num2, remainder;
    cin >> num1 >> num2;
    vector<int> num1_lst; // Reverse Order. e.g. "123" -> [3, 2, 1]
    for (int i = num1.size() - 1; i >= 0; i--) { num1_lst.push_back(num1[i] - '0'); }

    /* Calculation */
    vector<int> result = div(num1_lst, num2, remainder);

    /* Output */
    for (int i = result.size() - 1; i >= 0; i--) { printf("%d", result[i]); }
    cout << endl << remainder << endl;
    return 0;
}
```

### LeetCode 29. Divide Two Integers

[LeetCode 29. Divide Two Integers](https://leetcode.cn/problems/divide-two-integers/)

Given two integers `dividend` and `divisor`, divide two integers **without** using multiplication, division, and mod operator.

The integer division should truncate toward zero, which means losing its fractional part. For example, `8.345` would be truncated to `8`, and `-2.7335`would be truncated to `-2`.

Return *the **quotient** after dividing* `dividend` *by* `divisor`.

**Note:** Assume we are dealing with an environment that could only store integers within the **32-bit** signed integer range: `[−231, 231 − 1]`. For this problem, if the quotient is **strictly greater than** `231 - 1`, then return `231 - 1`, and if the quotient is **strictly less than** `-231`, then return `-231`.

 

**Example 1:**

```
Input: dividend = 10, divisor = 3
Output: 3
Explanation: 10/3 = 3.33333.. which is truncated to 3.
```

**Example 2:**

```
Input: dividend = 7, divisor = -3
Output: -2
Explanation: 7/-3 = -2.33333.. which is truncated to -2.
```

 

**Constraints:**

- `-231 <= dividend, divisor <= 231 - 1`
- `divisor != 0`



(These solutions did't use the division template.)

Solution 1 (long is allowed):

```java
class Solution {
    int INF = Integer.MAX_VALUE;
    public int divide(int _a, int _b) {
        long a = _a, b = _b;
        boolean flag = false;
        if ((a < 0 && b > 0) || (a > 0 && b < 0)) flag = true;
        if (a < 0) a = -a;
        if (b < 0) b = -b;
        long l = 0, r = a;
        while (l < r) {
            long mid = l + r + 1 >> 1;
            if (mul(mid, b) <= a) l = mid;
            else r = mid - 1;
        }
        r = flag ? -r : r;
        if (r > INF || r < -INF - 1) return INF;
        return (int)r;
    }
    long mul(long a, long k) {
        long ans = 0;
        while (k > 0) {
            if ((k & 1) == 1) ans += a;
            k >>= 1;
            a += a;
        }
        return ans;
    }
}

/* 
https://leetcode.cn/problems/divide-two-integers/solutions/1042784/gong-shui-san-xie-dui-xian-zhi-tiao-jian-utb9/
*/
```

Solution 2 (long is prohibited):

```java
class Solution {
    int MIN = Integer.MIN_VALUE, MAX = Integer.MAX_VALUE;
    int LIMIT = -1073741824; // MIN 的一半
    public int divide(int a, int b) {
        if (a == MIN && b == -1) return MAX;
        boolean flag = false;
        if ((a > 0 && b < 0) || (a < 0 && b > 0)) flag = true;
        if (a > 0) a = -a;
        if (b > 0) b = -b;
        int ans = 0;
        while (a <= b){
            int c = b, d = -1;
            while (c >= LIMIT && d >= LIMIT && c >= a - c){
                c += c; d += d;
            }
            a -= c;
            ans += d;
        }
        return flag ? ans : -ans;
    }
}

/* 
https://leetcode.cn/problems/divide-two-integers/solutions/1042784/gong-shui-san-xie-dui-xian-zhi-tiao-jian-utb9/
*/
```

