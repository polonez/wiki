---
layout  : wiki
title   : Reverse singly linked list
summary :
date    : 2020-11-03 16:50:26 +0900
updated : 2020-11-03 18:08:56 +0900
tags    : algorithm reverse single linked list
toc     : true
public  : true
parent  : algorithm
latex   : false
---
* TOC
{:toc}

# Reverse singly linked list

```swift

class Node<T> {
    let item: T
    var next: Node<T>?

    init(item: T) {
        self.item = item
    }

    init(item: T, next: Node<T>) {
        self.item = item
        self.next = next
    }
}

// setup
var head = Node(item: 1)
head.next = Node(item: 2)
head.next?.next = Node(item: 3)
head.next?.next?.next = Node(item: 4)
head.next?.next?.next?.next = Node(item: 5)

func reverse<T>(head: Node<T>) -> Node<T> {
    var now: Node<T> = head
    var prior: Node<T>? = nil
    while let next = now.next {
        now.next = prior
        prior = now
        now = next
    }
    now.next = prior
    return now
}

// test
head = reverse(head: head)
print(head.item)
print(head.next?.item)
print(head.next?.next?.item)
print(head.next?.next?.next?.item)
print(head.next?.next?.next?.next?.item)
```
