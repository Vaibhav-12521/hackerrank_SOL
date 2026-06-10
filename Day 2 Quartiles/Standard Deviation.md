# Day 1: Standard Deviation

## Objective

In this challenge, we practice calculating standard deviation.

## Task

Given an array, `arr`, of `n` integers, calculate and print the standard deviation. Your answer should be in decimal form, rounded to a scale of 1 decimal place (i.e., `12.3` format). An error margin of `±0.1` will be tolerated for the standard deviation.

## Example

```
arr = [2, 5, 2, 7, 4]
```

The sum of the array values is `20` and there are `5` elements. The mean is `4.0`. Subtract the mean from each element, square each result, and take their sum:

```
(2 - 4)^2 = 4
(5 - 4)^2 = 1
(2 - 4)^2 = 4
(7 - 4)^2 = 9
(4 - 4)^2 = 0
```

Their sum is `18`. Take the square root of `18 / 5` to get `1.7`, the standard deviation.

## Function Description

Complete the `stdDev` function in the editor below.

`stdDev` has the following parameter:

- `int arr[n]`: an array of integers

### Prints

- `float`: the standard deviation to 1 place after the decimal

## Input Format

The first line contains an integer, `n`, denoting the size of `arr`.

The second line contains `n` space-separated integers that describe `arr`.

## Constraints

- `5 <= n <= 100`
- `0 < arr[i] <= 10^5`

## Output Format

Print the standard deviation on a new line, rounded to a scale of 1 decimal place (i.e., `12.3` format).

## Sample Input

```
STDIN              Function
-----              --------
5                  arr[] size n = 5
10 40 30 50 20     arr = [10, 40, 30, 50, 20]
```

## Sample Output

```
14.1
```

## Explanation

First, find the mean:

```
      sum(arr[i])
mu = ------------- = 30.0
          n
```

Next, calculate the squared distance from the mean, `(arr[i] - mu)^2`, for each `arr[i]`:

```
1. (arr[0] - mu)^2 = (10 - 30)^2 = 400
2. (arr[1] - mu)^2 = (40 - 30)^2 = 100
3. (arr[2] - mu)^2 = (30 - 30)^2 = 0
4. (arr[3] - mu)^2 = (50 - 30)^2 = 400
5. (arr[4] - mu)^2 = (20 - 30)^2 = 100
```

Now compute the sum `400 + 100 + 0 + 400 + 100 = 1000`, so:

```
        sum((arr[i] - mu)^2)       1000
sigma = sqrt(--------------------) = sqrt(------) = sqrt(200) = 14.1421356...
                    n                 5
```

Rounded to 1 decimal place, the answer is `14.1`.

## Solutions

### JavaScript (Node.js)

```javascript
'use strict';

process.stdin.resume();
process.stdin.setEncoding('utf-8');

let inputString = '';
let currentLine = 0;

process.stdin.on('data', function (inputStdin) {
    inputString += inputStdin;
});

process.stdin.on('end', function () {
    inputString = inputString.split('\n');
    main();
});

function readLine() {
    return inputString[currentLine++];
}

function stdDev(arr) {
    const n = arr.length;
    const mean = arr.reduce((acc, val) => acc + val, 0) / n;
    const variance = arr.reduce((acc, val) => acc + Math.pow(val - mean, 2), 0) / n;
    const standardDeviation = Math.sqrt(variance);

    console.log(standardDeviation.toFixed(1));
}

function main() {
    const n = parseInt(readLine().trim(), 10);
    const vals = readLine().replace(/\s+$/g, '').split(' ').map(valsTemp => parseInt(valsTemp, 10));

    stdDev(vals);
}
```

### Python 3

```python
import math


def std_dev(arr):
    n = len(arr)
    mean = sum(arr) / n
    variance = sum((x - mean) ** 2 for x in arr) / n
    return math.sqrt(variance)


n = int(input())
arr = list(map(int, input().split()))

print(round(std_dev(arr), 1))
```

### Java

```java
import java.util.*;

public class Solution {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr = new int[n];
        long sum = 0;
        for (int i = 0; i < n; i++) {
            arr[i] = sc.nextInt();
            sum += arr[i];
        }

        double mean = (double) sum / n;
        double variance = 0;
        for (int x : arr) {
            variance += Math.pow(x - mean, 2);
        }
        variance /= n;

        double standardDeviation = Math.sqrt(variance);
        System.out.printf("%.1f%n", standardDeviation);
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
    vector<int> arr(n);
    double sum = 0;
    for (int i = 0; i < n; i++) {
        cin >> arr[i];
        sum += arr[i];
    }

    double mean = sum / n;
    double variance = 0;
    for (int x : arr) {
        variance += pow(x - mean, 2);
    }
    variance /= n;

    double standardDeviation = sqrt(variance);
    cout << fixed << setprecision(1) << standardDeviation << "\n";
    return 0;
}
```

### How the Solution Works

1. **Mean** — sum all elements and divide by `n`.
2. **Variance** — for each element, subtract the mean and square the result; sum these squared distances and divide by `n` (population variance).
3. **Standard deviation** — take the square root of the variance.
4. **Print** the result rounded to 1 decimal place.
