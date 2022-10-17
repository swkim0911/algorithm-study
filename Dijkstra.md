## Dijkstra

### Version of Dijkstra
1. Only Shortest Path Length for each Node
2. Actual Path for each Node
3. All Possible Shortest Paths for each Node


### Idea
1. 모든 노드에 대해서 g(v0,u)를 잠정적으로 답을 적는다.(이건 direct edge이다.)
엣지가 없으면 무한대로 적는다.
> 잠정적인 답을 적은 다음 점점더 정확한 답을 넣는다.
2. 시작할 때는 source node만 Red Nodes set이다.
> Red Nodes set은 최종 답을 찾은 애들이다.
> Blue Nodes set은 잠정적으로 답을 가진 애들이다.
3. Red Nodes set의 개수가 전체 노드 개수 n이 될때까지 반복한다.
3-1. Blue set(Red set에 있지 않은 애들)들 중에서, 잠정적인 답이 가장 작은 놈(u)을 고른다.
3-2. 고른 놈(u)을 Red set에 넣는다. 
3-3. Blue set들 중에서, u를 거쳐서 가는 답과 원래 알고 있던 값들 중에서 비교해서 작은값을 잠정적인 답(Blue set)으로 바꾼다. 

### 증명.
1. 빨간 set은 source 노드에서 시작해서 각 노드로 가는 길의 길이의 최종 정답이다.  
2. 파란 set은 빨간 노드만 거쳐 각 노드로 가는 가장 짧은 길의 길이이다.   
3. 파란 set 값 중에 가장 작은 값 a는 정답이다.   
3-1. a가 정답이 아니라고 하자. (귀류법)
a(빨간 노드만 거처서 파란 노드로 가는 길 중 가장 작은 값)가 정답이 아니라고 하면 파란 걸 거쳐서 가는 길 중에 답이 있다는 거다. 즉,  파란 걸 거쳐서 a로 가는 길에 빨간 노드를 거치고 만나는 최초의 파란 노드(b)가 있다. 이미 파란 set 중에 가장 작은 것(a)을 골랐기 때문에 b는 a보다 크거나 같다. 즉, 파란 것을 더 거쳐서는 a보다 작은 게 없다.
4. 즉, a를 빨간 set에 넣는게 정당화 된다. 
5. 빨간 set에 a가 들어가서 set이 변경되었으니 파란 set중에 a와 direct 엣지가 있는 것들은 a에 대해서만 업데이트 해야한다.
5-1 즉, a를 거치고 다른 빨간 노드(x)를 거치고 파란 set의 노드(임의 p)로 가는 것은 고려할 필요 없다. 왜냐하면 x는 a보다 먼저 빨간 set에 들어가 있으니 x에는 빨간 것만 거쳐간 shortest path가 이미 있는 거고 이 path는 a를 포함하지 않는다. 이 길을 이미 p를 만드는데 고려되었다. 즉, 중간에 a를 거치는 path는 p보다 좋을 수 없다. 그래서 a가 바로 앞에 있는 path만 고려하면 된다. 따라서 파란 set중에 a와 direct 엣지가 없는 것은 고려할 필요없다.

그래서 이 코드가 모두 증명되었다.
1. 밖에 있는 애들 중에 red로 만든다.
2. red 를 만든 노드를 가지고 blue를 업데이트하는데 direct edge만 보면 된다.

두 개다 증명이 필요함.
> 이게 shortest path 길이만 구하는 과정

### 실제 path를 구하는 과정 (shortest path tree)
엣지를 update할 때 내 바로 앞에 것만 알고 있으면 된다. 그럼 거꾸로 따라 갈 수 있다. 그럼 모든 노드가 shortest path가 누구한테서 오는지 알 수 있다. 

### 모든 path를 찾는 과정 (shortest path DAG)
엣지를 update할 때, 더 작으면 기억해 둔 것을 버리고 새로운 것으로 바꾸면 되고, 같으면 둘 다 기억하면 된다. 그럼 모든 path를 기억할 수 있다. 

### 구현

