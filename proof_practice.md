# Recursive Binary Search
## Proof by induction

> 만약 어떤 i에 대해서 a[i] = x를 만족한다면, 위 함수는 i를 리턴한다.

Base: n = 0 인 경우, "어떤 i에 대해서 a[i] = x"를 성립할 방법이 없고, -1을 리턴한다.

Step:   
  Case 1: a[m] = x인 경우, m을 리턴하므로 성립한다.   
  Case 2: a[m] > x인 경우, a[m], a[m+1], .. a[n]중에는 x와 같은 값이 없다. 따라서 a[i] = x인 경우가 있다면 i는 0, 1, .. m-1 중 하나이다.
  귀납적으로(재귀적으로) search(a,m,x)가 정확하다고 하면 답이 있다면 여기 밖에 없으니까 search(a,m,x)가 리턴한 값이 답이다.   
  Case 3: a[m] < x인 경우, a[0], a[1], ... a[m]중에는 x와 같은 값이 없다. 따라서 a[i] = x인 경우가 있다면 i는 m+1, m+2, .. n중에 하나이다. 귀납적으로 search(a+m+1, a-m-1, x)가 정확하다고 한다면 search(a+m+1, a-m-1, x)는 r을 리턴하고 인덱스를 조절하기 위해 r에 m+1을 하면 답이 된다.

# Selection Sort
## Proof of Correctness of Sorting
> Sorting이 됐다는 증명을 어떻게 할까?...
Sorting이 완료된 후 다음이 만족되어야 한다.   

입력: a[0],a[1] .. a[n-1] <- (정수)집합  

  조건1: 집합 조건이 깨지지 않는다.   
  조건2: sorting이 끝난 후 배열에 저장한 값들을
  b[0], b[1], .. b[n]이라고 할때, b[0]< b[1]..b[n]을 만족해야 한다.   (같은 배열이지만 구분하기 위해 이름을 다르게 했다.)

## Proof by Invariant
1.집합 조건을 깰 수 있는 코드가 없다. 즉, 위의 조건1을 만족한다.   
2.Invariant: k번째 루프가 끝난 후에   
* (1). a[0] < a[1].. a[k-1]  -> a[0] < a[1] .. a[n-1]  
k = n이면 a[0] < a[1] .. a[n-1]이 되고 이는 우리가 원하는 위에 있는 조건2가된다.  
(n번째 loop를 돌았으면 k = n이 되고 아래조건은 x > n-1이 되는 x가 없으므로 trivial한 조건으로 의미가 없어진다.)   
* (2). a[k-1] < a[x] if x >k-1   

즉, Invariant가 항상 성립라는 것을 증명하면 (1)과 (2)가 사실이니까 sorting이 되었다는 걸 증명한다.   
그럼 이제 수학적 귀납법으로 Invariant가 항상 성립한다는 것을 증명하면 된다.
## Proof Invariant by induction
base: k=0일 때, (1)은 a[0]부터 a[-1]까지가 되며 아무것도 없으므로 true(null condition)이다. (2)도 a[-1]은 없으니까 null condition이 된다.   
Step:   
k번째 루프가 끝났을 때 Invariant가 성립한다면, k+1번째 루프가 끝났을 때도 Invariant가 성립하는 걸 보이면 된다.
k번째 루프를 돌고나면 귀납적으로 a[0] < a[1].. a[k-1], a[k-1] < a[x] if x >k-1 을 만족한다.   
그리고 a[k], .. a[n-1]중에 최소 값을 a[k]로 옮겼다.   
k+1번째 루프가 끝났을 때,   
a[0] < a[1].. a[k-1] < a[k]에서 a[0] < a[1].. a[k-1]은 귀납적으로 이미 만족하며, a[k-1] < a[k]에서 a[k]는 (2)에서 x가 k일 때이므로 성립한다.   
a[k] < a[x] if x > k 은 최소값을 a[k]로 옮겼기 때문에 성립한다.  

# Recursive Selection Sort
## Proof of Correctness of Sorting
>Sorting이 되었다는 것을 어떻게 증명할까... 위 조건과 마찬가지이다.

조건1. sorting이 끝나고 집합조건을 깨면 안된다.
조건2. sorting이 끝나고 배열에 저장된 값을 b[0],b[1]..b[n-1]이라고 할때, b[0] < b[1]..< b[n-1]을 만족한다.

