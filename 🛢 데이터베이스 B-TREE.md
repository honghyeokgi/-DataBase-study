## 🛢 데이터베이스 B-TREE

--------------------------------------------------------------------------------

### B-TREE :  최상단 ROOT 노드부터 자식노드의 개수가  두개 이상의 트리이다.

노드내 최대 데이터의 수가 2개라면 2차 B-TREE  이런식으로 N개라면 N차 B-TREE 라고 부른다.

###### 성립조건 

1. 노드의 데이터수가 n개라면 자식노드의 개수는 n+1개 이다.

![img](https://hyungjoon6876.github.io/jlog/assets/img/20180720/btree_1.png)

2. 노드의 데이터는 반드시 정렬되 어있어야 한다.

3. 노드의 자식노드의 데이터들은 노드 데이터 기준으로 데이터보다 작은 값은 왼쪽 서브트리

   큰값은 오른쪽 서브트리에 이루어진다.

![](https://hyungjoon6876.github.io/jlog/assets/img/20180720/btree_3.png)

위 그림에서 노드의 데이터가 8,13 이므로 8보다 작은 데이터는 왼쪽 서브트리

가운데 서브트리에는 8과 13의 사이 데이터

13보다 큰 데이터는 오른쪽 서브트리에 담긴다.

4. ROOT 노드가 자식이 있다면 반드시 2개 이상의 자식을 가져야한다.
5. ROOT 노드를 제외한 모든 노드는 적어도 N/2 개의 데이터를 가져야 한다.

![img](https://hyungjoon6876.github.io/jlog/assets/img/20180720/btree_4.png)

위 그림을 보면 왼쪽 노드의 데이터가 1개 이므로 5번 조건에 해당 되지 않으므로 부합합니다.

6. LEAF 노드로 가는 경로의 길이는 모두 같아야 한다.( LEAF 노드는 모두 같은 레벨에 존재해야한다.)
7. 입력 자료는 중복이 불가하다.

--------------------------------------------------------------------------------

##### 탐색

탐색하고자 하는 값을 ROOT 노드부터 시작해 하향식으로 탐색한다.

##### 삽입

![](https://hyungjoon6876.github.io/jlog/assets/img/20180720/btree_6.png)

- 초기 삽입시 ROOT 노드를 생성 ( insert 5) 3차 B-TREE 기준

insert 7 에서 노드의 개수가 가득 찼으므로 분리

insert 12 에서 7,9,12 의 데이터를 가진 노드가 가득차서 분리를 진행했지만 B-TREE 조건에 위배되므로

1. LEAF 노드를 모두 같은 레벨에 존재하도록 변경
2. ROOT 노드와 merge로 조건을 충족



##### 삭제

1. LEAF 노드를 삭제하는경우
2. LEAF 노드가 아닌 데이터를 삭제하는 경우



![](https://hyungjoon6876.github.io/jlog/assets/img/20180720/btree_8.png)

1의 LEAF 노드를 삭제하는경우 B-TREE 구조가 깨지므로 삭제한 노드의 부모 노드로 올라가며 데이터를 가져온다.

1의 부모 노드와, 형제 노드를 merge 하며 부모 노드에서 자식노드로 값을 가져와 자식노드의 형제노드와

merge를 진행 root 까지 올라가며 B-TREE 조건에 맞을때 까지 반복한다.

(2) 1의 값을 가진 LEAF 노드 삭제 

(3) 부모노드로 올라가 5의 값을 가져옵니다

(4) 5의 부모노드로 올라가 9를 가져오며 형제노드와 merge 를 진행하여 완전한 B-TREE 구조를 생성





![](https://hyungjoon6876.github.io/jlog/assets/img/20180720/btree_9.png)

LEAF 노드에 위치하지 않는 데이터를 삭제할 경우

노드에서 데이터를 삭제하고 왼쪽 서브트리에서 가장 큰값을 노드에 위치시킵니다.

같은 방식으로 부모노드에서 자식노드로 값을 가져와 형제노드와 merge 시키며 

B-TREE 조건이 맞을때 까지 반복합니다.





출처 

- https://hyungjoon6876.github.io/jlog/2018/07/20/btree.html
- https://www.cs.usfca.edu/~galles/visualization/BTree.html
- [https://ko.wikipedia.org/wiki/B_%ED%8A%B8%EB%A6%AC#%EC%82%BD%EC%9E%85](https://ko.wikipedia.org/wiki/B_트리#삽입)



