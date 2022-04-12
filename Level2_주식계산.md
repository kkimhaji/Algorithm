# 주식가격
## 문제 설명
초 단위로 기록된 주식가격이 담긴 배열 prices가 매개변수로 주어질 때, 가격이 떨어지지 않은 기간은 몇 초인지를 return 하도록 solution 함수를 완성하세요.

## 제한사항
prices의 각 가격은 1 이상 10,000 이하인 자연수입니다.    
prices의 길이는 2 이상 100,000 이하입니다.     
## 입출력 예
### prices	return   
[1, 2, 3, 2, 3]	[4, 3, 1, 1, 0]       

### 입출력 예 설명
1초 시점의 ₩1은 끝까지 가격이 떨어지지 않았습니다.    
2초 시점의 ₩2은 끝까지 가격이 떨어지지 않았습니다.       
3초 시점의 ₩3은 1초뒤에 가격이 떨어집니다. 따라서 1초간 가격이 떨어지지 않은 것으로 봅니다.     
4초 시점의 ₩2은 1초간 가격이 떨어지지 않았습니다.      
5초 시점의 ₩3은 0초간 가격이 떨어지지 않았습니다.    
※ 공지 - 2019년 2월 28일 지문이 리뉴얼되었습니다.       


stack을 써서 기존 리스트에서 차례대로 돌며 감소하지 않았다면 추가함. 감소했다면 스택에 저장된 인덱스 값을 pop으로 빼고, 몇 번 째 만에 나온 값인지를 위해 i에서 해당 값을 뺀다.(answer에 넣음)    
마지막의 while문은 끝까지 감소하지 않은 경우라서 스택이 빌 때 까지 돌리면서 전체 길이에서 마지막값(+1)을 뺀다.

### 풀이 코드 

```java
import java.util.*;

class Solution {
    public int[] solution(int[] prices) {
        int[] answer = new int[prices.length];
        Stack <Integer> idx = new Stack<>();
        for(int i=0;i<prices.length;i++){
            while(!idx.isEmpty()&&prices[i]<prices[idx.peek()]){
                answer[idx.peek()] = i - idx.pop();
            }
            idx.push(i);
        }
        while(!idx.isEmpty()){
            answer[idx.peek()] = prices.length-1-idx.pop();
        }
        
        return answer;
    }
}
```

# 다른 사람의 풀이

```java
class Solution {
    public int[] solution(int[] prices) {
        int len = prices.length;
        int[] answer = new int[len];
        int i, j;
        for (i = 0; i < len; i++) {
            for (j = i + 1; j < len; j++) {
                answer[i]++;
                if (prices[i] > prices[j])
                    break;
            }
        }
        return answer;
    }
}
```
stack을 쓰진 않았지만 깔끔 레전드...    
나는 이중 for문 쓰려다 너무 느릴 것 같아서 안 썼음    
