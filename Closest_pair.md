문제: 주어진 점들 중에서 가장 가까운 쌍을 찾기 (2차원)

입력: 2차원 좌표로 제공

방법론 3가지

1.   기준으로 반으로 나누어서 재귀적으로 구한다.
   => 이 경우, 그룹 A와 B로 나뉘어져, 각 그룹에서 가장 가까운 쌍을 찾게 되면 그것이 답이겠으나, A와 B에 동시에 속한 쌍이 가장 가까운 경우를 구할 수 없다.
   => 그렇다면 해당 경우를 검사하면 된다. 그룹 A에서의 가장 가까운 쌍의 거리를 DA, B에서 가장 가까운 쌍의 거리를 DB라고 할때, 그 중 더 짧은 거리를 D라고 하자. 각 그룹을 나누는 선에서 부터 거리 D안에 존재하는 쌍들의 거리만 고려하면 된다. (이제 두 영역을 걸치는 쌍만 구하는데 밴드 밖의 점은 중간선까지 오는데만 D를 넘기 때문이다.) BUT, 이 경우에도 D구간 안에 대부분의 점이 존재하는 경우, 시간복잡도가 커지는 단점(n^2)이 있다.

      +그래서 더 좋은 idea는 바로  

2. x좌표를 기준으로 나누어서 D구간에 속하는 점들을 골라낸다. 이후 그 점들을 다시 y좌표를 기준으로 sorting한다. (작은쪽에서 큰쪽으로) 이후, 제일 y값이 작은 점부터 시작하여 기준이 되는 점보다 y좌표가 큰 5개의 점들과의 거리를 구한다. (5개인 이유는 정사각형 2개에 거리가 D보다 작지 않은 점들이 최대 5개가 들어 갈 수 있다. 한 점을 기준으로 반대편 영역의 정사각형 영역(넉넉잡아)만 고려하면 되니 5개만 보면 되는데 생각을 쉽게 하기 위해 우리는 y로 정렬하여 위에있는 점 5개만 보기로 했고 5개 초과인 점부터는 5개 이하인 점들보다 거리가 클 수 밖에 없기 때문이다.) 
-> n(logn)(logn)   

   why? : nlog^2(n) => y좌표를 기준으로 sorting = nlogn, band 내부의 최소거리를 찾는 과정이 1, 분할 정복 기법으로 logn
   => (1 + nlogn) x logn = nlog^2(n)
   처음에 x좌표를 기준으로 sorting 1번 = nlogn, 각각 y좌표를 기준으로 sorting = nlogn, n개 data가 5개씩 보니 n, 이걸 분할 정복 기법으로 logn번 수행하니.
   nlogn + (nlogn + n) * logn = O(nlog^2(n))

3. y좌표를 기준으로 정렬한 뒤, 원소에 대해서 D구간에 속하는 점들간을 비교한다. (이 비교는 최대 5 x N개로 고정된다 -> 후술)
   => 우리에게 주어진 정보는 다음과 같다 : y좌표로 정렬된 배열, D의 길이(D 구간의 속하는 x좌표의 구간)
   => 따라서 y좌표로 정렬된 배열을 훑으면서, D에 속하는 점들을 골라 낼 수 있다. 이 D에 속하는 점들 중 붙어있는 5개의 점들을 비교하여, D 구간에 속하는 점들간의 최소 길이를 찾는 것이다. 찾아낸 최소 길이와 D값을 비교하여 작은값이 최소 길이가 된다.
   => 이후 merge 과정에서 배열이 변경되지 않았으므로 추가적으로 sorting이 일어나지 않는다. (sorting은 처음에 전체 배열에 대하여 1회 발생)

nlog(n) 방법론 정리

1. 기본적인 순서는 다음과 같다.
   a. 배열을 A, B 두개의 배열로 나누어, 각 배열에서의 최소길이 dist_A와 dist_B를 재귀적으로 구한다. 그 중 짧은 값을 ds라고 하자.
   b. 전체 배열에서 고려하지 않은 구간 D에 속하는 점들간의 거리중 제일 짧은 거리 dl을 구한다.
   c. dl과 ds중 작은 값이 최소길이가 된다.

2. n <= 3일때
   배열의 원소의 개수가 3개 이하면, 걸리는 시간이 매우 적으므로, 일일히 구한다.

3. n > 3일때   
   a. 기준이 되는 x좌표인 norm_x를 구한다 (배열을 2개로 나누는 x좌표)   
   b. norm_x를 기준으로 배열을 하위 배열 A와 B로 나눈다.   
   c. 재귀적으로 하위 배열 A와 B의 최소 길이 dist_A와 dist_B를 구하여, ds구한다.   
   d. D를 알게 되었다. (norm_x - ds) <= D <= (norm_x + ds)   
   e. 배열을 y좌표로 정렬한뒤, dl을 구한다.   
   --e1. 배열의 원소 e가 D에 속하면 임의의 큐 Q에 넣는다. (Q의 size는 6이며, 6일때 추가로 들어오면 맨 앞 원소가 나간다)   
   --e2. 넣을때마다 Q의 첫번째 원소와 들어온 원소의 거리를 계산하여, 지금까지 구한 d_min와 비교한뒤 작은 값을 d_min로 갱신한다.   
   (설명을 위해서 queue를 사용했을뿐 실제로 queue를 사용하지는 않는다. -> 임의접근이 필요하기 때문)   
   f. dl과 ds를 비교하여 작은 값이 최종 답인 최소길이가 된다.   

4. 이때 정렬은 3-e에서, y좌표로의 정렬밖에 일어나지 않는다.
   그렇다면 최상위 배열에서 y좌표로 1회만 정렬하면, 정렬의 기준이 항상 동일하므로 하위 배열에서 정렬할 필요가 없게 된다.
   따라서 만약 최상위 배열에서 y좌표 정렬을 수행한 경우, e-3의 정렬은 불필요 하다.

5. 따라서 시간 복잡도 T(n)
   3-a. = 1
   3-b. = 1
   3-c. = 2 x T(n/2)
   3-d. = 1
   3-e. = n (정렬이 없으며, queue의 size가 6으로 고정되므로 원소당 최대 접근 횟수는 6회이다)
   3-f. = 1

   종합하면
   T(n) = 2 x T(n/2) + n = nlog(n)
   (4번의 정렬은 1회만 일어나지만 동일하게 nlog(n)이므로 시간복잡도에는 영향을 주지 않는다.)


   처음에 x, y로 정렬 한번씩 > nlogn
   (지금은 아마 x로 sorting되어 있겠조)   
   이미 정렬된 두 y집합을 merge하면 정렬이 되니 n   
   comparision은 역시 n  
   분할과 정복 > logn   
   2nlogn + (n+n)logn = O(nlogn)

   같은 집합인데 x로 정렬된 배열 X, y로 정렬된 배열 Y 이 있음.
   정렬된 배열 X에서 norm_x를 구함