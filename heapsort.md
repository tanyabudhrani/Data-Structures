# heapsort

# priority queues

- a priority queue is an ADT that has insert and removeMax/removeMin, and can also support getMax/getMin and size as well

> an abstract data type (ADT) defines
- a **state of an object**, and 
- **operations that act on the object**, possibly changing the state
> 

## using priority queue to sort

- insert the elements, one by one, into the PQ
- remove the elements, one by one, from the PQ

```java
public class PQUsingLL {
	static class Node {
		int data; 
		int priority; //extra field to keep track of priority -- greater value = higher priority
		Node next; 
	}
	
	private Node head; 
	
	static Node newNode(int d, int p) { //initializing values 
		Node temp = new Node(); 
		temp.data = d; 
		temp.priority = p; 
		temp.next = null; 
		
		return temp; 
	}
	
	static int peek(Node head) { //returns topmost element 
		return head.data; 
	}
	
	static Node pop(Node head) {
		head = head.next; //update head to the next value in the list 
		return head; //return the popped head 
	}
	
	static Node push(Node head, int d, int p) {
		Node start = head; 
		Node temp = newNode(d, p); //creating newNode with data value d and priority p 
		
		if(head.priority < p) { //if current head has a lower priority
			temp.next = head; //we replace head with the newNode with a higher priority 
			head = temp; 
		} else {
			while(start.next != null && start.next.priority > p) { //otherwise, we traverse the list until we reach the end OR current node has a greater priority than newNode
				start = start.next; 
			}
			
			temp.next = start.next; //set newNode to come right after current node whose priority is greater than newNode 
			start.next = temp; //make current node point to newNode 
		}
		return head; //return node with HIGHEST PRIORITY 
	}
	
	static int isEmpty(Node head) {
		return (head == null) ? 1: 0; //checks if current queue is empty
	}
	
	public static void main(String[] args) {
		Node pq = newNode(4, 1); 
		pq = push(pq, 5, 2); 
		pq = push(pq, 6, 3); 
		pq = push(pq, 7, 0); 
		
		while(isEmpty(pq) == 0) {
			System.out.println(peek(pq));
			pq = pop(pq);
		}
	}
}
```

- exercise
    - which of the following can be used to implement a priority queue?
        
        
        | sorted array | elements can be inserted into the array using their priority as an index |
        | --- | --- |
        | maximum heap | the root node represents the node with the highest priority  |
        | minimum heap | the root node represents the node with the highest priority  |
        | binary search tree | accessing a BST using inorder will allow for ascending access  |
        | self-balancing BST | highest priority will always be at the root of the tree  |

- priority queues ≠ heaps but priority queues are almost always implemented using a heap

```java
/*
	 * every item has a priority associated with it 
	 * an element with high priority is dequeued first 
	 * if two elements have the same priority, it depends on their order in the queue 
	 * 
	 * since the heap is maintained as a COMPLETE BINARY TREE, we can represent it as an array
	 * where the root of the array is the max elem (or in this case, highest priority) 
 */

public class PQUsingHeap {
	static int[] H = new int[50]; //initialization of the array to store the heap 
	static int size = -1; //array is currently empty 
	
	
	static void up(int i) {
		int parent = (i-1)/2;
		
		while(i>0 & H[parent] < H[i]) { //while parent is less than current node
			swap(parent, i); //swap values 
			i = parent; //update current node as new parent 
		}
	}
	
	static void down(int i) {
		int lC = (2*i)+1, rC = (2*i)+2, max = i; 
		
		if(lC <= size && H[lC] > H[max]) { //if leftChild is greater than current index
			max = lC; //update max to leftChild
		}
		if(rC <= size && H[rC] > H[max]) { //if rightChild is greater than current index
			max = rC; //update max to rightChild
		}
		
		if(i != max) { //base case -- if current node is not the max
			swap(i, max); //swap values recursively until i is the max 
			down(max); 
		}
	}
	
	static void insert(int p) {
		H[size] = p; //insert value at the END of the array 
		up(size++); //bubble up the newly added value and update size 
	}
	
	static int extractMax() { 
		int res = H[0]; //store max (ROOT) as res
		H[0] = H[--size]; //update root to the last value of the array and update size
		down(0); //bubble down the new root 
		return res; 
	}
	
	static void priority(int i, int p) { //USED TO UPDATE PRIORITY 
		int old = H[i]; //set current priority as "old" 
		H[i] = p; //update index's priority to new priority 
		
		if(p>old) { up(i); } //if new priority is greater than old, we bubble up 
		else { down(i); } //otherwise, we bubble down
	}
	
	//essentially, values are stored not based on their value but on their priority, where root = highest priority 
	
	static int getMax() {
		return H[0]; //retrieves root 
	}
	
	static void remove(int i) {
		H[i] = getMax() + 1; //update index as value right after root 
		up(i); //bubble up 
		extractMax(); 
	}
	
	static void swap(int i, int j) {
		int temp = H[i]; 
		H[i] = H[j]; 
		H[j] = temp; 
	}
}
```

