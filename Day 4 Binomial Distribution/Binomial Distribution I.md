# Day 4: Binomial Distribution I
   
## Objective

In this challenge, we learn about binomial distributions.

## Task

The ratio of boys to girls for babies born in Russia is `1.09 : 1`. If there is `1` child born per birth, what proportion of Russian families with exactly `6` children will have **at least 3 boys**?

Write a program to compute the answer using the above parameters. Then print your result, rounded to a scale of 3 decimal places (i.e., `1.234` format).
 
## Input Format

A single line containing the following values:

```
1.09 1
```

If you do not wish to read this information from stdin, you can hard-code it into your program.

## Output Format

Print a single line denoting the answer, rounded to a scale of 3 decimal places (i.e., `1.234` format).

## Sample Output

```
0.696
```

## Explanation

This is a **binomial distribution** problem. Each birth is an independent trial with two outcomes (boy or girl), so the probability of getting exactly `x` boys out of `n` children is:

```
B(x; n, p) = C(n, x) * p^x * (1 - p)^(n - x)
```

where:

- `n = 6` — the number of children
- `p` — the probability of a boy
- `q = 1 - p` — the probability of a girl
- `C(n, x)` — the number of combinations of `n` items taken `x` at a time

**Step 1 — find `p`.** The ratio of boys to girls is `1.09 : 1`, so out of every `2.09` births, `1.09` are boys:

```
p = 1.09 / (1.09 + 1) = 1.09 / 2.09 ≈ 0.5215
q = 1 - p ≈ 0.4785
```

**Step 2 — "at least 3 boys"** means `x = 3, 4, 5, 6`. Sum the binomial probabilities for each:

```
P(X >= 3) = sum_{x=3}^{6} C(6, x) * p^x * q^(6-x) ≈ 0.696
```

Rounded to 3 decimal places, the answer is `0.696`.

## Solutions

### JavaScript (Node.js)

```javascript
function processData(input) {
    const p = 1.09 / 2.09;
    const q = 1 - p;
    const n = 6;

    function fact(num) {
        return num <= 1 ? 1 : num * fact(num - 1);
    }
    function combinations(n, r) {
        return fact(n) / (fact(r) * fact(n - r));
    }

    let probability = 0;
    for (let x = 3; x <= n; x++) {
        probability += combinations(n, x) * Math.pow(p, x) * Math.pow(q, n - x);
    }

    console.log(probability.toFixed(3));
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
from math import comb

p = 1.09 / 2.09
q = 1 - p
n = 6

probability = sum(comb(n, x) * (p ** x) * (q ** (n - x)) for x in range(3, n + 1))

print(round(probability, 3))
```

### Java

```java
public class Solution {
    static long fact(int num) {
        long result = 1;
        for (int i = 2; i <= num; i++) result *= i;
        return result;
    }

    static double combinations(int n, int r) {
        return (double) fact(n) / (fact(r) * fact(n - r));
    }

    public static void main(String[] args) {
        double p = 1.09 / 2.09;
        double q = 1 - p;
        int n = 6;

        double probability = 0;
        for (int x = 3; x <= n; x++) {
            probability += combinations(n, x) * Math.pow(p, x) * Math.pow(q, n - x);
        }

        System.out.printf("%.3f%n", probability);
    }
}
```

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

long long fact(int num) {
    long long result = 1;
    for (int i = 2; i <= num; i++) result *= i;
    return result;
}

double combinations(int n, int r) {
    return (double) fact(n) / (fact(r) * fact(n - r));
}

int main() {
    double p = 1.09 / 2.09;
    double q = 1 - p;
    int n = 6;

    double probability = 0;
    for (int x = 3; x <= n; x++) {
        probability += combinations(n, x) * pow(p, x) * pow(q, n - x);
    }

    cout << fixed << setprecision(3) << probability << "\n";
    return 0;
}
```

### How the Solution Works

1. **Probability of a boy** — convert the `1.09 : 1` ratio to `p = 1.09 / 2.09`, and `q = 1 - p` for a girl.
2. **Binomial formula** — for each outcome count `x`, the probability is `C(n, x) * p^x * q^(n-x)`.
3. **At least 3 boys** — sum the binomial probabilities for `x = 3, 4, 5, 6`.
4. **Print** the total rounded to 3 decimal places.
