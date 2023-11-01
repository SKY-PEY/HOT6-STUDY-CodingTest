## [1. 5214번: 환승](https://www.acmicpc.net/problem/5214)

[5214번: 환승](https://www.acmicpc.net/problem/5214)

역 K개를 서로 연결하는 하이퍼튜브를 타고 1번역에서 N번역으로 가는데 방문하는 최소 역 수를 구하는 문제입니다.

### 문제 해결 아이디어

일종의 최단 거리 문제라고 볼 수 있습니다. 지나야 하는 역의 개수를 구하는 문제이기 때문에 거리 값을 구해서 2로 나누고 +1을 해주면 정답 판정을 받을 수 있습니다.

- 하이퍼튜브도 노드로 간주하고 너비 우선 탐색을 진행합니다.
- 각 하이퍼튜브가 연결하는 노드들을 모두 양방향 간선으로 연결합니다.

### **정답 코드 예시**

```tsx
// 1. 5214번: 환승
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
// N = 역의 개수, K = 간선의 개수, M = 하이퍼튜브의 개수
let [n, k, m] = input[0].split(" ").map(Number);
// 그래프의 정보 (N개의 역과 M개의 하이퍼튜브는 모두 노드)
let graph = [];
for (let i = 1; i <= n + m; i++) graph[i] = [];
for (let i = 1; i <= m; i++) {
  let arr = input[i].split(" ").map(Number);
  for (let x of arr) {
    graph[x].push(n + i); // 노드 => 하이퍼튜브
    graph[n + i].push(x); // 하이퍼튜브 => 노드
  }
}
let visited = new Set([1]); // 1번 노드에서 출발
let queue = new Queue();
queue.add([1, 1]); // 거리, 노드 번호
let found = false;

while (queue.size() !== 0) {
  let [dist, now] = queue.pop();
  // N번 노드에 도착한 경우
  if (now === n) {
    // 절반은 하이퍼튜브
    console.log(parseInt(dist / 2) + 1);
    found = true;
    break;
  }
  // 인접한 노드를 하나씩 확인
  for (let y of graph[now]) {
    // 아직 방문하지 않았다면
    if (!visited.has(y)) {
      queue.add([dist + 1, y]);
      visited.add(y);
    }
  }
}
if (!found) console.log(-1);
```

## [2. 5567번: 결혼식](https://www.acmicpc.net/problem/5567)

[5567번: 결혼식](https://www.acmicpc.net/problem/5567)

상근이라는 사람이 자신의 친구와 친구의 친구를 결혼식에 초대하려고 할 때 결혼식에 초대할 사람의 수를 구하는 문제입니다.

### 문제 해결 아이디어

BFS 호출 이후에 최단 거리가 2 이하인 노드의 수를 계산하면 됩니다.

### **정답 코드 예시**

```tsx
// 2. 5567번: 결혼식
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
const n = Number(input[0]);
const m = Number(input[1]);
const graph = [[]];
let distance = new Array(n + 1).fill(-1);
distance[1] = 0; // 시작점은 거리가 0
for (let i = 1; i <= n; i++) graph[i] = [];
for (let i = 2; i <= m + 1; i++) {
  const [x, y] = input[i].split(" ").map(Number);
  graph[x].push(y);
  graph[y].push(x);
}
const queue = new Queue();
queue.add(1); // 1을 넣어서 자기 자신의 친구를 먼저 큐에서 확인하기

while (queue.size() !== 0) {
  const now = queue.pop();
  for (let next of graph[now]) {
    if (distance[next] === -1) {
      distance[next] = distance[now] + 1;
      queue.add(next);
    }
  }
}

let result = 0;
for (let i = 1; i <= n; i++)
  if (distance[i] !== -1 && distance[i] <= 2) result++;
console.log(result - 1); // 자기 자신은 제외하여 출력
```