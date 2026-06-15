# Day 4: Geometric Distribution II

## Objective

In this challenge, we go further with geometric distributions. We recommend reviewing the Geometric Distribution tutorial before attempting this challenge.

## Task

The probability that a machine produces a defective product is `1/3`. What is the probability that the `1st` defect is found during the first `5` inspections?

## Input Format

The first line contains the respective space-separated numerator and denominator for the probability of a defect, and the second line contains the inspection we want the probability of the first defect being discovered by:

```
1 3
5
```

If you do not wish to read this information from stdin, you can hard-code it into your program.

## Output Format

Print a single line denoting the answer, rounded to a scale of 3 decimal places (i.e., `1.234` format).

## Sample Output

```
0.868
```

## Explanation

This is a **geometric distribution** problem. We want the probability that the first defect occurs **on or before** the `n`-th inspection — i.e., the cumulative geometric probability.

The probability that the first success occurs on exactly trial `x` is:

```
g(x; p) = (1 - p)^(x - 1) * p
```

So the probability that the first defect occurs within the first `n` inspections is the sum from `x = 1` to `n`:

```
P(X <= n) = sum_{x=1}^{n} (1 - p)^(x - 1) * p
```

This has a neat closed form — the complement of "no defect in any of the first `n` trials":

```
P(X <= n) = 1 - (1 - p)^n
```

For this problem:

```
p = 1 / 3
n = 5

P(X <= 5) = 1 - (2/3)^5
          = 1 - 0.1317...
          ≈ 0.868
```

Rounded to 3 decimal places, the answer is `0.868`.

## Solutions

### JavaScript (Node.js)

```javascript
function processData(input) {
    const lines = input.split('\n');
    const [numerator, denominator] = lines[0].split(' ').map(Number);
    const n = parseInt(lines[1]);

    const p = numerator / denominator;
    const q = 1 - p;

    const result = 1 - Math.pow(q, n);

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
q = 1 - p
result = 1 - q ** n

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
        double q = 1 - p;
        double result = 1 - Math.pow(q, n);

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
    double q = 1 - p;
    double result = 1 - pow(q, n);

    cout << fixed << setprecision(3) << result << "\n";
    return 0;
}
```

### How the Solution Works

1. **Read input** — the defect probability as a fraction (`numerator / denominator`) and the number of inspections `n`.
2. **Cumulative geometric** — "first defect within the first `n` inspections" is the complement of "no defect at all in `n` trials": `P(X <= n) = 1 - (1 - p)^n`.
3. **Print** the result rounded to 3 decimal places.