## Proof (Invariant) by Induction
1.집합 조건을 깰 수 있는 코드는 지금도 없다.   
Base: n=1일 때, 아무것도 안해도 이미 sorting이 되어 있다.   
Step: n-1일 때, sort() 함수가 성공한다면... 즉, 재귀 호출이 끝나고 a[1] < a[2] .. < a[n-1]이라면 n일 때, sort()함수가 성공한다. 즉, a[0] < a[1] < a[2] .. < a[n]을 성립한다 를 보이면 된다.

코드에서 재귀를 호출하기 전에 a[0] < a[1], a[0] < a[2].. a[0] < a[n-1]이 성립한다. 그 다음에 재귀호출이 다 끝난 다음에 a[0] < a[1]가 성립한다.(집합 조건 깨지지 않기 때문에 재귀 호출전 a[1]은 a[1], a[2]..a[n-1]중에 하나이니까), 따라서, 귀납적으로 n-1일 때, a[1] < a[2] .. < a[n-1]을 만족하고 a[0] < a[1]이 성립하므로 a[0] < a[1] < a[2] .. < a[n-1]도 성립한다.   
결국 조건1, 조건2를 만족하므로 sorting이 된다.

# Reculsive Merge Sort
## Proof by Induction

1.집합 조건을 깰 수 있는 코드는 지금도 없다.   
* 코드에서 a 에서 b로 그대로 copy하고, sort(b, h)와 sort(b+h, n-h)는 재귀적으로 집합조건을 깨지 않는다. 재귀가 끝난 후에 b에 있는 원소들을 Merge해도 원래 집합과 같다.   
(Merge 알고리즘은 합집합을 하면서 sorting되게 만들기 때문에 집합조건을 깨지 않는다.)

Base: n=1. 아무것도 안해도 이미 sorting이 되어 있다.   
Step: n/2일 때 sort()함수가 성공한다면... 즉, 재귀호출이 끝나고 b[0] < b[1] .. b[n/2-1]와 b[n/2] < b[n/2+1] .. b[n-1]을 만족한다면 n일 때 sort()함수가 성공한다... 즉, Merge의 정확성(Merge 알고리즘은 합집합을 하면서 sorting되게 만들기 때문에 집합조건을 깨지 않는다.)에 의해서 b[0] < b[1] < .. b[n-1]가 성립한다. 

# Quick Sort
## Proof by Induction
1.집합 조건을 깰 수 있는 코드는 지금도 없다.   
Base: n=1. sorting할게 없다.   
Step: qsort(a,d)와 qsort(a+d+1, n-d-1)가 성공한다면... 즉, 재귀 호출이 끝난 후 a[0] < a[1] .. a[d-1] 그리고 a[d+1] < a[d+2] .. a[n-1]이라면,   
qsort(a,n)이 성공한다... 즉, a[0] < a[1] ... a[d-1] < a[d] < a[d+1] ... a[n-1]이 성립한다.   
왜냐하면 a[0] < a[1] ... a[d-1]와 a[d+1] < a[d+2] .. a[n-1] 은 위에서 성공하는 것으로 하였고 a[d-1] < a[d] 와 a[d] < a[d+1]은 a[d-1]은 a[x]중에 하나이고 a[d+1]은 a[y]중에 하나이기 때문에 성립한다.

* Rearrange 부분에서 재귀호출을 부르기전에 if x < d 이면, a[x] < a[d] 와 if y > d 이면, a[y] > a[d]임을 보장했기 때문에


> 즉, Invariant는 항상 성립하며 조건1, 조건2가 사실이므로 그 결과 sorting된다.


## Merge 알고리즘의 정확성 증명
> sorting된 두 배열을 합집합해서 sorting된 배열을 만든다.

* invariant: 어떤 두 집합에 대해 Merge 알고리즘을 수행하고 나면 집합 조건이 깨지지 않고 sorting되어 있다.

### Proof by Induction
1. 집합조건을 깰 수 있는 부분이 없다. 단지 두 집합을 합집합하기 때문이다.

2.Base n = 0: vacously true이다.
step: 
귀납적으로 두 집합에 대해 n-1번째까지 Merge algorithm을 성공한다면 n번째도 성공한다.  n-1번째까지 merge에 성공했으니까 n-1개의 원소 모두가 정렬이 되었다는 뜻이다. 즉, a1~a(n-1)을 두 집합의 원소라고 할 때
a1 < a2 < a3 ... a(n-1) 이다. 남은 an은 Merge(n-1)에서 a(n-1) < a(n) 이었기 때문에 Merge(n-1)했을 때 정렬된 집합에 a(n-1)이 들어갔으므로 Merge(n)은 (a1 < a2 < a3 ... a(n-1) < a(n))을 성립한다.

따라서 Merge 알고리즘은 정확하다.