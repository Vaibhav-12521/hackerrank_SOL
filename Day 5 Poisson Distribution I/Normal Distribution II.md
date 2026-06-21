# Day 5: Normal Distribution II

## Objective

In this challenge, we go further with normal distributions. We recommend reviewing the previous challenge's Tutorial before attempting this problem.

## Task

The final grades for a Physics exam taken by a large group of students have a mean of `μ = 70` and a standard deviation of `σ = 10`. If we can approximate the distribution of these grades by a normal distribution, what percentage of the students:

1. Scored higher than `80` (i.e., have a `grade > 80`)?
2. Passed the test (i.e., have a `grade >= 60`)?
3. Failed the test (i.e., have a `grade < 60`)?

Find and print the answer to each question on a new line, rounded to a scale of 2 decimal places.

## Input Format

There are `3` lines of input (shown below):

```
70 10
80
60
```

The first line contains `2` space-separated values denoting the respective mean and standard deviation for the exam. The second line contains the number associated with question 1. The third line contains the pass/fail threshold number associated with questions 2 and 3.

If you do not wish to read this information from stdin, you can hard-code it into your program.

## Output Format

There are three lines of output. Your answers must be rounded to a scale of 2 decimal places (i.e., `1.23` format):

1. On the first line, print the answer to question 1 (i.e., the percentage of students having `grade > 80`).
2. On the second line, print the answer to question 2 (i.e., the percentage of students having `grade >= 60`).
3. On the third line, print the answer to question 3 (i.e., the percentage of students having `grade < 60`).

## Sample Output

```
15.87
84.13
15.87
```

## Explanation

This is a **normal distribution** problem. The cumulative distribution function (CDF) gives the probability that a grade is **at most** `x`:

```
P(grade < x) = (1/2) * (1 + erf((x - mu) / (sigma * sqrt(2))))
```

We convert each probability to a percentage by multiplying by `100`.

**Question 1 — `grade > 80`** is the complement of the CDF:

```
P(grade > 80) = 1 - CDF(80) ≈ 1 - 0.8413 ≈ 0.1587  →  15.87%
```

**Question 2 — `grade >= 60`** is also the complement of the CDF:

```
P(grade >= 60) = 1 - CDF(60) ≈ 1 - 0.1587 ≈ 0.8413  →  84.13%
```

**Question 3 — `grade < 60`** is the CDF directly:

```
P(grade < 60) = CDF(60) ≈ 0.1587  →  15.87%
```

(Notice questions 2 and 3 are complements: `84.13% + 15.87% = 100%`.)

## Solutions

### JavaScript (Node.js)

```javascript
function processData(input) {
    const lines = input.split('\n');
    const [mean, stdDev] = lines[0].split(' ').map(Number);
    const higherThan = Number(lines[1]);
    const passMark = Number(lines[2]);

    function erf(z) {
        const t = 1.0 / (1.0 + 0.5 * Math.abs(z));
        const ans = 1 - t * Math.exp(-z * z - 1.26551223 +
            t * (1.00002368 +
            t * (0.37409196 +
            t * (0.09678418 +
            t * (-0.18628806 +
            t * (0.27886807 +
            t * (-1.13520398 +
            t * (1.48851587 +
            t * (-0.82215223 +
            t * 0.17087277)))))))));
        return z >= 0 ? ans : -ans;
    }

    function cdf(x, m, s) {
        return 0.5 * (1 + erf((x - m) / (s * Math.sqrt(2))));
    }

    const p1 = (1 - cdf(higherThan, mean, stdDev)) * 100;
    const p2 = (1 - cdf(passMark, mean, stdDev)) * 100;
    const p3 = cdf(passMark, mean, stdDev) * 100;

    console.log(p1.toFixed(2));
    console.log(p2.toFixed(2));
    console.log(p3.toFixed(2));
}

process.stdin.resume();
process.stdin.setEncoding("ascii");
let _input = "";
process.stdin.on("data", function (input) {
    _input += input;
});
process.stdin.on("end", function () {
    processData(_input);
});
```

### Python 3

```python
from math import erf, sqrt


def cdf(x, mean, std):
    return 0.5 * (1 + erf((x - mean) / (std * sqrt(2))))


mean, std = map(float, input().split())
higher_than = float(input())
pass_mark = float(input())

print(round((1 - cdf(higher_than, mean, std)) * 100, 2))
print(round((1 - cdf(pass_mark, mean, std)) * 100, 2))
print(round(cdf(pass_mark, mean, std) * 100, 2))
```

### Java

```java
import java.util.*;

public class Solution {
    static double erf(double z) {
        double t = 1.0 / (1.0 + 0.5 * Math.abs(z));
        double ans = 1 - t * Math.exp(-z * z - 1.26551223 +
            t * (1.00002368 +
            t * (0.37409196 +
            t * (0.09678418 +
            t * (-0.18628806 +
            t * (0.27886807 +
            t * (-1.13520398 +
            t * (1.48851587 +
            t * (-0.82215223 +
            t * 0.17087277)))))))));
        return z >= 0 ? ans : -ans;
    }

    static double cdf(double x, double mean, double std) {
        return 0.5 * (1 + erf((x - mean) / (std * Math.sqrt(2))));
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        double mean = sc.nextDouble();
        double std = sc.nextDouble();
        double higherThan = sc.nextDouble();
        double passMark = sc.nextDouble();

        System.out.printf("%.2f%n", (1 - cdf(higherThan, mean, std)) * 100);
        System.out.printf("%.2f%n", (1 - cdf(passMark, mean, std)) * 100);
        System.out.printf("%.2f%n", cdf(passMark, mean, std) * 100);
    }
}
```

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

double cdf(double x, double mean, double std) {
    return 0.5 * (1 + erf((x - mean) / (std * sqrt(2))));
}

int main() {
    double mean, std, higherThan, passMark;
    cin >> mean >> std >> higherThan >> passMark;

    cout << fixed << setprecision(2) << (1 - cdf(higherThan, mean, std)) * 100 << "\n";
    cout << fixed << setprecision(2) << (1 - cdf(passMark, mean, std)) * 100 << "\n";
    cout << fixed << setprecision(2) << cdf(passMark, mean, std) * 100 << "\n";
    return 0;
}
```

### How the Solution Works

1. **Read input** — the mean and standard deviation, the high-score threshold (question 1), and the pass/fail threshold (questions 2 and 3).
2. **CDF** — the normal CDF gives `P(grade < x)`.
3. **Greater than** — `P(grade > x) = 1 - CDF(x)`, used for questions 1 and 2.
4. **Less than** — `P(grade < x) = CDF(x)`, used for question 3.
5. **Print** each probability as a percentage (`× 100`), rounded to 2 decimal places.

> **Note:** JavaScript and Java have no built-in error function, so the solution implements `erf` with a numerical approximation. Python (`math.erf`) and C++ (`erf`) provide it natively.
