# Day 4: Geometric Distribution I

## Objective

In this challenge, we learn about geometric distributions.

## Task

The probability that a machine produces a defective product is `1/3`. What is the probability that the `1st` defect occurs on the `5th` item produced?

## Input Format

The first line contains the respective space-separated numerator and denominator for the probability of a defect, and the second line contains the inspection we want the probability of being the first defect for:

```
1 3
5
```

If you do not wish to read this information from stdin, you can hard-code it into your program.

## Output Format

Print a single line denoting the answer, rounded to a scale of 3 decimal places (i.e., `1.234` format).

## Sample Output

```
0.066
```

## Explanation

This is a **geometric distribution** problem. The geometric distribution gives the probability that the first success (here, the first defect) occurs on the `n`-th trial. If each trial succeeds with probability `p`, then:

```
g(n; p) = (1 - p)^(n - 1) * p
```

This means the first `n - 1` items are non-defective (each with probability `1 - p`), and the `n`-th item is the first defect (with probability `p`).

For this problem:

```
p = 1 / 3
n = 5

g(5; 1/3) = (1 - 1/3)^(5 - 1) * (1/3)
          = (2/3)^4 * (1/3)
          ≈ 0.1975 * 0.3333
          ≈ 0.066
```

Rounded to 3 decimal places, the answer is `0.066`.

## Solutions

### JavaScript (Node.js)

```javascript
function processData(input) {
    const lines = input.split(/\s+/);
    const numerator = parseInt(lines[0]);
    const denominator = parseInt(lines[1]);
    const n = parseInt(lines[2]);

    const p = numerator / denominator;

    const result = Math.pow(1 - p, n - 1) * p;

    console.log(result.toFixed(3));
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
numerator, denominator = map(int, input().split())
n = int(input())

p = numerator / denominator
result = ((1 - p) ** (n - 1)) * p

print(round(result, 3))
```

### Java

```java
import java.util.*;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int numerator = sc.nextInt();
        int denominator = sc.nextInt();
        int n = sc.nextInt();

        double p = (double) numerator / denominator;
        double result = Math.pow(1 - p, n - 1) * p;

        System.out.printf("%.3f%n", result);
    }
}
```

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int numerator, denominator, n;
    cin >> numerator >> denominator >> n;

    double p = (double) numerator / denominator;
    double result = pow(1 - p, n - 1) * p;

    cout << fixed << setprecision(3) << result << "\n";
    return 0;
}
```

### How the Solution Works

1. **Read input** — the defect probability as a fraction (`numerator / denominator`) and the trial number `n`.
2. **Geometric formula** — the probability the first success occurs on trial `n` is `(1 - p)^(n - 1) * p`: the first `n - 1` trials fail, and the `n`-th succeeds.
3. **Print** the result rounded to 3 decimal places.
