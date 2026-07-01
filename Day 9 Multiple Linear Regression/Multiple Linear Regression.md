# Day 9: Multiple Linear Regression

## Objective

In this challenge, we practice using multiple linear regression.

## Task

Andrea has a simple equation:

```
Y = a + b_1*f_1 + b_2*f_2 + ... + b_m*f_m
```

for `(m + 1)` real constants `(a, b_1, b_2, ..., b_m)`. We can say that the value of `Y` depends on `m` features. Andrea studies this equation for `n` different feature sets `(f_1, f_2, ..., f_m)` and records each respective value of `Y`. If she has `q` new feature sets, can you help Andrea find the value of `Y` for each of the sets?

> **Note:** You are not expected to account for bias and variance trade-offs.

## Input Format

The first line contains `2` space-separated integers, `m` (the number of observed features) and `n` (the number of feature sets Andrea studied), respectively.

Each of the `n` subsequent lines contains `m + 1` space-separated decimals; the first `m` elements are features `(f_1, f_2, ..., f_m)`, and the last element is the value of `Y` for that feature set.

The next line contains a single integer, `q`, denoting the number of feature sets Andrea wants to query for.

Each of the `q` subsequent lines contains `m` space-separated decimals describing a feature set.

## Constraints

- `1 <= m <= 10`
- `5 <= n <= 100`
- `0 <= a <= 1`
- `0 <= f_i <= 1`
- `0 <= Y <= 10^6`
- `1 <= q <= 10`

## Output Format

For each of the `q` feature sets, print the value of `Y` on a new line (i.e., print `q` total lines of output).

## Sample Input

```
2 7
0.18 0.89 109.85
1.0 0.26 155.72
0.92 0.11 137.66
0.07 0.37 76.17
0.85 0.16 139.75
0.99 0.41 162.6
0.87 0.47 151.77
4
0.49 0.18
0.57 0.83
0.56 0.64
0.76 0.18
```

## Sample Output

```
105.22
142.68
132.94
129.71
```

## Explanation

This is a **multiple linear regression** problem — the generalization of the "line of best fit" (Day 8) from one predictor to `m` predictors. Instead of fitting a line in 2D, we fit a **hyperplane** in `(m + 1)`-dimensional space.

### The model

We assume each output `Y` is a weighted sum of the features plus a constant offset:

```
Y = a + b_1*f_1 + b_2*f_2 + ... + b_m*f_m
```

- `a` — the **intercept** (the predicted `Y` when every feature is `0`)
- `b_j` — the **weight** of feature `j` (how much `Y` changes per unit increase in `f_j`, holding the others fixed)

Our job is to learn the coefficient vector `B = [a, b_1, ..., b_m]` from the `n` training rows, then use it to predict `Y` for the `q` query rows.

### Setting up the matrices

We stack the training data into a **design matrix** `X` and an output vector `Y`. The trick is to prepend a column of `1`s to `X` — that column multiplies the intercept `a`, so the intercept can be treated as just another coefficient:

```
      | 1  f_11  f_12 ... f_1m |            | Y_1 |
      | 1  f_21  f_22 ... f_2m |            | Y_2 |
X  =  | .   .     .        .   |     Y  =   |  .  |
      | 1  f_n1  f_n2 ... f_nm |            | Y_n |

       (n x (m+1))                          (n x 1)
```

The whole model then compresses to a single matrix equation: `Y ≈ X · B`.

### The least squares normal equation

Because there are usually more equations (`n` rows) than unknowns (`m + 1` coefficients), we can't solve `X · B = Y` exactly. Instead we find the `B` that **minimizes the sum of squared errors** `Σ(Y_i − predicted_i)²`. The `B` that achieves this minimum is the solution of the **normal equation**:

```
(Xᵀ X) · B = Xᵀ Y      →      B = (Xᵀ X)⁻¹ Xᵀ Y
```

- `Xᵀ X` is a small `(m + 1) x (m + 1)` matrix — its size depends on the number of *features*, not the number of *rows*.
- `Xᵀ Y` is an `(m + 1)` vector.

Rather than explicitly inverting `Xᵀ X` (slower and less stable), the JavaScript/Java/C++ solutions solve the linear system `(Xᵀ X) · B = Xᵀ Y` directly with Gauss-Jordan elimination.

### Worked sample

For the sample input (`m = 2`, `n = 7`), fitting the model yields approximately:

```
a ≈ 52.41,   b_1 ≈ 90.69,   b_2 ≈ 46.46
```

So the learned equation is roughly `Y ≈ 52.41 + 90.69*f_1 + 46.46*f_2`. Predicting on the first query row `(0.49, 0.18)`:

```
Y = 52.41 + 90.69*0.49 + 46.46*0.18 ≈ 105.22
```

Doing the same for all four query rows produces the sample outputs `105.22`, `142.68`, `132.94`, and `129.71`.

