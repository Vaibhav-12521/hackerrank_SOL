# Day 1: Quartiles

## Objective

In this challenge, we practice calculating quartiles.

## Task

Given an array, `arr`, of `n` integers, calculate the respective first quartile (`Q1`), second quartile (`Q2`), and third quartile (`Q3`). It is guaranteed that `Q1`, `Q2`, and `Q3` are integers.

## Example

```
arr = [9, 5, 7, 1, 3]
```

The sorted array is `[1, 3, 5, 7, 9]`, which has an odd number of elements. The lower half consists of `[1, 3]`, and its median is `(1 + 3) / 2 = 2`. The middle element is `5` and represents the second quartile. The upper half is `[7, 9]` and its median is `(7 + 9) / 2 = 8`. Return `[2, 5, 8]`.

```
arr = [1, 3, 5, 7]
```

The array is already sorted. The lower half is `[1, 3]` with a median `= (1 + 3) / 2 = 2`. The median of the entire array is `(3 + 5) / 2 = 4`, and of the upper half is `(5 + 7) / 2 = 6`. Return `[2, 4, 6]`.

## Function Description

Complete the `quartiles` function in the editor below.

`quartiles` has the following parameter:

- `int arr[n]`: the values to segregate

### Returns

- `int[3]`: the medians of the left half of `arr`, `arr` in total, and the right half of `arr`.

## Input Format

The first line contains an integer, `n`, the number of elements in `arr`.

The second line contains `n` space-separated integers, each an `arr[i]`.

## Constraints

- `5 <= n <= 50`
- `0 < arr[i] < 100`, where `arr[i]` is the `i`-th element of the array.

## Sample Input

```
STDIN                    Function
-----                    --------
9                        arr[] size n = 9
3 7 8 5 12 14 21 13 18   arr = [3, 7, 8, 5, 12, 14, 21, 13, 18]
```

## Sample Output

```
6
12
16
```

## Explanation

`arr_sorted = [3, 5, 7, 8, 12, 13, 14, 18, 21]`. There is an odd number of elements, and the middle element (the median) is `12`.

As there are an odd number of data points, we do not include the median (the central value in the ordered list) in either half:

```
Lower half (L): 3, 5, 7, 8
Upper half (U): 13, 14, 18, 21
```

Now find the quartiles:

- `Q1` is the `median(L)`. So, `Q1 = (5 + 7) / 2 = 6`.
- `Q2` is the `median(X)`. So, `Q2 = 12`.
- `Q3` is the `median(U)`. So, `Q3 = (14 + 18) / 2 = 16`.

## Solutions

### JavaScript (Node.js)

```javascript
'use strict';

const fs = require('fs');

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

function quartiles(arr) {
    arr.sort((a, b) => a - b);
    const n = arr.length;

    function getMedian(data) {
        const len = data.length;
        const mid = Math.floor(len / 2);
        if (len % 2 !== 0) {
            return data[mid];
        } else {
            return Math.floor((data[mid - 1] + data[mid]) / 2);
        }
    }

    const midIndex = Math.floor(n / 2);
    const q2 = getMedian(arr);

    const lowerHalf = arr.slice(0, midIndex);
    const upperHalf = (n % 2 === 0) ? arr.slice(midIndex) : arr.slice(midIndex + 1);

    const q1 = getMedian(lowerHalf);
    const q3 = getMedian(upperHalf);

    return [q1, q2, q3];
}

function main() {
    const ws = fs.createWriteStream(process.env.OUTPUT_PATH);

    const n = parseInt(readLine().trim(), 10);
    const data = readLine().replace(/\s+$/g, '').split(' ').map(dataTemp => parseInt(dataTemp, 10));

    const res = quartiles(data);

    ws.write(res.join('\n') + '\n');
    ws.end();
}
```

### Python 3

```python
def get_median(data):
    length = len(data)
    mid = length // 2
    if length % 2 != 0:
        return data[mid]
    return (data[mid - 1] + data[mid]) // 2


def quartiles(arr):
    arr.sort()
    n = len(arr)
    mid = n // 2

    q2 = get_median(arr)
    lower = arr[:mid]
    upper = arr[mid:] if n % 2 == 0 else arr[mid + 1:]

    return [get_median(lower), q2, get_median(upper)]


n = int(input())
data = list(map(int, input().split()))

for value in quartiles(data):
    print(value)
```

### Java

```java
import java.util.*;

public class Solution {
    static int getMedian(int[] data, int start, int end) {
        int length = end - start;
        int mid = start + length / 2;
        if (length % 2 != 0) {
            return data[mid];
        }
        return (data[mid - 1] + data[mid]) / 2;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int n = sc.nextInt();
        int[] arr = new int[n];
        for (int i = 0; i < n; i++) arr[i] = sc.nextInt();

        Arrays.sort(arr);
        int mid = n / 2;

        int q2 = getMedian(arr, 0, n);
        int q1 = getMedian(arr, 0, mid);
        int q3 = getMedian(arr, (n % 2 == 0) ? mid : mid + 1, n);

        System.out.println(q1);
        System.out.println(q2);
        System.out.println(q3);
    }
}
```

### C++

```cpp
#include <bits/stdc++.h>
using namespace std;

int getMedian(const vector<int>& data, int start, int end) {
    int length = end - start;
    int mid = start + length / 2;
    if (length % 2 != 0) {
        return data[mid];
    }
    return (data[mid - 1] + data[mid]) / 2;
}

int main() {
    int n;
    cin >> n;
    vector<int> arr(n);
    for (int i = 0; i < n; i++) cin >> arr[i];

    sort(arr.begin(), arr.end());
    int mid = n / 2;

    int q2 = getMedian(arr, 0, n);
    int q1 = getMedian(arr, 0, mid);
    int q3 = getMedian(arr, (n % 2 == 0) ? mid : mid + 1, n);

    cout << q1 << "\n" << q2 << "\n" << q3 << "\n";
    return 0;
}
```

### How the Solution Works

1. **Sort** the array in non-decreasing order.
2. **Q2 (median)** - the median of the whole array. For an odd count, it's the middle element; for an even count, the average of the two middle elements.
3. **Split into halves** - the lower half is everything before the middle. For an odd count, exclude the middle element from both halves; for an even count, split exactly down the middle.
4. **Q1 and Q3** - `Q1` is the median of the lower half and `Q3` is the median of the upper half.
5. **Print** `Q1`, `Q2`, and `Q3`, each on its own line.
