# Day 0: Weighted Mean

## Objective

In the previous challenge, we calculated a mean. In this challenge, we practice calculating a **weighted mean**.

## Task

Given an array, `X`, of `N` integers and an array, `W`, representing the respective weights of `X`'s elements, calculate and print the weighted mean of `X`'s elements. Your answer should be rounded to a scale of 1 decimal place (i.e., `12.3` format).

## Example

```
X = [1, 2, 3]
W = [5, 6, 7]
```

The array of values `X[i] * W[i] = [5, 12, 21]`. Their sum is `38`. The sum of `W = 18`. The weighted mean is `38 / 18 = 2.11111...`. Print `2.1` and return.

## Function Description

Complete the `weightedMean` function in the editor below.

`weightedMean` has the following parameters:

- `int X[n]`: an array of values
- `int W[n]`: an array of weights

### Prints

- `float`: the weighted mean to one decimal place

## Input Format

The first line contains an integer, `N`, the number of elements in arrays `X` and `W`.

The second line contains `N` space-separated integers that describe the elements of array `X`.

The third line contains `N` space-separated integers that describe the elements of array `W`.

## Constraints

- `5 <= N <= 50`
- `0 < X[i] < 100`, where `X[i]` is the `i`-th element of array `X`.
- `0 < W[i] < 100`, where `W[i]` is the `i`-th element of array `W`.

## Output Format

Print the weighted mean on a new line. Your answer should be rounded to a scale of 1 decimal place (i.e., `12.3` format).

## Sample Input

```
STDIN            Function
-----            --------
5                X[] and W[] size n = 5
10 40 30 50 20   X = [10, 40, 30, 50, 20]
1 2 3 4 5        W = [1, 2, 3, 4, 5]
```

## Sample Output

```
32.0
```

## Explanation

We use the following formula to calculate the weighted mean:

```
        sum(x_i * w_i)
m_w = ------------------
          sum(w_i)
```

Substituting the sample values:

```
        10*1 + 40*2 + 30*3 + 50*4 + 20*5     480
m_w = ------------------------------------ = ----- = 32.0
              1 + 2 + 3 + 4 + 5               15
```

And then print our result to a scale of 1 decimal place (`32.0`) on a new line.

## Solutions

### JavaScript (Node.js)

```javascript
function processData(input) {
    const lines = input.trim().split('\n');
    const n = parseInt(lines[0]);
    const x = lines[1].split(' ').map(Number);
    const w = lines[2].split(' ').map(Number);

    let weightedSum = 0;
    let weightSum = 0;

    for (let i = 0; i < n; i++) {
        weightedSum += x[i] * w[i];
        weightSum += w[i];
    }

    const weightedMean = weightedSum / weightSum;

    console.log(weightedMean.toFixed(1));
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
n = int(input())
x = list(map(int, input().split()))
w = list(map(int, input().split()))

weighted_mean = sum(xi * wi for xi, wi in zip(x, w)) / sum(w)

print(round(weighted_mean, 1))
```

### Java

```java
import java.util.*;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] x = new int[n];
        int[] w = new int[n];
        for (int i = 0; i < n; i++) x[i] = sc.nextInt();
        for (int i = 0; i < n; i++) w[i] = sc.nextInt();

        double weightedSum = 0, weightSum = 0;
        for (int i = 0; i < n; i++) {
            weightedSum += (double) x[i] * w[i];
            weightSum += w[i];
        }

        System.out.printf("%.1f%n", weightedSum / weightSum);
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
    vector<int> x(n), w(n);
    for (int i = 0; i < n; i++) cin >> x[i];
    for (int i = 0; i < n; i++) cin >> w[i];

    double weightedSum = 0, weightSum = 0;
    for (int i = 0; i < n; i++) {
        weightedSum += (double) x[i] * w[i];
        weightSum += w[i];
    }

    cout << fixed << setprecision(1) << (weightedSum / weightSum) << "\n";
    return 0;
}
```

### How the Solution Works

1. **Parse input** - Read `N` from the first line, the values array `X` from the second line, and the weights array `W` from the third line.
2. **Accumulate** - Loop over each index `i`, adding `X[i] * W[i]` to a running `weightedSum` and `W[i]` to a running `weightSum`.
3. **Divide** - The weighted mean is `weightedSum / weightSum`.
4. **Format** - Print the result rounded to 1 decimal place (`toFixed(1)` / `round(..., 1)` / `%.1f`).
