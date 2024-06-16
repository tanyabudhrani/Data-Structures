# binary heaps

Created time: November 4, 2023 2:02 AM

### review of binary trees

| trees | basic concepts  |
| --- | --- |
| binary trees | link-based storage, traversals  |
| binary search trees | insert, search, predecessor, successor |
- a binary tree is not a linear data structure, all of its operations are essentially following a linked list

> trees are inherently recursive
> 

## balanced trees

![Screenshot 2023-11-04 at 5.08.07 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-04_at_5.08.07_PM.png)

- a binary tree is balanced if for any two leaves the difference of the depth is at most 1

- the absolute difference of heights of left and right subtrees at any node is less than 1

## balance factor

![Screenshot 2023-11-04 at 5.16.33 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-04_at_5.16.33_PM.png)

- each node has a balance factor of **{-1, 0, 1}**: height of the right subtree - height of the left subtree
- it is impossible for the BF of all non-leaf nodes to be 1 because this means that for every node in the tree, the height of the right subtree is one more than the height of the right subtree but for the n-2 level to have a BF of 1, then the n-1 level needs to have one more right sub-child which will, in turn, make the BF of the root = 2 which violates the AVL property

- example: removing 5
    
    ![Screenshot 2023-11-04 at 5.30.56 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-04_at_5.30.56_PM.png)
    
    - this violates the AVL tree property, so we need to rearrange the shape using rotations
        
        ![Screenshot 2023-11-04 at 5.36.56 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-04_at_5.36.56_PM.png)
        
        - right child takes over parent, then parent becomes the left subtree & parent to its original child (which is now its left subtree) and the left subtree of the new parent
        
- example: adding 79
    
    ![Screenshot 2023-11-04 at 5.55.41 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-04_at_5.55.41_PM.png)
    
    - this again violates the property of AVL
    
    ![IMG_7075.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/IMG_7075.png)
    

## rotations

| if there is an imbalance in the left child's right sub-tree | perform a left-right rotation |
| --- | --- |
| if there is an imbalance in the left child's left sub-tree | perform a right rotation |
| if there is an imbalance in the right child's right sub-tree | perform a left rotation |
| if there is an imbalance in the right child's left sub-tree | perform a right-left rotation |

### left rotation

![Screenshot 2023-11-05 at 10.16.41 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_10.16.41_PM.png)

### right rotation

![Screenshot 2023-11-05 at 10.16.54 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_10.16.54_PM.png)

## red-black

- every “real” node is given 0, 1, or 2 “fake” NIL children to ensure that it has two children; and every node is colored either red or black
    - every root node is black and every leaf is black
    - if a node is red, then both its children are black
    - every path from a node to a leaf contains the same number of black nodes
- a chain of 3 nodes is not possible
    
    ![Screenshot 2023-11-04 at 6.15.59 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-04_at_6.15.59_PM.png)
    
- the depth of a leaf node is between 0 and n-1
    - its depth is equal to the number of edges in the path from the root to that leaf

![Screenshot 2023-11-04 at 6.19.23 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-04_at_6.19.23_PM.png)

![Screenshot 2023-11-04 at 6.19.39 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-04_at_6.19.39_PM.png)

### comparison

- AVL trees are more balanced but they may cause more rotations during insertion and deletion

## self-balancing search trees

- the depth of an AVL tree or a red-black tree on n nodes is O(log n)— so all of search, insert, and delete can be done in **O(log n)** time

# binary heaps

- **heap ordering property**: every node has a key greater than or equal to the keys of its both children
    - a **binary heap** is a complete binary tree satisfying the heap ordering property
- the key of a node is greater than or equal to than any node in the subtree rooted at it

- to find the maximum key of a max heap, we would need **O(1)** time since the root is always the max

- to find the minimum key of a max heap, we would need **O(log (n))** time since min is one of the leaves

### two heaps on the same set of elements

![Screenshot 2023-11-05 at 3.57.00 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_3.57.00_PM.png)

## insertion into a heap

![Screenshot 2023-11-05 at 3.57.56 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_3.57.56_PM.png)

![Screenshot 2023-11-05 at 3.58.28 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_3.58.28_PM.png)