## Solutions

### Python 3 (NumPy)

```python
import numpy as np

m, n = map(int, input().split())
data = np.array([list(map(float, input().split())) for _ in range(n)])

X = np.concatenate((np.ones((n, 1)), data[:, :m]), axis=1)
Y = data[:, m]

# Least squares normal equation: B = (X^T X)^-1 X^T Y
B = np.linalg.inv(X.T @ X) @ X.T @ Y

q = int(input())
for _ in range(q):
    features = list(map(float, input().split()))
    y = B[0] + sum(B[i + 1] * features[i] for i in range(m))
    print(round(y, 2))
```

### JavaScript (Node.js)

```javascript
function processData(input) {
    const lines = input.trim().split('\n');
    let idx = 0;
    const [m, n] = lines[idx++].trim().split(/\s+/).map(Number);
    const k = m + 1;

    const X = [];
    const Y = [];
    for (let i = 0; i < n; i++) {
        const row = lines[idx++].trim().split(/\s+/).map(Number);
        X.push([1, ...row.slice(0, m)]);
        Y.push(row[m]);
    }

    // Build normal equations: A = X^T X (k x k), b = X^T Y (k)
    const A = Array.from({ length: k }, () => new Array(k).fill(0));
    const b = new Array(k).fill(0);
    for (let r = 0; r < k; r++) {
        for (let c = 0; c < k; c++) {
            for (let i = 0; i < n; i++) A[r][c] += X[i][r] * X[i][c];
        }
        for (let i = 0; i < n; i++) b[r] += X[i][r] * Y[i];
    }

    const B = gaussianSolve(A, b, k);

    const q = parseInt(lines[idx++].trim());
    const out = [];
    for (let i = 0; i < q; i++) {
        const f = lines[idx++].trim().split(/\s+/).map(Number);
        let y = B[0];
        for (let j = 0; j < m; j++) y += B[j + 1] * f[j];
        out.push(y.toFixed(2));
    }
    console.log(out.join('\n'));
}

// Solve A * x = b by Gauss-Jordan elimination
function gaussianSolve(A, b, k) {
    for (let i = 0; i < k; i++) A[i].push(b[i]);
    for (let col = 0; col < k; col++) {
        let piv = col;
        for (let r = col + 1; r < k; r++) {
            if (Math.abs(A[r][col]) > Math.abs(A[piv][col])) piv = r;
        }
        [A[col], A[piv]] = [A[piv], A[col]];
        for (let r = 0; r < k; r++) {
            if (r === col) continue;
            const factor = A[r][col] / A[col][col];
            for (let c = col; c <= k; c++) A[r][c] -= factor * A[col][c];
        }
    }
    return A.map((row, i) => row[k] / row[i]);
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

### Java

```java
import java.io.*;
import java.util.*;
import java.text.*;
import java.math.*;
import java.util.regex.*;

public class Solution {

