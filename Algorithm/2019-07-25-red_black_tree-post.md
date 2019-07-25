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

```python
class RBNode:
    def __init__(self, key):
        #트리 내에서 유일한 키
        self.key=key
        #노드의 색 : RED or BLACK
        #트리에 insert 연산을 할 때 먼저 새 노드의 색은 RED로 한다.
        self.color="RED"
        
        self.left_child=None
        self.right_child=None

        #부모
        self.parent=None

    def __str__(self):
        return str(self.key)

class RedBlackTree:
    def __init__(self):
        self.__root=None
        self.__NIL = RBNode(None)
        self.__NIL.color="BLACK"

    def get_root(self):
        return self.__root

    def preorder_traverse(self, cur, func, *args, **kwargs):
        if cur == self.__NIL:
            return

        func(cur, *args, **kwargs)
        self.preorder_traverse(cur.left_child, func, *args, **kwargs)
        self.preorder_traverse(cur.right_child, func, *args, **kwargs)

    def __left_rotate(self, n):
        #n's right child
        r=n.right_child
        #r's left child
        l=r.left_child

        #l을 n의 오른쪽 자식으로
        l.parent=n
        n.right_child=l

        #n.parent를 r.parent로
        #n이 루트라면, 트리 루트도 업데이트
        if n==self.__root:
            self.__root=r
        elif n.parent.left_child==n:
            n.parent.left_child=r
        else:
            n.parent.right_child=r
        r.parent=n.parent

        #n을 r의 왼쪽 자식으로
        r.left_child=n
        n.parent=r

    def __right_rotate(self, n):
        #n's left child
        l=n.left_child
        #lc's right child
        r=l.right_child

        #r을 n의 왼쪽 자식으로
        r.parent=n
        n.left_child=r

        #n.parent를 l.parent로
        #n이 루트라면 트리의 루트도 업데이트
        if n==self.__root:
            self.__root=l
        elif n.parent.left_child==n:
            n.parent.left_child=l
        else:
            n.parent.right_child=l
        l.parent=n.parent

        #n을 lc의 오른쪽 자식으로
        l.right_child=n
        n.parent=l

    def __insert_fix(self, n):
        #pn: n's parent
        #gn: n's grand parent
        #un: pn's sibling 
        pn=gn=un=None

        pn=n.parent
        #n이 루트가 아니고 
        #n.parent가 RED --> 연속된 RED
        while pn != None and pn.color=="RED":
            #pn이 RED이면 반드시 gn이 존재: 루트는 BLACK이므로 pn은 루트가 될 수 없다
            gn=pn.parent
            #1. pn이 gn의 왼쪽 자식일 때
            if gn.left_child==pn:
                un=gn.right_child
                
                #XYr : 부모 형제가 RED일 때
                if un.color=="RED":
                    #부모, 부모 형제와 조부모의 색을 변경
                    gn.color="RED"
                    pn.color=un.color="BLACK"

                    #gn을 새로운 n으로 만든 후 연속된 레드가 또 일어나는지 확인
                    n=gn
                    pn=n.parent
                    
                #XYb : 부모 형제가 BLACK일 때
                else:
                    #LRb일 때 
                    if pn.right_child==n:
                        #LEFT-ROTATE(pn)
                        self.__left_rotate(pn)
                        n, pn = pn, n
                    #LLb일 때
                    #부모와 조부모의 색을 바꾸고
                    pn.color, gn.color=gn.color, pn.color

                    #RIGHT-ROATE(gn)
                    self.__right_rotate(gn)
            #2. pn이 gn의 오른쪽 자식일 때
            else:
                #조부모의 왼쪽 자식이 외부 노드일 때
                #부모 형제를 en으로 대체
                un=gn.left_child
                if un.color=="RED":
                    gn.color="RED"
                    pn.color=un.color="BLACK"

                    n=gn
                    pn=n.parent
                else:
                    if pn.left_child==n:
                        self.__right_rotate(pn)
                        n, pn = pn, n
                    pn.color, gn.color=gn.color, pn.color
                    self.__left_rotate(gn)

        #연속된 레드가 루트까지 올라갔을 경우에는 
        #루트를 BLACK으로 만들어주면 된다
        self.__root.color="BLACK"

    def insert(self, key):
        new_node=RBNode(key)
        new_node.left_child=self.__NIL
        new_node.right_child=self.__NIL

        cur=self.__root
        if not cur:
            self.__root=new_node
            #루트 노드는 BLACK
            self.__root.color="BLACK"
            return

        while True:
            parent=cur
            if key < cur.key:
                cur=cur.left_child
                if cur==self.__NIL:
                    parent.left_child=new_node
                    #노드의 parent 설정
                    new_node.parent=parent
                    break
            else:
                cur=cur.right_child
                if cur==self.__NIL:
                    parent.right_child=new_node
                    #노드의 parent 설정
                    new_node.parent=parent
                    break
        #노드 삽입 후 처리
        self.__insert_fix(new_node)

    def search(self, target):
        cur=self.__root
        while cur!=self.__NIL:
            if cur.key==target:
                return cur
            elif cur.key > target:
                cur=cur.left_child
            elif cur.key < target:
                cur=cur.right_child
        return None

    def __remove_recursion(self, cur, target):
        if not cur:
            return self.__NIL, self.__NIL
        elif target < cur.key:
            cur.left_child, rem_node=self.__remove_recursion(cur.left_child, target)
            #왼쪽 자식 노드의 부모 설정
            if cur.left_child!=self.__NIL:
                cur.left_child.parent=cur
        elif target > cur.key:
            cur.right_child, rem_node=self.__remove_recursion(cur.right_child, target)
            #오른쪽 자식 노드의 부모 설정
            if cur.right_child!=self.__NIL:
                cur.right_child.parent=cur
        else:
            if cur.left_child==self.__NIL and cur.right_child==self.__NIL:
                rem_node=cur
                cur=self.__NIL
            elif cur.right_child==self.__NIL:
                rem_node=cur
                cur=cur.left_child
            elif cur.left_child==self.__NIL:
                rem_node=cur
                cur=cur.right_child
            else:
                replace=cur.left_child
                while replace.right_child!=self.__NIL:
                    replace=replace.right_child
                cur.key, replace.key=replace.key, cur.key
                cur.left_child, rem_node=self.__remove_recursion(cur.left_child, replace.key)
                #왼쪽 자식 노드의 부모 설정
                if cur.left_child!=self.__NIL:
                    cur.left_child.parent=cur
        return cur, rem_node

    def __remove_fix(self, c):
        #노드 c가 루트가 아니고 : 루트면 extra BLACK 제거 후 종료
        #노드 c가 BLACK이면 : RED이면 BLACK으로 만들고 종료
        while c is not self.__root and c.color=="BLACK":
            #노드 c가 왼쪽 자식 노드일 때
            if c.parent.left_child==c:
                #s: sibling
                s=c.parent.right_child

                #case 1: s.color = RED
                #case 2로 만든다
                if s.color=="RED":
                    #c.parent와 s의 컬러를 바꾼다
                    c.parent.color, s.color=s.color, c.parent.color
                    #LEFT-ROTATE(c.parent)
                    self.__left_rotate(c.parent)
                    
                #case 2: s.color = BLACK
                else:
                    #case 2-1: s.left and s.right --> BLACK
                    if s.left_child.color=="BLACK" and s.right_child.color=="BLACK":
                        #tack black from c, s
                        s.color="RED"
                        #give black to p
                        c=c.parent

                    #case 2-2: s.left --> RED
                    elif s.right_child.color=="BLACK":
                        s.color, s.left_child.color=s.left_child.color, s.color
                        self.__right_rotate(s)

                    #case 2-3: s.right --> RED
                    else:
                        s.color=c.parent.color
                        c.parent.color=s.right_child.color="BLACK"
                        self.__left_rotate(c.parent)
                        #while문을 빠져나간다
                        c=self.__root
            #노드 c가 오른쪽 자식일 때
            else:
                s=c.parent.left_child
                if s.color=="RED":
                    c.parent.color, s.color=s.color, c.parent.color
                    self.__right_rotate(c.parent)
                else:
                    if s.left_child.color=="BLACK" and s.right_child.color=="BLACK":
                        s.color="RED"
                        c=c.parent
                    elif s.left_child.color=="BLACK":
                        s.color, s.right_child.color=s.right_child, s.color
                        self.__left_rotate(s)
                    else:
                        s.color=c.parent.color
                        c.parent.color=s.left_child.color="BLACK"
                        self.__right_rotate(c.parent)

                        c=self.__root 
    
        c.color="BLACK"
        
    def remove(self, target):
        self.__root, removed_node=self.__remove_recursion(self.__root, target)
 
        #삭제된 노드가 블랙 노드인 경우
        #삭제된 노드의 자식 노드를 
        #remove_fix의 인자로 전달
        if removed_node!=self.__NIL and removed_node.color=="BLACK":
            if removed_node.left_child!=self.__NIL:
                rem_child=removed_node.left_child
            elif removed_node.right_child!=self.__NIL:
                rem_child=removed_node.right_child
            else:
                rem_child=self.__NIL
                rem_child.parent=removed_node.parent
            self.__remove_fix(rem_child)

        if removed_node:
            removed_node.left_child=removed_node.right_child=removed_node.parent=None
        return removed_node

    def print_node(self, rbn):
        if rbn:
            print("node : {}, ".format(rbn.key), end="")
            if rbn.color=="RED":
                print("color : RED, ", end="")
            else:
                print("color : BLACK, ", end="")
            if rbn.left_child:
                print("left : {}, ".format(rbn.left_child.key), end="")
            if rbn.right_child:
                print("right : {}, ".format(rbn.right_child.key), end="")
            if rbn.parent:
                print("parent : {}".format(rbn.parent.key), end="")
            print()

if __name__=="__main__":
    print('*'*100)
    rbt=RedBlackTree()
	
    for i in range(10):
	     rbt.insert(i)

    rbt.preorder_traverse(rbt.get_root(), rbt.print_node)
    # rbt.preorder_traverse(rbt.get_root(), 
    #     lambda x: print(x, end="  "))
    print("\n\n")

    # for i in range(9, -1, -1):
    #     rbt.remove(i)
    #     # rbt.preorder_traverse(rbt.get_root(), 
    #     # lambda x: print(x.key, end="  "))
    #     # print()
    #     rbt.preorder_traverse(rbt.get_root(),
    #             lambda x: print(x, end="  "))
    #     print()
    #     # rbt.preorder_traverse(rbt.get_root(), rbt.print_node)
    # print("the last root is {}".format(rbt.get_root()))
    
    rbt.remove(5)
    
    # rbt.preorder_traverse(rbt.get_root(),
    #             lambda x: print(x, end="  "))
    rbt.preorder_traverse(rbt.get_root(),
                    rbt.print_node)
    print("\n")
    print('*'*100)
```
