# linked lists

# linked lists

- we see linked lists in back buttons, web browsers, music queues
- separate blocks of storage (nodes) are linked together using reference/pointers where each node has two fields:
    - (1) a data component, and (2) a link that points to its succeeding node
    - head points to the first node, and its null if and only if the list is empty
- you may find the previous node of a linked list by utilizing a doubly linked list which has links that point to both its succeeding and preceding nodes

## inserting at the beginning

- first, we create a new node with the data 75, then we make it the new head, and let its link point to the old head
  
```java
public void addFirst(int data) {
	if(head == null) {
		head = new Node(data, null); //set head as the new node and set the new node to point to the last head 
	} else {
		head = new Node(data, head);
	}
}
```

## inserting at the end

- first, we create a new node with the data 38, then we find the tail and make it point to the new node and then make the new node point to null
- what is the running time? what if the list was empty?
    - for inserting at the beginning, run time is O(1)
        - if the list was empty, insertion either at the front or the rear would be the same, with run time at O(1)
    - for inserting at the end, run time is O(n) since we need to search for the last value THEN insert after
- boundary case
    
    ```java
    public class LinkedList <T> { //uses a generic type to work with different elements
    	static class Node <T> { 
    		T element; 
    		Node <T> next;  
    	} //each node object represents a single element which consists of the element to store the value and the next
    	
    	Node <T > head;
    	
    	public void insertFirst (T a) {
    		Node <T> newNode = new Node <T>(a); //creates the new node with the data
    			newNode.next = head; //sets newnode to point to the old head
    			head = newNode; //sets head = the new node
    		}
    
    	public void insertLast (T a) {
    		if(head == null) { 
    			insertFirst (a); 
    				return;
    		}
    		
    		Node <T> cur = head;
    		while (cur.next != null) //until the end of the list
    			cur = cur.next; //iterate through
    		
    		Node <T> newNode = new Node <T>(a);
    		newNode.next = null; //set newnode to point to null
    		cur.next = newNode; //set the last node to point to newnode
    }
    ```
    

```java
public void addLast(int data) {
	if(head == null) {
		addFirst(data); //if list is empty, just add the data into it 
	} else { 
		Node temp = head; //use a temp variable to represent the first value 
		while(temp.next != null) { //traverse the list until you reach the end 
			temp = temp.next; //set the temp variable to be the value right after the LAST node
		}
		temp.next = new Node(data, null); //set the last value to the new node, then set it to point to null
	}
}
```

## removing the first node

- let head point to the next node and return the first element
- do we need to remove the link?
    - since we are already updating the head to point to the next node, there already is no link between the first two nodes— however, we can set its pointer to null to effectively disconnect it from the list

## removing the last element

- find the last node of the list, let the link of its previous node point to null
- what are the boundary cases if:
    - the list is empty — if the list is empty, we can return null since head should point to null
    - it only has one element — if there is only one element, we can delete that, then set head and tail to point to null
- boundary case
    
    ```java
    public T removeFirst () {
    	if (head == null) { 
    		System.out.println ("underflow"); 
    		return null;
    	}
    	head = head.next; //head points to the next value
    	temp.next = null ; // optional but suggested 
    	return temp.element; //returns the element that was just removed
    }
    
    public T removeLast () {
    	if (head == null || head.next == null) 
    		return removeFirst(); //if only one node 
    	}
    	Node <T> secondToLast = head; //secondtolast is head 
    	Node <T > last = secondToLast.next; //last is the second value
    
    	while (last.next != null) { //until last node is reached
    	secondToLast = secondToLast.next; //second to the last iterates
    	last = last.next; //last iterates until it actually is the last node
    	}
    
    	secondToLast.next = null ; //second to the last will point to null, effectively breaking the link
    	return last.element; //return the element you just removed
    }
    ```
    

