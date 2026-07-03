# Day 8: Least Square Regression Line

## Objective

In this challenge, we practice using linear regression techniques. 

## Task

A group of five students enrolls in Statistics immediately after taking a Math aptitude test. Each student's Math aptitude test score, `x`, and Statistics course grade, `y`, can be expressed as the following list of `(x, y)` points:

```
1. (95, 85)
2. (85, 95)
3. (80, 70)
4. (70, 65)
5. (60, 70)
```

If a student scored an `80` on the Math aptitude test, what grade would we expect them to achieve in Statistics? Determine the equation of the best-fit line using the least squares method, then compute and print the value of `y` when `x = 80`.

## Input Format

There are five lines of input; each line contains two space-separated integers describing a student's respective `x` and `y` grades:

```
95 85
85 95
80 70
70 65
60 70
```

If you do not wish to read this information from stdin, you can hard-code it into your program.

## Output Format

Print a single line denoting the answer, rounded to a scale of 3 decimal places (i.e., `1.234` format).

## Sample Output

```
78.288
```

## Explanation

The **least squares regression line** is the straight line `y = a + b*x` that minimizes the sum of squared vertical distances between the line and the data points. Its slope `b` and intercept `a` are:

```
        n * sum(x*y) - sum(x) * sum(y)
b = ---------------------------------------
            n * sum(x^2) - (sum(x))^2

a = ȳ - b * x̄
```

where `x̄` and `ȳ` are the means of `x` and `y`.

For the given data (`n = 5`):

```
sum(x) = 390     sum(y) = 385
sum(x*y) = 30635 sum(x^2) = 30950
x̄ = 78           ȳ = 77
```

Computing the slope and intercept:

```
b = (5 * 30635 - 390 * 385) / (5 * 30950 - 390^2)
  = (153175 - 150150) / (154750 - 152100)
  = 3025 / 2650
  ≈ 1.1415

a = 77 - 1.1415 * 78 ≈ -12.0377
```

Finally, predict `y` at `x = 80`:

```
y = a + b * 80 = -12.0377 + 1.1415 * 80 ≈ 78.288
```

Rounded to 3 decimal places, the answer is `78.288`.

## Solutions

### Java

```java
import java.io.*;
import java.util.*;

public class Solution {

    public static void main(String[] args) {
        int[] x = {95, 85, 80, 70, 60};
        int[] y = {85, 95, 70, 65, 70};
        int n = x.length;

        double sumX = 0, sumY = 0, sumXY = 0, sumX2 = 0;

        for (int i = 0; i < n; i++) {
            sumX += x[i];
            sumY += y[i];
            sumXY += x[i] * y[i];
            sumX2 += Math.pow(x[i], 2);
        }

        double xBar = sumX / n;
        double yBar = sumY / n;

        double b = (n * sumXY - sumX * sumY) / (n * sumX2 - Math.pow(sumX, 2));

        double a = yBar - b * xBar;

        double result = a + b * 80;

        System.out.printf("%.3f%n", result);
    }
}
```

### JavaScript (Node.js)

```javascript
function processData(input) {
    const lines = input.trim().split('\n');
    const x = [];
    const y = [];
    for (const line of lines) {
        const [xi, yi] = line.trim().split(/\s+/).map(Number);
        x.push(xi);
        y.push(yi);
    }
    const n = x.length;

    let sumX = 0, sumY = 0, sumXY = 0, sumX2 = 0;
    for (let i = 0; i < n; i++) {
        sumX += x[i];
        sumY += y[i];
        sumXY += x[i] * y[i];
        sumX2 += x[i] * x[i];
    }

    const xBar = sumX / n;
    const yBar = sumY / n;

    const b = (n * sumXY - sumX * sumY) / (n * sumX2 - sumX * sumX);
    const a = yBar - b * xBar;

    console.log((a + b * 80).toFixed(3));
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
x = []
y = []
for _ in range(5):
    xi, yi = map(int, input().split())
    x.append(xi)
    y.append(yi)

n = len(x)
sum_x = sum(x)
sum_y = sum(y)
sum_xy = sum(x[i] * y[i] for i in range(n))
sum_x2 = sum(xi ** 2 for xi in x)

x_bar = sum_x / n
y_bar = sum_y / n

b = (n * sum_xy - sum_x * sum_y) / (n * sum_x2 - sum_x ** 2)
a = y_bar - b * x_bar

print(round(a + b * 80, 3))
```

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n = 5;
    vector<double> x(n), y(n);
    for (int i = 0; i < n; i++) cin >> x[i] >> y[i];

    double sumX = 0, sumY = 0, sumXY = 0, sumX2 = 0;
    for (int i = 0; i < n; i++) {
        sumX += x[i];
        sumY += y[i];
        sumXY += x[i] * y[i];
        sumX2 += x[i] * x[i];
    }

    double xBar = sumX / n;
    double yBar = sumY / n;

    double b = (n * sumXY - sumX * sumY) / (n * sumX2 - sumX * sumX);
    double a = yBar - b * xBar;

    cout << fixed << setprecision(3) << (a + b * 80) << "\n";
    return 0;
}
```

### How the Solution Works

1. **Accumulate sums** - compute `sum(x)`, `sum(y)`, `sum(x*y)`, and `sum(x^2)` over the five data points.
2. **Slope** - `b = (n * sum(x*y) - sum(x) * sum(y)) / (n * sum(x^2) - (sum(x))^2)`.
3. **Intercept** - `a = ȳ - b * x̄`.
4. **Predict** - evaluate the best-fit line `y = a + b * x` at `x = 80`.
5. **Print** the prediction rounded to 3 decimal places.