### Alternate Explanation from BFS - 직관적인 수준
모든 엣지의 weight를 1로 두고 가까운 것부터 간다.   
(예를 들어 weight가 3이면 edge 3개로 만든다.)   
여기서 BFS를 돌리면 shortestpath가 나온다.
더
#### Dijkstra Selects Node from Nearest(Smaller Shortest Path) to Furthest.... Why?(제가 설명한 것에도 유도가 됩니다.)
s는 시작노드 이다. 그리고 빨간 set에 직전에 들어간 node u가 있다고 가정하자. 바로 다음에 들어가는 노드 v가 있다고 하자. (s ..... u,v,...)   그러면 dmin(u) < dmin(v) 을 증명할 수 있다.   
왜냐하면 u가 빨간 set에 들어갈 때, u와 v는 둘다 빨간 set밖에 있있는데 그 시점에 dmin(u)와 dmin(v)를 비교했을 때 dmin(u)가 더 작거나 같다. 왜냐하면 u를 골라서 빨간 set에 넣었기 때문이다.   
그리고 u가 업데이트를 할 때 u를 거쳐 v로 가는게 더 작아지면 dmin(u) < dmin(v)이 성립 안 할 수 있다. 그런데 dmin(v)는 원래 값d'min(v)와 dmin(u)+g(u,v)d을 비교하는데 원래 값d'min(v)는 dmin(u)보다 크고, dmin(u)+g(u,v)d 도 dmin(u)보다 크니  dmin(u) < dmin(v)일 수 밖에 없다.   
즉, 다음 노드(u,v,...)가 항상 크거나 같으니 다익스트라 Alg는 가까운 거에서 먼걸로 계속 추가가 된다. 

### Alternate Explanation from 이걸 가지고
S에서 C는 4번 째로 가까운건데 
다음 번 가까운 것은 이미 알고 있는 것과 이미 직접 연결되어 있다. 이미 알고 있는 것들은 직접 연결되어 있는 edge들과 다 반영해서 바깥쪽걸 update했다. 다 봤으니까 제일 작은 걸 찾으면 된다.

#### Prim vs Dikstra
거의 비슷함.

## Deadline Scheduling - deadline, profit으로 최대 이익을 보자.
DEADLINE: 그전까지만 하면 된다.

#### Assumptions
1. deadline은 N을 넘지 않는다. 
deadline이 n을 넘으면 앞으로 땡기면 된다. 빈자리가 있으니까 
job이 배치되어 있는데 앞쪽으로 가는 건 상관없다. 
2. profit이 앞쪽이 크거나 같다.
3. optimal에서는 Deadline 뒤쪽에 job이 나오면 어차피 profit이 0이니까 빼도 된다. 

### 직관
프로핏이 큰 것부터 본다. 
빈자리가 있으면, 제일 뒤쪽(Deadline과 가까운)에 넣는다. 내가 먼저 뒤쪽에 넣으면 나중에 볼 job들에게 더 많은 option이 생기니까.   

### Correctness (항상 옳음)
1. A는 현재 스케줄 전체(빈자리와 job들이 있는 상태)이다. 알고리즘이 위에 방식으로 A에 스케줄을 하고 있다.   
* 이 Invariant는 mst에서 본 것과 유사한다. (모든 단계(Invariant)에서 Prim이 만들어 가는 답 T가 항상 T⊆Tmst를 유지한다는 것을 증명한다.)

optimal solution S(at least one optiomal solution)가 있고 Algorithm은 S를 만들어 가고 있다(현재는 A). i번째 Algorithm을 돌렸을 때, A에 있는 i번 전까지 이미 본 job들은 S에 있고 S에 있는 job들은 A에 있다. 그리고 위치도 똑같다.

