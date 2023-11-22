## 1. 9466번: 텀 프로젝트

[9466번: 텀 프로젝트](https://www.acmicpc.net/problem/9466)

모든 학생이 프로젝트를 함께하고 싶은 학생을 한 명씩 선택합니다. (자기 자신 선택도 가능)

이 때 어느 프로젝트 팀에도 속하지 못한 학생들의 수를 계산하는 문제 입니다.

### 문제 해결 아이디어

사이클을 구성하는 부분 그래프에 포함된 노드의 개수를 세서 해결할 수 있습니다.

### 정답 예시 코드

```tsx
// 1. 9466번: 텀 프로젝트
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
let testCase = Number(input[0]);

function isCycle(x, graph, visited, finished, result) {
  visited[x] = true; // 현재 노드 방문 처리
  let y = graph[x]; // 내가 원하는 팀원을 선택하기
  // 내가 선택한 팀원이 아직 선택되지 않았다면
  if (!visited[y]) {
    isCycle(y, graph, visited, finished, result);
  } else if (!finished[y]) {
    // 팀원 선택을 한적이 있고, 완료되지 않았다면 사이클이 발생한 상황입니다.
    while (y !== x) {
      result.push(y); // 선택한 팀원을 결과에 넣고
      y = graph[y]; // 선택한 팀원이 선택한 사람을 다음에 검사
    }
    result.push(x);
  }
  // 현재 노드의 처리가 완료됨
  finished[x] = true;
}

for (let i = 0; i < testCase; i++) {
  let n = Number(input[i * 2 + 1]);
  let graph = [0, ...input[i * 2 + 2].split(" ").map(Number)];
  let visited = new Array(n + 1).fill(false);
  let finished = new Array(n + 1).fill(false);
  let result = [];

  // 모든 노드에 사이클이 있는지 확인 합니다.
  for (let i = 1; i <= n; i++) {
    // 방문한적이 없는 요소를 확인
    if (!visited[i]) isCycle(i, graph, visited, finished, result);
  }

  console.log(n - result.length);
}
```

## 2. 2668번: 숫자고르기

[2668번: 숫자고르기](https://www.acmicpc.net/problem/2668)

첫째 줄에 1~N까지 숫자가 있고 둘째 줄에 1 이상 N 이하의 정수가 들어있습니다.

| 1 | 2 | 3 | 4 | 5 | 6 | 7 |
| --- | --- | --- | --- | --- | --- | --- |
| 3 | 1 | 1 | 5 | 5 | 4 | 6 |

이중 첫째 줄에서 숫자를 적절히 뽑아서 뽑힌 정수와 그 밑에 있는 정수의 집합이 일치하게끔 숫자를 뽑는 문제입니다.

### 문제 해결 아이디어

[9466번: 텀 프로젝트](https://www.acmicpc.net/problem/9466) 문제와 동일하게 사이클을 구성하는 노드의 개수를 세서 해결 할 수 있으므로 로직이 거의 동일 합니다.

### 정답 예시 코드

```tsx
// 2. 2668번: 숫자고르기
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
let n = Number(input[0]);
let graph = [0];
for (let i = 1; i <= n; i++) {
  let y = Number(input[i]);
  graph.push(y);
}
let visited = new Array(n + 1).fill(false);
let finished = new Array(n + 1).fill(false);
let result = [];

function dfs(x) {
  visited[x] = true;
  let y = graph[x];
  if (!visited[y]) {
    dfs(y);
  } else if (!finished[y]) {
    while (y !== x) {
      result.push(y);
      y = graph[y];
    }
    result.push(x);
  }

  finished[x] = true;
}

for (let i = 1; i <= n; i++) {
  if (!visited[i]) dfs(i);
}
console.log(result.length);
result.sort((a, b) => a - b);
for (let x of result) console.log(x);
```