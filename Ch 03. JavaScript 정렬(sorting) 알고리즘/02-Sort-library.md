## 이미 구현되어있는 sort() 메서드를 활용하자

알고리즘 문제 풀이시에는 사용 가능하다면 Javascript 배열에 포함 되어 있는 sort 메서드를 사용하는것을 더 추천한다.

### Array.prototype.sort 메서드 사용방법

자바스크립트 정렬 메서드에서는 정렬 기준 함수를 사용해서 반환값에 따라 오름차순, 내림차순 정렬합니다.

**정렬 기준 함수**

정렬 함수에서는 정렬 기준 함수의 반환 값을 기준으로 정렬합니다.

두 개의 원소 a,b를 입력으로 받습니다.

- 반환 값이 0보다 작은 경우 ⇒ a가 우선순위가 높아, 앞에 위치합니다.
- 반환 값이 0보다 큰 경우 ⇒ b가 우선순위가 높아, 앞에 위치합니다.
- 반환 값이 0인 경우 ⇒ a, b의 순서를 변경하지 않습니다.

### 정수에 대한 오름차순 정렬 예시

a가 b보다 작을 때, 반환 값이 음수가 되어 a가 앞에 위치합니다.

```jsx
let arr = [1,8,5,9,21,3,7,2,15];
arr.sort((a,b)=>a-b);
```

### 정수에 대한 내림차순 정렬 예시

a가 b보다 클 때, 반환 값이 음수가 되어 a가 앞에 위치합니다.

```jsx
let arr = [1,8,5,9,21,3,7,2,15];
arr.sort((a,b)=>b-a);
```

### 문자열에 대한 오름차순 정렬 예시

별도로 비교 함수를 작성하지 않으면 유니코드 순으로 정렬됩니다.

```jsx
let arr = ["fineapple", "banana", "durian", "apple", "carrot"];
arr.sort(); // apple, banana, carrot, durian, fineapple
```

### 문자열에 대한 오름차순 정렬 예시 (대소문자 구분 X)

문자열을 모두 대문자로 변경 한 후 비교하면 대소문자 구분 없이 정렬 할 수 있습니다.

```jsx
let arr = ["fineapple", "banana", "durian", "apple", "carrot"];

function compare(a, b){
	let upperCaseA = a.toUpperCase();
	let upperCaseB = a.toUpperCase();
	if(upperCaseA > upperCaseB) return -1;
	else if(upperCaseA < upperCaseB) return 1;
	else return 0;
}

arr.sort(compare);
```

### 문자열에 대한 내림차순 정렬 예시

비교 함수를 사용해서 문자열을 내림차순 정렬할 수 있습니다.

```jsx
let arr = ["fineapple", "banana", "durian", "apple", "carrot"];

function compare(a, b){
	if(a>b) return -1;
	else if(a<b) return 1;
	else return 0;
}

arr.sort(compare);
```

### 객체에 원하는 속성을 기준으로 정렬

문자열, 숫자와 마찬가지로 비교 함수를 사용하여 비교 할 수 있습니다.

```jsx
let arr = [
	{name:'홍길동', score: 90},
	{name:'김철수', score: 85},
	{name:'박영희', score: 97},
];
function compare(a, b){
	return b.score - a.score; // 높은 점수 순으로 정렬
}
arr.sort(compare);
```