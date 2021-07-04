---
title: LCA
date: 2021-05-19 09:24:26
tags: [数据结构,PAT]
---

## LCA

1. LCA的定义:



1. 方法1: 递归算法

   ```c++
   //root是树的根结点,二叉树使用
   struct Node{
       int val;
       Node* left;
       Node* right;
       Node(int v):val(v),left(nullptr),right(nullptr){};
   }
   
   Node* LCA(Node* root, int u,int v){
       if(root==nullptr){
           return nullptr;
       }
       if(root->val == u || root->val == v){//root是其中一个根
           return root;
       }
       Node* left = LCA(root->left,u,v);
       Node* right = LCA(root->right,u,v);
       if(left!=nullptr && right!=nullptr){
           return root;
       }else if(left!=nullptr){ //出现在左子树上
           return left;
       }else if(right!=nullptr){//出现在右子树上
           return right;
       }else{
           return nullptr;
       }
   }
   ```

   

3. 倍增寻找(ST算法)

>​	算法执行之前,我们需要统计每个结点v的深度depth(v).
>
>​     LCA的特点: 如果结点w是结点u和结点v的最近公共祖先的话:
>
>​			让u往上走(depth(w) - depth(u) )步, 让v往上走(depth(v) - depth(w) )步,u,v都将走到结点w

**寻找结点u和结点v LCA的 倍增算法的基本思路:**	

> ​	结点w未知,我们首先让u和v中较深的点往上走|depth(u)-depth(v)|步,再一起往上走.
>
>    并且利用二分搜索求出可到达的最近公共祖先的最小步数.

不过其中要维护一个father数组 , 并且`father[i][j]` 表示 结点i 往上走j-1 步得到的祖先结点,father[]

[源码]

```c++
//
// Created by doriswang on 2021/5/19.
//
#include<iostream>
#include<vector>
#include <cmath>

using namespace std;
const int MAX = 1000;
int father[MAX][MAX];
int depth[MAX];
int N ;//结点的个数
vector<int> tree[MAX] ;
/**
 *  利用dfs,初始化father数组
 * @param cur:当前结点
 * @param pre: 当前结点
 * @param d
 */
void dfs(int cur,int pre,int d){
    father[cur][0] = pre;
    depth[cur] = d;
    if(tree[cur].empty()){
        return;
    }
    int size = tree[cur].size();
    for(int i=0;i<size;i++){
        int ch =tree[cur][i];
        if(ch!=pre){ //防止出现回路
            dfs(ch,cur,d+1);
        }
    }
}

void init(int root){ ////i的2^j祖先就是i的（2^(j-1))祖先的2^(j-1)祖先
    dfs(root,-1,0);//root为根结点
    for(int j=0;(1<<(j+1))<N;j++){ // 1<<(j+1)表示 2^(j+1),j表示高度
        for(int i= 0;i<N;i++){
            if(father[i][j]<0) father[i][j+1] = -1; //到达根结点
            else father[i][j+1] = father[father[i][j]][j];
        }

    }
}

int LCA(int u,int v){
    if(depth[u]>depth[v]){
        swap(u,v);
    }
    int temp = depth[v] - depth[u];
    //原来是这么写的,我觉得有点难记忆
//    for(int i=0;(1<<i)<=temp;i++){ //使u,v再同一高度,i表示树高
//        //(1<<i)&f找到f化为2进制后1的位置，移动到相应的位置
//        if((1<<i)& temp){   //比如f=5，二进制就是101,所以首先移动2^0祖先，然后再移动2^2祖先
//            v= father[v][i];
//        }
//    }
    v = father[v][temp-1];
    if(v==u){
        return u;
    }
    //father[u][i] 是找到u的往上第i-1个祖先,这里利用了二分的思想,所以 u=father[u][i], i是不断减小的
    for( int i=(int)log2(N*1.0);i>=0;i--){
        if(father[u][i]!= father[v][i]){
            u = father[u][i];
            v = father[v][i];
        }
    }
    return father[u][0];
}

int main(){
    N =8;
    for(int i=1;i<=N;i++){
        int m=0;
        cin>>m;
        for(int j=0;j<m;j++){
            int temp;
            cin>>temp;
            tree[i].push_back(temp);
        }
    }
    init(5);
    cout<<LCA(3,2);
}


/***
0
0
2 7 6
0
2 3 8
1 4
1 2
1 1

8 //应该输出5
*/

```





