# Level 2 - 프린터
## 문제 설명
일반적인 프린터는 인쇄 요청이 들어온 순서대로 인쇄합니다. 그렇기 때문에 중요한 문서가 나중에 인쇄될 수 있습니다. 이런 문제를 보완하기 위해 중요도가 높은 문서를 먼저 인쇄하는 프린터를 개발했습니다. 이 새롭게 개발한 프린터는 아래와 같은 방식으로 인쇄 작업을 수행합니다.
          
1. 인쇄 대기목록의 가장 앞에 있는 문서(J)를 대기목록에서 꺼냅니다.      
2. 나머지 인쇄 대기목록에서 J보다 중요도가 높은 문서가 한 개라도 존재하면 J를 대기목록의 가장 마지막에 넣습니다.      
3. 그렇지 않으면 J를 인쇄합니다.       
예를 들어, 4개의 문서(A, B, C, D)가 순서대로 인쇄 대기목록에 있고 중요도가 2 1 3 2 라면 C D A B 순으로 인쇄하게 됩니다.        
         
내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 알고 싶습니다. 위의 예에서 C는 1번째로, A는 3번째로 인쇄됩니다.
         
현재 대기목록에 있는 문서의 중요도가 순서대로 담긴 배열 priorities와 내가 인쇄를 요청한 문서가 현재 대기목록의 어떤 위치에 있는지를 알려주는 location이 매개변수로 주어질 때, 내가 인쇄를 요청한 문서가 몇 번째로 인쇄되는지 return 하도록 solution 함수를 작성해주세요.

## 제한사항
현재 대기목록에는 1개 이상 100개 이하의 문서가 있습니다.      
인쇄 작업의 중요도는 1~9로 표현하며 숫자가 클수록 중요하다는 뜻입니다.       
location은 0 이상 (현재 대기목록에 있는 작업 수 - 1) 이하의 값을 가지며 대기목록의 가장 앞에 있으면 0, 두 번째에 있으면 1로 표현합니다.      
## 입출력 예
priorities	location	return       
[2, 1, 3, 2]	2	1     
[1, 1, 9, 1, 1, 1]	0	5       
### 입출력 예 설명
#### 예제 #1
       
문제에 나온 예와 같습니다.

#### 예제 #2
 
6개의 문서(A, B, C, D, E, F)가 인쇄 대기목록에 있고 중요도가 1 1 9 1 1 1 이므로 C D E F A B 순으로 인쇄합니다.        

풀이 코드 - 우선순위 큐 사용    
```java
import java.util.*;

class Solution {
    public int solution(int[] priorities, int location) {
        int answer = 1;
        PriorityQueue<Integer> priq = new PriorityQueue<>(Collections.reverseOrder());
        //우선순위 큐 - 우선순위가 높은 순으로
        for(int i=0;i<priorities.length;i++){
            priq.add(priorities[i]); //대기목록
        }
        while(!priq.isEmpty()){
            for(int i=0;i<priorities.length;i++){
                if(priq.peek()==priorities[i]){
                    if(location == i) {
                        return answer;
                    }
                    priq.poll();
                    answer++;
                }
            }
        }
        
        return -1;
    }
}
```

근데 우선순위 큐의 시간 복잡도가 O(NLogN) 이라서 시간적인 면에서의 효율성은 조금 떨어지는 것 같음. (내부구조가 힙으로 구성됨 - 이진트리 구조)


# 다른 사람의 풀이 
```java

import java.util.*;

class Solution {
    public int solution(int[] priorities, int location) {
        int answer = 0;
        int l = location;

        Queue<Integer> que = new LinkedList<Integer>();
        for(int i : priorities){
            que.add(i);
        }

        Arrays.sort(priorities);
        int size = priorities.length-1;



        while(!que.isEmpty()){
            Integer i = que.poll();
            if(i == priorities[size - answer]){
                answer++;
                l--;
                if(l <0)
                    break;
            }else{
                que.add(i);
                l--;
                if(l<0)
                    l=que.size()-1;
            }
        }

        return answer;
    }
}

```

```java

import java.util.LinkedList;
import java.util.Queue;

public class Solution {
    class Task{
        int location;
        int priority;
        public Task(int location, int priority) {
            this.location = location;
            this.priority = priority;
        }
    }
    public int solution(int[] priorities, int location) {
        int answer = 0;

        Queue<Task> queue = new LinkedList<>();

        for(int i=0; i<priorities.length; i++){
            queue.add(new Task(i, priorities[i]));
        }

        int now=0;
        while(!queue.isEmpty()){
            Task cur = queue.poll();
            boolean flag = false;
            for(Task t : queue){
                if(t.priority>cur.priority){
                    flag = true;
                }
            }
            if(flag) { // 우선순위 높은게 있으면 뒤로 보낸다
                queue.add(cur);
            }else{
                now++;
                if(cur.location == location) {
                    answer = now;
                    break;
                }

            }
        }
        return answer;
    }
}
```
class로 요소를 새로 정의해서 queue에 넣는 형식이었음 이게 우선순위 큐보다 효율이 좋았다....

link: https://velog.io/@yanghl98/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4-%ED%94%84%EB%A6%B0%ED%84%B0-JAVA%EC%9E%90%EB%B0%94


# 우선순위 큐
## Pritority Queue
```java
PriorityQueue<Integer> priq = new PriorityQueue<>();
//우선순위가 낮은 숫자 순
PriorityQueue<Iteger> priq = new PriorityQueue<>(Collections.reverseOrder());
//우선순위가 높은 숫자 순     
```

추가, 삭제는 기존 큐와 같음    

이 문제에서는 높은 우선순위부터 하나씩 가져오는 것이기 때문에 reverseOrder()를 해줘야 함    
