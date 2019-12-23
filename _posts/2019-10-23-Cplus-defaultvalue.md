---
title: "[C++] 매개변수의 디폴트 값"
categories:
  - C++
tags:
  - C++
  - Default Value
comments:
  - true
toc: true
toc_sticky: true
---
## [C++] 매개변수의 디폴트 값
* 매개변수에 디폴트 값이 설정되어 있으면, 선언된 매개변수의 수보다 적은 수의 인자전달이 가능하다. 그리고 전달되는 인자는 왼쪽에서부터 채워져 나가고, 부족부분은 디폴트 값으로 채워진다.

```cpp
#include <iostream>
using namespace std;

void dv_func(int num = 7, int num2 = 8) {
	cout << "num: " << num << endl;
	cout << "num2: " << num2 << endl;
	cout << "num+num2 : " << num + num2 << endl;
}

int main() {
	dv_func(); 
  // 인자를 전달하지 않으므로, num과 num2 모두 디폴트 값이 전달된 것으로 간주
  cout << endl;
	dv_func(5); 
  // 하나만 전달할 경우 첫 번째 매개변수로 전달되고, num2는 디폴트 값이 전달된 것으로 간주
  cout << endl;
	dv_func(1, 2);
  cout << endl;
	return 0;
}
```

![](/assets/img/programming_language/1910232.png)

