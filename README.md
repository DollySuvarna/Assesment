# circular_queue
#include <stdio.h>
#include <stdlib.h>

#define MAX_SIZE 5

typedef struct {
    int *array;
    int capacity;
    int front;
    int rear;
} CircularQueue;

// Function prototypes
void initializeQueue(CircularQueue *queue, int size);
int is_empty(CircularQueue *queue);
int is_full(CircularQueue *queue);
void enqueue(CircularQueue *queue, int data);
int dequeue(CircularQueue *queue);
void read_queue(CircularQueue *queue);
void clear_queue(CircularQueue *queue);

int main() {
    CircularQueue cq;
    initializeQueue(&cq, MAX_SIZE);

    enqueue(&cq, 1);
    enqueue(&cq, 2);
    enqueue(&cq, 3);
    read_queue(&cq);

    dequeue(&cq);
    read_queue(&cq);

    enqueue(&cq, 4);
    enqueue(&cq, 5);
    enqueue(&cq, 6);  // Overwrites oldest data (1)
    read_queue(&cq);

    clear_queue(&cq);
    read_queue(&cq);

    free(cq.array);

    return 0;
}

void initializeQueue(CircularQueue *queue, int size) {
    queue->array = (int *)malloc(size * sizeof(int));
    queue->capacity = size;
    queue->front = queue->rear = -1;
}

int is_empty(CircularQueue *queue) {
    return (queue->front == -1);
}

int is_full(CircularQueue *queue) {
    return ((queue->rear + 1) % queue->capacity == queue->front);
}

void enqueue(CircularQueue *queue, int data) {
    if (is_empty(queue)) {
        queue->front = queue->rear = 0;
    } else if (is_full(queue)) {
        queue->front = (queue->front + 1) % queue->capacity;
    }

    queue->rear = (queue->rear + 1) % queue->capacity;
    queue->array[queue->rear] = data;
}

int dequeue(CircularQueue *queue) {
    if (is_empty(queue)) {
        printf("Queue is empty\n");
        return -1;
    }

    int data = queue->array[queue->front];

    if (queue->front == queue->rear) {
        queue->front = queue->rear = -1;
    } else {
        queue->front = (queue->front + 1) % queue->capacity;
    }

    return data;
}

void read_queue(CircularQueue *queue) {
    if (is_empty(queue)) {
        printf("Queue is empty\n");
    } else {
        int i = queue->front;
        do {
            printf("%d ", queue->array[i]);
            i = (i + 1) % queue->capacity;
        } while (i != (queue->rear + 1) % queue->capacity);
        printf("\n");
    }
}

void clear_queue(CircularQueue *queue) {
    queue->front = queue->rear = -1;
}
