# queues


- review question
    - what is the meaning of == applied to primitive types and objects?
        - primitive types: used to compare types and check if their **values are equal**
        - object: checks whether two objects have the **same memory addresses**
- exercises on stacks
    1. suppose that integers 0 through 9 are pushed into an originally empty stack in order— there are ten successful pop operations where the pop value is printed out— which sequence cannot be the output?
    2. what is the running time of push and pop?
        - both are constant, therefore their runtime is **O(1)**
    3. suppose that P,O,L,Y,U are pushed onto an originally empty stack in order and there are 4 successful pop operations— in the end, there is one letter remaining in the stack, which letter is it? 
        - all of them are possible to be left out

# queues

- a queue’s state is modeled as a sequence of elements, initially empty
    
    
    | enqueue(x) | inserts x at the rear or tail of the queue  |
    | --- | --- |
    | dequeue() | removes the item at the front of the queue and returns the element that was deleted |
    | isEmpty() | tests if the queue is empty |
    | peek() | looks at the object in the front |
- implemented using first-in first-out

## indices

```java
//enqueue only changes rear 
//dequeue only changes front 

class QueueArray {
    int rear, front, capacity; 
    int q1[]; 

    QueueArray(int size) { //constructor used to make the queue object later
        capacity = size; 
        q1 = new int[capacity];
        rear = -1; // 0 and -1 at initialization since when there is only one element in the queue
        front = 0; // both rear and front will point to the same element so we only change rear + 1 
    }
    
    boolean isFull() {
    	return (rear == capacity -1);
    }
    
    boolean isEmpty() {
    	return(rear == 0);
    }

	void enqueue(int value) {}
	void dequeue() {}
	void print() {}
}
```

- exercise
    - how many possible combinations for this: enqueue(1) enqueue(2) enqueue(3) enqueue(4) enqueue(5) enqueue(6) dequeue() dequeue() dequeue() dequeue() dequeue() dequeue()
    - there is only ONE possible combination
    

## overflow

- there must be an error during underflow, but for overflow, we have two choices:
    1. increase the stack/queue size when its full
        - create a new queue with double capacity and copy the elements from the original queue to the new one
    2. let the user check before inserting 
        - utilizing the isEmpty() operation to alert the user to prevent overflow

### difference between stacks and queues

- in stacks and queues, only one data item can be accessed
    - in a stack, this is the last item inserted
    - in a queue, this is the first item inserted

- operations have special names
    - in a stack, it is push/pop
    - in a queue, it is enqueue/dequeue
- both can be implemented with (fixed-length) arrays
- for a queue, the indices wrap around from the end of an array to the beginning using a circular array
- arithmetic expressions are typically evaluated by translating infix notation (3-1) to postfix notation (31-), then evaluating the postfix notation

## priority queues

- a priority queue allows access to the smallest or largest item by inserting and removing the item with the smallest key
    - exercise
        - store a priority queue as an array
            - put the element in the proper position
            - search the element with the smallest key when removing
            
            ```java
            package queuesll;
            
            class QueueArray {
                int rear, front, capacity; 
                int q1[]; 
            
                QueueArray(int size) { //constructor used to make the queue object later
                    capacity = size; 
                    q1 = new int[capacity];
                }
            
                void enqueue(int value) {
                    if(rear == capacity) { 
                        System.out.println("Queue is full");
                    } else {
                        q1[rear] = value; //sets last value to the value
                        rear++; //shifts rear position
                        
                        insertionSort(); //sort the queue each time we insert a new value
                    }
                }
            
                void dequeue() {
                    if(rear == 0) {
                        System.out.println("Queue is empty");
                    } else {
                    	rear--; //remove the element with the highest priority (at the front) 
                    	System.out.println(q1[0]); //since queue is sorted, highest priority is q1[0]
                        for(int i=0; i<rear-1; i++) {
                            q1[i] = q1[i+1]; //shifts each value to the right
                        }
                    }
                }
            
                void display() {
                    if(rear == 0) {
                        System.out.println("Queue is empty");
                    } else {
                        for(int i=0; i<rear; i++) {
                            System.out.print(q1[i] + " ");
                        }
                    }
                }
                
                private void insertionSort() {
                	for(int i=1; i<rear; i++) {
                		int key = q1[i];
                		int j = i-1;
                		while(j>=0 && q1[j] > key) {
                			q1[j+1] = q1[j];
                			j--;
                		}
                		q1[j+1] = key;
                	}
                }
            }
            
            public class Qeueu {
            	public static void main(String[] args) {
            	  QueueArray q2 = new QueueArray(5);
            		  q2.enqueue(2);
            			q2.enqueue(3);
            			q2.enqueue(4);
            			q2.display();
            	    System.out.println();
            			q2.dequeue();
            	    q2.display();
            	 }
            }
            ```
            
        - in the ordered way, do you want to sort them increasingly or decreasingly?
            - we sort them increasingly
