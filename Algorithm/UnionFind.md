## UnionFind

공통 원소가 없는, 즉 "상호 배타적" 인 부분 집합 들로 나눠진 원소들에 대한 정보를 저장하고 조작하는 자료구조

위의 설명으로는 감이 안온다. 예를 들어보자!!!

ex ) 수학여행가서 노래틀어 놓고 3명! 외치면 3명씩 뭉치는 게임을 해봤을것이다. 
그런것처럼 서로 관계가 없는 각자들이 모여 3명이되고 6명을 외치면 3명과 3명이 뭉쳐 6명이 되는 과정. 쉽게 말해 공통적인 요소가 없는 부분 들이 모여 하나의 집합을 이루는 것이다.




- 초기화 : N 개의 원소가 각자가 자신의 부모가 되도록 초기화한다.
         초기에 트리의 level은 1로 초기화한다

```
   // 초기에는 자신의 부모는 자기 자신
    for (int i = 1; i <= n; i++)
    {
        parent[i] = i;
        level[i] = 1;
    }
```
- Find (찾기) 연산 : 어떤 원소 a 가 주어질 때, 이 원소가 속한 집합을 반환합니다.
```
int find(int u)
{
    // 루트 노드이면 return u
    if (u == parent[u])
        return u;
 
    // 그 외에는 자신의 루트를 찾으러 간다. 그리고 최상위 부모를 바로 자신의 부모로 만든다.
    return parent[u] = find(parent[u]);
}
```
쉽게 말해 경로 압축화가 유니온 파인드이다. 

만약 u가 3번째 레벨에 있고 위에는 w와 v가 관계가 있다. u->w->v라면 u->v로 만드는 것이다. 
이렇게 경로를 압축하는 과정이 find이다.



- Union (합치기) 연산 : 두 원소 a, b 가 주어질 때, 이들이 속한 두 집합을 하나로 합칩니다.
```
void union(int u, int v)
{
    u = find(u);
    v = find(v);
 
    // 루트가 같다면 이미 같은 트리
    if (u == v)
        return;
 
    // u가 v보다 더 깊은 트리라면 swap
    if (level[u] > level[v])
        swap(u, v); // 항상 u가 더 작은 트리가 되도록 한다. 
 
    // u의 루트가 v가 되도록
    parent[u] = v;
 
    // u와 v의 깊이가 같을 때 v의 깊이를 늘려준다.
    if (level[u] == level[v])
        ++level[v];
}
```
유니온 과정을 통해 u는 항상 작은 트리 v는 항상 큰트리로 만들고
u를 v에 붙인다.
만약 같다면 v의 깊이를 증가시키고 u를 붙인다.



 - 시간복잡도
일단 경로 압축을 통해 우리는 트리를 압축시켰다.

따라서 find의 시간 복잡도는 다음과 같게 변한다.

find :: O(lgN)

그다음 union의 시간 복잡도를 보자.

union도 자세히 보면 find에 지배 받음을 알 수 있다.

결국 union의 시간 복잡도도 다음과 같아진다.

union :: O(lgN)

사실 아주 정확하게 말하자면 find 연산은 호출할 때마다 수행 시간이 변한다. 따라서 매우 까다로운 시간 복잡도를 가지고 있는데

실제 시간 복잡도는 O(α(N))이라고 한다. α는 애커만 함수라고 하는데 N이  2^65536일때 애커만 함수 값이 5가 된다.

따라서 그냥 상수라고 봐도 무방하다.



참고-> http://www.crocus.co.kr/683 [Crocus]




