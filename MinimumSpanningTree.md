# Minimum Spanning Tree
> Given a Graph, Find subset of edges so that a connected Graph results with minimum sum of edge costs.

## Prim Algorithm
### Idea
1) 하나의 노드를 가진 트리에서 시작. 
2) 트리에 인접한 엣지중 가장 작은 weight를 가진 edgh 추가.
3) Spanning Tree가 될때 까지 반복.

### 정확성 증명

> 모든 단계(Invariant)에서 Prim이 만들어 가는 답 T가 항상 T⊆Tmst를 유지한다는 것을 증명한다.

T: prim Alg가 만들어 가는 답 edge set   
Tmst: 정답 중 하나인 edge set(적어도 하나) 

* Tmst가 정해진게 아니고 정답들 중에 어떤 최소 하나에 대해서는 T가 Tmst의 부분집합임을 유지한다.
* 귀납적 가정법으로 Invariant를 증명한다.   

base: T의 size가 0일 때(공집합), T⊆Tmst이니까 맞다.   
step: │T│ = k일 때, T⊆Tmst 이면 (수학적 귀납법으로 그냥 된다고 가정) 그 다음에 Alg이 한번의 선택을 진행한다. >> 
T' = T U {e}

case 1) T'이 여전히 같은 Tmst의 부분집합일 때 (T'⊆Tmst) 여전히 유지가 된다.   
case 2) T'이 Tmst의 부분집합이 안될 때...   
Tmst에 e를 포함하면 cycle이 생긴다. (e와 같은 순간에 선택사항이었던 e'을 찾아서 e와 e'의 weight를 비교해서 prim Alg가 올바른 선택을 했는지 확인하기 위해.. 그리고 트리에 엣지 하나를 추가해서 cycle이 생기면 그 clycle에서 엣지를 아무거나 빼도 다시 tree가 됨.)

* Tmst는 모든 노드를 연결한 graph이고 여기서 edge하나를 추가하면 cycle이 반드시 생길 수 밖에 없다.   

이 cycle에는 T'에 속하지 않고 T에 인접한 edge e'이 존재한다.   

* 왜냐하면 e의 양쪽 노드 중에 T에 인접한 노드(a)와 T에 인접하지 않는 노드(b)가 반드시 있는데 a에서 cycle을 돌아 b로 오는 길에 T에 인접한 노드와 인접하지 않는 노드를 연결하는 edge가 반드시 있는데 이 edge가 e'이다.  

이때, e'은 Algorithm이 e를 선택할 때 같이 고려하는 대상이였다.

1. w(e) < w(e')이면 Tmst에서 e'을 빼고 e를 넣는게 더 낫다.즉,Tmst가 정답인데 더 좋은 정답이 있으므로 모순된다.   
2. w(e) > w(e')이면 우리 Algorithm이 e가 아닌 e'을 골라야 한다. 즉, ah순이다.
3. w(e) = w(e')이면 e와 e'을 바꾸어도 여전히 Tmst의 정답안에 있다.

### Prim Algorithm 요약
* 항상 알고리즘의 큰 문제 2개가 정확하냐 빠르냐임.
1. 큰 틀은 정답이 여러개 임... 이 중에 정답 하나를 잡고 알고리즘이 돌아가는 걸 본다.
2. 알고리즘이 우리가 생각한 답에 있는걸로 계속 엣지를 추가해 나가면(하필 그 답으로 가고있음) 괜찮음(잘하고 있음)
3. 그런데 알고리즘이 우리 정답에 없는 엣지를 추가한 경우
4. 이때 e'을 찾는게 관건이다. (e'은 이번 라운드에 고려대상이 되었던 다른 엣지.)   
5. e와 e'을 비교해서 e가 더 작으면 모순(Tmst 정답자체에 모순이 생김)이 생기고 e'이 weight가 더 작으면 알고리즘이 e를 추가할 수 없다. 즉, 우리 알고리즘은 틀렸다는 모순이 생긴다.
6. e와 e'이 같으면 정답에서 바꿔넣어도 여전히 정답이다. 즉, Alg가 다른 정답을 가고 있는 거다.

> 정답을 놓고 알고리즘이 정답과 계속 똑같은걸 계속하고 있으면 OK. 정답과 다른 걸 하면 "이런일은 생길 수 없다." 혹은 "이것도 정답이다"로 끝남.

## Kruskal Algorithm
### Idea
1) 가장 작은 weight를 가진 엣지 순서로 cycle이 만들어지지 않게 추가한다.
2) 엣지의 개수가 N-1이 될 때까지 한다.