### sorting with a heap ≠ heapsort

```java
void sortWithHeap (int [] keys , T[] data) {
	Heap <T> heap = new Heap <T>(keys.length);
	for (int i = 0; i < keys.length ; i ++)
		heap.insert(keys[i], data[i]);
	for (int i = keys.length - 1; i >= 0; i--)
		data [i] = heap.removeMax();
}
```
## heapsort code

```java
public class HeapSort {
	public void sort(int[] arr) {
		int n = arr.length; 
		
		for(int i=n/2-1; i>=0; i--) { //loop starts from last non-leaf node (n/2-1) and iterates backwards till the root (i>=0)
			heapify(arr, n, i); //heapify is called to rearrange the array and ensure the heap property is satisfied
		}
		
		//we start from the last non-leaf node because we only care about the parents when satisfying HOP 
		
		for(int i=n-1; i>=0; i--) { //starts from last element of array and iterates backwards
			int temp = arr[0]; //swaps root with current element 
			arr[0] = arr[i];
			arr[i] = temp; 
			
			heapify(arr, i, 0); //heapify is called on the reduced heap (reducing heap size by one) 
		}
	}
	
	/*
	 * when we extract the max element from the heap and palce it at the end of the array, we essentially 
	 * remove that element from the heap, so we call heapify to keep ensuring that the remaining elements satisfy the HOP
	 * we exclude the last value so we can find the NEXT max until heap size = 0 
	 * which indicates that all elements have been extracted and placed in their correct location 
	 */
	
	void heapify(int[] arr, int n, int i) {
		int largest = i; 
		int l = 2*i+1; //left child
		int r = 2*i+2; //right child 
		
		if(l<n && arr[l] > arr[largest]) { //if left child > root
			largest = l; //update max value
		}
		if(r<n && arr[r] > arr[largest]) { //if right child > root 
			largest = r; //update max value 
		}
		
		if(largest != i) { //if current root is NOT the largest element
			int swap = arr[i]; //we need to swap largest and root 
			arr[i] = arr[largest];
			arr[largest] = swap;
			heapify(arr, n, largest); //then recursively heapify to bubble down -- essentially sort again 
		}
	}
	
	static void print(int[] arr) {
		for(int i=0; i<arr.length; i++) {
			System.out.print(arr[i] + " ");
		}
		System.out.println();
	}
	
	public static void main(String[] args) {
		int[] arr = {10, 8, 9, 7, 6, 6, 7, 1, 4, 2, 2, 1, 5, 3};
		HeapSort hs = new HeapSort();
		hs.sort(arr);
		
		print(arr);
	}
}
```

### removeMax
- heapify can be done in O(n) time
- it’s not hard to get an upper bound (O) of the complexity of an algorithm, but it is nontrivial to get a tight one (Θ)

- a trivial best case: [7, 7, 7, 7.., 7]
- how about a sorted array, a reversely sorted array (with distinct keys)?
    - sorted array represents the worst case as the smallest value would be at the root
    - reversely sorted array would also be worst case since the reduced heap would result in the max value getting placed at the end

### is heapsort stable?

- consider an array [21, 20a, 20b] which is already in max-heap format
- when we first heapsort, 21 is removed and placed in the last index— then 20a is removed and placed in the last-1, then 20b is removed and placed in last-2
- so, after heapsort, the array will look like [20b, 20a, 21] which **does not preserve order**

> when sorting in ascending order, heapsort first picks the largest element and puts it at the end of the list— so, the element picked first stays, and the element picked second stays second to the last
> 

### summary of heapsort

- a heap is usually implemented as an array representing a complete binary tree
- the root is at index 0 and the last item at index n-1
- the parent-child relation can be easily decided by the index calculations
- a heap can be based on a binary tree
- the last occupied node and the first tree node can be found in **O(log(n))** time
- conceptually, heapsort makes n insertions into a heap, followed by n removals
- the same array can be used for the initial unordered data, for the heap array, and for the final sorted data
- heapsort takes **O(n log(n))** time and O(1) space

---

### applications

- [65, 85, 17, 88, 66, 71, 45, 38, 95, 48, 18, 68, 60, 96, 55]— how to find fifth smallest element?
    1. naive: sort the array, and return a[4] 
    2. build a `minheap` on all values, then do `removeMin` five times 
        - keep track of the 4th smallest element
            - [65, 85, 17, 88, 66]
            - [65, 85, 17, 88, 66]→[65, 85, 17, 88, 66, 71]— 71 replaces 88; there are 5 elements < 88, so 88 cannot be the answer
            - [65, 85, 17, 88, 66] →[65, 85, 17, 88, 66, 71] →[65, 85, 17, 66, 71, 45]— 45 replaces 85
            - [65, 85, 17, 88, 66] →[65, 85, 17, 88, 66, 71] →[65, 85, 17, 66, 71, 45] → …→ [17, 45, 38, 48, 18, 85]
        - now, we need removeMin
