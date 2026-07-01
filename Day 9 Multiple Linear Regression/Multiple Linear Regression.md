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

This is a **multiple linear regression** problem. We fit the model:

```
Y = a + b_1*f_1 + ... + b_m*f_m
```

The coefficient vector `B = [a, b_1, ..., b_m]` is found with the **least squares normal equation**:

```
B = (Xᵀ X)⁻¹ Xᵀ Y
```

where `X` is the `n x (m + 1)` matrix of feature rows with a leading column of `1`s (for the intercept `a`), and `Y` is the vector of observed outputs.

Once we have `B`, the prediction for a new feature set is simply:

```
Y = a + b_1*f_1 + ... + b_m*f_m
```

Applying this to each of the `q` query rows produces the four sample outputs above.

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
import java.util.*;

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
