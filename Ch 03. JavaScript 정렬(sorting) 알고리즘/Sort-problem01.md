## 1. 2752번 세수정렬

[2752번: 세수정렬](https://www.acmicpc.net/problem/2752)

정수 3개가 주어지면 가장 작은 수, 그 다음 수, 가장 큰 수를 출력하는 문제입니다.

### 내가 시도한 방법

```jsx
// 1. 2752번 세수정렬
let fs = require("fs");
let input = fs.readFileSync("stdin.txt").toString().split('\n');
let arr = input[0].split(" ").map(Number)

console.log(...(arr.sort((a,b) => a-b)))
```

간단하게 javascript sort 메서드를 사용해서 정렬하는 방법이 가장 쉬운 방법 같습니다.

또한 시간 복잡도 *O(NlogN)* 을 보장하기 때문에 성능 에서도 이점이 있습니다.

## 2. 2750번 수 정렬하기

[2750번: 수 정렬하기](https://www.acmicpc.net/problem/2750)

N개의 수가 주어 졌을때, 오름차순으로 정렬하는 프로그램을 작성하는 문제입니다.

입력값은 1 ~ 1000까지 정수이기 때문에 시간 복잡도 에서 비교적 자유롭습니다.

### 내가 시도한 방법

```jsx
// 2. 2750번 수 정렬하기
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split('\n');
let arr = input.slice(1);
let answer = "";

arr.sort((a,b) => a-b);
for(const num of arr){
  answer += num + "\n";
}
console.log(answer)
```

저는 arr.sort 메서드를 사용해서 오름차순 으로 정렬후에 answer 변수에 한 줄 씩 띄어서 저장한 다음 콘솔로 출력하는 방법을 시도했습니다. 분명 에디터 상에서도 정답 예시와 똑같이 출력 되었으나, 백준 채점창에서는 계속해서 ‘출력 형식이 잘못되었습니다’ 라는 문구만 나왔습니다.

정확한 원인을 파악하지 못하였으나 입력받은 문자를 배열로 만드는 과정에서 slice 메서드를 사용했는데, 아마도 정답 코드와 그 부분이 다르기 때문에 문제가 생긴것으로 추측됩니다.

### 정답 코드

```jsx
// 1. 2752번 세수정렬
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split('\n');
let n = Number(input[0])
let arr = [];

for(let i = 1; i <= n; i++){
  arr.push(Number(input[i]))  
}
arr.sort((a,b)=>a-b);
let answer = "";

for(let i = 0; i < arr.length; i++){
  answer += arr[i] + "\n";
}
console.log(answer)
```

빈 배열을 하나 만들고 input에 있는 숫자를 for 반복문을 사용해서 한 줄씩 배열에 넣어주고, 그 다음 정렬을 진행하는 방식을 사용했습니다.

## 3. 2751번 수 정렬하기2

[2751번: 수 정렬하기 2](https://www.acmicpc.net/problem/2751)

2750번 문제와 유사하지만 입력값으로 1 ~ 1,000,000 까지 주어지기 때문에 성능을 고려해서 풀어야 하는 문제입니다.

### 정답 코드

```jsx
// 3. 2751번 수 정렬하기2
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split('\n');
let n = Number(input[0])
let arr = [];

for(let i = 1; i <= n; i++){
  arr.push(Number(input[i]))  
}
arr.sort((a,b)=>a-b);
let answer = "";

for(let i = 0; i < arr.length; i++){
  answer += arr[i] + "\n";
}
console.log(answer)
```

2750번 문제도 javascript 배열 내장 메서드인 sort를 사용했기 때문에 똑같은 코드를 제출해도 정답 판정을 받을 수 있습니다.

## 4. 11004번 K번째 수

[11004번: K번째 수](https://www.acmicpc.net/problem/11004)

N개의 수가 주어지면 오름차순 정렬한 후 앞에서 부터 K 번째 수를 구하는 프로그램을 작성하는 문제입니다.

첫 줄에는 N, K 가 주어지고 두 번째 줄에는 정렬이 필요한 수가 제공됩니다.

### 정답 코드

```jsx
// 4. 11004번 K번째 수
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split('\n');
let [n,k] = input[0].split(" ").map(Number);
let arr = input[1].split(" ").map(Number);
// 배열을 오름차순 정렬 합니다.
arr.sort((a,b)=>a-b);
// k번째 (인덱스 번호니까 k - 1) 수를 출력합니다.
console.log(arr[k-1])
```

첫 번째 줄에 제공된 문자들을

1. 공백을 기준으로 배열로 만들고
2. map(Number)를 사용해서 정수로 변환하고
3. 구조 분해 할당을 통해서 첫 번째 숫자를 n, 두 번째 숫자를 k로 저장합니다.

두 번째 줄에 제공된 문자도 마찬가지로 공백을 기준으로 배열 만든 후 정수로 변환합니다.

정수로 저장된 배열은 오름차순 정렬 후 `arr[k-1]` 를 출력합니다.