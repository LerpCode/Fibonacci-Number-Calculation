# Fibonacci Number Calculation


This repository contains various methods to calculate the nth Fibonacci number using C++. Considered using bigInt.


Written by Lap Nguyen.

<details>
<summary><span style="font-size: 24px">Demonstration of bigInt:</span></summary>

```c++
#include <vector>
#include <string>
#include <iostream>
#include <cstdio>
using namespace std;

const int BASE = 10000;
typedef vector <int> BigInt;
BigInt Init(string s) {
    BigInt a; if (s.size() == 0) { a.push_back(0); return a; }
    while (s.size() % 4 != 0) s = '0' + s;
    for (int i = 0; i < (int)s.size(); i += 4) {
        int value = 0;
        for (int j = 0; j < 4; j++)
            value = value * 10 + (s[i + j] - '0');
        a.insert(a.begin(), value);
    } return a;
}
void Print(const BigInt a) {
    int L = a.size(); printf("%d", a[L - 1]);
    for (int i = L - 2; i >= 0; i--) printf("%04d", a[i]);
    printf("\n");
}
BigInt operator + (const BigInt a, const BigInt b) {
    BigInt c; int carry = 0;
    for (int i = 0; i < (int)a.size() || i < (int)b.size(); i++) {
        if (i < (int)a.size()) carry += a[i];
        if (i < (int)b.size()) carry += b[i];
        c.push_back(carry % BASE); carry /= BASE;
    }
    if (carry) c.push_back(carry); return c;
}
BigInt operator - (const BigInt a, const BigInt b) {
    BigInt c; int carry = 0;
    for (int i = 0; i < (int)a.size(); i++) {
        int s = a[i] - carry;
        if (i < (int)b.size()) s -= b[i];
        if (s < 0) s += BASE, carry = 1; else carry = 0;
        c.push_back(s);
    }
    while (*c.rbegin() == 0 && c.size() > 1) c.pop_back(); return c;
}
BigInt operator * (const BigInt a, const int mul) {
    BigInt c; int carry = 0;
    for (int i = 0; i < (int)a.size(); i++) {
        carry += a[i] * mul; c.push_back(carry % BASE); carry /= BASE;
    }
    if (carry) c.push_back(carry); return c;
}
BigInt operator * (const BigInt a, const BigInt b) {
    BigInt c(a.size() + b.size() + 5, 0);
    for (int i = 0; i < (int)a.size(); i++) {
        int carry = 0;
        for (int j = 0; j < (int)b.size(); j++) {
            int k = i + j; c[k] += a[i] * b[j] + carry;
            carry = c[k] / BASE; c[k] %= BASE;
        }
        if (carry) c[i + b.size()] += carry;
    }
    while (*c.rbegin() == 0 && c.size() > 1) c.pop_back(); return c;
}
bool operator < (BigInt a, BigInt b) {
    while (a.size() && *a.rbegin() == 0) a.pop_back();
    while (b.size() && *b.rbegin() == 0) b.pop_back();
    if (a.size() != b.size()) return a.size() < b.size();
    for (int i = a.size() - 1; i >= 0; i--)
        if (a[i] != b[i]) return a[i] < b[i];
    return false;
}
```
</details>




