
# Programmers Level 2 - 기능개발
## 문제 설명
프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.     

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.     

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.

## 제한 사항
작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.    
작업 진도는 100 미만의 자연수입니다.    
작업 속도는 100 이하의 자연수입니다.     
배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다.     
### 입출력 예
progresses	speeds	return      
[93, 30, 55]	[1, 30, 5]	[2, 1]      
[95, 90, 99, 99, 80, 99]	[1, 1, 1, 1, 1, 1]	[1, 3, 2]      
#### 입출력 예 설명
##### 입출력 예 #1
첫 번째 기능은 93% 완료되어 있고 하루에 1%씩 작업이 가능하므로 7일간 작업 후 배포가 가능합니다.      
두 번째 기능은 30%가 완료되어 있고 하루에 30%씩 작업이 가능하므로 3일간 작업 후 배포가 가능합니다. 하지만 이전 첫 번째 기능이 아직 완성된 상태가 아니기 때문에 첫 번째 기능이 배포되는 7일째 배포됩니다.     
세 번째 기능은 55%가 완료되어 있고 하루에 5%씩 작업이 가능하므로 9일간 작업 후 배포가 가능합니다.     

따라서 7일째에 2개의 기능, 9일째에 1개의 기능이 배포됩니다.     

##### 입출력 예 #2
모든 기능이 하루에 1%씩 작업이 가능하므로, 작업이 끝나기까지 남은 일수는 각각 5일, 10일, 1일, 1일, 20일, 1일입니다. 어떤 기능이 먼저 완성되었더라도 앞에 있는 모든 기능이 완성되지 않으면 배포가 불가능합니다.

---
큐가 처음이라 익숙하지 않아서 배열로 풀었음 ..다른 문제 다 풀고 오니까 ㅋㅋ ㅠㅠㅜ 정리해야하긴 할 듯
     
```java
import java.util.*;

class Solution {
    public int[] solution(int[] progresses, int[] speeds) {
        int[] answer = new int[progresses.length];

        for (int i = 0; i < progresses.length; i++) {
            int prog = progresses[i];
            int workingTime = 0;
            while (true) {
                if (prog >= 100)
                    break;
                prog += speeds[i];
                workingTime++; 
            }
            answer[i] = workingTime;
        }
        System.out.println(Arrays.toString(answer));
                ArrayList<Integer> list = new ArrayList<>();
        for(int i=0 ; i < answer.length ; i++){  //기준값
            int origin = answer[i];
            int count = 1;
            if(origin < 0){continue;} //조사가 완료된 대상이면 건너뛰기
            for(int j=i+1 ; j < answer.length ; j++){  //기준값 다음의 값]
            	int compare = answer[j];
            	if(origin >= compare){
            		answer[j] = -1; //조사가 완료되었으므로 대상에서 제거
            		count++;
            	} else {
            		break;
            	}
            }
            list.add(count);
        }        
        answer = list.stream().mapToInt(i ->i).toArray();
        return answer;
    }
}


```

## 다른 사람의 풀이

```java

import java.util.ArrayList;
import java.util.Arrays;
class Solution {
    public int[] solution(int[] progresses, int[] speeds) {
        int[] dayOfend = new int[100];
        int day = -1;
        for(int i=0; i<progresses.length; i++) {
            while(progresses[i] + (day*speeds[i]) < 100) {
                day++;
            }
            dayOfend[day]++;
        }
        return Arrays.stream(dayOfend).filter(i -> i!=0).toArray();
    }
}

````

```java
import java.util.*;

class Solution {
    public int[] solution(int[] progresses, int[] speeds) {
        Queue<Integer> q = new LinkedList<>();
        List<Integer> answerList = new ArrayList<>();

        for (int i = 0; i < speeds.length; i++) {
            double remain = (100 - progresses[i]) / (double) speeds[i];
            int date = (int) Math.ceil(remain);

            if (!q.isEmpty() && q.peek() < date) {
                answerList.add(q.size());
                q.clear();
            }

            q.offer(date);
        }

        answerList.add(q.size());

        int[] answer = new int[answerList.size()];

        for (int i = 0; i < answer.length; i++) {
            answer[i] = answerList.get(i);
        }

        return answer;
    }
}
```





## Queue in java
```java
import java.util.LinkedList; //import
import java.util.Queue; //import
Queue<Integer> queue = new LinkedList<>(); //int형 queue 선언, linkedlist 이용
Queue<String> queue = new LinkedList<>(); //String형 queue 선언, linkedlist 이용
```
큐는 linkedList로 선언해서 사용해야 한다   
추가는 .add(value) 또는 .offer(value)      
삭제는 .remove(), .poll()을 사용하면 제일 첫번째 값을 제거하고, .clear()를 하면 모든 값 삭제. poll()은 첫 번째 값이 비어있으면 null을 반환하고 remove()는 에러를 발생시킴. remove(value)로 넣으면 value에 해당하는 객체를 삭제함. 중복값이 있다면 더 앞에 있는 것부터 삭제      
.peek()을 사용하면 첫 번째 값을 참조     
queue.size()로 크기를 구할 수 있음 
