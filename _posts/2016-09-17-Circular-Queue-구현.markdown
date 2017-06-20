---
layout: post
title: "Circular Queue 구현"
author: 뻘짓마스터(hsnks100@gmail.com)
date: 2016-09-17 08:46 +0900
tags: Circular Queue
category: programming
comments: true
---
* table of contents
{:toc}


``` cpp
template <typename Node>
class CircularQueue
{
private:
    Node* node; // 노드 배열
    int capacity; // 큐의 용량
    int front; // 전단의 인덱스
    int rear; // 후단의 인덱스
public:
    CircularQueue(int capacity)
    {
        this->capacity = capacity + 1; // 노드 배열의 크기는 실제 용량에서 1을 더한 크기 (더미 공간 때문)
        node = new Node[this->capacity]; // Node 구조체 capacity + 1개를 메모리 공간에 할당한다
        front = 0; rear = 0; // 전단과 후단의 초기화
    }
     
    ~CircularQueue()
    {
        delete []node; // 노드 배열 소멸
    }
    void clear()
    {
        front = rear = 0;
    }
    void enqueue(const Node& data)
    {
        int pos; // 데이터가 들어갈 인덱스
        pos = rear;
        rear = (rear + 1) % (capacity);
        node[pos] = data; // pos 번째의 노드의 데이터에 data를 대입한다
    }
    Node dequeue() {
        int pos = front; // pos에 전단의 인덱스 대입
        front = (front + 1) % capacity;
        return node[pos]; // 제외되는 데이터를 반환한다
    }
    int getSize() {
        if (front <= rear) // 전단의 인덱스가 후단의 인덱스와 같거나 그보다 작다면
            return rear - front; // 후단의 인덱스에서 전단의 인덱스를 뺀값을 반환한다
        else // 전단의 인덱스가 후단의 인덱스보다 크다면
            return capacity - front + rear; // 용량에서 전단의 인덱스를 뺀 뒤에 후단의 인덱스를 더한 값을 반환한다
    }
    bool isEmpty() {
        return front == rear; // 전단의 인덱스와 후단의 인덱스가 같을 경우 true, 아니면 false
    }
    bool isFull() {
        return front == (rear + 1) % capacity;
    }
    int getRear() { return rear; }
    int getFront() { return front; }
    vector<Node> getTotalData()
    {
        int tempFront = front;
        vector<Node> T;
        while(tempFront != rear)
        {
            T.push_back(node[tempFront]);
            tempFront = (tempFront + 1) % capacity;
        }
        return T;
    }
};

```


배열로 간단히 구현한 버전..
어디서 들고 왔는지 까먹었는데 간단히 쓰기에 좋음.
