---
layout: post
title:  "C++ priority_queue 사용법"
date:   2017-06-06 15:30:00 +0900
categories: jekyll update
---
<!-- MarkdownTOC -->

- [Primitive type](#primitive-type)
- [Userdefined type](#userdefined-type)

<!-- /MarkdownTOC -->

<a name="primitive-type"></a>
### Primitive type

```c++
#include <iostream>
#include <queue>

using namespace std;

int main() {
	priority_queue<int, vector<int> > pq;

	pq.push(3);
	pq.push(6);
	pq.push(5);
	pq.push(7);
	pq.push(4);

	for (int i = 0; i < 5; ++i) {
		cout << pq.top() << endl;
		pq.pop();
	}
	return 0;
}
```
```
7
6
5
4
3
```

priority_queue는 default Comparator가 less<type> 이므로 내림차순으로 정렬하기 위해선
다음과 같이 <functional> 헤더에 있는 greater<type>을 이용한다.
```
priority_queue<int, vector<int>, greater<int> > pq;
```

<a name="userdefined-type"></a>
### Userdefined type
사용자 정의 타입을 우선순위 큐에 적용하기 위해서는 Comparator를 타입에 맞게 작성해줘야 한다.
```c++
class UserType {
public:
	int x;
	int y;
	UserType(int x, int y) {
		this->x = x;
		this->y = y;
	}
};

class Comparator {
public:
	bool operator()(const UserType &a, const UserType &b) {
		return a.x > b.x;
	}
};

priority_queue<UserType, vector<UserType>, Comparator > pq;
int main() {

	pq.push(UserType(3,3));
	pq.push(UserType(1, 5));
	pq.push(UserType(2, 4));

	for (int i = 0; i < 3; ++i) {
		cout << pq.top().x << "," << pq.top().y << endl;;
		pq.pop();
	}
	return 0;
}
```
```
1,5
2,4
3,3
```