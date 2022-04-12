# Programmers Level 2 - 다리를 지나는 트럭
## 문제 설명
트럭 여러 대가 강을 가로지르는 일차선 다리를 정해진 순으로 건너려 합니다. 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 알아내야 합니다. 다리에는 트럭이 최대 bridge_length대 올라갈 수 있으며, 다리는 weight 이하까지의 무게를 견딜 수 있습니다. 단, 다리에 완전히 오르지 않은 트럭의 무게는 무시합니다.
         
예를 들어, 트럭 2대가 올라갈 수 있고 무게를 10kg까지 견디는 다리가 있습니다. 무게가 [7, 4, 5, 6]kg인 트럭이 순서대로 최단 시간 안에 다리를 건너려면 다음과 같이 건너야 합니다.
           
경과 시간	다리를 지난 트럭	다리를 건너는 트럭	대기 트럭       
0	[]	[]	[7,4,5,6]     
1~2	[]	[7]	[4,5,6]       
3	[7]	[4]	[5,6]      
4	[7]	[4,5]	[6]    
5	[7,4]	[5]	[6]    
6~7	[7,4,5]	[6]	[]     
8	[7,4,5,6]	[]	[]           
따라서, 모든 트럭이 다리를 지나려면 최소 8초가 걸립니다.     
          
solution 함수의 매개변수로 다리에 올라갈 수 있는 트럭 수 bridge_length, 다리가 견딜 수 있는 무게 weight, 트럭 별 무게 truck_weights가 주어집니다. 이때 모든 트럭이 다리를 건너려면 최소 몇 초가 걸리는지 return 하도록 solution 함수를 완성하세요.
      
## 제한 조건
bridge_length는 1 이상 10,000 이하입니다.      
weight는 1 이상 10,000 이하입니다.          
truck_weights의 길이는 1 이상 10,000 이하입니다.     
모든 트럭의 무게는 1 이상 weight 이하입니다.     
## 입출력 예
bridge_length	weight	truck_weights	return       
2	10	[7,4,5,6]	8      
100	100	[10]	101      
100	100	[10,10,10,10,10,10,10,10,10,10]	110    

  
풀이 코드

기본적으로 다리를 큐로 놓고, 무게의 합이 weight보다 작으면 새 트럭을 추가하고 넘으면 0을 넣어서 처리함 
시간은 마지막 트럭은 올라가는 것까지만 계산해서 추가로 다리의 길이만큼 더해주기 



```java
import java.util.*;

class Solution {
    public int solution(int bridge_length, int weight, int[] truck_weights) {
        int answer = 0;
        int sum = 0;    //트럭 무게 합
        
        Queue <Integer> onbridge = new LinkedList<>();
        for(int truck: truck_weights){
            while(true){
                if(onbridge.isEmpty()){
                    sum+=truck;
                    onbridge.add(truck);
                    answer++;
                    break;
                }else if(onbridge.size() == bridge_length){
                    sum-=onbridge.poll();
                }else{
                    if(sum+truck<=weight){
                        answer++;
                        sum+=truck;
                        onbridge.add(truck);
                        break;
                        
                    }else{
                        onbridge.add(0);
                        answer++;
                    }
                }
            }
        }
        return answer+bridge_length;
    }
}
```




# 다른 사람의 풀이 (Programmers)
```java
import java.util.*;

class Solution {
    class Truck {
        int weight;
        int move;

        public Truck(int weight) {
            this.weight = weight;
            this.move = 1;
        }

        public void moving() {
            move++;
        }
    }

    public int solution(int bridgeLength, int weight, int[] truckWeights) {
        Queue<Truck> waitQ = new LinkedList<>();
        Queue<Truck> moveQ = new LinkedList<>();

        for (int t : truckWeights) {
            waitQ.offer(new Truck(t));
        }

        int answer = 0;
        int curWeight = 0;

        while (!waitQ.isEmpty() || !moveQ.isEmpty()) {
            answer++;

            if (moveQ.isEmpty()) {
                Truck t = waitQ.poll();
                curWeight += t.weight;
                moveQ.offer(t);
                continue;
            }

            for (Truck t : moveQ) {
                t.moving();
            }

            if (moveQ.peek().move > bridgeLength) {
                Truck t = moveQ.poll();
                curWeight -= t.weight;
            }

            if (!waitQ.isEmpty() && curWeight + waitQ.peek().weight <= weight) {
                Truck t = waitQ.poll();
                curWeight += t.weight;
                moveQ.offer(t);
            }
        }

        return answer;
    }
}

```
앞에서처럼 class로 object만들어서 넣는 방식. 코드가 깔끔해서 맘에 듦... 내 코드 더러워서 맘에 안 들었는데 class 사용하는 거 좀 찾아봐야 할 듯 

```java
import java.util.HashMap;
import java.util.Map;
import java.util.Stack;

class Solution {
    public int solution(int bridge_length, int weight, int[] truck_weights) {
            Stack<Integer> truckStack = new Stack<>();
            Map<Integer, Integer> bridgeMap = new HashMap<>();

            for (int i = truck_weights.length-1; i >= 0; i--)
                truckStack.push(truck_weights[i]);

            int answer = 0;
            int sum = 0;
            while(true) {
                answer++;

                if (bridgeMap.containsKey(answer))
                    bridgeMap.remove(answer);

                sum = bridgeMap.values().stream().mapToInt(Number::intValue).sum();

                if (!truckStack.isEmpty())
                    if (sum + truckStack.peek() <= weight)
                        bridgeMap.put(answer + bridge_length, truckStack.pop());

                if (bridgeMap.isEmpty() && truckStack.isEmpty())
                    break;


            }
            return answer;
    }
}
```

아래 코드는 Map을 사용해서 지워나간다는 발상이 신선했음 


