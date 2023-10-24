## 1. 15686번: 치킨 배달

[15686번: 치킨 배달](https://www.acmicpc.net/problem/15686)

N x N 크기의 도시에 0은 빈칸, 1은 집, 2는 치킨집일 때, 프랜차이즈 본사에서 M개의 치킨집만 남기기로 결정했다면 이 때 집들과 거리가 가까운 치킨집을 남기고 싶다.

| 0 | 0 | 1 | 0 | 0 |
| --- | --- | --- | --- | --- |
| 0 | 0 | 2 | 0 | 1 |
| 0 | 1 | 2 | 0 | 0 |
| 0 | 0 | 1 | 0 | 0 |
| 0 | 0 | 0 | 0 | 2 |

M개의 치킨집만 남겼을 때 **모든 집의 치킨 거리의 합을** 출력하는 문제입니다.

> 💡 **치킨 거리**는 집과 가장 가까운 치킨 집 사이의 거리를 뜻합니다.
> 

### 문제 해결 아이디어

선택된 치킨집들 사이에서는 그 순서를 바꾼다고 하더라도 결과 값은 같기 때문에 모든 조합을 뽑는다고 이해할 수 있습니다.

백트래킹 알고리즘에서 사용한 조합을 찾는 알고리즘을 응용하면 문제 해결하는데 도움이 됩니다.

**문제 해결 순서**

1. 치킨집, 집을 나누어서 배열에 저장합니다.
2. 모든 치킨집에 대해서 m개까지 중복 없이 조합을 해봅니다.
3. 조합한 치킨집에서 모든 집들의 치킨 거리 합을 구해봅니다.
4. 조합중에 가장 합이 작은 것만 저장합니다. 
`Math.min(현재까지 최소 값, 모든 집들의 치킨 거리 합)`

### 정답 코드 예시

```tsx
// 1. 15686번: 치킨 배달
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
const [n, m] = input[0].split(" ").map(Number);
const chicken = [];
const house = [];
for (let i = 0; i < n; i++) {
  const line = input[i + 1].split(" ").map(Number);
  for (let j = 0; j < n; j++) {
    if (line[j] === 1) house.push([i, j]); // 집일 경우
    if (line[j] === 2) chicken.push([i, j]); // 치킨집일 경우
  }
}
const visited = new Array(chicken.length).fill(false);
const selected = [];
let answer = 1e9;

// 백트래킹 알고리즘으로 중복없는 치킨집 조합을 찾습니다.
function dfs(depth, start) {
  // depth가 m에 도달하면 종료
  if (depth === m) {
    const combination = [];
    for (let i of selected) combination.push(chicken[i]);
    let sum = 0;
    // 각 집마다 치킨집과의 거리를 확인하기
    for (let [hx, hy] of house) {
      let distance = 1e9; // 치킨집과의 거리
      // 모든 치킨집 순회
      for (let [cx, cy] of combination) {
        distance = Math.min(distance, Math.abs(hx - cx) + Math.abs(hy - cy)); // 집 => 모든 치킨집 거리중 제일 작은거 선택
      }
      sum += distance; // 모든 치킨집을 돌아보고 가장 가까운 치킨집과의 거리들의 합을 구하기
    }
    answer = Math.min(answer, sum); // 최종적으로 모든 조합중 가장 작은 값만 선택
    return;
  }
  for (let i = start; i < chicken.length; i++) {
    if (visited[i]) continue; // 이미 처리된 원소라면 무시
    selected.push(i);
    visited[i] = true;
    dfs(depth + 1, i + 1);
    selected.pop();
    visited[i] = false;
  }
}

dfs(0, 0);
console.log(answer);
```

## 2. 2667번: 단지번호붙이기

[2667번: 단지번호붙이기](https://www.acmicpc.net/problem/2667)

정사각형 모양의 지도에서 1은 집이 있는 곳, 0은 집이 없는 곳을 나타냅니다.

여기서 상하좌우로 연결된 집의 모임을 단지라고 정의합니다. 이때 단지의 개수와 집의 수를 오름차순 정렬하여 출력하는 문제입니다.

### 문제 해결 아이디어

본 문제는 연결 요소를 찾는 문제 입니다. 구체적으로는 각 연결 요소에 포함된 요소의 개수를 계산할 필요가 있습니다.

연결 요소를 찾는다는 점에서 [1012번: 유기농 배추](https://www.acmicpc.net/problem/1012) 문제와 유사한 구조를 가지고 있습니다.

**문제 해결 순서**

1. 단지를 찾기 위해서 2중 for문을 사용해서 dfs를 호출합니다.
2. dfs에서는 집을 찾으면 그 개수를 `result`에 담아서 반환합니다.
3. dfs의 반환 값이 0이라면 단지를 못 찾은 경우이고, 0보다 크다면 단지를 찾은 상황입니다.

### 정답 코드 예시

단지의 개수를 찾는 것 까지는 유기농 배추의 문제와 동일해서 어렵지 않았습니다.

하지만 재귀 함수를 통해서 계속해서 return 값으로 누적해서 단지의 수를 구할 생각은 하지 못 했던 것 같습니다. 조금씩 이런 유형의 문제에 익숙해져서 다음에는 꼭 풀 수 있도록 해야 하겠습니다.

```tsx
// 2. 2667번: 단지번호붙이기
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
let n = Number(input[0]);
let graph = [];
const answer = [];
for (let i = 0; i < n; i++) graph[i] = input[i + 1].split("").map(Number);

for (let i = 0; i < n; i++) {
  for (let j = 0; j < n; j++) {
    let current = dfs(i, j);
    if (current > 0) answer.push(current);
  }
}

function dfs(x, y) {
  if (x <= -1 || x >= n || y <= -1 || y >= n) return 0;
  if (graph[x][y] === 1) {
    graph[x][y] = -1;
    let result = 1;
    result += dfs(x - 1, y);
    result += dfs(x, y - 1);
    result += dfs(x + 1, y);
    result += dfs(x, y + 1);
    return result;
  }

  return 0;
}
answer.sort((a, b) => a - b);
console.log(answer.length + "\n" + answer.join("\n"));
```