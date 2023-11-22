## [1. 16234번: 인구 이동](https://www.acmicpc.net/problem/16234)

[16234번: 인구 이동](https://www.acmicpc.net/problem/16234)

N X N개의 땅이 있고, 그 안에 1x1 칸마다 국가가 있습니다. 하루동안 인구 이동을 진행한다고 했을때 아래 방법에 의해 인구이동이 없을 때까지 반복합니다. 각 나라의 인구수가 주어지면 며칠 동안 인구이동이 발생하는지 구하는 문제입니다.

- 국경선을 공유하는 두 나라의 인구 차이가 L명 이상, R명 이하라면, 두 나라가 공유하는 국경선을 오늘 하루 동안 연다.
- 위의 조건에 의해 열어야하는 국경선이 모두 열렸다면, 인구 이동을 시작한다.
- 국경선이 열려있어 인접한 칸만을 이용해 이동할 수 있으면, 그 나라를 오늘 하루 동안은 연합이라고 한다.
- 연합을 이루고 있는 각 칸의 인구수는 (연합의 인구수) / (연합을 이루고 있는 칸의 개수)가 된다. 편의상 소수점은 버린다.
- 연합을 해체하고, 모든 국경선을 닫는다.

### 문제 해결 아이디어

1. 모든 나라의 위치에서 상하좌우로 국경선을 열 수 있는지 확인해야 합니다.
2. 같은 연합끼리 인구를 동일하게 분배합니다.

### **정답 코드 예시**

주변 국가를 찾아서 인구이동을 하는 것은 여려 BFS 문제를 풀다 보니 감이 왔는데, 종료 조건을 만드는 부분이나 몇 번 이동했는지를 찾는 것이 어렵게 느껴졌습니다.

```tsx
// 1. 16234번: 인구 이동
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
const [n, l, r] = input[0].split(" ").map(Number);
const graph = [];
for (let i = 1; i <= n; i++) {
  let row = input[i].split(" ").map(Number);
  graph.push(row);
}
const dx = [-1, 1, 0, 0];
const dy = [0, 0, -1, 1];
let totalCount = 0;

// 더 이상 인구 이동을 할 수 없을 때까지 반복합니다.
while (true) {
  let union = Array.from(new Array(n), () => new Array(n).fill(-1));
  let index = 0; // 연합 번호를 구분하기 위한 인덱스
  // 모든 나라들을 검사해서 연합한 국가 찾기
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {
      // 아직 처리하지 않은 국가라면
      if (union[i][j] === -1) {
        // 연합국가 찾고 인구이동 진행
        bfs(i, j, index, union);
        index++;
      }
    }
  }
  if (index === n * n) break; // 모든 인구이동이 끝난경우
  totalCount += 1; // 1번 인구이동 완료할 때마다 +1
}
function bfs(x, y, index, union) {
  // (x,y) 위치와 연결된 나라 정보를 담는 리스트 입니다.
  let united = [[x, y]];
  const queue = new Queue();
  queue.add([x, y]);
  union[x][y] = index; // 현재 연합의 번호를 할당
  let summary = graph[x][y]; // 현재까지 인구 저장하기
  let cnt = 1; // 현재 연합국가의 수 저장
  while (queue.size() !== 0) {
    let [x, y] = queue.pop();
    for (let i = 0; i < 4; i++) {
      let nx = x + dx[i];
      let ny = y + dy[i];
      // 영역을 넘지 않고 연합이 아직 없는 국가라면
      if (nx >= 0 && nx < n && ny >= 0 && ny < n && union[nx][ny] === -1) {
        let diff = Math.abs(graph[nx][ny] - graph[x][y]); // 주변국가와 인구차이 계산
        // 차이가 L보다 크거나 같고 R보다 작거나 같은경우
        if (l <= diff && diff <= r) {
          queue.add([nx, ny]);
          union[nx][ny] = index; // 현재 연합에 추가
          summary += graph[nx][ny]; // 연합 총 인구에 추가
          cnt += 1;
          united.push([nx, ny]);
        }
      }
    }
  }
  for (let unit of united) {
    let [i, j] = unit;
    graph[i][j] = parseInt(summary / cnt);
  }
}
console.log(totalCount);
```

## [2. 3190번: 뱀](https://www.acmicpc.net/problem/3190)

[3190번: 뱀](https://www.acmicpc.net/problem/3190)

‘Dummy’라는 도스 게임이 있습니다. 뱀이 나와서 기어다니다가 사과를 먹으면 뱀이 길어지고 
벽 또는 자기 자신의 몸과 부딪히면 게임이 끝나게 됩니다. 
사과의 위치와 뱀의 이동경로가 주어지면 몇 초에 게임이 끝나는지 시뮬레이션하는 문제입니다.

### 문제 해결 아이디어

2차원 배열상의 맵에서 뱀이 이동합니다. 따라서 뱀이 동, 남, 서, 북의 위치로 이동하는 기능을 구현해야하며
매 시점마다 뱀이 존재하는 위치를 항상 2차원 리스트에 기록해야 합니다.

### **정답 코드 예시**

문제를 정확히 이해하지 못해서 풀지 못 했던 것 같습니다. 

최대한 문제를 많이 풀어서 자신감도 쌓고 요령도 터득해가야 할 것 같습니다.

```tsx
// 2. 3190번: 뱀
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
const k = Number(input[1]);
const data = [];
for (let i = 0; i < n + 1; i++) {
  data.push(new Array(n + 1).fill(0));
}
for (let i = 2; i <= k + 1; i++) {
  let [a, b] = input[i].split(" ").map(Number);
  data[a][b] = 1; // 사과가 있는곳은 1로 표시
}
const l = Number(input[k + 2]); // 뱀의 방향전환 횟수
const info = [];
for (let i = k + 3; i < k + l + 3; i++) {
  let [x, c] = input[i].split(" ");
  info.push([Number(x), c.charCodeAt()]); // D = 68, L = 76으로 바꾸어서 저장
}

// 처음에는 오른쪽을 보고 있으므로 (동, 남, 서, 북)
const dx = [0, 1, 0, -1];
const dy = [1, 0, -1, 0];
function turn(direction, c) {
  if (c === 76) {
    direction = direction - 1;
    if (direction === -1) direction = 3; // 동쪽에서 왼쪽으로 꺾으면 북쪽으로
  } else direction = (direction + 1) % 4;
  return direction;
}

let [x, y] = [1, 1]; // 뱀의 머리 위치
data[x][y] = 2;
let direction = 0; // 처음은 동쪽을 보고 있음. 0, 1, 2, 3 (동, 남, 서, 북)
let time = 0; // 시작한 뒤에 지난 '초' 시간
let index = 0; // 다음에 회전할 정보
const q = new Queue();
q.add([x, y]);
while (true) {
  let nx = x + dx[direction];
  let ny = y + dy[direction];
  // 맵 범위안에 있고, 뱀의 몸통이 없는 곳
  if (1 <= nx && nx <= n && 1 <= ny && ny <= n && data[nx][ny] !== 2) {
    // 사과가 없다면 이동 후에 꼬리를 제거
    if (data[nx][ny] === 0) {
      data[nx][ny] = 2;
      q.add([nx, ny]); // 새로운 머리위치를 큐에 넣기
      let [px, py] = q.pop(); // 꼬리를 제거
      data[px][py] = 0; // 꼬리는 0으로 변경
    }
    // 사과가 있다면 이동 후에 꼬리 그대로 두기
    if (data[nx][ny] === 1) {
      data[nx][ny] = 2;
      q.add([nx, ny]); // 새로운 머리위치를 큐에 넣기
    }
  } else {
    time += 1;
    break;
  }
  [x, y] = [nx, ny];
  time += 1;
  // 현재 시간이 회전할 시간인 경우
  if (index < l && time === info[index][0]) {
    // 방향을 전환하기
    direction = turn(direction, info[index][1]);
    index += 1;
  }
}
console.log(time);
```