# [3번 과제] CH 2 템플릿 및 STL
## 필수 기능
- 클래스의 이름은 SimpleVector라고 합니다.
- 타입에 의존하지 않고 데이터를 받을수 있는 배열을 멤버변수로 갖습니다.
- 생성자는 아래와 같이 구현 합니다.
    - 기본 생성자는 크기가 10인 배열을 만듭니다.
    - 숫자를 하나 받는 생성자는 해당 숫자에 해당되는 크기의 배열을 만듭니다.
- 아래와 같은 멤버함수를 구현 합니다.
    - push_back  인자로 받은 원소를  맨 뒤에 추가 합니다. 반환값은 없습니다. 배열의 크기가 꽉 찼는데 원소가 더 들어올경우 아무 동작도 하지 않습니다.
    - pop_back은 벡터의 마지막 원소를 제거 합니다. 만약 제거할 원소가 없다면 아무 동작도 하지 않으며, 인자 및 반환값은 없습니다.
    - size는 인자가 없고 현재 원소의 개수를 반환합니다.
    - capacity 현재 내부 배열의 크기를 반환합니다.

구현을 한 뒤에 클래스의 구조는 아래와 같습니다.

![image](https://github.com/user-attachments/assets/02b5f249-ef73-43cc-99f2-bf8ba09641c5)

## 평가 기준
- 배열의 맨 끝에 원소를 삽입/삭제 하는 기능이 요구사항에 맞게 동작하는지 확인
- 템플릿을 적용한 경우와 적용하지 않은 경우를 비교해서 설명할 수 있는지 확인
- 요구사항에 있는 생성자 구현시, 다수의 생성자를 만들지 않고, 기본인자를 활용해서 하나의 생성자로 처리할 수 있는지(처리했는지) 확인

```c#
#include <iostream>

using namespace std;

template<typename T>
class SimpleVector
{
private:
	T* data;
	int currentSize;
	int currentCapacity;

	void resize() {}

public:
	SimpleVector(int capacity = 10) : currentCapacity(capacity), currentSize(0)
	{
		cout << "생성자 호출! -> capacity : "<< currentCapacity << " / size : " << currentSize << "으로 배열 초기화" << endl;
		data = new T[capacity];
	}

	~SimpleVector()
	{
		cout << "소멸자 호출! -> 배열 메모리 해제 " << endl;
		delete[] data;
	}

	void push_back(const T& value)
	{
		if (currentSize < currentCapacity)
		{
			cout << value << "추가" << endl;
			data[currentSize] = value;
			currentSize++;
		}
		else
		{
			cout << "배열이 가득 찼습니다!" << endl;
		}
	}

	void pop_back()
	{
		if (currentSize > 0)
		{
			cout << data[currentSize - 1] << "삭제" << endl;
			currentSize--;
		}
		else
		{
			cout << "배열이 비어 있습니다!" << endl;
		}
	}

	int size() { return currentSize; }
	int capacity() { return currentCapacity; }
};

int main()
{
	SimpleVector<int> v(15);

	for (int i = 0; i <= v.capacity(); i++)
	{
		v.push_back(i + 1);
	}

	for (int i = 0; i <= v.capacity(); i++)
	{
		v.pop_back();
	}

	return 0;
}
```
