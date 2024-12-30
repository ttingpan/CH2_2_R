# [3번 과제] CH 2 템플릿 및 STL
## 도전 기능
- 필수 기능을 모두 완료한 후, 아래 기능을 추가 합니다.
- 복사 생성자를 구현 합니다.
- 아래 멤버함수를 추가로 변경/구현 합니다.
    - push_back에서 배열의 크기가 꽉 찼는데 원소가 더 들어올경우, 기존 배열보다 크기를 5만큼 더 늘리고 새로운 원소까지 추가됩니다.(기존에 있던 값도 유지되야 합니다.)
    - resize는 정수 하나를 인자로 받습니다.  해당 정수가 현재 배열의 크기보다 작으면 아무 동작도 하지 않습니다. 만약 현재 배열보다 크기가 크면 해당 값만큼 크기를 재할당 합니다.(기존 원소는 그대로 있어야 합니다.)
    - sortData는 내부 데이터를 정렬하는 함수 입니다. 직접 정렬하지 않고 STL의 sort함수를 활용해서 정렬 합니다.
    
- 구현을 한 뒤에 클래스의 구조는 아래와 같습니다.

![image](https://github.com/user-attachments/assets/3be7edca-b271-4933-8e65-6d356f08af5f)


## 평가 기준
- 배열 크기 확장 및 정렬 로직이 요구사항에 맞게 동작하는지 확인
- 원소 추가시 메모리를 재 할당하는 경우에 어떤 방식으로 코드가 동작하는지 그림을 그려서 설명할 수 있는지 확인
- 요구사항에 있는 생성자 구현시, 다수의 생성자를 만들지 않고, 기본인자를 활용해서 하나의 생성자로 처리할 수 있는지(처리했는지) 확인
- 메모리를 재 할당해야 하는 경우에 예외가 발생하지 않고 정상적으로 동작하는지 확인
- 소멸자에서 동적할당한 메모리가 제대로 해지되고 있는지 확인

```c#
#include <iostream>
#include <algorithm>

using namespace std;

template<typename T>
class SimpleVector
{
private:
	T* data;
	int currentSize;
	int currentCapacity;

	void resize(int newCapacity)
	{
		if (newCapacity > currentCapacity)
		{
			T* newData = new T[newCapacity];

			for (int i = 0; i < currentSize; i++)
			{
				newData[i] = data[i];
			}

			delete[] data;

			data = newData;

			currentCapacity = newCapacity;
			cout << "배열의 크기가 " << newCapacity << "으로 늘어났습니다" << endl;
		}
	}

public:
	SimpleVector(int capacity = 10) : currentCapacity(capacity), currentSize(0)
	{
		cout << "생성자 호출! -> capacity : " << currentCapacity << " / size : " << currentSize << "으로 배열 초기화" << endl;
		data = new T[capacity];
	}

	SimpleVector(const SimpleVector& other) : currentCapacity(other.currentCapacity), currentSize(other.currentSize) {}

	~SimpleVector()
	{
		cout << "소멸자 호출! -> 배열 메모리 해제 " << endl;
		delete[] data;
	}


	void push_back(const T& value)
	{
		if (currentSize == currentCapacity)
		{
			resize(currentCapacity + 5);
		}

		cout << value << "추가" << endl;
		data[currentSize] = value;
		currentSize++;
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

	void sortData()
	{
		sort(data, data + currentSize);

		cout << "정렬 후 : ";
		for (int i = 0; i < currentSize; i++)
		{
			cout << data[i] << " ";
		}
		cout << endl;
	}
};

int main()
{
	SimpleVector<int> v(5);

	for (int i = v.capacity() + 1; i > 1; i--)
	{
		v.push_back(i);
	}

	v.push_back(1);

	v.sortData();

	int beforeSize = v.size();
	for (int i = 0; i <= beforeSize; i++)
	{
		v.pop_back();
	}

	return 0;
}
```
