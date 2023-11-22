## DFS와 BFS

대표적인 탐색 알고리즘이며, 코딩 테스트에 자주 등장하는 유형입니다.

그래프 탐색 알고리즘인 DFS, BFS는 깊이를 우선해서 탐색할지 너비를 우선해서 탐색할지에 따라 갈립니다.

- **DFS**는 기본적으로 **스택(Stack) 자료구조**를 사용합니다.
- 그에 반면 **BFS**는 스택과 반대 개념인 **큐(Queue) 자료구조**를 기본적으로 사용합니다.

## BFS 알고리즘 이해하기

너비 우선 탐색을 이해하려면 우선 큐 자료구조와 그래프에 대해 이해할 필요가 있다.

그 이유는 **DFS**는 기본적으로 **스택(Stack) 자료구조**를 사용했지만 **BFS**는 그 반대인 **큐(Queue) 자료구조**를 주로 사용하기 때문입니다.

### BFS를 이해하기 전 알아야 할 큐 자료구조

FIFO(First In First Out) 구조로 저장하는 형식인 Queue 자료구조는

나중에 집어 넣은 데이터가 먼저 나오는 Stack과는 반대되는 개념입니다.

- **JavaScript 큐(Queue) 구현하기**
    
    ```tsx
    class Queue {
    	constructor(){
    		this.items = {};
    		this.headIndex = 0;
    		this.tailIndex = 0;
    	}
    	enqueue(item) {
    		this.items[this.tailIndex] = item;
    		this.tailIndex++;
    	}
    	dequeue() {
    		const item = this.items[this.headIndex];
    		delete this.items[this.headIndex];
    		this.headIndex++;
    		return item;
    	}
    	peek() {
    		return this.items[this.headIndex];
    	}
    	getLength() {
    		return this.tailIndex - this.headIndex;
    	}
    }
    ```
    
    **구현된 큐(Queue) 라이브러리 사용**
    
    ```tsx
    const queue = new Queue();
    
    // 삽입(5) -> 삽입(2) -> 삽입(3) -> 삽입(7)
    //  -> 삭제() -> 삽입(1) -> 삽입(4) ->  삭제()
    queue.enqueue(5);
    queue.enqueue(2);
    queue.enqueue(3);
    queue.enqueue(7);
    queue.dequeue();
    queue.enqueue(1);
    queue.enqueue(4);
    queue.dequeue();
    
    while (queue.getLength() != 0) {
      console.log(queue.dequeue());
    }
    /** 실행결과
     * 3
     * 7
     * 1
     * 4
     */
    ```
    

### 너비 우선 탐색(BFS) 기본 동작 방식

![Animated_BFS.gif](images/Animated_BFS.gif)

> By Blake Matheny - en.wikipedia에서 공용으로 옮겨왔습니다., CC BY-SA 3.0, https://commons.wikimedia.org/w/index.php?curid=1864649
> 

1. 시작 노드를 큐에 넣고 방문 처리 합니다.
2. 큐에서 원소를 꺼내어 방문하지 않은 인접 노드가 있는지 확인합니다.
    1. 있다면, 방문하지 않은 인접 노드를 큐에 삽입하고 방문 처리 합니다.
3. 2번 과정을 더 이상 반복할 수 없을 때까지 반복합니다.

### 너비 우선 탐색(BFS) 사용 예시

1. 간선 비용이 동일한 상황에서 **최단 거리** 문제를 해결하는 경우
2. 완전 탐색을 위해 사용한 DFS 솔루션이 메모리/시간 초과를 받은 경우
→ DFS와 BFS 모두 그래프 탐색 목적으로 사용할 수 있으나, 구현이 익숙하다면 BFS를 추천!
    1. 코딩 테스트에서는 DFS로 해결할 수 있는 문제는 BFS로도 해결할 수 있는 경우가 많습니다.
    2. DFS는 일반적인 최단 거리 문제를 해결할 수 없습니다.

### 너비 우선 탐색(BFS) 소스 코드 예시

- **대표적인 BFS 예시**
    
    ```tsx
    import Queue from "./Queue"; // 이전에 만든 Queue 라이브러리 불러오기
    
    const graph = [[], [2, 3, 4], [1], [1, 5, 6], [1, 7], [3, 8], [3], [4], [5]];
    const visited = new Array(9).fill(false);
    
    // BFS 메서드 정의
    function bfs(graph, start, visited) {
      const queue = new Queue();
      // 현재 노드를 방문 처리
      queue.enqueue(start);
      // 큐가 빌 때까지 반복
      while (queue.getLength() !== 0) {
        // 큐에서 하나의 원소를 뽑아 출력하기
        const v = queue.dequeue();
        console.log(v);
        // 아직 방문하지 않은 인접한 원소들을 큐에 삽입
        for (let i of graph[v]) {
          if (!visited[i]) {
            queue.enqueue(i);
            visited[i] = true;
          }
        }
      }
    }
    
    bfs(graph, 1, visited);
    ```