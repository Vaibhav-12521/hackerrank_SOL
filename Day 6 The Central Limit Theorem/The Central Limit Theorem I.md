# Day 6: The Central Limit Theorem I

## Objective

In this challenge, we practice solving problems based on the Central Limit Theorem.

## Task

A large elevator can transport a maximum of `9800` pounds. Suppose a load of cargo containing `49` boxes must be transported via the elevator. The box weight of this type of cargo follows a distribution with a mean of `μ = 205` pounds and a standard deviation of `σ = 15` pounds. Based on this information, what is the probability that all `49` boxes can be safely loaded into the freight elevator and transported?

## Input Format
 
There are `4` lines of input (shown below):

```
9800
49
205
15
```

The first line contains the maximum weight the elevator can transport. The second line contains the number of boxes in the cargo. The third line contains the mean weight of a cargo box, and the fourth line contains its standard deviation.

If you do not wish to read this information from stdin, you can hard-code it into your program.

## Output Format

Print the probability that the elevator can successfully transport all `49` boxes, rounded to a scale of 4 decimal places (i.e., `1.2345` format).

## Sample Output

```
0.0098
```

## Explanation

This is a **Central Limit Theorem (CLT)** problem. The CLT tells us that the sum of `n` independent samples from any distribution is approximately normally distributed, with:

```
mean of the sum:  μ_sum = n * μ
std dev of sum:   σ_sum = sqrt(n) * σ
```

We want the probability that the **total** weight of the 49 boxes is at most the elevator's limit (`9800`).

**Step 1 — distribution of the total weight:**

```
μ_sum = 49 * 205 = 10045
σ_sum = sqrt(49) * 15 = 7 * 15 = 105
```

**Step 2 — standardize** the limit into a z-score:

```
z = (limit - μ_sum) / σ_sum = (9800 - 10045) / 105 ≈ -2.333
```

**Step 3 — evaluate the normal CDF** at that z-score:

```
P(X <= 9800) = (1/2) * (1 + erf(z / sqrt(2))) ≈ 0.0098
```

Rounded to 4 decimal places, the answer is `0.0098`.

## Solutions

### JavaScript (Node.js)

```javascript
function processData(input) {
    const lines = input.split('\n');
    const limit = parseFloat(lines[0]);
    const n = parseFloat(lines[1]);
    const mu = parseFloat(lines[2]);
    const sigma = parseFloat(lines[3]);

    const muSum = n * mu;
    const sigmaSum = Math.sqrt(n) * sigma;

    function errorFunction(x) {
        const t = 1 / (1 + 0.5 * Math.abs(x));
        const tau = t * Math.exp(-Math.pow(x, 2) - 1.26551223 + 1.00002368 * t + 0.37409196 * Math.pow(t, 2) + 0.09678418 * Math.pow(t, 3) - 0.18628806 * Math.pow(t, 4) + 0.27886807 * Math.pow(t, 5) - 1.13520398 * Math.pow(t, 6) + 1.48851587 * Math.pow(t, 7) - 0.82215223 * Math.pow(t, 8) + 0.17087277 * Math.pow(t, 9));
        return x >= 0 ? 1 - tau : tau - 1;
    }

    const z = (limit - muSum) / (sigmaSum);
    const probability = 0.5 * (1 + errorFunction(z / Math.sqrt(2)));

    console.log(probability.toFixed(4));
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

limit = float(input())
n = float(input())
mu = float(input())
sigma = float(input())

mu_sum = n * mu
sigma_sum = sqrt(n) * sigma

z = (limit - mu_sum) / sigma_sum
probability = 0.5 * (1 + erf(z / sqrt(2)))

print(round(probability, 4))
```

### Java

```java
import java.util.*;

public class Solution {
    static double erf(double x) {
        double t = 1 / (1 + 0.5 * Math.abs(x));
        double tau = t * Math.exp(-x * x - 1.26551223 + 1.00002368 * t
            + 0.37409196 * Math.pow(t, 2) + 0.09678418 * Math.pow(t, 3)
            - 0.18628806 * Math.pow(t, 4) + 0.27886807 * Math.pow(t, 5)
            - 1.13520398 * Math.pow(t, 6) + 1.48851587 * Math.pow(t, 7)
            - 0.82215223 * Math.pow(t, 8) + 0.17087277 * Math.pow(t, 9));
        return x >= 0 ? 1 - tau : tau - 1;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        double limit = sc.nextDouble();
        double n = sc.nextDouble();
        double mu = sc.nextDouble();
        double sigma = sc.nextDouble();

        double muSum = n * mu;
        double sigmaSum = Math.sqrt(n) * sigma;

        double z = (limit - muSum) / sigmaSum;
        double probability = 0.5 * (1 + erf(z / Math.sqrt(2)));

        System.out.printf("%.4f%n", probability);
    }
}
```

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    double limit, n, mu, sigma;
    cin >> limit >> n >> mu >> sigma;

    double muSum = n * mu;
    double sigmaSum = sqrt(n) * sigma;

    double z = (limit - muSum) / sigmaSum;
    double probability = 0.5 * (1 + erf(z / sqrt(2)));

    cout << fixed << setprecision(4) << probability << "\n";
    return 0;
}
```

### How the Solution Works

1. **Read input** — the weight limit, number of boxes `n`, the per-box mean `μ`, and standard deviation `σ`.
2. **Distribution of the sum** — by the CLT, the total weight is approximately normal with mean `n * μ` and standard deviation `sqrt(n) * σ`.
3. **Standardize** — convert the elevator limit into a z-score: `z = (limit - μ_sum) / σ_sum`.
4. **Normal CDF** — `P(X <= limit) = (1/2) * (1 + erf(z / sqrt(2)))`.
5. **Print** the probability rounded to 4 decimal places.

> **Note:** JavaScript and Java have no built-in error function, so the solution implements `erf` with a numerical approximation. Python (`math.erf`) and C++ (`erf`) provide it natively.
