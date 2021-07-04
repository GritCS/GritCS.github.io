---
title: AVL
date: 2021-05-14 20:35:56
tags: [AVL]
---

```c++
struct Node{
    int height;
    int val;
    Node* left;
    Node* right;
    Node(int v):val(v),left(nullptr),height(1),right(nullptr){};
};
```



### 查找

```c++
Node* find(Node* root ,int val){
	if(root==nullptr){
		return nullptr;
	}
	if(root->val == val){
		return root;
	}else if (root->val>val){
        
		find(root->left , val);
        
	}else{
        
        find(root->right,val);
    }
}
```



### 插入

#### 左旋操作

```c++
void L(Node*& root){
    Node* temp = root->right;
    root->right = temp->left;
    temp->left = root;
    updateHeight(root);
    updateHeight(temp);
    root = temp;
}
```

#### 右旋操作

```c++
void R(Node* & root){//指针引用型
    Node* temp = root->left;
    root ->left = temp->right;
    temp->right = root;
    updateHeight(root);
    updateHeight(temp);
    root = temp;//根结点
}
```

#### 插入

```c++
/*
 * 左右子树高度的差值称为该结点的平衡因子
 */
int getBalanceFactor(Node* root){
    return getHeight(root->left) - getHeight(root->right);
}
void updateHeight(Node* root){
    root->height = max(getHeight(root->left),getHeight(root->right))+1;
}
```



```c++
void insert(Node*& root, int v){
    if(root == NULL){
        root = new Node(v);
        return ;
    }else{
        if(root->val>v){//向左子树插
            insert(root->left,v);//
            if(getBalanceFactor(root)==2){//如果不平衡,向左插,只会增加1
                if(getBalanceFactor(root->left)==1){//LL型
                    R(root);
                }else if(getBalanceFactor(root->left)==-1){
                    L(root->left);
                    R(root);
                }

            }
        }else{
            insert(root->right,v);
            if(getBalanceFactor(root)==-2){
                if(getBalanceFactor(root->right)==-1){//RR
                    L(root);
                }else if(getBalanceFactor(root->right)==1){//RL
                    R(root->right);
                    L(root);
                }
            }
        }
    }
       updateHeight(root);//插入之后记得update一下高度
}
```

**之前就是插入之后,忘记updateHeight(),出错了**

### 删除

<==================较为复杂,之后再填坑======>



### 建立AVL

```c++
Node* CreatAVL(vector<int> nodes){
    Node* root = NULL;
    int size = nodes.size();
    for(int i=0;i<size;i++){
        insert(root,nodes[i]);
    }
    return root;
}
```

