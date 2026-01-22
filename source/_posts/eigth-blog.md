---
title: 排序
tags: 算法
date: 2025-03-20
top_img: transparent
comments: false
---

# 排序

## sort 函数

ort 是 C++ <algorithm> 库中的一个高效排序函数，基于 快速排序 或 内省排序 实现，平均时间复杂度 O(n log n)

**默认升序排序（从小到大）**

而实现其他排序方式可以自己编写一个函数 cmp 作为 sort 的第三个参数
例：

```cpp
#include<bits/stdc++.h>
using namespace std;
struct di{
    int x;
    int y;
    int z;
};
bool cmp(di a,di b){
    if(a.z>b.z){
        return 1;
    }
    else{
        return 0;
    }
}
double jl(di a,di b){
    return sqrt(pow(a.x-b.x,2)+pow(a.y-b.y,2)+pow(a.z-b.z,2));
}
int n;
int main(){
    cin>>n;
    struct di din[n+2];
    for (int i = 0; i < n; i++){
        cin>>din[i].x>>din[i].y>>din[i].z;
    }
    sort(din,din+n,cmp);
    double sum=0.0;
    for (int i = 0; i < n-1; i++){
        sum+=jl(din[i+1],din[i]);
    }
    cout<<fixed<<setprecision(3)<<sum<<endl;
    return 0;
}
```

## 桶排序

```cpp
#include<bits/stdc++.h>
using namespace std;
int g;
int main(){
    int n;
    int N[105];
    bool a[1001]={0};
    cin>>n;
    //去重，排序
    //桶排序
    for (int i = 0; i < n; i++){
        cin>>N[i];
        if(a[N[i]]==0){
            g++;
        }
        a[N[i]]=1;
    }
    cout<<g<<endl;
    for (int i = 0; i < 1000; i++){
        if(a[i]){
            cout<<i<<' ';
        }
    }
    return 0;
}
```

比较大小时可考虑利用字典序，有奇效