### "이 Invaraint는 항상 된다"를 증명한다.
Base i=0일 때, vacuously true
Step, i번 째 진행했을 때 Invariant가 성립함을 가정하고 i+1을 증명한다.
case 1) Algorithm does not schedule J(i+1)
이 것은, A에 빈자리가 없어서 Algorithm이 J(i+1)을 버렸다는 뜻이다.즉, J(1) ~ J(i)까지 D(i+1)앞으로 다 채워져다는 뜻이고 invariant하다고 가정했기 때문에 S에도 똑같이 다 채워져있다. J(i+1)이 D(i+1)이후로 나오는 건 없어도 되니 S에 J(i+1)이 없는 거다.   
그러면 Algorithm도 J(i+1)도 버리고 S에도 없으니까 ok이다.   
case 2)Algorithm이 J(i+1)을 t번째 slot에 schedule했다.   
case 2-a) no J(i+1) in S   
case 2-a-1) S에 slot t가 비어 있는 경우   
contradiction(모순)이다. 즉, S에 J(i+1)을 t에 추가하는 것이 더 이득이다.   
case 2-a-2) slot t has J(x) in S   
이러면 무조건 x > i+1인데 그러면 P(x) <= P(i+1)인데 J(x)를 J(i+1)으로 바꾸면 이익이 같거나 큰데 같으면 여전히 최적 solution이고 크면 S가 정답이 아니였던거라 모순이 생긴다.   
case 2-b) J(i+1) is at t' in S   
t'=t이면 ok
t'>t이면 불가능하다. Algorithm은 제일 뒤에 넣는데 Algorithm이 t에 넣었다는 것은 t뒤에는 이미 진행된 J(1) ~ J(i)으로 꽉 차있다. 뒤가 꽉차있다는 것은 S도 꽉차있다는 걸 말하니 J(i+1)이 뒤에 있을 순 없다.
t' < t이면 S에서 slot t와 slot t'을 바꾼다. t에 job이 있다면 t에 있는 job이 이미 profit을 얻었으면 앞으로 가도 profit을 얻는다. A에서 J(i+1)을 t에 넣었다는 것은 Deadline 이전이라는 거니까 S에서 t'에 있는 J(i+1)을 뒤 t에 넣어도 괜찮다. 이건 t가 비어있어도 상관없다. 즉, 바꿔도 상관없다.

## Performance

## Another Job Scheduling Problem (제일 많은 개수의 job schedule)
> Ji = (Si, Ti)     
Si = Start Time, Ti = End Time (same profit)


### Solution
1.끝나는 시간으로 sorting한다. (끝나는 시간이 증가하는 순으로 본다.)
2.스케줄이 가능하면 넣는다.
3.가장 빨리 끝나는 job을 무조건 넣는다.

알고리즘이 만들어가는 걸 A, 정답 중에 하나를 S라고 하자.
Base: n = 0일때 vacuosly ture
Step
case: 알고리즘이 첫 job을 배치했을 때, S에도 똑같은 job이 있다. > ok
case: 알고리즘이 어떤 job을 버렸을 때, S에 똑같은 job이 있을 수 없다. 왜냐하면 알고리즘이 여태까지 배치한 job이 S에도 똑같이 배치되어 있기 때문이다. (그럼 여전히 invariant가 유지가 되죠.)
case: 알고리즘이 배치를 했는데, S에 똑같이 있으면 > ok
case: 알고리즘이 배치(t)를 했는데, S에 없을 때..
S-A에서 endtime이 가장 빠른 답(k)을 가지고 온다.(이건 t보다 endtime이 같거나 뒤이다.. 왜냐하면 A가 endtime이 빠른 것 부터 보고 있기 때문에 앞일 수 없다.) 그러면 k를 빼고 t를 넣는다. starttime이 k가 t보다 더 뒤이면 n번째 배치한 job과 k사이에 job이 있을 수 없다.   starttime이 k가 t보다 더 앞에 있으면 이 때도 사이에 job이 있을 수 없다. 그래서 바꿔도 된다. 

nlogn + n

## Tape Storage
N 개의 data item이 있다.
Li: data의 사이즈(테이프의 길이)
Fi: 사용 빈도
* Write once, everything
* Read many times
* EACH READ STARTS from the BEGINNING of Tape

### Idea
큰 게 뒤에 있어여 하고 자주 쓰는게 앞에 있어여 한다.

* F/l이 증가하지 않는(not increasing)로 data를 저장한다. 

### Corectiness
> 우리 배치와 다른 경우가 한군데라도 있다고 가정하자. 그럼 optimal이 아니라고 보일 수 있다.