## 1. 18870번: 좌표 압축

[18870번: 좌표 압축](https://www.acmicpc.net/problem/18870)

N개의 좌표를 입력 받아서 각 값을 크기 순위로 변경하는 문제 입니다.

### 문제 해결 아이디어

**좌표 압축** 문제는 쉽게 말해 각 값을 **크기 순위로 변경**하는 것입니다.

그래서 숫자를 키, 순위를 값으로 할당해서 각 수 가 순위가 몇 위 인지를 저장해 놓습니다.

그리고 하나씩 키를 사용해서 순위들을 불러오면 되는 문제입니다.

1. 입력 배열: arr = [2, 4, -10, 4, 9]
2. 중복 제거를 위해서 집합으로 만듭니다.
3. 중복 제거한 집합을 배열로 바꾸어 오름차순 정렬을 수행합니다.
4. 숫자를 키, 순위를 값으로 한 맵을 만듭니다.
    
    ```jsx
    myMap = {
    	-10: 0,
    	-9: 1,
    	2: 2,
    	4: 3
    }
    ```
    
5. 하나씩 매핑 값을 출력 합니다.

### 내가 시도한 코드(시간 초과)

```jsx
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
let n = Number(input[0]);
const arr1 = input[1].split(" ").map(Number);
const arr2 = [...arr1];
arr2.sort((a, b) => a - b);

let answer = "";
arr1.forEach(function (value) {
  answer += arr2.indexOf(value) + " ";
});
console.log(answer.trim());
```

저는 순위를 정하기 위해서 기존 배열 `arr1`과 정렬이 완료된 배열 `arr2`을 만들어서

정렬 완료된 배열을 검사해서 각각 인덱스 번호가 몇인지를 출력하려고 했습니다.

그러나 여러 배열 관련 메서드 들이 많이 들어가서 인지 시간 초과로 실패하였습니다.

### 정답 코드

```jsx
// 1. 18870번: 좌표 압축
let fs = require("fs");
let input = fs.readFileSync("dev/stdin").toString().split("\n");
const arr = input[1].split(" ").map(Number);
// 키값으로 만들어야 하기 때문에 중복을 제거 합니다.
const uniqueArr = [...new Set(arr)];
uniqueArr.sort((a, b) => a - b);

// 숫자를 키 값으로 사용할 수 있고,
// 키 의 순서도 기억하는 Map을 사용한다.
let myMap = new Map();
for (let i = 0; i < uniqueArr.length; i++) {
  myMap.set(uniqueArr[i], i);
}

let answer = "";
for (const x of arr) {
  answer += myMap.get(x) + " ";
}
console.log(answer);
```

순위를 숫자를 키로 해서 저장하려면 Map 이라는 자료 구조를 사용해야 하는데요, 같은 숫자가 있으면 제대로 순위가 저장이 안 되서 중복을 제거합니다.

그 다음 Map 자료 구조를 사용해서 숫자를 키 값으로 하고 인덱스 번호를 값으로 주어서 순위를 저장합니다.

그리고 원래 배열에 값은 키 값이니까, 키 값을 가지고 불러오는 get 메서드를 사용해서 순위를 불러옵니다.

## 2. 10814번: 나이순 정렬

[10814번: 나이순 정렬](https://www.acmicpc.net/problem/10814)

### 내가 시도한 코드(정답 인정)

```jsx
// 2. 10814번: 나이순 정렬
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
let n = Number(input[0]);
let arr = [];

// 2차원 배열로 나이와 이름을 담아 줍니다.
for (let i = 1; i <= n; i++) {
  const [age, name] = input[i].split(" ");
  arr.push([Number(age), name]);
}

function compareFn(a, b) {
	// 나이가 같지 않다면 오름차순 정렬합니다.
  if (a[0] !== b[0]) return a[0] - b[0];
  // 나이가 같다면 원래 순서대로 놔둡니다.
  else {
    return 0;
  }
}
arr.sort(compareFn);

let answer = "";
for (let i = 0; i < arr.length; i++) {
  answer += arr[i][0] + " " + arr[i][1] + "\n";
}
console.log(answer);
```

이 문제도 일종에 객체에 대한 정렬로 이해할 수 있습니다.

그래서 나이가 같다면 원래 순서인 가입 순으로 정렬하면 되기 때문에 비교 함수 실행해서 나이가 같은 경우는 0을 리턴 했습니다.

## 3. 1427번: 소트인사이드

[1427번: 소트인사이드](https://www.acmicpc.net/problem/1427)

수를 각 자리 수마다 내림차순 정렬하는 문제 입니다. 

### 문제 해결 아이디어

하나의 수가 들어오면 각 자리수 마다 0~9까지의 숫자가 몇 번 등장했는지를 세서 빈도 수 만큼 출력해서 정답을 구할 수 있습니다.

### 내가 시도한 코드(정답 이긴함..)

```jsx
// 3. 1427번: 소트인사이드
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
let arr = input[0].split("");

arr.sort((a, b) => b - a);

console.log(arr.join(""));
```

간단하게 들어온 숫자를 다 나누어서 배열로 만들고

정렬한 후에 다시 join메서드로 합쳐주어서 정답을 구했습니다.

### 강사님 정답 코드

```jsx
// 3. 1427번: 소트인사이드
/**
 * 강사님 정답 코드
 * 0~9까지 수가 몇 번 등장했는지 세고
 * 빈도 수 만큼 출력하는 방식으로 정답을 구했습니다.
 */
let fs = require("fs");
let input = fs.readFileSync("/dev/stdin").toString().split("\n");
let n = input[0];
let count = new Array(10).fill(0);

// 한개의 문자 마다 숫자를 확인 합니다.
for (let x of n) {
  // 숫자가 등장하면 그 인덱스 번호의 수를 하나씩 올려줍니다.
  count[Number(x)]++;
}
let answer = "";
// 내림차순이므로 9부터 출력합니다.
for (let i = 9; i >= 0; i--) {
  // 등장한 숫자만큼 반복하여 출력합니다.
  for (let j = 0; j < count[i]; j++) answer += i + "";
}
console.log(answer);
```

1. 문자를 하나하나 확인해서 몇 번 숫자가 나왔는지를 셉니다.
예를 들어 `500613009` 이 들어왔다면 5 부터 시작해서 처음에는 5가 1번 등장했으니까 5번 인덱스에 숫자를 1씩 올려줍니다. 그 다음 문자도 검사해서 해당 인덱스에 카운트를 한 개씩 올려줍니다.
2. 9부터 시작해서 0까지 (내림차순이기 때문에) 등장한 숫자 만큼 반복해서 출력합니다. 예를 들자면 시작번호인 9번 인덱스를 검사해서 9는 1번만 등장했으니까 반복문을 통해서 1번만 반복해서 정답 문자열에 더해 줍니다.