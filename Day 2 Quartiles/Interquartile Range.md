# Day 1: Interquartile Range

## Objective

In this challenge, we practice calculating the interquartile range. We recommend you complete the Quartiles challenge before attempting this problem.

## Task

The interquartile range of an array is the difference between its first (`Q1`) and third (`Q3`) quartiles (i.e., `Q3 - Q1`).

Given an array, `values`, of `n` integers and an array, `freqs`, representing the respective frequencies of `values`'s elements, construct a data set, `S`, where each `values[i]` occurs at frequency `freqs[i]`. Then calculate and print `S`'s interquartile range, rounded to a scale of 1 decimal place (i.e., `12.3` format).

> **Tip:** Be careful to not use integer division when averaging the middle two elements for a data set with an even number of elements, and be sure to not include the median in your upper and lower data sets.

## Example

```
values = [1, 2, 3]
freqs  = [3, 2, 1]
```

Apply the frequencies to the values to get the expanded array `S = [1, 1, 1, 2, 2, 3]`. Here `left = [1, 1, 1]`, `right = [2, 2, 3]`. The median of the left half, `Q1`, is `1.0`, the middle element. For the right half, `Q3 = 2.0`. Print the difference to one decimal place: `Q3 - Q1 = 2.0 - 1.0 = 1.0`, so print `1.0`.

## Function Description

Complete the `interQuartile` function in the editor below.

`interQuartile` has the following parameters:

- `int values[n]`: an array of integers
- `int freqs[n]`: `values[i]` occurs `freqs[i]` times in the array to analyze

### Prints

- `float`: the interquartile range to 1 place after the decimal

## Input Format

The first line contains an integer, `n`, the number of elements in arrays `values` and `freqs`.

The second line contains `n` space-separated integers describing the elements of array `values`.

The third line contains `n` space-separated integers describing the elements of array `freqs`.

## Constraints

- `5 <= n <= 50`
- `0 < values[i] <= 100`
- `0 < sum(freqs[i]) <= 10^3`
- The number of elements in `S` is equal to `sum(freqs)`.

## Output Format

Print the interquartile range for the expanded data set on a new line. Round the answer to a scale of 1 decimal place (i.e., `12.3` format).

## Sample Input

```
STDIN              Function
-----              --------
6                  arrays size n = 6
6 12 8 10 20 16    values = [6, 12, 8, 10, 20, 16]
5 4 3 2 1 5        freqs  = [5, 4, 3, 2, 1, 5]
```

## Sample Output

```
9.0
```

## Explanation

The given data is:

| Element | Frequency |
|:-------:|:---------:|
| 6 | 5 |
| 12 | 4 |
| 8 | 3 |
| 10 | 2 |
| 20 | 1 |
| 16 | 5 |

First, we create data set `S` containing the data from set `values` at the respective frequencies specified by `freqs`:

```
S = {6, 6, 6, 6, 6, 8, 8, 8, 10, 10, 12, 12, 12, 12, 16, 16, 16, 16, 16, 20}
```

As there are an even number of data points in the original ordered data set, we will split this data set exactly in half:

```
Lower half (L): 6, 6, 6, 6, 6, 8, 8, 8, 10, 10
Upper half (U): 12, 12, 12, 12, 16, 16, 16, 16, 16, 20
```

Next, we find `Q1`. There are 10 elements in the lower half, so `Q1` is the average of the middle two elements, `6` and `8`: `Q1 = (6 + 8) / 2 = 7.0`.

Next, we find `Q3`. There are 10 elements in the upper half, so `Q3` is the average of the middle two elements, `16` and `16`: `Q3 = (16 + 16) / 2 = 16.0`.

From this, we calculate the interquartile range as `Q3 - Q1 = 16.0 - 7.0 = 9.0` and print `9.0` as our answer.

## Solutions

### JavaScript (Node.js)

