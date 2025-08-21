# Priority-Queue-based-on-task-manager
#include <stdio.h>
#include <stdlib.h>

#define MAX 100

// Structure to represent the priority queue
typedef struct {
int items[MAX];
int size;
} PriorityQueue;

18

// Function to enqueue (add) an element with a given priority
void enqueue(PriorityQueue* pq, int element) {
if (pq->size >= MAX) {
printf("Priority queue is full. Cannot enqueue.\n");
return;
}
pq->items[pq->size++] = element;

// Restore the heap property by comparing with parent
int child = pq->size - 1;
int parent = (child - 1) / 2;
while (child > 0 && pq->items[child] < pq->items[parent]) {
// Swap child and parent
int temp = pq->items[child];
pq->items[child] = pq->items[parent];
pq->items[parent] = temp;

child = parent;
parent = (child - 1) / 2;
}
}

// Function to dequeue (remove) the highest-priority element
int dequeue(PriorityQueue* pq) {
if (pq->size == 0) {
printf("Priority queue is empty. Cannot dequeue.\n");
return -1; // Return a sentinel value
}
int root = pq->items[0];

18
pq->items[0] = pq->items[--pq->size];

// Restore the heap property by comparing with children
int parent = 0;
while (1) {
int leftChild = 2 * parent + 1;
int rightChild = 2 * parent + 2;
int minChild = parent;

if (leftChild < pq->size && pq->items[leftChild] < pq->items[minChild])
minChild = leftChild;
if (rightChild < pq->size && pq->items[rightChild] < pq->items[minChild])
minChild = rightChild;

if (minChild == parent)
break; // Heap property restored

// Swap parent and minChild
int temp = pq->items[parent];
pq->items[parent] = pq->items[minChild];
pq->items[minChild] = temp;

parent = minChild;
}

return root;
}

// Function to peek (retrieve) the highest-priority element without removing it

18

int peek(PriorityQueue* pq) {
if (pq->size == 0) {
printf("Priority queue is empty.\n");
return -1; // Return a sentinel value
}
return pq->items[0];
}

int main() {
PriorityQueue pq = { .size = 0 };

// Get user input
int choice;
printf("Priority Queue Operations\n");
printf("1. Enqueue\n");
printf("2. Dequeue\n");
printf("3. Peek\n");
printf("4. Exit\n");
while (1) {
printf("Enter your choice: ");
scanf("%d", &choice);

switch (choice) {
case 1:
printf("Enter element to enqueue: ");
int element;
scanf("%d", &element);
enqueue(&pq, element);
break;

18

case 2:
if (pq.size == 0) {
printf("Priority queue is empty. Cannot dequeue.\n");
break;
}
printf("Dequeued element: %d\n", dequeue(&pq));
break;
case 3:
if (pq.size == 0) {
printf("Priority queue is empty.\n");
break;
}
printf("Highest-priority element: %d\n", peek(&pq));
break;
case 4:
return 0;
default:
printf("Invalid choice. Please try again.\n");
}
}

return 0;
}
