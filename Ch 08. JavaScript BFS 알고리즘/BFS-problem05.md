## [1. 2638번: 치즈](https://www.acmicpc.net/problem/2638)

[2638번: 치즈](https://www.acmicpc.net/problem/2638)

치즈의 4변 중에서 2변 이상이 실내 공기와 접촉해 있다면 1시간 만에 녹아 없어집니다.

치즈의 모양 맵이 주어지면 몇 시간 만에 치즈가 녹아 없어질지 구하는 문제입니다.

### 문제 해결 아이디어

**[BFS파트]**

0, 0의 자리는 무조건 치즈가 없으므로 그곳부터 시작해서 bfs를 사용해서 상하좌우로 외곽의 치즈를 찾습니다.

결과적으로 **bfs는 공기에 대한 정보(0)만 진행합니다. 치즈에 대한 정보(1)는 카운트만 진행합니다.**

**[녹이기 파트]**

카운트가 2 이상인 치즈를 없앱니다.

- 시간만 충분하다면 모든 치즈는 무조건 없어진다고 볼 수 있습니다.
- 한 면만 닿은 경우는 무시해야 합니다.

### **정답 코드 예시**

0, 0 부터 시작해서 2변이 둘러싸여 있는지를 판단하는 것이 매우 어렵게 느껴졌습니다.

맵에 카운트를 올려줌으로써 해결할 수 있다는 점도 배웠고, 녹이기 파트를 따로 빼서 다 녹였는지 확인하는 것도 익혀둘 필요가 있을 것 같습니다.

```tsx
// 1. 2638번: 치즈
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
class Queue {
  constructor() {
    this.storage = new Map();
    this.front = 0;
    this.rear = 0;
  }

  size() {
    return this.storage.size;
  }

  add(value) {
    if (!this.storage.size) {
      this.storage.set(0, value);
    } else {
      this.rear += 1;
      this.storage.set(this.rear, value);
    }
  }

  pop() {
    const item = this.storage.get(this.front);
    if (this.storage.size === 1) {
      this.storage.clear();
      this.front = 0;
      this.rear = 0;
    } else {
      this.storage.delete(this.front);
      this.front += 1;
    }
    return item;
  }
}
const [n, m] = input[0].split(" ").map(Number);
const dx = [-1, 1, 0, 0];
const dy = [0, 0, -1, 1];

let graph = []; // 2차원 배열 맵 입력받기
for (let i = 1; i <= n; i++) {
  let row = input[i].split(" ").map(Number);
  graph.push(row);
}
function bfs() {
  const queue = new Queue();
  let visited = [];
  for (let i = 0; i < n; i++) visited.push(new Array(m).fill(false));
  visited[0][0] = true; // 제일 왼쪽 위에서 출발
  queue.add([0, 0]); // 0, 0 부터 너비 우선탐색 진행

  // 큐가 빌때까지 반복하기
  while (queue.size() !== 0) {
    const [x, y] = queue.pop();
    for (let i = 0; i < 4; i++) {
      const nx = x + dx[i];
      const ny = y + dy[i];
      // 맵을 벗어나는 경우는 무시
      if (nx < 0 || nx >= n || ny < 0 || ny >= m) continue;
      if (!visited[nx][ny]) {
        if (graph[nx][ny] >= 1) graph[nx][ny] += 1;
        else {
          queue.add([nx, ny]);
          visited[nx][ny] = true;
        }
      }
    }
  }
}
function isMelted() {
  let finish = true;
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < m; j++) {
      // 녹일 치즈라면
      if (graph[i][j] >= 3) {
        graph[i][j] = 0; // 녹이기
        finish = false;
      } else if (graph[i][j] === 2) graph[i][j] = 1; // 한 면만 닿은 경우는 무시;
    }
  }
  return finish;
}

let result = 0;
while (true) {
  bfs();
  if (isMelted()) {
    console.log(result); // 전부 녹았다면
    break;
  } else result += 1; // 녹지 않았다면 1시간을 더해준다.
}
```

## [2. 16953번: A → B](https://www.acmicpc.net/problem/16953)

[16953번: A → B](https://www.acmicpc.net/problem/16953)

정수 A를 B로 바꾸려고 할 때 아래 두 조건에 따라 계산하여 바꾼다면 몇 번 연산하면 될지 최솟값을 구하는 문제입니다.

- 2를 곱한다.
- 1을 수의 가장 오른쪽에 추가한다. `(10 * A) + 1`

### 문제 해결 아이디어

각각의 수를 노드로 표현해서 그래프 형식으로 BFS를 수행하면 해결할 수 있습니다.

### **정답 코드 예시**

```tsx
// 2. 16953번: A → B
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
class Queue {
  constructor() {
    this.storage = new Map();
    this.front = 0;
    this.rear = 0;
  }

  size() {
    return this.storage.size;
  }

  add(value) {
    if (!this.storage.size) {
      this.storage.set(0, value);
    } else {
      this.rear += 1;
      this.storage.set(this.rear, value);
    }
  }

  pop() {
    const item = this.storage.get(this.front);
    if (this.storage.size === 1) {
      this.storage.clear();
      this.front = 0;
      this.rear = 0;
    } else {
      this.storage.delete(this.front);
      this.front += 1;
    }
    return item;
  }
}
const [a, b] = input[0].split(" ").map(Number);
const visited = new Set();
let found = false;

const queue = new Queue();
queue.add([a, 0]);

while (queue.size() !== 0) {
  const [now, depth] = queue.pop();
  // 범위를 벗어나는 경우 무시
  if (now > 1e9) continue;
  // 목표값에 도달한 경우
  if (now === b) {
    console.log(depth + 1); // 지금까지 연산 횟수 + 1 (맨처음 숫자도 연산으로 치는것같다.)
    found = true;
    break;
  }
  for (let oper of ["*", "+"]) {
    let next = now;
    if (oper === "*") next *= 2;
    if (oper === "+") {
      next *= 10;
      next += 1;
    }
    if (!visited.has(next)) {
      queue.add([next, depth + 1]);
      visited.add(next);
    }
  }
}
if (!found) console.log(-1);
```