```javascript
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

function interQuartile(values, freqs) {
    let arr = [];

    for (let i = 0; i < values.length; i++) {
        for (let j = 0; j < freqs[i]; j++) {
            arr.push(values[i]);
        }
    }

    arr.sort((a, b) => a - b);

    function median(a) {
        let n = a.length;
        if (n % 2 === 0) {
            return (a[n / 2 - 1] + a[n / 2]) / 2;
        } else {
            return a[Math.floor(n / 2)];
        }
    }

    let n = arr.length;
    let lower, upper;

    if (n % 2 === 0) {
        lower = arr.slice(0, n / 2);
        upper = arr.slice(n / 2);
    } else {
        lower = arr.slice(0, Math.floor(n / 2));
        upper = arr.slice(Math.floor(n / 2) + 1);
    }

    let q1 = median(lower);
    let q3 = median(upper);

    console.log((q3 - q1).toFixed(1));
}

function main() {
    const n = parseInt(readLine().trim(), 10);
    const val = readLine().replace(/\s+$/g, '').split(' ').map(valTemp => parseInt(valTemp, 10));
    const freq = readLine().replace(/\s+$/g, '').split(' ').map(freqTemp => parseInt(freqTemp, 10));

    interQuartile(val, freq);
}
```

### Python 3

```python
def median(a):
    n = len(a)
    mid = n // 2
    if n % 2 == 0:
        return (a[mid - 1] + a[mid]) / 2
    return a[mid]


def inter_quartile(values, freqs):
    arr = []
    for value, freq in zip(values, freqs):
        arr.extend([value] * freq)
    arr.sort()

    n = len(arr)
    mid = n // 2
    if n % 2 == 0:
        lower, upper = arr[:mid], arr[mid:]
    else:
        lower, upper = arr[:mid], arr[mid + 1:]

    return median(upper) - median(lower)


n = int(input())
values = list(map(int, input().split()))
freqs = list(map(int, input().split()))

print(round(inter_quartile(values, freqs), 1))
```

### Java

```java
import java.util.*;

public class Solution {
    static double median(List<Integer> a) {
        int n = a.size();
        int mid = n / 2;
        if (n % 2 == 0) {
            return (a.get(mid - 1) + a.get(mid)) / 2.0;
        }
        return a.get(mid);
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] values = new int[n];
        int[] freqs = new int[n];
        for (int i = 0; i < n; i++) values[i] = sc.nextInt();
        for (int i = 0; i < n; i++) freqs[i] = sc.nextInt();

        List<Integer> arr = new ArrayList<>();
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < freqs[i]; j++) {
                arr.add(values[i]);
            }
        }
        Collections.sort(arr);

        int size = arr.size();
        int mid = size / 2;
        List<Integer> lower = arr.subList(0, mid);
        List<Integer> upper = (size % 2 == 0) ? arr.subList(mid, size) : arr.subList(mid + 1, size);

        double iqr = median(upper) - median(lower);
        System.out.printf("%.1f%n", iqr);
    }
}
```

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

double median(const vector<int>& a, int start, int end) {
    int n = end - start;
    int mid = start + n / 2;
    if (n % 2 == 0) {
        return (a[mid - 1] + a[mid]) / 2.0;
    }
    return a[mid];
}

int main() {
    int n;
    cin >> n;
    vector<int> values(n), freqs(n);
    for (int i = 0; i < n; i++) cin >> values[i];
    for (int i = 0; i < n; i++) cin >> freqs[i];

    vector<int> arr;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < freqs[i]; j++) {
            arr.push_back(values[i]);
        }
    }
    sort(arr.begin(), arr.end());

    int size = arr.size();
    int mid = size / 2;
    double q1 = median(arr, 0, mid);
    double q3 = (size % 2 == 0) ? median(arr, mid, size) : median(arr, mid + 1, size);

    cout << fixed << setprecision(1) << (q3 - q1) << "\n";
    return 0;
}
```

### How the Solution Works

1. **Expand the data** — build the full data set `S` by repeating each `values[i]` exactly `freqs[i]` times.
2. **Sort** `S` in non-decreasing order.
3. **Split into halves** — for an even count, split exactly down the middle; for an odd count, exclude the median element from both halves.
4. **Q1 and Q3** — `Q1` is the median of the lower half and `Q3` is the median of the upper half. Use floating-point division when averaging the two middle elements.
5. **Interquartile range** — print `Q3 - Q1`, rounded to 1 decimal place.
