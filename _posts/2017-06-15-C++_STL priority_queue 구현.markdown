---
layout: post
title:  "C++ STL priority_queue 구현"
date:   2017-11-04 20:40:00 +0900
categories: jekyll update
---


```c++
#include <iostream>
#include <stdlib.h>

using namespace std;
void swap(int &a, int &b) {
    int tmp = a;
    a = b;
    b = tmp;
}
class PriorityQueue {
public:
    PriorityQueue(int size) {
        A = (int*) malloc(sizeof(int)*size);
        count = 0;
    }
    int top() {
        if (count==0) return 0;
        else if (count==1) {
            count--;
            return A[1];
        }
        else if (count==2) {
            int ret = A[1];
            A[1] = A[count];
            count--;
            return ret;
        }
        else {
            int ret = A[1];
            A[1] = A[count];
            count--;
            shiftDown(1);
            return ret;
        }
    }
    void shiftDown(int node) {
        if (node*2 == count) {
            if (A[node] > A[node*2]) {
                swap(A[node],A[node*2]);
                //shiftDown(node*2);
            }
        }
        else if (node*2+1 <= count) {
            if (A[node*2] < A[node*2+1]) {
                if (A[node] > A[node*2]) {
                    swap(A[node],A[node*2]);
                    shiftDown(node*2);
                }
            }
            else {
                if (A[node] > A[node*2+1]) {
                    swap(A[node],A[node*2+1]);
                    shiftDown(node*2+1);
                }
            }
        }
    }
    void shiftUp(int node) {
        if (node==1) return;
        if (A[node/2] > A[node]) {
            swap(A[node/2],A[node]);
            shiftUp(node/2);
        }
    }
    void insert(int data) {
        count++;
        A[count] = data;
        shiftUp(count);
    }
private:
    int *A;
    int count;
};

int main() {
    int N;
    cin >> N;
    PriorityQueue pq(N+1);
    for (int i = 0; i < N; ++i) {
        int num;
        cin >> num;
        if (num==0) {
            cout << pq.top() << endl;
        }
        else {
            pq.insert(num);
        }
    }
    return 0;
}
```