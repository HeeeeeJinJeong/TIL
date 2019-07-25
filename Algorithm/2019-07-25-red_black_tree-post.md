# Red-Black Tree
모든 노드의 컬러가 레드 혹은 블랙인 이진 탐색 트리

### 균형 이진 트리 (balanced binary tree)
- insert, search, delete 최악의 경우 O(log2n)
1. AVL
2. Red-Black

### 레드 블랙 트리의 특징
1. 트리의 모든 노드는 레드 아니면 블랙
2. 루트와 외부 노드의 컬러는 블랙
3. 루트에서 외부 노드로의 경로 중에 레드 노드가 연속으로 나올 수 없다.
4. 루트에서 외부 노드로의 모든 경로에서 블랙 노드의 수는 같다.

### 레드 블랙 트리의 높이
n개의 노드가 있는 레드 블랙 트리 높이 : 2log2(n+1)

### Red Black Node
1. key : 트리 내에서 유일한 키
2. color : ‘red’ or ‘black’
3. left_child : 왼쪽 서브 트리
4. right_child : 오른쪽 서브 트리
5. parent : 부모 노드

### insert
- BST에 노드를 상입한 후 새로운 노드의 색을 red로 한다.
- insert_fix 를 호출해 노드를 균형있게 재배열 한다.

### insert_fix
- 루트 노드가 red : black으로 바꾼다.
- 삽입된 노드와 그 부모 노드가 모두 red라면, red 노드가 연속되어 나올 수 없다는 규칙에 위반

### delete
BST에서 노드를 삭제한 후 삭제된 노드가 black이라면 delete_fix를 호출해 노드를 균형 있게 재배열 한다.

### delete_fix
- 삭제된 노드가 루트, 새로운 루트가 red : 루트노드를 블랙으로 바꾼다.
- 삭제된 노드가 블랙이므로 루트에서 외부 노드까지 모든 경로의 블랙 노드의 수는 같다는 규칙에 위반
