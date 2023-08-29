## 1. 11650번: 좌표 정렬하기

[11650번: 좌표 정렬하기](https://www.acmicpc.net/problem/11650)

2차원 평면 위에 점 N개가 있을 때, 
x, y 좌표 N개가 주어 집니다. 이때, x좌표를 기준으로 정렬하되, 
x좌표가 같으면 y좌표를 기준으로 오름차순 정렬하는 문제입니다.

그렇기 때문에 한 좌표 값을 객체로 하여 배열에 담아서 풀면 쉽게 풀 수 있는 문제입니다.

### 문제 해결 아이디어

본 문제에서 정렬할 데이터는 (x, y) 좌표 값을 가진 **객체 형태** 입니다.

다음 기준에 따라서 정렬을 수행합니다.

1. 기본적으로 x 좌표가 증가하는 순으로 정렬합니다.
2. 만약에 x 좌표가 같으면 y 좌표가 증가하는 순으로 정렬합니다.

### 내가 시도한 코드(정답 인정)

```jsx
// 1. 11650번: 좌표 정렬하기
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split('\n');
let n = Number(input[0]);
let arr = [];
let answer = "";

for(let i = 1; i <= n; i++){
  const [x,y] = input[i].split(" ").map(Number);
  let arrObj = {x,y};
  arr.push(arrObj);
}

function compareFn(a,b){
  if(a.x === b.x){
    return a.y - b.y
  } else {
    return a.x - b.x
  }
}

arr.sort(compareFn)

for(i = 0; i < arr.length; i++){
  answer += (arr[i].x + " " + arr[i].y + "\n");
}

console.log(answer)
```

문제 풀이 아이디어 대로 구현 시도해보았습니다. 비교하는 함수에서 

만약 비교할 두 요소의 x 좌표가 같다면 y 좌표를 기준으로 오름차순 (a.y - b.y) 정렬 했고, 

그렇지 않다면 서로 x좌표가 다른 경우니까, x 좌표를 기준으로 오름차순 (a.x - b.x) 정렬했습니다.

### 강사님 정답 코드

```jsx
let fs = require("fs");
let input = fs.readFileSync("./dev/stdin").toString().split('\n');

let n = Number(input[0]);
let data = [];

for(let i = 1; i <= n; i++){
  const [x,y] = input[i].split(" ").map(Number);
  data.push([x,y]);
}

function compareFn(a,b){
  if(a[0] !== b[0]) return a[0] - b[0]
  else return a[1] - b[1]
}
data.sort(compareFn)

let answer = "";
for(let point of data){
  answer += (point[0] + " " + point[1] + "\n");
}

console.log(answer)
```

제 코드와 대 부분 비슷하고 대신 새로운 객체를 할당하지 않고 2차원 배열로 만들어서 비교한것이 다른 점 입니다.

## 2. 11651번: 좌표 정렬하기 2

[11651번: 좌표 정렬하기 2](https://www.acmicpc.net/problem/11651)

아까 위에서 푼 문제와 매우 유사하며, 다른 점이라면 x 좌표를 우선으로 기준 하지 않고 y좌표를 우선으로 기준을 한다는 점이 달라서 문제 해결 아이디어도 비슷합니다.

### 내가 시도한 코드(정답 인정)

```jsx
// 2. 11651번: 좌표 정렬하기 2
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split('\n');
let n = Number(input[0]);
let arr = [];
let answer = "";

for(let i = 1; i <= n; i++){
  const [x,y] = input[i].split(" ").map(Number);
  let arrObj = {x,y};
  arr.push(arrObj);
}

function compareFn(a,b){
  if(a.y !== b.y){
    return a.y - b.y
  } else {
    return a.x - b.x
  }
}

arr.sort(compareFn)

for(i = 0; i < arr.length; i++){
  answer += (arr[i].x + " " + arr[i].y + "\n");
}

console.log(answer)
```

위에 문제와 비슷하고 비교 함수에 비교 **조건이 조금 달라졌습니다.**

기존에는 x 좌표가 같다면 y를 비교했는데 이번에는 y 좌표가 같지 않다면 y를 비교 하는 코드로 바꾸었습니다. 이렇게 하면 조건문 밑 줄에 코드는 변경 안 해도 정답이 완성이 됩니다.

## 3. 1181번: 단어 정렬

[1181번: 단어 정렬](https://www.acmicpc.net/problem/1181)

알파벳 소문자로 이루어진 N개의 단어를

1. 길이가 짧은 순으로
2. 길이가 같으면 사전 순으로 

위 조건을 사용해서 정렬하는 문제 입니다. 단 중복되는 단어는 제거 해야 합니다.

### 문제 해결 아이디어

이 문제는 중복되는 문자는 제거 해야 하므로 집합(set) 자료 형을 사용해서 문제를 풀면 간단하게 중복을 제거 할 수 있습니다.

중복이 제거된 집합 자료를 다시 배열로 변경하고, 다음과 같은 조건에 따라 정렬을 수행하여야 합니다.

1. 길이가 짧은 단어를 더 앞에 오게 오름차순 정렬합니다.
2. 길이가 같다면 사전 순으로 오름차순 정렬 합니다.

### 내가 시도한 방법 (오답 처리)

```jsx
// 3. 1181번: 단어 정렬
let fs = require("fs");
let input = fs.readFileSync("./dev/stdin").toString().split('\n');
let n = Number(input[0]);
let data = new Set(input.slice(1))
let arr = Array.from(data);

arr.sort((a,b) => {
  if(a.length !== b.length) return a.length - b.length
  else return a - b;
});

let answer = "";
for(let word of arr){
  answer += word + "\n";
}
console.log(answer);
```

중복 원소 제거는 set 자료 구조에 입력 값 들을 넣어서 중복을 제거 했고, 그 다음 다시 배열로 변경하여 sort 메서드를 사용할 수 있게 변경하였습니다.

정렬 시 비교 함수 에서 길이가 같지 않을 경우는 길이 순으로 정렬하는 조건까지는 잘 수행했으나,

그 다음 **else 에서 처리에서 오류가 난 것 같습니다.**

문자열의 경우 a - b로 빼기 연산자를 사용해도 문자열이기 때문에 

**서로 연산이 안될 것**을 예상하지 못했던 것 같습니다. 

이 경우 서로 값을 비교해서 -1, 1, 0 을 리턴 해주는 방식을 사용해야 할 것 같습니다.

다음부터는 문자열을 비교할 때는 빼기 연산자를 사용하는 실수를 하지 않게 조심해야 하겠습니다.

### 정답 코드

```jsx
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split('\n');
let n = Number(input[0]);
let data = new Set();

for(let i = 1; i <= n; i++){
  data.add(input[i]);
}
const arr = Array.from(data);

arr.sort((a,b) => {
  if(a.length !== b.length) return a.length - b.length
  else {
      if(a < b) return -1;
      else if(a > b) return 1;
      else return 0;
  };
});

for(let i = 0; i < arr.length; i++){
  console.log(arr[i])
}
```

만약 문자열의 길이가 서로 같다면 

사전 에서 더 앞에 오는지 뒤에 오는지 조건문을 통해 비교하고,

앞에 오는 단어라면 -1, 뒤에 오는 단어라면 1, 같다면 0을 반환하게 해서 

문자열을 정렬하면 해결 할 수 있습니다.