![Screenshot 2023-11-05 at 3.58.48 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_3.58.48_PM.png)

![Screenshot 2023-11-05 at 4.00.00 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_4.00.00_PM.png)

![Screenshot 2023-11-05 at 4.00.31 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_4.00.31_PM.png)

![Screenshot 2023-11-05 at 4.00.44 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_4.00.44_PM.png)

- after swapping 63 with its parent node 25, we no longer need to compare it with its sibling since, if it does swap with 36, it can be greater than OR equal to its children
- exercise
    
    ![Screenshot 2023-11-05 at 4.02.40 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_4.02.40_PM.png)
    
    ![IMG_7080.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/IMG_7080.png)
    

## deletion

![Screenshot 2023-11-05 at 4.07.10 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_4.07.10_PM.png)

![Screenshot 2023-11-05 at 4.07.20 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_4.07.20_PM.png)

![Screenshot 2023-11-05 at 4.08.50 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_4.08.50_PM.png)

![Screenshot 2023-11-05 at 4.07.51 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_4.07.51_PM.png)

![Screenshot 2023-11-05 at 4.08.11 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_4.08.11_PM.png)

## modifying a heap

| insert(x) | make a new node with data x in the tree in the next available location 

bubble x up the tree until it finds the correct place— if the key of x is larger than its parent’s key, swap them then continue |
| --- | --- |
| removeMax()  | remove the rightmost node y on the bottom level, and put it in the root

bubble down the new root’s y until it finds the correct place— if the key of y is smaller than at least one child’s key, swap y with the largest child’s key and continue |
- both insertion and deletion can be done in O(log n) time

## one item changed

![Screenshot 2023-11-05 at 4.16.54 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_4.16.54_PM.png)

![Screenshot 2023-11-05 at 4.17.10 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_4.17.10_PM.png)

![Screenshot 2023-11-05 at 4.17.27 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_4.17.27_PM.png)

- exercise
    
    ![2.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/2.png)
    
    ![3.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/3.png)
    

# heapsort

- the information we have: root and the children of each node
- a **level-wise traversal** needs **O(n)** time
- once we know the number of nodes, we can calculate the positions:
    - $2^h ≤ n < 2^h$$^+$$^1$ ($h = ⌊logn⌋$)
    - $n-2^h$ tells us which side to go → if $n-2^h <2^h$$^-$$^1$, go left
    - leaves at the bottom: $2^h, 2^h+1$…

![Screenshot 2023-11-05 at 4.25.20 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_4.25.20_PM.png)

![Screenshot 2023-11-05 at 4.25.31 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_4.25.31_PM.png)

- left and right children of node i are $2*i+1$ and $2*i+2$ respectively
    - if 2*i+1 ≥ the number of elements in the heap, then node i has no child
    - if 2*i+2 = the number of elements in the heap, then node i has only left child
- for $0<k<n$, the parent of node k is ⌊k-1/2⌋

> a heap can be stored without references
> 

```java
public class MaxHeapArray {
	void swap(int i, int j) { //swapping values 
		int temp = i; 
		i = j; 
		j = temp; 
	}
	
	private static class Node { //node class to keep track of current value and a reference object  
		int key; 
		Node obj; 
		
		public Node(int key, Node obj) { //constructor to create the Nodes of the tree
			this.key = key;
			this.obj = obj;
		}
	}
	
	private Node[] data; //using an array to store the elements, which we can access through the use of indexes
	int size; //once we know the number of nodes, we can calculate the positions of each Node
	
	public MaxHeapArray(int capacity) { //constructor to initialize the array 
	  data = new Node[capacity];
	  size = 0;
	}
}
```

### insert

```java
void insert(int key, Node x) {
		if(size == data.length) { System.out.println("Overflow"); return; }
		data[size] = new Node(key, x); //create a new Node with the value of key at the end of the array -- SHAPE FIRST
		up(size++); //bubble up!
	}
	
	void up(int val) {
		if(val == 0) { return; } //if only one Node is in the array, trivial 
		int parent = (val-1)/2; //parent of the node 
		if(data[val].key <= data[parent].key) { return; } //base case -- if new Node's value <= parent's value, we're done
		swap(val, parent); //otherwise, swap new Node with its parent recursively until base case is reached
		up(parent); 
	}

```

