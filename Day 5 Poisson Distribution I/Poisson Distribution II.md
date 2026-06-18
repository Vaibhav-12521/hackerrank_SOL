# Day 5: Poisson Distribution II

## Objective

In this challenge, we go further with Poisson distributions. We recommend reviewing the previous challenge's Tutorial before attempting this problem.

## Task 

The manager of a industrial plant is planning to buy a machine of either type `A` or type `B`. For each day's operation:

- The number of repairs, `X`, that machine `A` needs is a Poisson random variable with mean `0.88`. The daily cost of operating `A` is `C_A = 160 + 40 * X^2`.
- The number of repairs, `Y`, that machine `B` needs is a Poisson random variable with mean `1.55`. The daily cost of operating `B` is `C_B = 128 + 40 * Y^2`.

Assume that the repairs take a negligible amount of time and that the machines are maintained nightly to ensure that they operate like new at the start of each day. Find and print the expected daily cost for each machine.

## Input Format

A single line comprised of `2` space-separated values denoting the respective means for `A` and `B`:

```
0.88 1.55
```

If you do not wish to read this information from stdin, you can hard-code it into your program.

## Output Format

There are two lines of output. Your answers must be rounded to a scale of 3 decimal places (i.e., `1.234` format):

1. On the first line, print the expected daily cost of machine `A`.
2. On the second line, print the expected daily cost of machine `B`.

## Sample Output

```
226.176
286.100
```

## Explanation

The cost depends on the **square** of the number of repairs, so we need `E[X^2]`, not just `E[X]`.

For any random variable, the variance relates the second moment to the mean:

```
Var(X) = E[X^2] - (E[X])^2   =>   E[X^2] = Var(X) + (E[X])^2
```

A key property of the **Poisson distribution** is that its mean and variance are equal: `Var(X) = E[X] = λ`. Therefore:

```
E[X^2] = λ + λ^2
```

Now apply the expectation to the cost (expectation is linear, so constants and coefficients pass straight through):

```
E[C_A] = 160 + 40 * E[X^2] = 160 + 40 * (λ_A + λ_A^2)
E[C_B] = 128 + 40 * E[Y^2] = 128 + 40 * (λ_B + λ_B^2)
```

Plugging in the sample values:

```
λ_A = 0.88:  E[C_A] = 160 + 40 * (0.88 + 0.88^2) = 160 + 40 * 1.6544 ≈ 226.176
λ_B = 1.55:  E[C_B] = 128 + 40 * (1.55 + 1.55^2) = 128 + 40 * 3.9525 ≈ 286.100
```

## Solutions

### JavaScript (Node.js)

```javascript
function processData(input) {
    const lines = input.split('\n');
    const [meanA, meanB] = lines[0].split(' ').map(Number);

    const expX2 = meanA + Math.pow(meanA, 2);
    const expY2 = meanB + Math.pow(meanB, 2);

    const costA = 160 + 40 * expX2;
    const costB = 128 + 40 * expY2;

    console.log(costA.toFixed(3));
    console.log(costB.toFixed(3));
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
mean_a, mean_b = map(float, input().split())

exp_x2 = mean_a + mean_a ** 2
exp_y2 = mean_b + mean_b ** 2

cost_a = 160 + 40 * exp_x2
cost_b = 128 + 40 * exp_y2

print(round(cost_a, 3))
print(round(cost_b, 3))
```

### Java

```java
import java.util.*;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        double meanA = sc.nextDouble();
        double meanB = sc.nextDouble();

        double expX2 = meanA + Math.pow(meanA, 2);
        double expY2 = meanB + Math.pow(meanB, 2);

        double costA = 160 + 40 * expX2;
        double costB = 128 + 40 * expY2;

        System.out.printf("%.3f%n", costA);
        System.out.printf("%.3f%n", costB);
    }
}
```

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    double meanA, meanB;
    cin >> meanA >> meanB;

    double expX2 = meanA + pow(meanA, 2);
    double expY2 = meanB + pow(meanB, 2);

    double costA = 160 + 40 * expX2;
    double costB = 128 + 40 * expY2;

    cout << fixed << setprecision(3) << costA << "\n";
    cout << fixed << setprecision(3) << costB << "\n";
    return 0;
}
```

### How the Solution Works

1. **Read input** — the two Poisson means `λ_A` and `λ_B`.
2. **Second moment** — for a Poisson variable, `E[X^2] = λ + λ^2` (since variance equals the mean).
3. **Expected cost** — apply linearity of expectation: `E[C] = constant + 40 * E[X^2]`.
4. **Print** each expected cost on its own line, rounded to 3 decimal places.
