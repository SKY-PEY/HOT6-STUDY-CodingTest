# JS Algorithm Template by flamozzi

## Info

- 코딩테스트 대비 및 학습용 JS 알고리즘 정리 템플릿입니다.
- 내용 및 파일 트리는 계속해서 업데이트 됩니다.
- 폴더 명과 동일한 명의 js 파일은 전반적인 내용정리용 파일입니다.
- BJ_N 은 백준의 N번째 문제를 뜻합니다.
- 각 문제 폴더의 input.txt에 입력 예시 텍스트가 위치합니다.

```javascript
/*
const filePath = process.platform === 'linux' ? '/dev/stdin' : '/input.txt' 코드를 통해 백준 및 vscode에서 정상적으로 입력 파일을 받아올 수 있도록 조건을 달아줍니다.
*/

const fs = require('fs')
const filePath = process.platform === 'linux' ? '/dev/stdin' : '/input.txt'
let input = fs
  .readFileSync(__dirname + filePath)
  .toString()
  .split('\n')

... 이후 로직 코드
```

## File Tree

```
📦JS_Template
 ┣ 📂01.Stack
 ┃ ┗ 📜stack.js
 ┣ 📂02.Queue
 ┃ ┗ 📜queue.js
 ┣ 📂03.Heap
 ┃ ┗ 📜heap.js
 ┣ 📂04.Graph
 ┃ ┗ 📜graph.js
 ┣ 📂05.Sorting
 ┃ ┣ 📂problems
 ┃ ┃ ┣ 📂BJ_10814
 ┃ ┃ ┃ ┣ 📜index.js
 ┃ ┃ ┃ ┗ 📜input.txt
 ┃ ┃ ┣ 📂BJ_11004
 ┃ ┃ ┃ ┣ 📜index.js
 ┃ ┃ ┃ ┗ 📜input.txt
 ┃ ┃ ┣ 📂BJ_11650
 ┃ ┃ ┃ ┣ 📜index.js
 ┃ ┃ ┃ ┗ 📜input.txt
 ┃ ┃ ┣ 📂BJ_1181
 ┃ ┃ ┃ ┣ 📜index.js
 ┃ ┃ ┃ ┗ 📜input.txt
 ┃ ┃ ┣ 📂BJ_1427
 ┃ ┃ ┃ ┣ 📜index.js
 ┃ ┃ ┃ ┗ 📜input.txt
 ┃ ┃ ┣ 📂BJ_18870
 ┃ ┃ ┃ ┣ 📜index.js
 ┃ ┃ ┃ ┗ 📜input.txt
 ┃ ┃ ┣ 📂BJ_2750
 ┃ ┃ ┃ ┣ 📜index.js
 ┃ ┃ ┃ ┗ 📜input.txt
 ┃ ┃ ┗ 📂BJ_2752
 ┃ ┃ ┃ ┣ 📜index.js
 ┃ ┃ ┃ ┗ 📜input.txt
 ┃ ┗ 📜sorting.js
 ┣ 📂06.Greedy
 ┃ ┣ 📂problems
 ┃ ┃ ┣ 📂BJ_11047
 ┃ ┃ ┃ ┣ 📜index.js
 ┃ ┃ ┃ ┗ 📜input.txt
 ┃ ┃ ┣ 📂BJ_11399
 ┃ ┃ ┃ ┣ 📜index.js
 ┃ ┃ ┃ ┗ 📜input.txt
 ┃ ┃ ┣ 📂BJ_1541
 ┃ ┃ ┃ ┣ 📜index.js
 ┃ ┃ ┃ ┗ 📜input.txt
 ┃ ┃ ┣ 📂BJ_16953
 ┃ ┃ ┃ ┣ 📜index.js
 ┃ ┃ ┃ ┗ 📜input.txt
 ┃ ┃ ┗ 📂BJ_2839
 ┃ ┃ ┃ ┣ 📜index.js
 ┃ ┃ ┃ ┗ 📜input.txt
 ┃ ┗ 📜greedy.js
 ┗ 📜README.md
```
