## 1. 9663번: N-Queen

[9663번: N-Queen](https://www.acmicpc.net/problem/9663)

N X N 크기의 체스판에 퀸 N개를 놓았을때 서로 공격할 수 없게 놓는 방법의 수를 구하는 문제 입니다.

### 문제 해결 아이디어

1. 퀸은 본인의 행에 위치한 말을 공격할 수 있어서 1행에는 1개의 퀸만 놓을 수 있습니다.
2. 이미 존재하는 퀸의 상하좌우 및 대각선 위치가 아닌 위치에 다른 퀸 놓을 수 있습니다.

위와 같은 조건에 상충되지 않는 조건을 만족하는 위치에 대해서만 재귀 함수를 호출합니다.

### 정답 예시 코드

서로 공격을 못하게 하려면 x축의 위치, y축의 위치, 대각선(a-x의 절대값)이 아니어야 합니다.

그렇다는 것은 **한 줄에 1개의 퀸**만 놓을 수 있다는 것이므로

`possible` 함수로 한 줄에서 퀸이 서로 공격할 수 있는지 검사해서

depth가 N이면 모두 서로 공격못하는 경우의 수라는 뜻 이므로

현재 탐색을 종료하고 경우의 수에 1을 더하면 됩니다.

```jsx
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
const n = Number(input[0]);
let queens = [];
let answer = 0;

function possible(x, y) {
  for (let [a, b] of queens) {
    if (a === x || b === y) return false;
    if (Math.abs(a - x) === Math.abs(b - y)) return false;
  }
  return true;
}

function dfs(row) {
  if (row === n) answer += 1;
  for (let i = 0; i < n; i++) {
    if (!possible(row, i)) continue;
    queens.push([row, i]);
    dfs(row + 1);
    queens.pop();
  }
}
dfs(0);
console.log(answer);
```

## 2. 1987번: 알파벳

[1987번: 알파벳](https://www.acmicpc.net/problem/1987)

C X R 칸으로 된 보드가 있습니다. 각 칸에는 대문자 알파벳이 적혀 있고 1행 1열에 말이 있습니다.

말은 상하좌우로 움직일 수 있는데, 지금까지 지나온 알파벳이 적힌 칸은 못지나갈때 말이 최대한 몇칸 갈수있는지 확인하는 문제 입니다.

### 문제 해결 아이디어

한 번 나온 알파벳은 못 지나가기 때문에 26개의 대문자 알파벳의 방문 여부를 저장할 `visited` 배열을 만들어서 해결하는 것이 좋습니다.

> **💡문제 해결 Tip!**
**charCodeAt()을 사용해서 대문자를 숫자로 변경**해서 검사해야지 정답 판정을 받을 수 있습니다. 숫자로 안 바꾸면 시간 초과로 통과를 못하는 것 같습니다.
> 

### 정답 예시 코드

상하좌우로 4방향 모두 검사하면서 이미 방문했던 문자 코드를 가지고 있는지 확인합니다.

보드 밖으로 나가거나, 이미 방문한 알파벳이면 무시하고 계속 진행하고 가장 높은 depth 인지를 확인해서 저장하면 정답 판정을 받을 수 있습니다.

```jsx
// 2. 1987번: 알파벳
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
const [r, c] = input[0].split(" ").map(Number);
const board = [];
for (let i = 1; i <= r; i++) board.push(input[i]);
const charCode = (x, y) => board[x][y].charCodeAt() - 65;
const dx = [-1, 1, 0, 0]; // 상하좌우 방향 선택을 위한 상수
const dy = [0, 0, -1, 1];
const visited = new Array(26).fill(false); // 방문한적 있는지 체크
let answer = 0;

function dfs(depth, x, y) {
  answer = Math.max(answer, depth);
  for (let i = 0; i < 4; i++) {
    // 4방향을 검사하기
    let nx = x + dx[i]; // 검사할 x방향
    let ny = y + dy[i]; // 검사할 y방향
    if (nx < 0 || nx >= r || ny < 0 || ny >= c) continue; // 보드를 넘어가면 무시
    if (visited[charCode(nx, ny)]) continue; // 이미 있는 알파벳이면 무시
    visited[charCode(nx, ny)] = true;
    dfs(depth + 1, nx, ny);
    visited[charCode(nx, ny)] = false;
  }
}
visited[charCode(0, 0)] = true;
dfs(1, 0, 0);
console.log(answer);
```

## 3. 2529번: 부등호

[2529번: 부등호](https://www.acmicpc.net/problem/2529)

부등호가 주어지면 부등호 앞뒤에 숫자를 넣어서 서로 관계를 만족하는 수들 중에 최소값, 최대값을 구하는 문제 입니다. (같은 수를 두번 고를 수 없습니다)

### 문제 해결 아이디어

총 10개의 숫자가 있는 상황에서 중복없는 모든 순열을 고려해야 합니다.

최대 `10!` (10 factorial)개의 경우의 수가 나오더라도 1,000,000 이 넘지 않아서 완전 탐색으로 진행하여도 문제가 없습니다.

부등호 보다 1개 더 수를 골라서 부등호 앞뒤로 조건을 만족하는지 모든 경우의 수에 검사합니다.

### 정답 예시 코드

여기서 가장 작은 수는 어떻게 구해야 하고 큰수는 어떻게 구할지 너무 어려웠습니다.

정답코드에서는 첫번째로 나온 조합이 가장 작은 수일것이고, 맨 마지막으로 나온 조합이 가장큰 수 이기 때문에 아래 코드처럼 `first = ""` 일 경우 아직 조합이 안나온 경우니까 이때만 한 번 `first`에 저장하고 그 다음 부터는 계속 `last`에 덮어쓰기 하면 정답 판정을 받을 수 있었습니다.

```jsx
// 3. 2529번: 부등호
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
const k = Number(input[0]);
const arr = input[1].split(" ");
const visited = new Array(10).fill(false);
const selected = [];
let first = ""; // 가장 처음 통과한 문자열
let last = ""; // 가장 마지막으로 통과한 문자열

function dfs(num) {
  // 만약 부등호의 갯수 + 1 만큼 숫자를 다 뽑았으면
  let check = true; // 부등식을 계속해서 만족하는지 체크
  if (num === k + 1) {
    // 부등호의 갯수만큼 체크
    for (let i = 0; i < k; i++) {
      if (arr[i] === "<") {
        // 앞보다 뒤가 더 큰지
        if (selected[i] > selected[i + 1]) check = false;
      } else if (arr[i] === ">") {
        // 앞보다 뒤가 더 작은지
        if (selected[i] < selected[i + 1]) check = false;
      }
    }
    // 만약 위의 검사를 했는데도 계속 check 가 true 면
    if (check) {
      last = "";
      for (let x of selected) last += x + "";
      if (first === "") first = last;
    }
    return;
  }
  for (let i = 0; i < 10; i++) {
    if (visited[i]) continue;
    visited[i] = true;
    selected.push(i);
    dfs(num + 1);
    visited[i] = false;
    selected.pop();
  }
}
dfs(0);
console.log(last + "\n" + first);
```