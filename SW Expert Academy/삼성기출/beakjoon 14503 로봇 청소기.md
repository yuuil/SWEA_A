# 14503. 로봇 청소기

## 문제

로봇 청소기가 주어졌을 때, 청소하는 영역의 개수를 구하는 프로그램을 작성하시오.

로봇 청소기가 있는 장소는 N×M 크기의 직사각형으로 나타낼 수 있으며, 1×1크기의 정사각형 칸으로 나누어져 있다. 각각의 칸은 벽 또는 빈 칸이다. 청소기는 바라보는 방향이 있으며, 이 방향은 동, 서, 남, 북중 하나이다. 지도의 각 칸은 (r, c)로 나타낼 수 있고, r은 북쪽으로부터 떨어진 칸의 개수, c는 서쪽으로 부터 떨어진 칸의 개수이다.

로봇 청소기는 다음과 같이 작동한다.

1. 현재 위치를 청소한다.
2. 현재 위치에서 현재 방향을 기준으로 왼쪽방향부터 차례대로 탐색을 진행한다.
   1. 왼쪽 방향에 아직 청소하지 않은 공간이 존재한다면, 그 방향으로 회전한 다음 한 칸을 전진하고 1번부터 진행한다.
   2. 왼쪽 방향에 청소할 공간이 없다면, 그 방향으로 회전하고 2번으로 돌아간다.
   3. 네 방향 모두 청소가 이미 되어있거나 벽인 경우에는, 바라보는 방향을 유지한 채로 한 칸 후진을 하고 2번으로 돌아간다.
   4. 네 방향 모두 청소가 이미 되어있거나 벽이면서, 뒤쪽 방향이 벽이라 후진도 할 수 없는 경우에는 작동을 멈춘다.

로봇 청소기는 이미 청소되어있는 칸을 또 청소하지 않으며, 벽을 통과할 수 없다.

## 입력

첫째 줄에 세로 크기 N과 가로 크기 M이 주어진다. (3 ≤ N, M ≤ 50)

둘째 줄에 로봇 청소기가 있는 칸의 좌표 (r, c)와 바라보는 방향 d가 주어진다. d가 0인 경우에는 북쪽을, 1인 경우에는 동쪽을, 2인 경우에는 남쪽을, 3인 경우에는 서쪽을 바라보고 있는 것이다.

셋째 줄부터 N개의 줄에 장소의 상태가 북쪽부터 남쪽 순서대로, 각 줄은 서쪽부터 동쪽 순서대로 주어진다. 빈 칸은 0, 벽은 1로 주어진다. 장소의 모든 외곽은 벽이다.

로봇 청소기가 있는 칸의 상태는 항상 빈 칸이다.

## 출력

로봇 청소기가 청소하는 칸의 개수를 출력한다.



- 문제 설명을 잘못이해해서 3시간 동안 이상한 코드만 구현을 했다... 바로 3번 조건인데, 네 방향 모두 청소가 이미 되어있거나 벽인 경우에는 방향만 유지한 채로 후진을 하고 2번으로 돌아간다. 벽을 만났을 때 후진을 하는 것이 아니라 벽을 만나거나, 청소가 되어있을 경우를 의미한다.

- ```c++
  #include <iostream>
  
  using namespace std;
  
  typedef struct DIR{
  	int r,c;
  };
  
  DIR dir[4]={{-1,0},{0,1},{1,0},{0,-1}};
  int n,m,arr[50][50],sr,sc,d,ans;
  bool check[50][50];
  
  
  void solve(int r,int c){
  	
  	if(!check[r][c]){
  		check[r][c]=true;
  		ans++;
  	}
  	
  	for(int i=0;i<4;i++){
  		d=(d+3)%4;
  
  		int nr=r+dir[d].r;
  		int nc=c+dir[d].c;
  		if(!check[nr][nc]&&arr[nr][nc]==0){
  			solve(nr,nc);
  			return;			
  		}
  	}
  
  	int back=(d+2)%4;
  	int br=r+dir[back].r;
  	int bc=c+dir[back].c;
  	if(arr[br][bc]==1){
  		return;
  	}else{
  		solve(br,bc);
  		return;
  	}
  	
  	
  }
  
  int main(){
  	cin>>n>>m;
  	cin>>sr>>sc>>d;
  	for(int i=0;i<n;i++){
  		for(int j=0;j<m;j++){
  			cin>>arr[i][j];
  		}
  	}		
  	
  	solve(sr,sc);
  	cout<<ans;
  }
  ```

  