```java
public void remove(int index) {
	if(index == 0) { 
		head = head.next; //if we're removing the first value, just set head to point to the next value
	} else {
		Node temp = head; 
			for(int i = 0; i < index - 1; i++) { //iterate until the value right before the index 
				if(temp == null || temp.next == null) { //if you reach the end and value is not found, just exit 
					return;
				}
				temp = temp.next; //set temp to be the value right before the one to be removed
			}
			if(temp != null && temp.next != null) { //once the value is found, 
				temp.next = temp.next.next; //set the value to point to the value after the deleted node 
			}
	}
}
```

## complexity and improvements

|  | at/from the front | at/from the rear |
| --- | --- | --- |
| insert | O(1) | O(n) |
| delete | O(1) | O(n)  |
- how can we improve this?
    - while we cant improve the insertion and deletion from the front, we definitely can improve it from the rear by maintaining a tail pointer— by having a pointer referencing the last node in our list, we can directly access that node instead of searching the entire list
        
        ```java
        public class TailPointer {
        	class Node {
        	    int data; //data and node are accessible from outside the class
        	    Node next; 
        
        	    Node(int data, Node next) {
        	        this.data = data; 
        	        this.next = next; 
        	    }
        	}
        	public Node head;
        	public Node tail; 
        	
        	public TailPointer() {
        		head = null;
        		tail = null;
        	}
        	
        	public static void main(String[] args) {
        		TailPointer tp = new TailPointer();
        		//operations	
        	}
        	
        	 public void addFirst(int data) {
        		 if(head == null) {
        			 head = new Node(data, null); //set head as the new node and set the new node to point to the last head 
        			 tail = head; 
        	    } else {
        	    	head = new Node(data, head);
        	    }
        	 }
        
        	public void addLast(int data) {
        		if(tail == null) {
        			head = new Node(data, null);
        			tail = head; 
        		} else { 
        			tail.next = new Node(data, null);
        			tail = tail.next; 
        		}
        	}
        
        	public void removeLast() {
        		if(tail == null) {
        			return; 
        		} else if(head == tail) {
        			head = null; 
        			tail = null; 
        		} else {
        			Node secondToLast = head; 
        			while(secondToLast.next != tail) {
        				secondToLast = secondToLast.next;
        			}
        			secondToLast.next = null; 
        			tail = secondToLast;
        		}
        	}
        	
            public void printList() {
                Node temp = head; 
                System.out.print("["); 
                while(temp!= null) { 
                    System.out.print(temp.data); //until we reach the end, print each value
                    if(temp.next != null) { //after each value, add a comma 
                        System.out.print(", ");
                    }
                    temp = temp.next; //iteration 
                }
                System.out.println("]");
            }
        }
        ```
        

## augmenting

- more often than not, textbook data structures are not sufficient
    - we have seen non-standard operations like peek
    - we have also added data fields— one single reference (tail) can save us loads of time
- another field that may help us is the **size**

### pitfalls of linked lists

- be careful with boundary cases
    - the empty list
    - the list with one element
    - manipulating the first and last node
- don’t lose access to needed objects— there is usually one correct order to change the references (pointers)
- make sure a link is not null before following it
- mark the end of a list by setting the next link of the last node to point to null

