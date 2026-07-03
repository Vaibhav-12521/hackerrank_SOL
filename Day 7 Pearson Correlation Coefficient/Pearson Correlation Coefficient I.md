# Day 7: Pearson Correlation Coefficient I

## Objective

In this challenge, we practice calculating the Pearson correlation coefficient.

## Task

Given two `n`-element data sets, `X` and `Y`, calculate the value of the Pearson correlation coefficient.

## Input Format

The first line contains an integer, `n`, denoting the size of data sets `X` and `Y`.

The second line contains `n` space-separated real numbers (scaled to at most one decimal place), defining data set `X`.

The third line contains `n` space-separated real numbers (scaled to at most one decimal place), defining data set `Y`.

## Constraints

- `10 <= n <= 100`
- `1 <= x_i <= 500`, where `x_i` is the `i`-th value of data set `X`.
- `1 <= y_i <= 500`, where `y_i` is the `i`-th value of data set `Y`.
- Data set `X` contains unique values.
- Data set `Y` contains unique values.

## Output Format

Print the value of the Pearson correlation coefficient, rounded to a scale of 3 decimal places.

## Sample Input

```
10
10 9.8 8 7.8 7.7 7 6 5 4 2
200 44 32 24 22 17 15 12 8 4
```

## Sample Output

```
0.612
```

## Explanation

The **Pearson correlation coefficient** measures the strength and direction of the *linear* relationship between two variables. It ranges from `-1` (perfect negative correlation) through `0` (no linear correlation) to `+1` (perfect positive correlation).

The mean and standard deviation of data set `X` are:

```
μ_X = 6.73
σ_X = 2.39251
```

The mean and standard deviation of data set `Y` are:

```
μ_Y = 37.8
σ_Y = 55.1993
```

We use the following formula to calculate the Pearson correlation coefficient:

```
            sum((x_i - μ_X) * (y_i - μ_Y))
ρ_XY = ------------------------------------------
                    n * σ_X * σ_Y
```

Substituting the values gives `ρ_XY ≈ 0.612`, rounded to 3 decimal places.

## Solutions

### JavaScript (Node.js)

```javascript
function processData(input) {
    const lines = input.trim().split('\n');
    if (lines.length < 3) return;

    const n = parseInt(lines[0].trim());
    const X = lines[1].trim().split(' ').map(Number);
    const Y = lines[2].trim().split(' ').map(Number);

    const getMean = (arr) => arr.reduce((a, b) => a + b) / n;

    const getStdDev = (arr, mean) => {
        return Math.sqrt(arr.reduce((sq, val) => sq + Math.pow(val - mean, 2), 0) / n);
    };

    const meanX = getMean(X);
    const meanY = getMean(Y);
    const stdDevX = getStdDev(X, meanX);
    const stdDevY = getStdDev(Y, meanY);

    let numerator = 0;
    for (let i = 0; i < n; i++) {
        numerator += (X[i] - meanX) * (Y[i] - meanY);
    }

    const pearsonCoefficient = numerator / (n * stdDevX * stdDevY);
    console.log(pearsonCoefficient.toFixed(3));
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

n = int(input())
X = list(map(float, input().split()))
Y = list(map(float, input().split()))

mean_x = sum(X) / n
mean_y = sum(Y) / n

std_x = sqrt(sum((x - mean_x) ** 2 for x in X) / n)
std_y = sqrt(sum((y - mean_y) ** 2 for y in Y) / n)

numerator = sum((X[i] - mean_x) * (Y[i] - mean_y) for i in range(n))

pearson = numerator / (n * std_x * std_y)
print(round(pearson, 3))
```

### Java

```java
import java.util.*;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        double[] X = new double[n];
        double[] Y = new double[n];
        for (int i = 0; i < n; i++) X[i] = sc.nextDouble();
        for (int i = 0; i < n; i++) Y[i] = sc.nextDouble();

        double meanX = 0, meanY = 0;
        for (int i = 0; i < n; i++) { meanX += X[i]; meanY += Y[i]; }
        meanX /= n;
        meanY /= n;

        double varX = 0, varY = 0, numerator = 0;
        for (int i = 0; i < n; i++) {
            varX += Math.pow(X[i] - meanX, 2);
            varY += Math.pow(Y[i] - meanY, 2);
            numerator += (X[i] - meanX) * (Y[i] - meanY);
        }
        double stdX = Math.sqrt(varX / n);
        double stdY = Math.sqrt(varY / n);

        double pearson = numerator / (n * stdX * stdY);
        System.out.printf("%.3f%n", pearson);
    }
}
```

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n;
    cin >> n;
    vector<double> X(n), Y(n);
    for (int i = 0; i < n; i++) cin >> X[i];
    for (int i = 0; i < n; i++) cin >> Y[i];

    double meanX = 0, meanY = 0;
    for (int i = 0; i < n; i++) { meanX += X[i]; meanY += Y[i]; }
    meanX /= n;
    meanY /= n;

    double varX = 0, varY = 0, numerator = 0;
    for (int i = 0; i < n; i++) {
        varX += pow(X[i] - meanX, 2);
        varY += pow(Y[i] - meanY, 2);
        numerator += (X[i] - meanX) * (Y[i] - meanY);
    }
    double stdX = sqrt(varX / n);
    double stdY = sqrt(varY / n);

    double pearson = numerator / (n * stdX * stdY);
    cout << fixed << setprecision(3) << pearson << "\n";
    return 0;
}
```

### How the Solution Works

1. **Read input** - the size `n` and the two data sets `X` and `Y`.
2. **Means** - compute `μ_X` and `μ_Y`.
3. **Standard deviations** - compute the population standard deviations `σ_X` and `σ_Y` (dividing by `n`).
4. **Covariance term** - sum `(x_i - μ_X) * (y_i - μ_Y)` over all `i`.
5. **Pearson coefficient** - divide that sum by `n * σ_X * σ_Y`, and print rounded to 3 decimal places.