### removeMax

```java
Node removeMax() {
		if(size == 0) { System.out.println("Underflow"); return null; }
		Node answer = data[0].obj; //root of the tree
		data[0] = data[--size]; //replace root with the last value in the array and decrease size at the same time 
		down(0); //bubble down!
		return answer;
	}
	
	void down(int i) {
		if(size < 2*i+1) { return; } //if tree is now empty, trivial 
		int leftChild = 2*i+1, rightChild = leftChild+1, max = rightChild; //initialize leftChild and rightChild indexes 
		
		if(rightChild < size && data[leftChild].key < data[rightChild].key) { //if leftChild's key < rightChild's key, rightChild is max 
			max = rightChild;
		}
		if(data[i].key >= data[max].key) { //base case -- if key of root is greater than the right child and left child (or current max), we're done
			return; 
		}
		swap(i, max); //otherwise, swap root with max value recursively until base case is reached
		down(max);
		
	}
```

## minimum heap

![Screenshot 2023-11-05 at 4.47.30 PM.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/Screenshot_2023-11-05_at_4.47.30_PM.png)

```java
public class MinHeapArray {
	void swap(int i, int j) { //swapping values 
		int temp = i; 
		i = j; 
		j = temp; 
	}
	
	private static class Node { //node class to keep track of current value and a reference object  
		int key; 
		Node obj; 
		
		public Node(int key, Node obj) { //constructor to create the Nodes of the tree
			this.key = key;
			this.obj = obj;
		}
	}
	
	private Node[] data; //using an array to store the elements, which we can access through the use of indexes
	int size; //once we know the number of nodes, we can calculate the positions of each Node
	
	public MinHeapArray(int capacity) { //constructor to initialize the array 
	  data = new Node[capacity];
	  size = 0;
	}
	
	void insert(int key, Node x) {
		if(size == data.length) { System.out.println("Overflow"); return; }
		data[size] = new Node(key, x); //create a new Node with the value of key at the end of the array -- SHAPE FIRST
		up(size++); //bubble up!
	}
	
	void up(int val) {
		if(val == 0) { return; } //if only one Node is in the array, trivial 
		int parent = (val-1)/2; //parent of the node 
		if(data[val].key >= data[parent].key) { return; } //base case -- if new Node's value >= parent's value, we're done
		swap(val, parent); //otherwise, swap new Node with its parent recursively until base case is reached
		up(parent); 
	}
	
	Node removeMin() {
		if(size == 0) { System.out.println("Underflow"); return null; }
		Node answer = data[0].obj; //root of the tree
		data[0] = data[--size]; //replace root with the last value in the array and decrease size at the same time 
		down(0); //bubble down!
		return answer;
	}
	
	void down(int i) {
		if(size < 2*i+1) { return; } //if tree is now empty, trivial 
		int leftChild = 2*i+1, rightChild = leftChild+1, min = leftChild; //initialize leftChild and rightChild indexes 
		
		if(rightChild < size && data[leftChild].key < data[rightChild].key) { //if leftChild's key < rightChild's key, leftChild is min 
			min = leftChild;
		}
		if(data[i].key <= data[min].key) { //base case -- if key of root is less than the left child and right child (or current min), we're done
			return; 
		}
		swap(i, min); //otherwise, swap root with min value recursively until base case is reached
		down(min);
		
	}
}
```

- exercise
    - are the following heaps?
        - 10, 8, 9, 7, 6, 6, 7, 1, 4, 2, 2, 1, 5, 3
        - 10, 9, 4, 8, 6, 3, 2, 7, 7, 5, 6, 1, 2, 1
        - 1, 1, 2, 2, 3, 4, 5, 6, 6, 7, 7, 8, 9, 10
        - 65, 85, 17, 85, 66, 71, 45, 38, 95, 48, 18, 68, 60, 96, 55 — NO (after 38, there’s no place to keep 95)
        
        ![IMG_7101.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/IMG_7101.png)
        
        ![IMG_7102.png](binary%20heaps%209a72e3285f01415fab41379b2d311c4e/IMG_7102.png)
        

