# Ford-Fulkerson
네트워크 유량 알고리즘에 대표적인 알고리즘 포드 포커슨과 에드몬트 카프 알고리즘이 있습니다.

그렇다면 먼저 네트워크 유량에 대해서 간단히 알아보겠습니다.
#### 네트워크 유량
시작점에서부터 도착점으로 동시에 보낼 수 있는 데이터나 사물의 최대 양을 구하는 알고리즘입니다.

#### 이 알고리즘에서 쓰이는 용어들

Source: 시작점 , Sink: 도착점
Capacity: 용량 (정해진 용량에서 가능한 용량의 양)
Flow: 유량 (용량을 점유하고 있는 유량 값)
c(a, b): 정점 a 에서 b로 정해진 용량에서 아직 쓸 수 있는 용량 값
f(a, b): 정점 a 에서 b로 흐르고 있는 유량 값

#### 네트워크 유량의 특징
네트워크 유량 알고리즘을 이해하기 위해서는 네트워크 유량을 대표하는 3가지 특징을 알아야 합니다.

##### 첫 번째 : 용량의 제한
-각 간선에 흐르는 유량은 해당되는 간선의 용량을 넘어서 흐를 수 없습니다.
의사코드 : f(a,b) <= c(a,b)

##### 두 번째 : 유량의 보존
-각 정점에 들어오고 나가는 유량의 합은 같습니다.

##### 세 번째 : 유량의 대칭
-의사코드 : f(a,b) == -f(b,a)
※세 번째 특징을 이해하는 것에 있어서 많은 어려움이 있었습니다. 어떻게 해서 마이너스 유량이 흐를 수 있는 것 인지 뒤에서 코드를 보며 이해가 갔습니다.



```c++
#include <iostream>
#include <vector>
#include <queue>

#define MAX 100 //노드 개수 
#define INF 1000000000//무한대값 설정

using namespace std; //c++문법 쓰기 위해 

int n=6,result; //n은 정점의 개수이고 result는 결과 변수
int c[MAX][MAX], f[MAX][MAX],d[MAX]; //c는 용량 변수 f는 유량 변수 d는 특정한 노드를 방문했는지 알려주는 변수이다.
vector<int> a[MAX]; //연결된 간선을 표현할 수 있도록 a라는 변수 설정
    
//최대 유량을 구하는 함수이다. 시작점과 끝 점을 받는다.
void maxFlow(int start,int end) {
    while(1) { //반복적으로 모든 경로를 탐색할 수 있도록 무한루프를 설정해준다.
        fill(d,d+MAX,-1); //모든 정점은 방문하지 않은 상태로 -1로 초기화해준다.
        queue<int> q; //q를 하나 만들어서 모든 경로를 확인해본다.
        q.push(start);//시작점을 큐에 넣어준다.
        while(!q.empty()) {
            int x = q.front();//큐에서 한개를 꺼낸다음
            q.pop();
            for(int i=0; i < a.[x].size();i++) {
                int y = a[x][i];
                // 방문하지 않은 노드 중에서 용량이 남은 경우
                if(c[x][y] - f[x][y] > 0 && d[y] == -1) {//용량이 유량보다 많으면서 방문하지 않았을 경우 
                    q.push(y); //큐에 넣어준다.
                    d[y] = x; //경로를 기억한다.
                    if(y == end) break; //도착지에 도달을 한 경우
                }
            }
        }//도착지까지는 도달하지 않기 때문에 -1이 담겨 있다.
        if(d[end] == -1) break;//모든 경우를 찾았을 경우 탈출한다.
        int flow = INF; //가능한 용량중 작은 것을 넣어야하기 때문에 무한값을 넣어놓는다.
        for(int i=end;i != start; i = d[i]) {//거꾸로 최소 유량을 탐색합니다.
            flow = min(flow,c[d[i]][i] - f[d[i]][i]);
        }
        //최소 유량만큼 추가합니다.
        for(int i=end;i != start; i = d[i]) {
            f[d[i]][i] += flow;
            f[i][d[i]] -= flow;
        }
        result += flow; //최대 유량값을 구했다.
    }
    
} 


int main()
{
    a[1].push_back(2);
    a[2].push_back(1);
    c[1][2]= 12;
    
    a[1].push_back(4);
    a[4].push_back(1);
    c[1][4]= 11;
    
    a[2].push_back(3);
    a[3].push_back(2);
    c[2][3]= 6;
    
    a[2].push_back(4);
    a[4].push_back(2);
    c[2][4]= 3;
    
    a[2].push_back(5);
    a[5].push_back(2);
    c[2][5]= 5;
    
    a[2].push_back(6);
    a[6].push_back(2);
    c[2][6]= 9;
    
    a[3].push_back(6);
    a[6].push_back(3);
    c[3][6]= 8;
    
    a[4].push_back(5);
    a[5].push_back(4);
    c[4][5]= 9;
    
    a[5].push_back(3);
    a[3].push_back(5);
    c[3][5]= 3;
    
    a[5].push_back(6);
    a[6].push_back(5);
    c[5][6]= 4;
    
    maxFlow(1,6);
    printf("%d",result);
    
    return 0;
}
```


