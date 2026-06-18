# Day 5: Normal Distribution I
 
## Objective

In this challenge, we learn about normal distributions.
 
## Task

In a certain plant, the time taken to assemble a car is a random variable, `X`, having a normal distribution with a mean of `20` hours and a standard deviation of `2` hours. What is the probability that a car can be assembled at this plant in:

1. Less than `19.5` hours?
2. Between `20` and `22` hours?

## Input Format

There are `3` lines of input (shown below):

```
20 2
19.5
20 22
```

The first line contains `2` space-separated values denoting the respective mean and standard deviation for `X`. The second line contains the number associated with question 1. The third line contains `2` space-separated values describing the respective lower and upper range boundaries for question 2.

If you do not wish to read this information from stdin, you can hard-code it into your program.

## Output Format

There are two lines of output. Your answers must be rounded to a scale of 3 decimal places (i.e., `1.234` format):

1. On the first line, print the answer to question 1 (i.e., the probability that a car can be assembled in less than `19.5` hours).
2. On the second line, print the answer to question 2 (i.e., the probability that a car can be assembled in between `20` to `22` hours).

## Sample Output

```
0.401
0.341
```

## Explanation

This is a **normal distribution** problem. The cumulative distribution function (CDF) gives the probability that `X` is less than or equal to some value `x`:

```
P(X < x) = (1/2) * (1 + erf((x - mu) / (sigma * sqrt(2))))
```

where:

- `mu` — the mean (here, `20`)
- `sigma` — the standard deviation (here, `2`)
- `erf` — the error function

**Question 1 — `P(X < 19.5)`:**

```
P(X < 19.5) = (1/2) * (1 + erf((19.5 - 20) / (2 * sqrt(2)))) ≈ 0.401
```

**Question 2 — `P(20 < X < 22)`** is the difference of two CDF values:

```
P(20 < X < 22) = P(X < 22) - P(X < 20)
              ≈ 0.841 - 0.500
              ≈ 0.341
```

## Solutions

### JavaScript (Node.js)

```javascript
function processData(input) {
    const lines = input.split('\n');
    const [mean, stdDev] = lines[0].split(' ').map(Number);
    const limit1 = parseFloat(lines[1]);
    const [limit2Low, limit2High] = lines[2].split(' ').map(Number);

    const cdf = (x, mean, stdDev) => {
        return 0.5 * (1 + erf((x - mean) / (stdDev * Math.sqrt(2))));
    };

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

    const result1 = cdf(limit1, mean, stdDev);
    const result2 = cdf(limit2High, mean, stdDev) - cdf(limit2Low, mean, stdDev);

    process.stdout.write(result1.toFixed(3) + "\n");
    process.stdout.write(result2.toFixed(3) + "\n");
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
from math import erf, sqrt


def cdf(x, mean, std):
    return 0.5 * (1 + erf((x - mean) / (std * sqrt(2))))


mean, std = map(float, input().split())
limit1 = float(input())
low, high = map(float, input().split())

print(round(cdf(limit1, mean, std), 3))
print(round(cdf(high, mean, std) - cdf(low, mean, std), 3))
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
        double limit1 = sc.nextDouble();
        double low = sc.nextDouble();
        double high = sc.nextDouble();

        System.out.printf("%.3f%n", cdf(limit1, mean, std));
        System.out.printf("%.3f%n", cdf(high, mean, std) - cdf(low, mean, std));
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
    double mean, std, limit1, low, high;
    cin >> mean >> std >> limit1 >> low >> high;

    cout << fixed << setprecision(3) << cdf(limit1, mean, std) << "\n";
    cout << fixed << setprecision(3) << (cdf(high, mean, std) - cdf(low, mean, std)) << "\n";
    return 0;
}
```

### How the Solution Works

1. **Read input** — the mean and standard deviation, the single boundary for question 1, and the two boundaries for question 2.
2. **CDF** — the normal CDF, `P(X < x) = (1/2) * (1 + erf((x - mu) / (sigma * sqrt(2))))`, gives the probability of being below a value.
3. **Question 1** — directly evaluate the CDF at `19.5`.
4. **Question 2** — a range probability is `CDF(high) - CDF(low)`.
5. **Print** each result rounded to 3 decimal places.

> **Note:** JavaScript has no built-in error function, so the solution implements `erf` with a numerical approximation. Python (`math.erf`) and C++ (`erf`) provide it natively, which keeps those versions much shorter.