### 정확성 증명
> 모든 단계(Invariant)에서 Kruskal이 만들어 가는 답 T가 항상 T⊆Tmst를 유지한다는 것을 증명한다.

T: Kruskal Alg가 만들어 가는 답 edge set   
Tmst: 정답 중 하나인 edge set(적어도 하나) 

* Tmst가 정해진게 아니고 정답들 중에 어떤 최소 하나에 대해서는 T가 Tmst의 부분집합임을 유지한다.
* 귀납적 가정으로 Invariant를 증명한다.

base: T의 size가 0일 때(공집합), T⊆Tmst이니까 맞다.   
step: │T│ = k일 때, T⊆Tmst 이면 (수학적 귀납법으로 그냥 된다고 가정) 그 다음에 Alg이 한번의 선택을 진행한다. >> 
T' = T U {e}

case 1) T'이 여전히 같은 Tmst의 부분집합일 때 (T'⊆Tmst) 여전히 유지가 된다.

case 2) T'이 Tmst의 부분집합이 안될 때...   
Tmst에 엣지 e를 추가하면 cycle이 생긴다. 이 cycle 안에 e 말고 not in T인 edge가 존재한다. not in T에서는 아무거나 e'으로 잡는다.

1. w(e) < w(e')이면 Tmst에서 e'을 빼고 대신 e를 넣으면 이게 더 좋은 답이므로 모순이다.
2. w(e) > w(e')이면 e'이 이미 T에 있었어야 한다. (Algorithm이 작은 것 부터 보니까 e'을 먼저 보는데 지금 cycle이 없으므로 그 때도 cycle이 없었는데 e'을 넣지 않았다면  kruskal algorithm을 돌렸다는 것에 모순된다.)
3. w(e) = w(e')이면 e와 e'을 바꾸어도 여전히 Tmst의 정답안에 있다.

## Prim and Kruskal can find ANY Solution
(kruskal도 거의 똑같아서 prim으로만 증명)

### Proof
> Fix any solution Tmst   
> Show Prim can find THAT solution

Prim algorithm한테 내가 정한 정답을 찾으라고 한다.   
Prim algorithm이 계속해서 돌아가는데 Tmst안에서 계속 정답을 찾아간다.   
그러다 갑자기 Tmst가 아닌 e를 추가하면 지난번과 마찬가지로 e'을 찾는다.   
(Tmst에 e를 추가하면 cycle이 생기는데 엣지 e에 T에 인접한 노드 (a)와 T에 인접하지 않은 노드(b)가 있을 때 a에서 시작하여 cycle을 돌아 b로 가는 길에 한쪽 노드가 T에 인접하고 한쪽 노드는 인접하지 않는 노드를 갖는 엣지 e'가 반드시 있다.)

w(e) < w(e')이면 Tmst가 정답이 아니므로 모순   
w(e) > w(e')이면 우리 algorithm이 e'도 동시에 고려대상인데 더 작은 것(e')을 안 넣었으므로 모순.   
w(e) = w(e')이면 Prim에서 e'을 선택하는 경우가 존재한다.

결론은 정답으로 무엇을 지정하더라도 그 정답을 찾을 수 있으니까 Prim이 잘 조절하면 어떤 정답이든 찾을 수 있고 절대 못찾는 정답은 없다.
### Is that Property Important?
> Can be used to show that if all weights are different there is exacktly one solution to the MST problem.

모든 weight가 다르면 Prim과 kruskal은 유일한 답을 찾는다.

* 위에서 "어떤 정답이든 찾아 갈 수 있다"로 쉽게 증명할 수 있다.   
  
Kruskal인 경우 weight가 같은게 두 개가 있으면 뭘 먼저 놓을지 option이 있는데 weight가 같은게 없으면 무조건 순서대로 보게 되고 cycle이 없으면 추가한다. 다시 말해, 모든 weight가 다르면 Kruskal 돌릴 때, 다르게 algorithm이 돌아갈 방법이 없어서 답을 찾는게 1개 밖에 없다.   
즉, Kruskal은 모든 답을 찾을 수 있는데 어떻게 돌려도 답이 하나 밖에 안 나오니 그럼 내가 찾은 게 유일한 답이다.(Prim은 더 복잡한다.)