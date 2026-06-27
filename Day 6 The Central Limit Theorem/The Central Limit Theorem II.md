# Day 6: The Central Limit Theorem II
       
## Objective  

In this challenge, we practice solving problems based on the Central Limit Theorem. We recommend reviewing the Central Limit Theorem Tutorial before attempting this challenge.

## Task    

The number of tickets purchased by each student for the University X vs. University Y football game follows a distribution that has a mean of `μ = 2.4` and a standard deviation of `σ = 2.0`.   

A few hours before the game starts, `100` eager students line up to purchase last-minute tickets. If there are only `250` tickets left, what is the probability that all `100` students will be able to purchase tickets?
 
## Input Format

There are `4` lines of input (shown below):

```
250
100
2.4
2.0
```

The first line contains the number of last-minute tickets available at the box office. The second line contains the number of students waiting to buy tickets. The third line contains the mean number of purchased tickets, and the fourth line contains the standard deviation.

If you do not wish to read this information from stdin, you can hard-code it into your program.

## Output Format

Print the probability that `100` students can successfully purchase the remaining `250` tickets, rounded to a scale of 4 decimal places (i.e., `1.2345` format).

## Sample Output

```
0.6911
```

## Explanation

This is a **Central Limit Theorem (CLT)** problem. The total number of tickets the 100 students want is the sum of 100 independent samples, which by the CLT is approximately normal with:

```
mean of the sum:  μ_sum = n * μ
std dev of sum:   σ_sum = sqrt(n) * σ
```

We want the probability that this total demand is at most the `250` tickets available.

**Step 1 — distribution of the total demand:**

```
μ_sum = 100 * 2.4 = 240
σ_sum = sqrt(100) * 2.0 = 10 * 2.0 = 20
```

**Step 2 — evaluate the normal CDF** at the ticket limit (`250`):

```
P(X <= 250) = (1/2) * (1 + erf((250 - 240) / (20 * sqrt(2)))) ≈ 0.6911
```

Rounded to 4 decimal places, the answer is `0.6911`.

## Solutions

### Java

```java
import java.util.*;

public class Solution {

    public static double phi(double x, double mean, double std) {
        return 0.5 * (1 + erf((x - mean) / (std * Math.sqrt(2))));
    }

    public static double erf(double z) {
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
                t * (0.17087277))))))))));
        if (z >= 0) return ans;
        else return -ans;
    }

    public static void main(String[] args) {
        double maxTickets = 250;
        double n = 100;
        double mean = 2.4;
        double std = 2.0;

        double sumMean = n * mean;
        double sumStd = Math.sqrt(n) * std;

        double result = phi(maxTickets, sumMean, sumStd);
        System.out.printf("%.4f%n", result);
    }
}
```

### JavaScript (Node.js)

```javascript
function processData(input) {
    const lines = input.split('\n');
    const maxTickets = parseFloat(lines[0]);
    const n = parseFloat(lines[1]);
    const mean = parseFloat(lines[2]);
    const std = parseFloat(lines[3]);

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

    function phi(x, m, s) {
        return 0.5 * (1 + erf((x - m) / (s * Math.sqrt(2))));
    }

    const sumMean = n * mean;
    const sumStd = Math.sqrt(n) * std;

    console.log(phi(maxTickets, sumMean, sumStd).toFixed(4));
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


def phi(x, mean, std):
    return 0.5 * (1 + erf((x - mean) / (std * sqrt(2))))


max_tickets = float(input())
n = float(input())
mean = float(input())
std = float(input())

sum_mean = n * mean
sum_std = sqrt(n) * std

print(round(phi(max_tickets, sum_mean, sum_std), 4))
```

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

double phi(double x, double mean, double std) {
    return 0.5 * (1 + erf((x - mean) / (std * sqrt(2))));
}

int main() {
    double maxTickets, n, mean, std;
    cin >> maxTickets >> n >> mean >> std;

    double sumMean = n * mean;
    double sumStd = sqrt(n) * std;

    cout << fixed << setprecision(4) << phi(maxTickets, sumMean, sumStd) << "\n";
    return 0;
}
```

### How the Solution Works

1. **Read input** — the ticket limit, number of students `n`, the per-student mean `μ`, and standard deviation `σ`.
2. **Distribution of the sum** — by the CLT, total ticket demand is approximately normal with mean `n * μ` and standard deviation `sqrt(n) * σ`.
3. **Normal CDF** — `P(X <= limit) = (1/2) * (1 + erf((limit - μ_sum) / (σ_sum * sqrt(2))))`.
4. **Print** the probability rounded to 4 decimal places.

> **Note:** JavaScript and Java have no built-in error function, so the solution implements `erf` with a numerical approximation. Python (`math.erf`) and C++ (`erf`) provide it natively.