- implementing queue as a linked list
    
    ```java
    public class QueueList {
    	
    	private class Node { //creating Node with data and next
    		private int data; 
    		private Node next; 
    	
    		public Node(int data) {
    			this.data = data; 
    			this.next = null;
    		}
    	}
    	
    	private Node front; 
    	private Node rear;
    	private int length;
    
    	public QueueList() { //constructor 
    		this.front = null;
    		this.rear = null;
    		this.length = 0; //length of the queue is still 0 
    	}
    
    	public int length() { //method to return the length of a list
    		return length;
    	}
    
    	public boolean isEmpty() {
    		return length == 0; //method to check if the queue is empty 
    	}
    
    	public void enqueue(int data) {
    		Node temp = new Node(data); 
    		
    		if(isEmpty()) {
    			front = temp; //front is pointing to data (front and rear should be pointing to the same node) 
    			rear = temp; 
    		} else {
    			rear.next = temp; //rear will point to new node 
    			rear = temp; 
    		}
    		temp.next = null; 
    		length++;
    	}
    
    	public int dequeue() {
    		if(isEmpty()) {
    			System.out.println("Empty!");
    			return -1; 
    		}
    		int result = front.data; //we set the value to be removed as the FIRST value 
    		front = front.next; //assign front will now point to the next value
    		
    		if(front == null) { //if front is now null 
    			rear = null; //we also set rear to null
    		}
    		length--;
    		return result; 
    	}
    
    	public void print() {
    		if(isEmpty()) {
    			return;
    		} 
    		
    		Node cur = front; 
    		while(cur != null) { //iterates through the list
    			System.out.print(cur.data + " ");
    			cur = cur.next;
    		}
    		System.out.println();
    	}
    
    	public static void main(String[] args) {
    		QueueList q = new QueueList();
    		q.enqueue(5);
    		q.enqueue(10);
    		q.print();
    		q.dequeue();
    		q.print();
    		q.dequeue();
    		q.print();
    	}
    }
    ```
    
- what is the complexity of reversing a linked list? — O(n)
    
    ```java
    public class ReversedLL {
    	
    	class Node {
    	    int data; //data and node are accessible from outside the class
    	    Node next; 
    
    	    Node(int data, Node next) {
    	        this.data = data; 
    	        this.next = next; 
    	    }
    	}
        
    	public Node head;
        
        public static void main(String[] args) {
        	ReversedLL linkedList = new ReversedLL();
    			//operations
        }
    
        //appends to the beginning of the list 
        public void addFirst(int data) {
        	if(head == null) {
        		head = new Node(data, null); //set head as the new node and set the new node to point to the last head 
        	} else {
        		head = new Node(data, head);
        	}
        }
    
        //adding at the end of the list
        public void addLast(int data) {
            if(head == null) {
                addFirst(data); //if list is empty, just add the data into it 
            } else { 
                Node temp = head; //use a temp variable to represent the first value 
                while(temp.next != null) { //traverse the list until you reach the end 
                    temp = temp.next; //set the temp variable to be the value right after the LAST node
                }
                temp.next = new Node(data, null); //set the last value to the new node, then set it to point to null
            }
        }
    
        //method to remove a value at a given index
        public void remove(int index) {
            if(index == 0) { 
                head = head.next; //if we're removing the first value, just set head to point to the next value
            } else {
                Node temp = head; 
                for(int i = 0; i < index - 1; i++) { //iterate until the value right before the index 
                    if(temp == null || temp.next == null) { //if you reach the end and value is not found, just exit 
                        return;
                    }
                    temp = temp.next; //set temp to be the value right before the one to be removed
                }
                if(temp != null && temp.next != null) { //once the value is found, 
                    temp.next = temp.next.next; //set the value to point to the value after the deleted node 
                }
            }
        }
    
        //method to add a value at a certain index
        public void add(int index, int data) { 
            if(index == 0) { //if adding at the beginning of list, just call the addFirst method 
                addFirst(data);
            } else {
                Node temp = head; 
                for(int i=0; i<index-i; i++) { //iterate through the list until you reach the value right before the index
                    temp = temp.next; //set temp to be the value right before the index 
                }
                temp.next = new Node(data, temp.next); //set temp to point to the new node, then set the node to point to the next value
            }
        }
    
        public void reverse() {
            if(head == null) { //if list is empty, just return null
                return;
            } 
    
            Node cur = head; //current = head
            Node prev = null; //previous = null since nothing comes before cur
            Node next = null; //next = null
    
            while(cur != null) { //while we traverse the list to the end
                next = cur.next; //assign next to the second node 
                cur.next = prev; //assign the second node to the previous node-- reverses the direction 
                prev = cur; //assign the previous node to point to the first node-- moves through the list
                cur = next; //assign the first node to point to the second-- moves through the list
            }
            head = prev; 
        }
    
        public void printList() {
            Node temp = head; 
            System.out.print("["); 
            while(temp!= null) { 
                System.out.print(temp.data); //until we reach the end, print each value
                if(temp.next != null) { //after each value, add a comma 
                    System.out.print(", ");
                }
                temp = temp.next; //iteration 
            }
            System.out.println("]");
        }
    }
    ```
    

