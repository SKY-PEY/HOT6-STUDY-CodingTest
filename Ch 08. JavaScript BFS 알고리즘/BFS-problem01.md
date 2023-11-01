## 1. 1697번: 숨바꼭질

[1697번: 숨바꼭질](https://www.acmicpc.net/problem/1697)

수빈이와 동생의 위치가 주어지고, 수빈이가 1초에 이동할 수 있는 거리가 주어지면 몇 초안에 동생에게 갈 수 있는지 최소값을 구하는 문제 입니다.

### 문제 해결 아이디어

visited 배열에 각 위치까지 **몇 초가 걸릴지를** 임시로 **저장**할 수 있게 배열을 만듭니다. 

그리고 각 노드마다 1초에 이동할 수 있는 **[node - 1, node + 1, node * 2] 인 위치로 너비 우선 탐색**을 진행합니다. (큐에 담기) 

그리고 하나씩 꺼내면서 다시 1초에 이동 가능한 거리에 따라 다시 탐색하는 식으로 진행하다 보면, **가장 빨리 큐 에서 동생의 위치에 도달**하는 노드가 발생하게 되고

동생의 위치를 찾으면 **값을 반환한 후 종료**하는 방법으로 문제를 해결할 수 있습니다. 

### 정답 코드 예시

기본적인 BFS 문제이긴 했으나, 큐 자료구조에 대한 이해나 BFS의 작동 방식에 대한 이해가 부족하여 혼자 힘으로 풀지는 못했습니다.  

```tsx
// 1. 1697번: 숨바꼭질
let fs = require("fs");
let input = fs.readFileSync("dev/stdin").toString().split("\n");
const [n, k] = input[0].split(" ").map(Number);
const visited = new Array(100001).fill(0); // 각 위치까지 몇초가 걸리는지 저장

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

// BFS 메서드 정의
function bfs() {
  const queue = new Queue();
  // 현재 노드를 방문 처리
  queue.add(n);
  // 큐가 빌 때까지 반복
  while (queue.size() !== 0) {
    // 큐에서 하나의 원소를 뽑아 출력하기
    const v = queue.pop();
    if (v === k) return visited[v];

    // 아직 방문하지 않은 인접한 원소들을 큐에 삽입
    for (let i of [v - 1, v + 1, v * 2]) {
      // 바깥으로 나가는것을 방지
      if (i < 0 || i >= 100001) continue;
      if (visited[i] === 0) {
        // 현재 위치 걸린시간 = 이전 위치까지 걸린 시간 + 1
        visited[i] = visited[v] + 1;
        queue.add(i);
      }
    }
  }
}

console.log(bfs());
```

## 2. 7562번: 나이트의 이동

[7562번: 나이트의 이동](https://www.acmicpc.net/problem/7562)

나이트가 한 번에 이동할 수 있는 칸이 주어지면, 몇 번 안에 정해진 칸으로 이동할 수 있을지 최소값을 구하는 문제 입니다. ([이전 숨바꼭질 문제](https://www.acmicpc.net/problem/1697)와 매우 유사)

### 문제 해결 아이디어

숨바꼭질 문제와 유사성이 많습니다. 

갈 수 있는 방향이 8방향이라는 점과 이동 여부를 판단하는 배열이 2차원 배열이라는 점이 다르고

전체적인 로직은 비슷하게 진행됩니다.

### 정답 코드 예시

앞선 숨바꼭질 문제를 2차원 배열로 바꾼 버전이라고 생각하고 풀었더니 조금 쉽게 느껴졌습니다.

```tsx
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

let testCase = Number(input[0]);
let visited;
let moves = [
  [-2, -1],
  [-2, 1],
  [-1, -2],
  [-1, 2],
  [1, -2],
  [1, 2],
  [2, -1],
  [2, 1],
];

for (let i = 0; i < testCase; i++) {
  const l = Number(input[3 * i + 1]);
  const [x, y] = input[3 * i + 2].split(" ").map(Number);
  const [dx, dy] = input[3 * i + 3].split(" ").map(Number);

  visited = [];
  for (let j = 0; j < l; j++) visited[j] = new Array(l).fill(0);

  bfs(x, y, dx, dy, l);
}

function bfs(x, y, dx, dy, l) {
  const queue = new Queue();
  queue.add([x, y]);
  visited[x][y] = 1;
  // 큐가 빌 때까지 반복
  while (queue.size() !== 0) {
    // 큐에서 하나의 원소를 뽑아 출력하기
    const [cx, cy] = queue.pop();
    // 아직 방문하지 않은 인접한 원소들을 큐에 삽입
    for (let [mx, my] of moves) {
      let [nx, ny] = [cx + mx, cy + my];
      if (nx >= l || ny >= l || nx < 0 || ny < 0) continue;
      if (visited[nx][ny] === 0) {
        queue.add([nx, ny]);
        visited[nx][ny] = visited[cx][cy] + 1;
      }
    }
  }
  console.log(visited[dx][dy] - 1);
}
```