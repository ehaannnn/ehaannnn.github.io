---
layout: post
title:  "C++_UnionFind 함수"
date:   2017-07-12 20:50:00 +0900
categories: jekyll update
---

union-find 함수는 DisjointSet의 다른 이름으로
대표적으로 MST(최소 스패닝 트리) 구현 시 cycle을 확인하기 위하여 사용된다.
[BOJ 1922][BOJ 1922]을 풀때 union-find없이 cycle을 확인 하는 방법과 union-find를 적용하여 확인하는 방법으로 풀어보았다.

```c++
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;

typedef struct{
	int x,y,w;
}Edge;

bool cmp(const Edge a,const Edge b) {
	return a.w < b.w;
}
bool isCycle(const vector<int> &unionSet ,const int &x,const int &y) {
	if (unionSet[y] == unionSet[x]) {
		return true;
	}
	return false;
}
int main() {

	int V,E;
	cin >> V >> E;

	vector< Edge > e(E);
	for (int i = 0; i < E; ++i) {
		cin >> e[i].x >> e[i].y >> e[i].w;
	}

	
	sort(e.begin(), e.end(), cmp);

	vector<int> unionSet(V+1);
	for (int i = 1; i <= V; ++i) {
		unionSet[i] = i;
	}

	int res = 0;
	//unionSet[e[0].y] = e[0].x;
	//res += e[0].w;


	for (int i = 0; i < E; ++i) {
		int x = e[i].x;
		int y = e[i].y;
		int w = e[i].w;
		if (!isCycle(unionSet,x,y)) {	//싸이클을 만들지 않으면 엣지 추가
			res += w;
			int tmp = unionSet[y];

			for (int j = 1; j <= V; ++j) {
				if (unionSet[j] == tmp) {
					unionSet[j] = unionSet[x];
				}
			}

		}

	}

	cout << res << endl;

	return 0;
}

```

아래의 DisjointSet함수의 구현은 [blog][blog]를 참고하였다.
```c++
#include <iostream>
#include <algorithm>
#include <vector>

using namespace std;
class DisjointSet {
public:
	DisjointSet(int size) : rank(size, 1), parent(size) {
		for (int i = 1; i < size; ++i) {
			parent[i] = i;
		}
	}

	void merge(int a, int b) {
		a = find(a);
		b = find(b);
		if (a != b) {
			rank[a] < rank[b] ? parent[a] = b : parent[b] = a;
			if (rank[a] == rank[b]) rank[a]++;
		}
	}

	int find(int index) {
		if (index == parent[index]) return index;
		return parent[index] = find(parent[index]);
	}

private:
	vector<int> parent;
	vector<int> rank;
};
typedef struct {
	int x, y, w;
}Edge;

bool cmp(const Edge a, const Edge b) {
	return a.w < b.w;
}

int main() {

	int V, E;
	cin >> V >> E;

	vector< Edge > e(E);
	for (int i = 0; i < E; ++i) {
		cin >> e[i].x >> e[i].y >> e[i].w;
	}


	sort(e.begin(), e.end(), cmp);

	int res = 0;

	DisjointSet DS(V + 1);

	for (int i = 0; i < E; ++i) {
		int x = e[i].x;
		int y = e[i].y;
		int w = e[i].w;
		if (DS.find(x) != DS.find(y)) {
			DS.merge(x, y);
			res += w;
		}
	}

	cout << res << endl;

	return 0;
}
```

[blog]: http://bowbowbow.tistory.com/26
[BOJ 1922]: https://www.acmicpc.net/problem/1922