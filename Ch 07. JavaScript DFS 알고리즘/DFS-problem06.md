## 1. 10819번: 차이를 최대로

[10819번: 차이를 최대로](https://www.acmicpc.net/problem/10819)

N개의 정수가 주어지면 순서를 적절히 바꾸어서 아래 식대로 계산했을 때 최댓값을 구하는 문제입니다.

- |A[0] - A[1]| + |A[1] - A[2]| + ... + |A[N-2] - A[N-1]|

### 문제 해결 아이디어

DFS를 활용해서 중복 없는 조합의 수를 고른 후에, 고른 조합에 대해서 최댓값을 구하여 문제를 풀었습니다.

### 정답 코드 예시

수들의 순서를 바꿔가면서 중복 없는 조합을 찾는 로직을 활용해서 조합들을 찾고 그중 가장 큰 수를 저장하여 문제를 해결할 수 있습니다.

```tsx
// 1. 10819번: 차이를 최대로
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
const n = Number(input[0]);
const arr = input[1].split(" ").map(Number);
const visited = new Array(10).fill(false);
const result = [];
let answer = -1e9;

function dfs(depth) {
  if (depth === n) {
    let current = 0;
    for (let i = 0; i < n - 1; i++)
      current += Math.abs(result[i] - result[i + 1]);
    answer = Math.max(answer, current);
  }
  for (let i = 0; i < n; i++) {
    if (visited[i]) continue;
    visited[i] = true;
    result.push(arr[i]);
    dfs(depth + 1);
    visited[i] = false;
    result.pop();
  }
}

dfs(0);
console.log(answer);
```

## 2. 14888번: **연산자 끼워넣기**

[14888번: 연산자 끼워넣기](https://www.acmicpc.net/problem/14888)

N개의 수로 이루어진 수열 사이사이에 N-1개의 연산자 끼워 넣어서 얻을 수 있는 최솟값과 최댓값을 구하는 문제입니다.

### 문제 해결 아이디어

4개의 연산자의 개수를 이용해서 연산자들의 조합을 구해보고 마지막에 최솟값과 최댓값을 구하는 방식으로 문제를 해결할 수 있습니다.

### 정답 코드 예시

사실 이전 문제와 비슷할 줄 알았는데, 생각해 보니 연산자에 따라 계산법이 달라지기에 if문을 사용해서 각 상황마다 다르게 계산해야 하는 점을 알게 되었습니다.

```tsx
// 2. 14888번: 연산자 끼워넣기
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
const n = Number(input[0]);
const arr = input[1].split(" ").map(Number);
let [add, sub, mul, div] = input[2].split(" ").map(Number);

let minValue = 1e9;
let maxValue = -1e9;

function dfs(depth, current) {
  if (depth === n) {
    minValue = Math.min(minValue, current);
    maxValue = Math.max(maxValue, current);
    return;
  }
  if (add > 0) {
    add--;
    dfs(depth + 1, current + arr[depth]);
    add++;
  }
  if (sub > 0) {
    sub--;
    dfs(depth + 1, current - arr[depth]);
    sub++;
  }
  if (mul > 0) {
    mul--;
    dfs(depth + 1, current * arr[depth]);
    mul++;
  }
  if (div > 0) {
    div--;
    console.log(~~(current / arr[depth]));
    dfs(depth + 1, ~~(current / arr[depth]));
    div++;
  }
}
dfs(1, arr[0]);

console.log(maxValue);
console.log(minValue);
```