---
title: BFS_DFS
date: 2021-05-11 09:05:46
tags: [机试,数据结构]
---

# 广度优先搜索(BFS)

> ​	BFS:搜索过程中，遇到分叉口时，总是先依次访问从该分叉口能直接到达的所有结点，然后再按照这些结点被访问的顺序去依次访问他们能直接到达的所有结点。以此类推，直到所有结点都被访问为止。

## 解题思路：

- 状态：需要确定所求解问题的状态，通过状态的拓展，遍历所有状态
- 状态拓展的方式：用队列去存储状态，每次去队列头部进行拓展(FIFO)
- 有效状态：利用题目要求，对BFS搜索树进行剪枝。注意有效状态数的上限，是否符合复杂度要求
- 应用场景：
  - 解决最短路径(最优问题)





## 相关例题

### 计算矩阵中块的个数

![2021-05-13_07-13](BFS-DFS/2021-05-13_07-13.png)

```c++
#include<iostream>
#include<queue>
using namespace std;
```



### 走出迷宫的最小步数





### [玛雅人的密码](https://www.nowcoder.com/practice/761fc1e2f03742c2aa929c19ba96dbb0?tpId=60&tqId=29484&tPage=1&ru=%2Fkaoyan%2Fretest%2F1001&qru=%2Fta%2Ftsing-kaoyan%2Fquestion-ranking&tab=answerKey)

> ​	题解思路:
>
> ​	首先要先确定解题的状态(构造一个结构体,保存当前字符串,移位的次数)
>
> ​     合适拓展状态:
> ​         出队时,把当前字符串从0-k-1,都与右侧的元素交换, 移位次数加一(另外要有一个vis保存已经遍历过的字符串,已经判断过的字符串不再入队)
>
> ​    结束状态:
>
> ​        找到符合要求的字符串,返回移位次数
>
> ​        队列为空,返回.    

```c++
#include<iostream>
#include<string>
#include<algorithm>
#include<set>
#include<queue>
using namespace std;
struct status{
    string cur_str;
    int n;
    status(string cur , int n) : cur_str(cur),n(n){};
};
set<string> vis;
int bfs(queue<status> sset,int len){
    while(!sset.empty()){
        status current = sset.front();
        sset.pop();
        string cur = current.cur_str;
        int n = current.n;
        for(int i=0;i<len-1;i++){
            swap(cur[i],cur[i+1]);//交换
            if(cur.find("2012")!=-1 ){
                return n+1;
            }else{
                if(vis.find(cur)==vis.end()){
                    vis.insert(cur);
                    status next(cur,n+1);
                    sset.push(next);
                }
            }
            swap(cur[i],cur[i+1]);
        }
    }
    return -1;
}
int main(){
    string str;
    int size =0;

    cin>>size;
    cin>>str;
    if(str.find("2012")!=-1){
        cout<< 0;
    }else{ 
        queue<status> sset;
        status init(str,0);
        sset.push(init);
        cout<<bfs(sset,size);
    }
}
```

