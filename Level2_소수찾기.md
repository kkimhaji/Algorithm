# Programmers Level 2 - 소수찾기
## 문제 설명
한자리 숫자가 적힌 종이 조각이 흩어져있습니다. 흩어진 종이 조각을 붙여 소수를 몇 개 만들 수 있는지 알아내려 합니다.


각 종이 조각에 적힌 숫자가 적힌 문자열 numbers가 주어졌을 때, 종이 조각으로 만들 수 있는 소수가 몇 개인지 return 하도록 solution 함수를 완성해주세요.

## 제한사항
numbers는 길이 1 이상 7 이하인 문자열입니다.

numbers는 0~9까지 숫자만으로 이루어져 있습니다.

"013"은 0, 1, 3 숫자가 적힌 종이 조각이 흩어져있다는 의미입니다.

### 입출력 예
numbers	return

"17"	3

"011"	2

#### 입출력 예 설명
##### 예제 #1
[1, 7]으로는 소수 [7, 17, 71]를 만들 수 있습니다.

##### 예제 #2
[0, 1, 1]으로는 소수 [11, 101]를 만들 수 있습니다.

11과 011은 같은 숫자로 취급합니다.
```java

import java.util.*;
class Solution {
    static ArrayList<Integer> arr = new ArrayList<>();
    static boolean[] check = new boolean[7];
    
    public int solution(String numbers) {
        int answer = 0;
        for(int i=0; i<numbers.length(); i++){
            dfs(numbers,"",i+1);
        }
        
        for(int i=0; i<arr.size(); i++){
            if(prime(arr.get(i))) answer++;              
        }
        
        return answer;
  
    }
	//백트래킹
	static void dfs(String str, String temp, int m) {
            if(temp.length() == m){
                int num = Integer.parseInt(temp);
                if(!arr.contains(num)){
                    arr.add(num);
                }
            }
        
            for(int i=0; i<str.length(); i++){
                if(!check[i]){
                    check[i] = true;
                    temp += str.charAt(i);
                    dfs(str, temp, m);
                    check[i] = false;
                    temp = temp.substring(0, temp.length()-1);
                }
            }
		
	}
	//소수 판단
	static boolean prime(int n) {
		if(n<2) return false;
		
		for(int i=2; i*i<=n; i++) {
			if(n % i == 0) return false;
		}
		
		return true;
	}

}

```

# BackTracking

: 해를 찾는 도중 해가 아니어서 막히면, 되돌아가서 다시 해를 찾아가는 기법

최적화 문제, 결정 문제를 푸는 방법

# 깊이 우선 탐색(DFS)과 백트래킹

## 깊이 우선 탐색(DFS)

- DFS는 가능한 모든 경로(후보)를 탐색
- 불필요할 것 같은 경로를 사전에 차단X → 경우의 수를 줄이지 못함
- N! 가지의 수를 가진 문제는 DFS로 처리가 불가능

## 백트래킹(Backtracking)

- 해를 찾아가는 도중, 지금의 경로가 해가 되지 않을 것 같으면 더 이상 가지 않고 돌아감
- 반복문의 횟수를 줄일 수 있음 → 효율적
    - 가지치기라고 함
    - 가지치기를 얼마나 잘하느냐에 따라 효율성이 결정됨
