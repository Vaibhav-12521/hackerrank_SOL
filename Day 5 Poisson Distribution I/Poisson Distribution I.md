# Day 5: Poisson Distribution I

## Objective

In this challenge, we learn about Poisson distributions.

## Task

A random variable, `X`, follows Poisson distribution with mean of `2.5`. Find the probability with which the random variable `X` is equal to `5`.

## Input Format

The first line contains `X`'s mean. The second line contains the value we want the probability for:

```
2.5
5
``` 

If you do not wish to read this information from stdin, you can hard-code it into your program.

## Output Format

Print a single line denoting the answer, rounded to a scale of 3 decimal places (i.e., `1.234` format).

## Sample Output
 
```
0.067
```

## Explanation

This is a **Poisson distribution** problem. The Poisson distribution gives the probability of observing exactly `k` events in a fixed interval, given that events occur at an average rate `λ` (lambda) and independently of one another:

```
P(k; λ) = (λ^k * e^(-λ)) / k!
```

where:

- `λ` — the mean (average number of events)
- `k` — the number of events we want the probability for
- `e` — Euler's number (≈ 2.71828)
- `k!` — the factorial of `k`

For this problem:

```
λ = 2.5
k = 5

P(5; 2.5) = (2.5^5 * e^(-2.5)) / 5!
          = (97.65625 * 0.082085) / 120
          ≈ 0.067
```

Rounded to 3 decimal places, the answer is `0.067`.

## Solutions

### JavaScript (Node.js)

```javascript
function processData(input) {
    const lines = input.split('\n');
    const lambda = parseFloat(lines[0]);
    const k = parseInt(lines[1]);

    const factorial = (n) => (n === 0 ? 1 : n * factorial(n - 1));

    const result = (Math.pow(lambda, k) * Math.exp(-lambda)) / factorial(k);

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
from math import exp, factorial

lam = float(input())
k = int(input())

result = (lam ** k) * exp(-lam) / factorial(k)

print(round(result, 3))
```

### Java

```java
import java.util.*;

public class Solution {
    static long factorial(int n) {
        long result = 1;
        for (int i = 2; i <= n; i++) result *= i;
        return result;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        double lambda = sc.nextDouble();
        int k = sc.nextInt();

        double result = (Math.pow(lambda, k) * Math.exp(-lambda)) / factorial(k);

        System.out.printf("%.3f%n", result);
    }
}
```

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

long long factorial(int n) {
    long long result = 1;
    for (int i = 2; i <= n; i++) result *= i;
    return result;
}

int main() {
    double lambda;
    int k;
    cin >> lambda >> k;

    double result = (pow(lambda, k) * exp(-lambda)) / factorial(k);

    cout << fixed << setprecision(3) << result << "\n";
    return 0;
}
```

### How the Solution Works

1. **Read input** — the mean `λ` (lambda) and the target event count `k`.
2. **Poisson formula** — the probability of exactly `k` events is `(λ^k * e^(-λ)) / k!`.
3. **Print** the result rounded to 3 decimal places.
