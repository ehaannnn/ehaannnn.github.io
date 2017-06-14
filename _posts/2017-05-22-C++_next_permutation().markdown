---
layout: post
title:  "C++ next_permutation 사용법"
date:   2017-05-22 13:55:56 +0900
categories: jekyll update
---

<!-- MarkdownTOC -->

- [기본 사용법](#%EA%B8%B0%EB%B3%B8-%EC%82%AC%EC%9A%A9%EB%B2%95)
- [overloading](#overloading)

<!-- /MarkdownTOC -->

next_permutation은 주어진 배열에서 순열을 생성해주는 함수이다.

<a name="%EA%B8%B0%EB%B3%B8-%EC%82%AC%EC%9A%A9%EB%B2%95"></a>
### 기본 사용법

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <vector>

using namespace std;


int main() {
	int N = 3;
	vector<int> list;

	for (int i = 0; i < N; ++i) list.push_back(i+1);
	
	do {
		for (int i = 0; i < N; ++i) printf("%d ",list[i]);
		printf("\n");
	} while(next_permutation(list.begin(),list.end()));

	return 0;
}
```
```
1 2 3
1 3 2
2 1 3
2 3 1
3 1 2
3 2 1
```

오름차순으로 정렬되어 있을 경우에 모든 순열을 탐색 할 수 있다.
정렬 되어있지 않은 배열을 이용할 경우 아래와 같은 결과가 나온다.

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <vector>

using namespace std;


int main() {
	int N = 3;
	vector<int> list;

	for (int i = N-1; i >= 0; --i) list.push_back(i+1);
	
	do {
		for (int i = 0; i < N; ++i) printf("%d ",list[i]);
		printf("\n");
	} while(next_permutation(list.begin(),list.end()));

	return 0;
}
```
```
3 2 1
```
next_permutation은 "1,2,3"을 가장 작은 순열 "3 2 1"을 가장 큰 순열로 판단하여 
더 이상 큰 순열이 존재하지 않을 경우 false를 return하는 구조로 만들어졌기 때문에 이런 결과가 발생한다.

내림차순을 기준으로 순열을 생성하고 싶거나 사용자가 정의한 함수를 기준으로 생성하고 싶은 경우는 아래와 같이 사용 가능하다.

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <vector>
#include <functional>

using namespace std;


int main() {
	int N = 3;
	vector<int> list;

	for (int i = N-1; i >= 0; --i) list.push_back(i+1);
	
	do {
		for (int i = 0; i < N; ++i) printf("%d ",list[i]);
		printf("\n");
	} while(next_permutation(list.begin(),list.end(),greater<int>() ));

	return 0;
}
```
```
3 2 1
3 1 2
2 3 1
2 1 3
1 3 2
1 2 3
```

<a name="overloading"></a>
### overloading
next_permutation은 비교 연산자로 operator<를 사용하는데 이를 overloading하면
사용자가 정의한 자료구조에서도 사용 가능하다.

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <vector>

using namespace std;

struct MyStruct{
	int x,y;
	bool operator<(MyStruct &next) {
		return y < next.y;
	}
};

int main() {
	int N = 3;
	vector<MyStruct> list;

	list.push_back({3,1});
	list.push_back({2,2});
	list.push_back({1,3});

	cout << "before -" << endl;
	for (int i = 0; i < N; ++i) {
		cout << "(" << list[i].x << " " << list[i].y << ")" << " ";
	}
	cout << endl;
	
	cout << "After -" << endl;
	do {
		for (int i = 0; i < N; ++i) cout << "(" << list[i].x << " " << list[i].y << ")" << " ";
		cout << endl;
	} while(next_permutation(list.begin(),list.end()));
	
	return 0;
}
```
```
before -
(3 1) (2 2) (1 3)
After -
(3 1) (2 2) (1 3)
(3 1) (1 3) (2 2)
(2 2) (3 1) (1 3)
(2 2) (1 3) (3 1)
(1 3) (3 1) (2 2)
(1 3) (2 2) (3 1)
```
아래와 같이 수정하면
```
bool operator<(MyStruct &next) {
	return x < next.x;
}
```
```
before -
(3 1) (2 2) (1 3)
After -
(3 1) (2 2) (1 3)
```
와 같은 결과가 나온다.

