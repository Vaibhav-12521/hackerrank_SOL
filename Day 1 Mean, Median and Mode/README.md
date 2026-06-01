# Day 1: Mean, Median, and Mode

## Objective  

### In this challenge, we practice calculating the mean, median, and mode.

  
 
## Task

Given an array, `X`, of `N` integers, calculate and print the respective **mean**, **median**, and **mode** on separate lines. If your array contains more than one modal value, choose the numerically smallest one.
 
**Note:** Other than the modal value (which will always be an integer), your answers should be in decimal form, rounded to a scale of 1 decimal place (i.e., `12.3`, `7.0` format).

## Example

```
N = 6
X = [1, 2, 3, 4, 5, 5]
```

- The mean is `20 / 6 = 3.3`.
- The median is `(3 + 4) / 2 = 3.5`.
- The mode is `5` because `5` occurs most frequently.

## Input Format

The first line contains an integer, `N`, the number of elements in the array.

The second line contains `N` space-separated integers that describe the array's elements.

## Constraints

- `10 <= N <= 2500`
- `0 < x[i] <= 10^5`, where `x[i]` is the `i`-th element of the array.

## Output Format

Print 3 lines of output in the following order:

1. Print the **mean** on the first line to a scale of 1 decimal place (i.e., `12.3`, `7.0`).
2. Print the **median** on a new line, to a scale of 1 decimal place (i.e., `12.3`, `7.0`).
3. Print the **mode** on a new line. If more than one such value exists, print the numerically smallest one.

## Sample Input

```
10
64630 11735 14216 99233 14470 4978 73429 38120 51135 67060
```

## Sample Output

```
43900.6
44627.5
4978
```

## Explanation

### Mean

We sum all `N` elements in the array, divide the sum by `N`, and print our result on a new line.

```
      x0 + x1 + x2 + ... + x9     439006
mu = -------------------------- = -------- = 43900.6
              10                     10
```

### Median

To calculate the median, we need the elements of the array to be sorted in either non-increasing or non-decreasing order. The sorted array is:

```
X = {4978, 11735, 14216, 14470, 38120, 51135, 64630, 67060, 73429, 99233}
```

We then average the two middle elements:

```
          x4 + x5     89255
median = --------- = ------- = 44627.5
             2          2
```

and print our result on a new line.

### Mode

We find the number of occurrences of all the elements in the array:

```
 4978  : 1
11735  : 1
14216  : 1
14470  : 1
38120  : 1
51135  : 1
64630  : 1
67060  : 1
73429  : 1
99233  : 1
```

Every number occurs once, making `1` the maximum number of occurrences for any number in `X`. Because we have multiple values to choose from, we want to select the smallest one, `4978`, and print it on a new line.

## Solutions

### JavaScript (Node.js)

```javascript
function processData(input) {
    const lines = input.trim().split('\n');
    const n = parseInt(lines[0]);
    const arr = lines[1].split(' ').map(Number);

    // Mean
    const mean = arr.reduce((sum, num) => sum + num, 0) / n;

    // Median
    const sorted = [...arr].sort((a, b) => a - b);
    let median;
    if (n % 2 === 0) {
        median = (sorted[n / 2 - 1] + sorted[n / 2]) / 2;
    } else {
        median = sorted[Math.floor(n / 2)];
    }

    // Mode
    const freq = {};
    let maxFreq = 0;
    let mode = Infinity;

    for (const num of arr) {
        freq[num] = (freq[num] || 0) + 1;

        if (
            freq[num] > maxFreq ||
            (freq[num] === maxFreq && num < mode)
        ) {
            maxFreq = freq[num];
            mode = num;
        }
    }

    console.log(mean.toFixed(1));
    console.log(median.toFixed(1));
    console.log(mode);
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
from collections import Counter

n = int(input())
arr = list(map(int, input().split()))

# Mean
mean = sum(arr) / n

# Median
arr.sort()
if n % 2 == 0:
    median = (arr[n // 2 - 1] + arr[n // 2]) / 2
else:
    median = arr[n // 2]

# Mode: highest frequency, smallest value on a tie
freq = Counter(arr)
max_freq = max(freq.values())
mode = min(num for num, count in freq.items() if count == max_freq)

print(round(mean, 1))
print(round(median, 1))
print(mode)
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

        // Mean
        double mean = (double) sum / n;

        // Median
        Arrays.sort(arr);
        double median;
        if (n % 2 == 0) {
            median = (arr[n / 2 - 1] + arr[n / 2]) / 2.0;
        } else {
            median = arr[n / 2];
        }

        // Mode: highest frequency, smallest value on a tie
        Map<Integer, Integer> freq = new HashMap<>();
        int maxFreq = 0, mode = Integer.MAX_VALUE;
        for (int num : arr) {
            int count = freq.merge(num, 1, Integer::sum);
            if (count > maxFreq || (count == maxFreq && num < mode)) {
                maxFreq = count;
                mode = num;
            }
        }

        System.out.printf("%.1f%n", mean);
        System.out.printf("%.1f%n", median);
        System.out.println(mode);
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
    long long sum = 0;
    for (int i = 0; i < n; i++) {
        cin >> arr[i];
        sum += arr[i];
    }

    // Mean
    double mean = (double) sum / n;

    // Median
    sort(arr.begin(), arr.end());
    double median;
    if (n % 2 == 0) {
        median = (arr[n / 2 - 1] + arr[n / 2]) / 2.0;
    } else {
        median = arr[n / 2];
    }

    // Mode: highest frequency, smallest value on a tie
    map<int, int> freq;
    int maxFreq = 0, mode = INT_MAX;
    for (int num : arr) {
        int count = ++freq[num];
        if (count > maxFreq || (count == maxFreq && num < mode)) {
            maxFreq = count;
            mode = num;
        }
    }

    cout << fixed << setprecision(1) << mean << "\n";
    cout << fixed << setprecision(1) << median << "\n";
    cout << mode << "\n";
    return 0;
}
```

### How the Solution Works

1. **Parse input** — read `N` from the first line and the array of integers from the second line.
2. **Mean** — sum every element with `reduce`, divide by `N`, and format to 1 decimal place with `toFixed(1)`.
3. **Median** — sort a copy of the array ascending. For an even `N`, average the two middle elements (`sorted[n/2 - 1]` and `sorted[n/2]`); for an odd `N`, take the single middle element. Format to 1 decimal place.
4. **Mode** — count occurrences in a `freq` map. Track the value with the highest frequency, and on a tie keep the numerically smallest one (`num < mode`). Print the mode as an integer.
```