# doubly linked lists

```java
public void insertfirst(int value) {
	Node newNode = new Node(value);
        
	if(head == null) {
		head = newNode; //both head and tail will point to the new node 
		tail = newNode; 
	} else {
		head.prev = newNode; //head's previous pointer will point to the new node
		newNode.next = head; //newnode will point forward to the original head
		head = newNode; //the new head will be newnode
	} 
}
```

## deletion
```java
public Node deletefirst() {
	if(isEmpty()) {
		System.out.println("Empty!");
		return null; 
  }
        
  Node temp = head; //set temp to head
        
  if(head == tail) { //if there is only one element in the list
	  head = null; //set both head and tail to null 
    tail = null;
  } else {
    head.next.prev = null; //head.next.prev is the backward link from the second node to the first
  }
    head = head.next; //head is now the next node
    temp.next = null; //temp.next is the forward link from the first node to the second and we want to break that 
    return temp; //now both the forward and backward links point to null so the connection has been cut
	}

public Node deletelast() {
	if(isEmpty()) {
	  System.out.println("Empty!");
    return null; 
  }
  Node temp = tail;
        
  if(head == tail) { //if there is only one element in the list
	  head = null; //set both head and tail to null 
    tail = null;
  } else {
     tail.prev.next = null; //tail.prev.next is the forward link from the second last node to the last node 
  }
     tail = tail.prev; //tail is now the second last node
     temp.prev = null; //temp.prev is the backward link from the last node to the second last node and we want to break that 
     return temp; //now both forward and backward links point to null so the connection has been cut 
}
```

- boundary conditions?
    - **empty list**: when list is empty, both head and tail should point to null
        - **deleting**: if list becomes empty, then both head and tail should point to null otherwise, if list only has one value, both head and tail should point to it
        - **insertion**: when list is empty, both head and tail should point to the new node

## sorting linked lists

- binary search cannot be used on linked lists because it relies on the consecutive storage— instead, we can use insertion, bubble, and selection
- does this idea work?
    - transform the doubly linked lists into an array
    - sort the array
    - transform the array back into a doubly linked list

```java
public void Bubble() {
    	Node cur; 
    	int temp;
    	boolean swapped;
    	
    	if(head == null) {
    		return;
    	}
    	do {
    		swapped = false;
    		cur = head;
    		
    		while(cur.next!= null) {
    			if(cur.data > cur.next.data) {
    				temp = cur.data;
    				cur.data = cur.next.data;
    				cur.next.data = temp;
    			}
    			cur = cur.next;
    		}
    	} while(swapped);
    }
    
    void sortedinsert(Node nn) {
    	if(sorted == null || sorted.data >= nn.data) {
    		nn.next = sorted;
    		sorted = nn;
    	} else {
    		Node cur = sorted;
    		while(cur.next != null && cur.next.data < nn.data) {
    			cur = cur.next;
    		}
    		nn.next = cur.next;
    		cur.next = nn;
    	}
    }
    
    public void Insertion(Node headref) {
    	Node cur = headref;
    	sorted = null;
    	
    	while(cur != null) {
    		Node next = cur.next;
    		sortedinsert(cur);
    		cur = next; 
    	}
    	head = sorted; 
    }
```

> **java sort**: recursion + divide and conquer
> 
> - comparison, in-place, stable