## merging two heaps

- [10, 5, 6, 2] + [12, 7, 9] = [12, 10, 9, 2, 5, 7, 6]
    
    ```java
    class MergingBinaryHeaps {
    	static void mergeHeaps(int a[], int b[], int n, int m) {
    		boolean visita = false, visitb = false; //visita and visitb keep track of whether or not a node has been visited 
    		int i = 0, j = 0; //indexex for a and b, respectively 
    	
    		while (i != n && j != m) { //loop until the end of both arrays 
    			int max1 = i, max2 = j;
    	
    			if (i + 1 != n && !visita && a[i + 1] > a[i]) //if we havent visited the next node yet, and if next node > current node
    				max1 = i + 1; //update max to next node 
    	
    			if (j + 1 != m && !visitb && b[j + 1] > b[j]) //if we havent visited the next node yet, and if next node > current node
    				max2 = j + 1; //update max to next node 
    	
    			if (a[max1] > b[max2]) { //if array a has the maximum element 
    				System.out.print(a[max1]+" "); //print it out (this is our ROOT) 
    	
    				if (max1 == i + 1) //if the max == the next node in array a, that means that the next node has already been visited 
    					visita = true; //we set visit to true and just move onto the next index of array a 
    				else {
    					if (visita) { //otherwise, if max1 != next node but visita is true, that means the previous node has already been visited 
    						i += 2; //we skip the previous and the current 
    						visita = false; //reset visita to false
    					}
    					else
    						i++;
    				}
    			}
    			
    			/*
    			 * when max2 = j+1 is true, it means that b[max2] is the next element to be processed, so we set visitb = true 
    			 * if max2 != j+1 but visitb is true, that means that the previous element and the current was processed together, so we skip both 
    			 * and we reset visitb to false-- otherwise, we increment next element to be processed
    			 */
    			
    			else {
    				System.out.print(b[max2]+" ");
    	
    				// Update index and vis_next2 for array "b"
    				if (max2 == j + 1)
    					visitb = true;
    				else {
    					if (visitb) {
    						j += 2;
    						visitb = false;
    					}
    					else
    						j++;
    				}
    			}
    		}
    	
    		if (i == n) { //if all elements in array a are exhausted
    			while (j != m) { //until remaining elements of b are exhausted 
    				System.out.print(b[j]+" "); //print them out 
    				j++;
    				
    				if (visitb) { 
    					visitb = false; //if b[j+1] and b[j] have been processed together, reset visitb to false and visit the next element 
    					j++;
    				}
    			}
    		}
    		
    		else {
    			while (i != n) {
    				System.out.print(a[i]+" ");
    				i++;
    				if (visita) {
    					visita = false;
    					i++;
    				}
    			}
    		}
    		return;
    	}
    	public static void main (String[] args) {
    		int a[] = { 10, 5, 6, 2 };
    		int b[] = { 12, 7, 9 };
    	
    		mergeHeaps(a, b, a.length, b.length);
    	
    	}
    }
    ```
    
- does this array represent a heap:
    - [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
    - [79, 73, 63, 67, 42, 36, 1, 2, 7, 25, 24, 13]
    - [96, 95, 85, 85, 65, 17, 66, 71, 45, 38, 48, 18, 68, 60, 55]
    
    ```java
    package binaryheaps;
    
    public class IsHeap {
    	static boolean Heap(int[] arr, int i, int n) {
    		if(i >= (n-1)/2) {
    			return true;
    		}
    		
    		if(arr[i] >= arr[2*i+1] && arr[i] >= arr[2*2+2] && Heap(arr, 2*i+1, n) && Heap(arr, 2*i+2, n)) {
    			return true;
    		}
    		
    		return false;
    	}
    	
    	public static void main(String[] args) {
    		int[] arr = {96, 95, 85, 85, 65, 17, 66, 71, 45, 38, 48, 18, 68, 60, 55};
    		
    		System.out.println(Heap(arr, 0, arr.length-1));
    	}
    }
    ```