## Table of Contents
1. [Recursive Approach](#recursive-approach)
2. [Dynamic Programming - Memoization](#dynamic-programming---memoization)
3. [Dynamic Programming - Bottom-Up Approach](#dynamic-programming---bottom-up-approach)
4. [Iterative Approach](#iterative-approach)
5. [Matrix Exponentiation - Fast Exponentation](#matrix-exponentiation)
6. [Using Binet's Formula](#using-binets-formula)

---

### Recursive Approach

The simplest way to calculate Fibonacci numbers, but inefficient for large `n` due to exponential time complexity $O(2 ^ n)$


```cpp
#include <iostream>
using namespace std;

BigInt fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}

int main() {
    BigInt n;
    cin >> n;
    cout << fibonacci(n);
    return 0;
}
```
___

### Dynamic Programming - Memoization
Memoization is a dynamic programming technique that speeds up recursive algorithms by storing and reusing previously computed results, avoiding redundant calculations. It's effective for problems with overlapping subproblems and optimal substructure.The time complexity of this program is $O(n^2)$.

```cpp
#include <iostream>
#include <vector>

using namespace std;

BigInt fibonacci(BigInt n, vector<BigInt>& memo) {
    if (n <= 1) return n;
    if (memo[n] != -1) return memo[n];
    memo[n] = fibonacci(n - 1, memo) + fibonacci(n - 2, memo);
    return memo[n];
}

int main() {
    BigInt n;
    cin >> n;
    vector<BigInt> memo(n + 1, -1);
    cout << fibonacci(n, memo);

    return 0;
}
```
_____
### Dynamic Programming - Bottom-Up Approach
The bottom-up approach for calculating Fibonacci numbers involves iteratively computing each Fibonacci number from the ground up, starting from the base cases and building up to the desired value. This method is efficient and avoids the overhead of recursive calls.The complexity of this program $O(n)$ 

```cpp
#include <iostream>
#include <vector>

using namespace std;

BigInt fibonacci(BigInt n) {
    if (n <= 1) return n;
    vector<BigInt> dp(n + 1);
    dp[0] = 0;
    dp[1] = 1;
    for (BigInt i = 2; i <= n; ++i) {
        dp[i] = dp[i - 1] + dp[i - 2];
    }
    return dp[n];
}

int main() {
    BigInt n;
    cin >> n;
    cout << fibonacci(n);
    return 0;
}
```
___
### Iterative Approach
An iterative approach solves a problem by repeatedly applying a set of steps until a desired outcome is reached. It starts with an initial solution, refines it through successive iterations, and converges to the final answer. Unlike recursive methods, iterative solutions typically use loops and avoid function calls, often leading to better performance for large problem sizes.
```cpp
#include <iostream>
BigInt fibonacci(BigInt n) {
    if (n <= 1) {
        return BigInt(n);
    }

    BigInt fib_prev(0), fib_next(1), fib_curr;

    for (BigInt i = 2; i <= n; ++i) {
        fib_curr = fib_prev + fib_next;
        fib_prev = fib_next;
        fib_next = fib_curr;
    }

    return fib_curr;
}

int main() {
    BigInt n;
    cin >> n;

    cout << fibonacci(n);
}
```
___
### Matrix Exponentiation - Fast Exponentation

```math
\begin{bmatrix}
F(n) \\
F(n-1)
\end{bmatrix}
=
\begin{bmatrix}
1 & 1 \\
1 & 0
\end{bmatrix}^{n-1}
\begin{bmatrix}
F(1) \\
F(0)
\end{bmatrix}
```

Matrix exponentiation with fast exponentiation (exponentiation by squaring) efficiently computes the matrix raised to a power 
`n`. Start by initializing the result matrix as the identity matrix. Then, while the exponent is positive, if the exponent is odd, multiply the result by the matrix. Regardless of the exponent’s parity, square the matrix and halve the exponent. This method reduces the time complexity to $O(logn)$ by breaking down the problem into smaller, manageable steps.

```cpp
#include <iostream>
#include <vector>

using namespace std;

typedef vector<vector<BigInt>> Matrix;

Matrix multiply(Matrix A, Matrix B) {
    int n = A.size();
    Matrix C(n, vector<BigInt>(n, BigInt("0")));
    for (int i = 0; i < n; ++i) {
        for (int j = 0; j < n; ++j) {
            for (int k = 0; k < n; ++k) {
                C[i][j] = C[i][j] + A[i][k] * B[k][j];
            }
        }
    }
    return C;
}

Matrix power(Matrix M, BigInt p) {
    BigInt n = M.size();
    Matrix result(n, vector<BigInt>(n, BigInt("0")));
    for (int i = 0; i < n; ++i) {
        result[i][i] = BigInt("1");
    }

    while (p > 0) {
        if (p % 2 == 1) {
            result = multiply(result, M);
        }
        M = multiply(M, M);
        p /= 2;
    }
    return result;
}

BigInt fibonacci(int n) {
    if (n == 0) return BigInt("0");
    if (n == 1) return BigInt("1");

    Matrix F = {{BigInt("1"), BigInt("1")}, {BigInt("1"), BigInt("0")}};
    Matrix result = power(F, n - 1);
    return result[0][0];
}

int main() {
    BigInt n;
    cin >> n;
    cout << fibonacci(n);
    return 0;
}
```
___
### Binet's Formula

```math
F(n) = \frac{\phi^n - \psi^n}{\sqrt{5}}
```
where
```math
\phi = \frac{1 + \sqrt{5}}{2}
 ,\psi = \frac{1 - \sqrt{5}}{2}
```

Binet's Formula computes Fibonacci numbers directly using a closed-form expression involving irrational numbers. Although it provides immediate results, it suffers from precision issues due to floating-point arithmetic for large `n`. This method is less efficient and accurate compared to iterative or matrix exponentiation methods.
​
```cpp
#include <iostream>
#include <cmath>

using namespace std;

BigInt binetFibonacci(int n) {
    if (n < 0) return BigInt("0");
    if (n == 0) return BigInt("0");
    if (n == 1) return BigInt("1");

    double phi = (1 + sqrt(5)) / 2.0;
    double psi = (1 - sqrt(5)) / 2.0;

    double fib = (pow(phi, n) - pow(psi, n)) / sqrt(5);

    return BigInt(to_string(round(fib)));
}

int main() {
    BigInt n;
    cin >> n;

    cout << binetFibonacci(n);


    return 0;
}
```
____
## Source:

BigInt: https://vietcodes.github.io/code/78/index.html

## MIT License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