    // Solve A * x = b by Gauss-Jordan elimination
    static double[] gaussianSolve(double[][] A, double[] b, int k) {
        double[][] M = new double[k][k + 1];
        for (int i = 0; i < k; i++) {
            System.arraycopy(A[i], 0, M[i], 0, k);
            M[i][k] = b[i];
        }
        for (int col = 0; col < k; col++) {
            int piv = col;
            for (int r = col + 1; r < k; r++)
                if (Math.abs(M[r][col]) > Math.abs(M[piv][col])) piv = r;
            double[] tmp = M[col]; M[col] = M[piv]; M[piv] = tmp;

            for (int r = 0; r < k; r++) {
                if (r == col) continue;
                double factor = M[r][col] / M[col][col];
                for (int c = col; c <= k; c++) M[r][c] -= factor * M[col][c];
            }
        }
        double[] x = new double[k];
        for (int i = 0; i < k; i++) x[i] = M[i][k] / M[i][i];
        return x;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int m = sc.nextInt();
        int n = sc.nextInt();
        int k = m + 1;

        double[][] X = new double[n][k];
        double[] Y = new double[n];
        for (int i = 0; i < n; i++) {
            X[i][0] = 1;
            for (int j = 0; j < m; j++) X[i][j + 1] = sc.nextDouble();
            Y[i] = sc.nextDouble();
        }

        // Build normal equations: A = X^T X (k x k), b = X^T Y (k)
        double[][] A = new double[k][k];
        double[] b = new double[k];
        for (int r = 0; r < k; r++) {
            for (int c = 0; c < k; c++)
                for (int i = 0; i < n; i++) A[r][c] += X[i][r] * X[i][c];
            for (int i = 0; i < n; i++) b[r] += X[i][r] * Y[i];
        }

        double[] B = gaussianSolve(A, b, k);

        int q = sc.nextInt();
        StringBuilder out = new StringBuilder();
        for (int i = 0; i < q; i++) {
            double y = B[0];
            for (int j = 0; j < m; j++) y += B[j + 1] * sc.nextDouble();
            out.append(String.format("%.2f%n", y));
        }
        System.out.print(out);
    }
}
```

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

// Solve A * x = b by Gauss-Jordan elimination
vector<double> gaussianSolve(vector<vector<double>> M, int k) {
    for (int col = 0; col < k; col++) {
        int piv = col;
        for (int r = col + 1; r < k; r++)
            if (fabs(M[r][col]) > fabs(M[piv][col])) piv = r;
        swap(M[col], M[piv]);

        for (int r = 0; r < k; r++) {
            if (r == col) continue;
            double factor = M[r][col] / M[col][col];
            for (int c = col; c <= k; c++) M[r][c] -= factor * M[col][c];
        }
    }
    vector<double> x(k);
    for (int i = 0; i < k; i++) x[i] = M[i][k] / M[i][i];
    return x;
}

int main() {
    int m, n;
    cin >> m >> n;
    int k = m + 1;

    vector<vector<double>> X(n, vector<double>(k));
    vector<double> Y(n);
    for (int i = 0; i < n; i++) {
        X[i][0] = 1;
        for (int j = 0; j < m; j++) cin >> X[i][j + 1];
        cin >> Y[i];
    }

    // Augmented normal equations [ X^T X | X^T Y ]
    vector<vector<double>> M(k, vector<double>(k + 1, 0));
    for (int r = 0; r < k; r++) {
        for (int c = 0; c < k; c++)
            for (int i = 0; i < n; i++) M[r][c] += X[i][r] * X[i][c];
        for (int i = 0; i < n; i++) M[r][k] += X[i][r] * Y[i];
    }

    vector<double> B = gaussianSolve(M, k);

    int q;
    cin >> q;
    cout << fixed << setprecision(2);
    for (int i = 0; i < q; i++) {
        double y = B[0];
        for (int j = 0; j < m; j++) {
            double f;
            cin >> f;
            y += B[j + 1] * f;
        }
        cout << y << "\n";
    }
    return 0;
}
```

### How the Solution Works

1. **Read the training data** — build the design matrix `X` (each row is `[1, f_1, ..., f_m]`, the leading `1` handling the intercept `a`) and the output vector `Y`.
2. **Form the normal equations** — compute `A = Xᵀ X` (a `(m+1) x (m+1)` matrix) and `b = Xᵀ Y`.
3. **Solve for the coefficients** — solve `A · B = b`. NumPy does this with a matrix inverse; the other languages use Gauss-Jordan elimination (no external libraries needed).
4. **Predict** — for each query feature set, compute `Y = a + b_1*f_1 + ... + b_m*f_m`.
5. **Print** each prediction on its own line, rounded to 2 decimal places.

> **Note:** Python's NumPy makes this a one-liner via `np.linalg.inv`, but the same math is done manually in JavaScript, Java, and C++ by solving the normal equations with Gaussian elimination.

## Complexity

Let `k = m + 1` be the number of coefficients.

| Step | Cost |
|------|------|
| Building `Xᵀ X` | `O(k² · n)` |
| Building `Xᵀ Y` | `O(k · n)` |
| Solving the `k x k` system (Gauss-Jordan) | `O(k³)` |
| Answering all queries | `O(q · m)` |

With the given constraints (`m ≤ 10`, `n ≤ 100`, `q ≤ 10`) every step is tiny, so the whole program runs effectively instantly. The key insight is that `Xᵀ X` collapses the `n`-row data into a compact `k x k` system — the solve cost depends on the number of **features**, not the number of samples.

## Key Concepts

- **Design matrix & the intercept trick** — prepending a column of `1`s lets the constant term `a` be handled as just another coefficient, so the entire model is one clean matrix product `X · B`.
- **Least squares** — "best fit" means minimizing the sum of *squared* residuals. Squaring penalizes large misses more heavily and makes the objective smooth, which is exactly what yields the closed-form normal equation.
- **Normal equation vs. gradient descent** — for a small number of features, directly solving `(XᵀX)B = XᵀY` is exact and fast. Real-world ML with thousands of features usually prefers iterative methods (gradient descent) because inverting/solving a huge matrix becomes expensive and numerically fragile.
- **Interpreting coefficients** — each `b_j` is the marginal effect of feature `j` on `Y`, assuming the other features stay constant.

## Common Pitfalls

- **Forgetting the intercept column** — without the column of `1`s, the hyperplane is forced through the origin, giving wrong predictions.
- **Explicitly inverting `Xᵀ X`** — mathematically valid, but solving the linear system directly (as the JS/Java/C++ versions do) is faster and more numerically stable.
- **Reading input in the wrong order** — the last value on each training line is `Y`, not a feature; the query lines have only `m` features and no `Y`.
- **Output precision** — the answer must be rounded to **2** decimal places for this challenge (unlike most earlier problems that used 3).
