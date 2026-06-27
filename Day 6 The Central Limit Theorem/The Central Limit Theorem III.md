# Day 6: The Central Limit Theorem III

## Objective

In this challenge, we practice solving problems based on the Central Limit Theorem. We recommend reviewing the Central Limit Theorem Tutorial before attempting this challenge.

## Task

You have a sample of `100` values from a population with mean `μ = 500` and with standard deviation `σ = 80`. Compute the interval that covers the middle `95%` of the distribution of the sample mean; in other words, compute `A` and `B` such that `P(A < x < B) = 0.95`. Use the value of `z = 1.96`. Note that `z` is the z-score.

## Input Format

There are five lines of input (shown below):

```
100
500
80
.95
1.96
```

The first line contains the sample size. The second and third lines contain the respective mean (`μ`) and standard deviation (`σ`). The fourth line contains the distribution percentage we want to cover (as a decimal), and the fifth line contains the value of `z`. 

If you do not wish to read this information from stdin, you can hard-code it into your program.

## Output Format  

Print the following two lines of output, rounded to a scale of 2 decimal places (i.e., `1.23` format):

1. On the first line, print the value of `A`.
2. On the second line, print the value of `B`.

## Sample Output

```
484.32
515.68
```

## Explanation

This is a **Central Limit Theorem (CLT)** problem about the distribution of the **sample mean**. The CLT tells us the sample mean is approximately normal, centered at the population mean `μ`, with a spread measured by the **standard error**:  

```
standard error:  SE = σ / sqrt(n) 
```
 
A confidence interval covering the middle `95%` extends `z` standard errors on each side of the mean:

```
A = μ - z * SE
B = μ + z * SE
```

**Step 1 — standard error:**

```
SE = 80 / sqrt(100) = 80 / 10 = 8
```

**Step 2 — the interval bounds (z = 1.96):**

```
A = 500 - 1.96 * 8 = 500 - 15.68 = 484.32
B = 500 + 1.96 * 8 = 500 + 15.68 = 515.68 
```

So the middle 95% of the sample-mean distribution lies between `484.32` and `515.68`.

## Solutions
 
### Java

```java
import java.util.Scanner;

public class Solution {
    public static void main(String[] args) {
        Scanner scan = new Scanner(System.in);

        double n = scan.nextDouble();
        double mean = scan.nextDouble();
        double stdDev = scan.nextDouble();
        double intervalPercent = scan.nextDouble(); // not needed for the calculation
        double z = scan.nextDouble();

        scan.close();

        double SE = stdDev / Math.sqrt(n);

        double A = mean - (z * SE);
        double B = mean + (z * SE);

        System.out.printf("%.2f\n", A);
        System.out.printf("%.2f\n", B);
    }
}
```

### JavaScript (Node.js)

```javascript
function processData(input) {
    const lines = input.split('\n');
    const n = parseFloat(lines[0]);
    const mean = parseFloat(lines[1]);
    const stdDev = parseFloat(lines[2]);
    const z = parseFloat(lines[4]);

    const SE = stdDev / Math.sqrt(n);

    const A = mean - z * SE;
    const B = mean + z * SE;
    console.log(A.toFixed(2));
    console.log(B.toFixed(2));
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
from math import sqrt

n = float(input())
mean = float(input())
std_dev = float(input())
interval_percent = float(input())  
z = float(input())

se = std_dev / sqrt(n)

a = mean - z * se
b = mean + z * se

print(round(a, 2))
print(round(b, 2))
```

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    double n, mean, stdDev, intervalPercent, z;
    cin >> n >> mean >> stdDev >> intervalPercent >> z;

    double SE = stdDev / sqrt(n);

    double A = mean - z * SE;
    double B = mean + z * SE;

    cout << fixed << setprecision(2) << A << "\n";
    cout << fixed << setprecision(2) << B << "\n";
    return 0;
}
```

### How the Solution Works

1. **Read input** - sample size `n`, mean `μ`, standard deviation `σ`, the coverage percentage (unused in the math), and the z-score.
2. **Standard error** - the sample mean's spread is `SE = σ / sqrt(n)`.
3. **Confidence interval** — extend `z` standard errors on each side of the mean: `A = μ - z * SE`, `B = μ + z * SE`.
4. **Print** `A` and `B`, each on its own line, rounded to 2 decimal places.

> **Note:** The interval percentage (`0.95`) is given as input, but the actual width comes from the supplied z-score (`1.96`), so the percentage itself isn't used in the calculation. 

