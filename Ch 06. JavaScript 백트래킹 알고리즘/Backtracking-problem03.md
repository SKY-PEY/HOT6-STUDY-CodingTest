## 1. 10971번: 외판원 순회 2

[10971번: 외판원 순회 2](https://www.acmicpc.net/problem/10971)

1번부터 N번까지 번호가 매겨져 있는 도시들이 있고, 도시들 사이에는 길이 있습니다. (길이 없을 수도 있음)

- 한 도시에서 출발해서 모든 도시를 거쳐서 출발한 도시로 돌아와야 합니다.
- 한 번 갔던 도시는 다시 갈수없습니다.

위 조건을 만족하는 경로중에 최소비용으로 여행하는 경로의 비용을 출력하는 문제 입니다.

### 문제 해결 아이디어

1. 한 번 갔던 도시는 다시 갈수 없으니 visited 배열에 방문한적 있는지를 체크 합니다.
2. 어디서 시작 하던지 다시 돌아와야 하므로 임의로 출발 노드를 1번 노드 부터 출발한다고 가정합니다.

### 정답 예시 코드

도시가 최대 10개 까지므로 visited 배열은 11개로 설정(돌아오는것도 체크해야해서) 하고

맨 마지막에는 무조건 시작한 1번 노드로 돌아오기 때문에 `**가야할 도시의 수 - 1**` 번까지 방문했다면 종료합니다.

가지 못하는 곳은 cost가 0이니까 return으로 종료합니다.

모든 도시의 cost를 계산하고, 마지막 방문 도시 ⇒ 1번으로 가는 cost를 계산하고 최솟값인지를 확인 하면 됩니다.

```jsx
// 1. 10971번: 외판원 순회 2
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
const n = Number(input[0]);
const arr = []; // 1~n 까지 자연수를 담은 배열
for (let i = 1; i <= n; i++) {
  const city = input[i].split(" ").map(Number);
  arr.push(city);
}
const visited = new Array(11).fill(false); // 방문한 도시인지 저장하기 위한 배열
const result = []; // 현재까지 방문한 도시를 모아놓은 배열
let minValue = 1e9; // 순회에 필요한 비용 문자열

function dfs(city) {
  // city가 끝까지 도달 했을때 지금까지 선택한 숫자들을 출력을 위해 문자열로 변경
  if (city === n - 1) {
    let totalCost = 0; // 2~N까지 총 비용
    let cur = 0; // 1번 노드 부터 출발
    for (let i = 0; i < n - 1; i++) {
      let nextNode = result[i]; // 방문한 노드들 비용을 하나씩 계산
      let cost = arr[cur][nextNode];
      if (cost === 0) return;
      totalCost += cost;
      cur = nextNode;
    }
    // 마지막엔 1번 노드로 돌아옵니다.
    let cost = arr[cur][0];
    if (cost === 0) return;
    totalCost += cost;
    minValue = Math.min(minValue, totalCost);
  }
  for (let i = 1; i < n; i++) {
    if (visited[i]) continue;
    result.push(i); // 현재 숫자를 선택
    visited[i] = true; // 방문 처리
    dfs(city + 1); // 다음 city로 넘어감
    result.pop(); // 현재 숫자를 선택 취소
    visited[i] = false; // 방문 처리 해제
  }
}
dfs(0);
console.log(minValue);
```

## 2. 2961번: 도영이가 만든 맛있는 음식

[2961번: 도영이가 만든 맛있는 음식](https://www.acmicpc.net/problem/2961)

재료 N개가 주어지면 신맛과 쓴맛을 조합해서 신맛과 쓴맛의 차이가 가장 작은 요리를 만드는 문제 입니다.

- 신맛 = 재료들의 신맛 X 신맛 (신맛의 곱)
- 쓴맛 = 재료들의 쓴맛 + 쓴맛 (쓴맛의 합)

### 문제 해결 아이디어

모든 길이에 대한 모든 조합을 구하는 문제라고 볼 수 있습니다.

ex) 재료 (A, B, C)와 (A, C, B)를 사용한 것은 같은 결과값이 나옵니다.

1. 재료 1개만 조합 ⇒ (A), (B), (C), (D)
2. 재료 2개만 조합 ⇒ (A, B), (A, C), (A, D), (B, C), (B, D), (C, D)
3. 재료 3개만 조합 ⇒ (A, B, C), (A, B, D), (A, C, D), (B, C, D)
4. 재료 4개 모두 조합 ⇒ (A, B, C, D)

### 정답 예시 코드

한개 이상 조합 했다면 신맛 쓴맛의 차이를 검사 해야 합니다.

또한 동일한 결과가 나오는 조합은 체크할 필요가 없으니까 이전에 했던것 처럼 `i = start` 로 이미 넣은 재료는 스킵합니다.

```jsx
// 2. 2961번: 도영이가 만든 맛있는 음식
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
let n = Number(input[0]);
let graph = [];
for (let i = 1; i <= n; i++) graph.push(input[i].split(" ").map(Number));
const visited = new Array(10).fill(false); // 조합했던 재료인지 저장하기 위한 배열
const result = []; // 현재까지 조합했던 재료를 모아놓은 배열
let answer = 1000000001;

function dfs(depth, start) {
	// 1개이상 조합했을때 모두 검사합니다.
  if (depth >= 1) {
    let totalX = 1; // 선택한 재료들의 신맛 (곱해야 해서 1부터 시작)
    let totalY = 0; // 선택한 재료들의 쓴맛 (더해야 하니 0부터 시작)
    for (let i of result) {
      let [x, y] = graph[i];
      totalX *= x; // 재료들의 신맛을 곱합니다.
      totalY += y; // 재료들의 쓴맛을 더합니다.
    }
    // 차이가 더 적은 값을 저장합니다.
    answer = Math.min(answer, Math.abs(totalX - totalY));
  }
  for (let i = start; i < n; i++) {
    if (visited[i]) continue;
    result.push(i); // 현재 재료를 선택
    visited[i] = true; // 방문 처리
    dfs(depth + 1, i + 1); // 다음 재료로 넘어가기
    result.pop(); // 현재 재료를 선택 취소
    visited[i] = false; // 방문 처리 해제
  }
}
dfs(0, 0);
console.log(answer);
```

## 3. 6603번: 로또

[6603번: 로또](https://www.acmicpc.net/problem/6603)

독일 로또는 1~49까지에서 6개를 뽑습니다. 

번호 뽑을때 가장 유명한 전략은 7개 이상의 수가 있는 집합중에 서 6개를 뽑는 방법 입니다. 

문제에서는 집합이 주어지고 뽑을 수 있는 모든 경우의 수를 구하는 문제 입니다.

### 문제 해결 아이디어

단순히 모든 조합을 출력하는 문제이므로 `visited`, `selected` 변수를 사용해서 이미 뽑은 번호 인지 확인 하여 모든 조합을 콘솔로 출력하면 정답판정을 받을 수 있습니다.

### 정답 예시 코드

```jsx
// 3. 6603번: 로또
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");

for (let i = 0; i < input.length; i++) {
  const data = input[i].split(" ").map(Number);
  if (data[0] === 0) break; // 마지막줄에서는 종료 합니다.
  else {
    k = data[0]; // 번호 개수
    arr = data.slice(1); // 번호들의 집합
    visited = new Array(k).fill(false); // 방문 여부 체크
    selected = []; // 번호 선택
    answer = "";
    dfs(arr, 0, 0);
    console.log(answer);
  }
}

function dfs(arr, depth, start) {
  if (depth === 6) {
    // 번호가 6개 다 선택되었을때
    for (let x of selected) answer += arr[x] + " ";
    answer += "\n";
    return;
  }
  for (let i = start; i < arr.length; i++) {
    if (visited[i]) continue;
    selected.push(i); // 현재 번호를 선택
    visited[i] = true; // 방문 처리
    dfs(arr, depth + 1, i + 1); // 다음 번호로 넘어가기
    selected.pop(); // 현재 번호를 선택 취소
    visited[i] = false; // 방문 처리 해제
  }
}
```