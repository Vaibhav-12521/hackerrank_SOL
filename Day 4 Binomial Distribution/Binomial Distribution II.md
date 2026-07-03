# Day 4: Binomial Distribution II

## Objective

In this challenge, we go further with binomial distributions. We recommend reviewing the previous challenge's Tutorial before attempting this problem.

## Task

A manufacturer of metal pistons finds that, on average, `12%` of the pistons they manufacture are rejected because they are incorrectly sized. What is the probability that a batch of `10` pistons will contain:

1. No more than `2` rejects?
2. At least `2` rejects?

## Input Format

A single line containing the following values (denoting the respective percentage of defective pistons and the size of the current batch of pistons):

```
12 10
```

If you do not wish to read this information from stdin, you can hard-code it into your program.

## Output Format

Print the answer to each question on its own line:

1. The first line should contain the probability that a batch of `10` pistons will contain **no more than 2** rejects.
2. The second line should contain the probability that a batch of `10` pistons will contain **at least 2** rejects.

Round both of your answers to a scale of 3 decimal places (i.e., `1.234` format).

## Sample Output

```
0.891
0.342
```

## Explanation

This is a **binomial distribution** problem. Each piston is an independent trial, rejected with probability `p = 0.12`, in a batch of `n = 10`. The probability of exactly `x` rejects is:

```
B(x; n, p) = C(n, x) * p^x * (1 - p)^(n - x)
```

**Question 1 — no more than 2 rejects** means `x = 0, 1, 2`:

```
P(X <= 2) = sum_{x=0}^{2} C(10, x) * p^x * (1 - p)^(10 - x) ≈ 0.891
```

**Question 2 — at least 2 rejects** means `x = 2, 3, ..., 10`:

```
P(X >= 2) = sum_{x=2}^{10} C(10, x) * p^x * (1 - p)^(10 - x) ≈ 0.342
```

Each answer is rounded to 3 decimal places.

## Solutions

### JavaScript (Node.js)

```javascript
function factorial(n) {
    return n <= 1 ? 1 : n * factorial(n - 1);
}

function combinations(n, k) {
    return factorial(n) / (factorial(k) * factorial(n - k));
}

function binomial(n, k, p) {
    return combinations(n, k) * Math.pow(p, k) * Math.pow(1 - p, n - k);
}

function processData(input) {
    const lines = input.split(' ');
    const p = parseInt(lines[0]) / 100;
    const n = parseInt(lines[1]);

    let prob1 = 0;
    for (let i = 0; i <= 2; i++) {
        prob1 += binomial(n, i, p);
    }
    console.log(prob1.toFixed(3));

    let prob2 = 0;
    for (let i = 2; i <= n; i++) {
        prob2 += binomial(n, i, p);
    }
    console.log(prob2.toFixed(3));
}

process.stdin.resume();
process.stdin.setEncoding("ascii");
_input = "";
process.stdin.on("data", function (input) {
    _input += input;
});

process.stdin.on("end", function () {
    processData(_input);
});
```

### Python 3

```python
from math import comb


def binomial(n, k, p):
    return comb(n, k) * (p ** k) * ((1 - p) ** (n - k))


pct, n = map(int, input().split())
p = pct / 100

prob1 = sum(binomial(n, x, p) for x in range(0, 3))
prob2 = sum(binomial(n, x, p) for x in range(2, n + 1))

print(round(prob1, 3))
print(round(prob2, 3))
```

### Java

```java
import java.util.*;

public class Solution {
    static long fact(int num) {
        long result = 1;
        for (int i = 2; i <= num; i++) result *= i;
        return result;
    }

    static double combinations(int n, int k) {
        return (double) fact(n) / (fact(k) * fact(n - k));
    }

    static double binomial(int n, int k, double p) {
        return combinations(n, k) * Math.pow(p, k) * Math.pow(1 - p, n - k);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        double p = sc.nextInt() / 100.0;
        int n = sc.nextInt();

        double prob1 = 0;
        for (int i = 0; i <= 2; i++) prob1 += binomial(n, i, p);
        System.out.printf("%.3f%n", prob1);

        double prob2 = 0;
        for (int i = 2; i <= n; i++) prob2 += binomial(n, i, p);
        System.out.printf("%.3f%n", prob2);
    }
}
```

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

long long fact(int num) {
    long long result = 1;
    for (int i = 2; i <= num; i++) result *= i;
    return result;
}

double combinations(int n, int k) {
    return (double) fact(n) / (fact(k) * fact(n - k));
}

double binomial(int n, int k, double p) {
    return combinations(n, k) * pow(p, k) * pow(1 - p, n - k);
}

int main() {
    int pct, n;
    cin >> pct >> n;
    double p = pct / 100.0;

    double prob1 = 0;
    for (int i = 0; i <= 2; i++) prob1 += binomial(n, i, p);
    cout << fixed << setprecision(3) << prob1 << "\n";

    double prob2 = 0;
    for (int i = 2; i <= n; i++) prob2 += binomial(n, i, p);
    cout << fixed << setprecision(3) << prob2 << "\n";
    return 0;
}
```

### How the Solution Works

1. **Read input** - The defect percentage becomes a probability `p = pct / 100`, with batch size `n`.
2. **Binomial helper** - `binomial(n, k, p) = C(n, k) * p^k * (1 - p)^(n - k)` gives the probability of exactly `k` rejects.
3. **No more than 2 rejects** - Sum the binomial probabilities for `x = 0, 1, 2`.
4. **At least 2 rejects** - Sum the binomial probabilities for `x = 2, 3, ..., n`.
5. **Print** each result on its own line, rounded to 3 decimal places.