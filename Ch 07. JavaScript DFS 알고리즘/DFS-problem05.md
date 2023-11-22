## 1. 10026번: 적록색약

[10026번: 적록색약](https://www.acmicpc.net/problem/10026)

일반 사람이 보이는 구역과 적록 색약이 보는 구역의 수를 구하는 문제입니다.

### 문제 해결 아이디어

1. 먼저 일반 사람이 보는 구역의 수를 DFS를 이용하여 구한 후에
2. R ⇒ G로 바꾸고, visited 배열도 초기화 한 후에
3. 적록 색약이 보는 구역의 수를 구합니다.

### 정답 코드 예시

이전에 다른 문제 풀 때 문자열끼리 비교하면 시간 초과가 나는 경우가 있어서 문자열을 `charCodeAt()` 을 사용해서 숫자로 바꿔주었습니다.

```jsx
// 1. 10026번: 적록색약
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
const n = Number(input[0]);
const graph = [];
let visited = [];
for (let i = 0; i < n; i++) {
  visited.push(new Array(n).fill(false));
  graph.push(input[i + 1].split("").map((char) => char.charCodeAt()));
}

let result1 = 0; // 적록색약이 아닌사람
let result2 = 0; // 적록색약인 사람

const dx = [-1, 1, 0, 0];
const dy = [0, 0, -1, 1];

function dfs(x, y) {
  // 아직 방문하지 않았다면
  if (!visited[x][y]) {
    visited[x][y] = true;
    for (let i = 0; i < 4; i++) {
      let nx = x + dx[i];
      let ny = y + dy[i];
      // 바깥으로 나가는 경우라면 무시
      if (nx < 0 || ny < 0 || nx >= n || ny >= n) continue;
      if (graph[x][y] === graph[nx][ny]) dfs(nx, ny);
    }
    return true;
  }
  return false;
}
// 정상인 확인
for (let i = 0; i < n; i++)
  for (let j = 0; j < n; j++) if (dfs(i, j)) result1++;

// R => G로 바꾸어주기
for (let i = 0; i < n; i++)
  for (let j = 0; j < n; j++) if (graph[i][j] === 82) graph[i][j] = 71;

// visited 배열 초기화
visited = [];
for (let i = 0; i < n; i++) visited.push(new Array(n).fill(false));
// 적록색약 확인
for (let i = 0; i < n; i++)
  for (let j = 0; j < n; j++) if (dfs(i, j)) result2++;

console.log(result1 + " " + result2);
```

## 2. 14502번: 연구소

[14502번: 연구소](https://www.acmicpc.net/problem/14502)

상하좌우로 퍼지는 바이러스를 벽 3개를 세워서 최대한 안 퍼지게 하여 안전한 구역의 수를 최대로 하는 문제입니다.

### 문제 해결 아이디어

백트래킹 알고리즘으로 중복 없는 조합을 찾는 알고리즘을 응용해야 합니다.

1. 벽 3개를 설치하는 모든 경우의 수를 고려해야 합니다.
2. 안전 구역의 크기를 계산하여 그중 가장 큰 수만 저장합니다.

### 정답 코드 예시

```jsx
// 2. 14502번: 연구소
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
const [n, m] = input[0].split(" ").map(Number);
let data = []; // 초기 맵 리스트
let temp = []; // 벽을 설치한 뒤의 맵 리스트
for (let i = 1; i <= n; i++) {
  const line = input[i].split(" ").map(Number);
  data.push(line);
  temp.push(new Array(m).fill(0));
}
const dx = [-1, 1, 0, 0];
const dy = [0, 0, -1, 1];
let result = 0;

// DFS를 이용해서 바이러스가 사방으로 퍼지게 합니다.
function virus(x, y) {
  for (let i = 0; i < 4; i++) {
    let nx = x + dx[i];
    let ny = y + dy[i];
    if (nx < 0 || ny < 0 || nx >= n || ny >= m) continue;
    if (temp[nx][ny] === 0) {
      temp[nx][ny] = 2; // 해당위치에 바이러스 배치
      virus(nx, ny); // 남은 방향에 바이러스 배치 수행
    }
  }
}
// 현재 맵에서 안전영역의 크기를 계산합니다.
function getSafeCount() {
  let count = 0;
  for (let i = 0; i < n; i++)
    for (let j = 0; j < m; j++) if (temp[i][j] === 0) count++;
  return count;
}
// 울타리 설치하는 DFS 메서드입니다.
function dfs(depth) {
  if (depth === 3) {
    for (let i = 0; i < n; i++) {
      for (let j = 0; j < m; j++) {
        temp[i][j] = data[i][j];
      }
    }
    for (let i = 0; i < n; i++) {
      for (let j = 0; j < m; j++) {
        if (temp[i][j] === 2) virus(i, j); // 바이러스를 확산하기
      }
    }
    result = Math.max(result, getSafeCount()); // 각 경우의 수 마다 안전영역이 최대값인지 확인
    return;
  }
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < m; j++) {
      if (data[i][j] === 0) {
        data[i][j] = 1;
        dfs(depth + 1);
        data[i][j] = 0;
      }
    }
  }
}
dfs(0);
console.log(result);
```