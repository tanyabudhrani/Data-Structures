# binary search trees

> trees are reference-based and behave similarly to linked lists— although trees are more complicated, we almost always work on a list starting from the root to conduct an operation
> 

### binarySort

```java
void binarySort(Student[] a) {
	int n = a.length; 
	Student[] b = new Student[n]; 
	for(int i = 0; i<n; i++) {
		b[i] = a[i];
	}
	int girls = 0, boys = n-1; 
	for(int i=0; i<b.length; i++) {
		if(b[i].gender = 'F') {
			a[girls++] = b[i]; 
		} else {
			a[boys--] = b[i];
		}
	}
}
//make use of girls and boys as indices from the start and end of the array
//this is not inplace because we are creating a new array 
```

- to make this in-place, instead of creating a separate array to store the sorted elements, we can just perform the sorting directly on the input array
    
    ```java
    void sortGrades(Student[] a) {
        int n = a.length;
        Student[] copy = new Student[n];
        for (int i = 0; i < n; i++) {
            copy[i] = a[i];
        }
    
        int count[] = new int[4]; // Assuming grades are 'A', 'B', 'C', 'D'
        for (Student c : a) {
            count[c.grade - 'A']++;
        }
    
        int cur[] = new int[4];
        for (int i = 1; i < 4; i++) {
            cur[i] = count[i - 1] + cur[i - 1];
        }
    
        Student[] output = new Student[n];
        for (int i = n - 1; i >= 0; i--) {
            int index = cur[copy[i].grade - 'A'];
            output[index] = copy[i];
            cur[copy[i].grade - 'A']++;
        }
    
        for (int i = 0; i < n; i++) {
            a[i] = output[i];
        }
    }
    ```
    

### binarySort for 3 different keys

```java
void sortGrades(Student[] a) {
	int n = a.length; 
	Student[] copy = new Student[n]; 
	for(int i=0; i<n; i++) {
		copy[i] = a[i];
	} //takes an array of Student objects as input and creates a copy 

	int count[] = {0,0,0,0}; //counts occurrences of each grade A,B,C,D
	for(Student c: a) {
		count[c.grade - 'A']++; //current grade - A finds the difference between grades 
	} //then we increment the count array to move to the enxt element in the array

	int cur[] = {0,0,0,0};
	cur[1] = count[0]; //represents A 
	cur[2] = cur[1] + count[1]; //total counts of As and Bs and Cs 
	cur[3] = cur[2] + count[2]; //total of all grades 
	for(int i = 0; i<n; i++) {
		a[cur[copy[i].grade - 'A']++] = copy[i];
	}//iterates through copy array and assigns each Student object to its correct position in the original array 
}

```

# quicksort

- in every step, we choose a **pivot p**, and divide the array into two parts such that
    - every element in the left is no larger than p
    - and every element in the right is larger than p
- p itself can be the first, last, or middle element

## recursive quicksort

```java
package binarytrees;

public class RecursiveQuicksort {
	static void naive(int[] a, int low, int high) {
		if(low >= high) {
			return;
		}
		int pivot = a[high]; //p is the last element
		int[] b = new int[high - low + 1]; //new array with the length of original array 

		for(int i = 0; i<b.length; i++) { 
			b[i] = a[low+i]; //we copy the values of the original array to b 
		}

		int l = low, r = high;
		for(int i=0; i<b.length; i++) {
			if(b[i] <= pivot) { //if element in b is less than pivot 
				a[l++] = b[i]; //move it to the left of the original array
			} else {
				a[r--] = b[i]; //otheriwse move it to the right of the original array
			}
		}
		naive(a, low, l-2); //recursively sort the left subarray 
		naive(a, r+1, high); //recursively sort the right subarray
	}
	
	public static void main(String[] args) {
		int[] a = {14, 8, 10, 89, 32, 50, 77, 38};
		    
		naive(a, 0, a.length - 1);
		    
		for(int num : a) {
		   System.out.print(num + " ");
		}
	}
}

/*
after the partitioning loop, the variable l points to the index where the next element smaller than or equal to the pivot should be placed-- 
using l-1 would include the last element placed in the left subarray, leading to redundant comparisons-- by incrementing l before placing 
an element in the left subarray, l points to the first index where an element larger than the pivot is found. therefore, when making the recursive call, 
l-2 is used to exclude the last element placed in the left subarray 
*/
```

- it is a divide and conquer algorithm, so we consider #levels and work at each level
    - the work of each partition is proportional to the number of elements under concern, so each level uses O(n) time
    - the running time of the whole algorithm depends on #levels (iterations) which depends on whether the partitions are balanced, which depends on the pivot values chosen in each step
- so, if we always choose the last element as pivot, the worst case is a **sorted array**— this is because since quicksort always chooses the last element, when the array is already sorted, the pivot will always be the largest element— so as a result, the subarrays will be unbalanced since the other subarray is literally empty and this is inefficient because the partitioning step only reduces the problem size by one element per recursive call
 
- the worst cases: **n → n-1 → n-2 → n-3 → … → 1**

> in the worst case, quicksort takes o($n^2$) time | in the best case, it takes o(n(log(n)) time
> 
- the best cases are when:
    1. each partition is **almost even** 
    2. log(n) levels with O(n) at each level 

### comparison of merge sort and quicksort

|  | mergesort | quicksort |
| --- | --- | --- |
| divide | O(1) | Θ(n) |
| combine | Θ(n) | 0 |
| levels | Θ(logn) | log n ~ n - 1 |
| best time | Θ(n log(n)) | Θ(n log(n)) |
| average time | Θ(n log(n)) | Θ(n log(n)) |
| worst time | Θ(n log(n)) | Θ(n$^2$) |
- in quicksort, the only thing we need to do is divide

## improvements

- choice of “good” pivots:
    - **random pivot** — the worst case stays the same, but you shouldn’t always be unlucky
    - **median-of-three pivot** — choosing the median of the first, middle, and last elements of the partition is good for sorted arrays but bad cases still exist (1,x,…,x,1,y,…,y,1)
    - **dual pivots** — partition into three parts
    - using **insertion sort** when the partition is small

### hoare partitioning scheme

- two indices, each in its own while loop, start at opposite ends of the array and step toward each other, looking for items that need to be swapped
- when an index finds an item that needs to be swapped, its while loop exits
- when both while loops exit, the items are swapped — when both while loops exit, and the indices have been met or passed each other, the partition is complete

```java
public class QuickSort {
	private static void insertion(int[] a, int low, int high) { //use insertion sort to sort the array if the length is small-- ALMOST sorted
		for(int i=low+1; i<=high; i++) {
			int key = a[i];
			int j = i-1; 
			while(j >= low && key < a[j]) { 
				a[j+1] = a[j];
				j--;
			}
			a[j+1] = key;
		}
	}
	
	private static int partition(int[] a, int low, int high) {
		int pivot = a[high]; //pivot is the LAST element 
		int i = low, j = low; //i is used to iterate from low to high; j is used to keep track of the boundary between pivot 
		
		while(i <= high) {
			if(a[i] > pivot) { i++; } //if value at ith position is greater than pivot value, we move onto the next value (don't do anything)
			else { int temp = a[i]; a[i] = a[j]; a[j] = temp; i++; j++; } //else we swap the ith value with j which holds the boundary between greater than or less than pivots 
		}
		return j-1; //return the value right before the pivot 
	}
	
	public static void QS(int[] a, int low, int high) {
		while(low < high) { 
			if(high-low < 10) { //if length is less than 10, insertion sort!
				insertion(a, low, high);
				break;
			} else {
				int p = partition(a, low, high); //we partition to find the current location of the pivot 
				if(p-low < p-high) { //if left subarray is smaller than right subarray
					QS(a, low, p-1); //we sort the the left subarray 
					low = p+1; //when sorted, we update low as p+1
				} else {
					QS(a, p+1, high); //otherwise, we sort the right subarray
					high = p-1; //afterwards, we update high to p-1
				}
			}
		}
	}
	
	public static void main(String[] args) {
		int[] a = {14, 8, 10, 89, 32, 50, 77, 38};
	    
		QS(a, 0, a.length-1);
		    
		for(int num : a) {
		   System.out.print(num + " ");
		}
	}
}
```

|  | best sorting algorithm to use |
| --- | --- |
| sorted array  | bubble or insertion  |
| reversely sorted array | merge or quick sort  |
| array with identical elements  | any stable sort  |
| almost sorted array  | insertion sort  |
| random array  | merge or quick sort |

### you sort (almost) sorted arrays a lot

- sorting twice doesn’t make your array more sorted— it can take much longer actually
- most of the time, we are sorting an “almost” sorted array
- **insertion is the best**, then cocktail sort, then bidirectional bubble sort

## review: mergesort

- divide a big problem into smaller problems and solve each one separately
- both binary search and mergesort divide arrays in the same way
    - **binary search**: left half (⌊n−1/2⌋) and right half (⌊n/2⌋)
    - **mergesort**: left half (⌊n+1/2⌋) and right half (⌊n/2⌋)
- for iterative implementation: bottom-up


### terminologies

| path  | a sequence of edges, each starting with the node of the previous edge ends  |
| --- | --- |
| length of a path  | the number of edges in it  |
| root  | any node may be considered to be the root of a subtree which consists of its children, and its children’s children, and so on  |
| depth  | length of path from root to node |
| height of a node | length of longest path from that node down to a leaf |
| height of a tree | height of a tree is the height of its root 

all leaves have a height of 0 |

> #nodes in a tree of depth k is between k+1 and $2^k$$^+$$^1 -1$
> 

> depth of a tree of n nodes is between ⌊log n⌋ and n-1
> 

> a binary tree is perfect if all leaves on the same level and each non-leaf node has exactly two children
> 
> - all perfect binary trees are complete

### complete trees

- a binary tree is complete if all leaves at the bottom level are as far to the left as possible and every other level is completely filled

```java
public class BinaryTree<T> {
	private class Node<T> :
		T data; 
		public Node<T> leftChild, rightChild;
	}
	Node<T> root;
}
```

- the number of null references (absent children of the nodes) in a binary tree of n nodes is (n+1)

### implementations

- for each node, use a linked list to store references to its children— each node stores the reference to its first child and its next sibling
    - to augment a binary tree with bidirectional links, you can add either parent pointers or sibling pointers to each node

```java
// class containing left and right child
// of current node and key value
class Node {

	int key;
	Node left, right;
	public Node(int item) {
		key = item;
		left = right = null;
	}
}

class BinaryTree {
	
	// root of Binary Tree
	Node root;
	
	// constructors
	BinaryTree(int key) { 
		root = new Node(key); 
	}
	
	BinaryTree() { 
		root = null; 
	}

	public static void main(String[] args) {
		BinaryTree tree = new BinaryTree();
		
		// create root
		tree.root = new Node(1);
		/* Following is the tree after above statement
		1
		/ \
		null null 
		*/

		tree.root.left = new Node(2);
		tree.root.right = new Node(3);
		/* 2 and 3 become left and right children of 1
			1
			/ \
			2 3
		/ \ / \
	null null null null */
		tree.root.left.left = new Node(4);
		/* 4 becomes left child of 2
			1
			/ \
			2 3
			/ \ / \
		4 null null null
		/ \
		null null
		*/
	}
}
```

### traversals

```java
class Node {
	int key;
	Node left, right;

	public Node(int item) {
		key = item; //the actual data of each node
		left = right = null; //initialize left and right nodes as null
	} 
}

public class TreeTraversals {
	Node root;

	public TreeTraversals() {
		root = null;
	}

	void printInorder(Node node) {
		if (node == null) {
			return;
		}

		printInorder(node.left); //recurs all left nodes first
		System.out.print(node.key + " "); //then prints key IN BETWEEN nodes
		printInorder(node.right); //recurs all right nodes last
	}

	void printPostOrder(Node node) {
		if (node == null) {
			return;
		}

		printPostOrder(node.left); //recurs left nodes
		printPostOrder(node.right); //recurs right nodes
		System.out.print(node.key + " "); //prints key AFTER children have been visited
	}

	void printPreOrder(Node node) {
		if (node == null) {
			return;
		}

		System.out.print(node.key + " "); //prints key BEFORE children have been visited
		printPreOrder(node.left); //recurs left nodes
		printPreOrder(node.right); //recurs right nodes
	}

	public static void main(String[] args) {
		TreeTraversals tree = new TreeTraversals();
		
		tree.root = new Node(1);
		tree.root.left = new Node(2);
		tree.root.right = new Node(3);
		tree.root.left.left = new Node(4);
		tree.root.left.right = new Node(5);

		System.out.print("Inorder traversal of binary tree is "); 
		tree.printInorder(tree.root); // 4 2 5 1 3 -- left.left -> left -> left.right -> key -> right 
		
		System.out.println();

		System.out.print("PostOrder traversal of binary tree is ");
		tree.printPostOrder(tree.root); //4 5 2 3 1 -- left.left -> left.right -> left -> right -> key 
		
		System.out.println();

		System.out.print("PreOrder traversal of binary tree is ");
		tree.printPreOrder(tree.root); //1 2 4 5 3 -- key -> left -> left.left -> left.right -> right 
	}
}
```

## level-wise traversal

```java
import java.util.LinkedList;
import java.util.Queue;

public class LevelWise {
	
	static class Node {
		int data;
		Node left;
		Node right;
	
		Node(int data) {
		this.data = data;
		left = null;
		right = null;
		}
	}
	

	static void levelOrder(Node root) {
		if (root == null) { return; }
	
		Queue<Node> q = new LinkedList<>(); //we create a new node
	
		q.add(root); //pushing root node into queue
	
		q.add(null); //push a delimiter into the queue to indicate the END of a current level
	
		while (!q.isEmpty()) { //continue the loop until the queue is empty 
			Node curr = q.poll(); //remove the front node from the queue

			if (curr == null) { //if the current node is a delimiter 
				if (!q.isEmpty()) { //check if the queue is still not empty 
					q.add(null); //we push in another delimiter
					System.out.println(); //separate levels
				} 
				
			} else { //if current node is not null 
				if (curr.left != null) { q.add(curr.left); } //pushing left child of node
				if (curr.right != null) { q.add(curr.right); } //pushing right child of node
				System.out.print(curr.data + " "); //print the data of the current queue
			}
		}
	}
	
	public static void main(String[] args) {
	
		Node root = new Node(1);
		root.left = new Node(2);
		root.right = new Node(3);
		root.left.left = new Node(4);
		root.left.right = new Node(5);
		root.right.right = new Node(6);
	
		levelOrder(root);
	}
}
```

- two different trees can have the same inorder sequence

- how do we uniquely build the tree?
    - Inorder traversal: □□□X■■■■■
    Postorder traversal: □□□■■■■■X
    - Inorder traversal: □□□X■■■■■
    Preorder traversal: X□□□■■■■■
    - Postorder traversal: □□□■■■■■X
    Preorder traversal: X□□□■■■■■
    

# searching a tree

- in a binary search tree, every node is
    - larger than nodes in its left subtree, and
    - smaller than nodes in its right subtree
- example
    - 2 → 3 → 4 → 5 → 6 → 7 → 8 → 9 → 10 → 11 → 12 → 13 → 14

- the running time of search is O(d), where d is the **depth/height** of the tree, but the worst case is d = n-1, when the tree is skewed
- binary trees with depth O(log (n)) are considered balanced— there is balance between the number of nodes in the left subtree and the number of nodes in the right subtree of each node

```java
BinarySearchTree < Student > tree = new BinarySearchTree < Student >();
int [] ids = {214 ,562 ,83 ,115 ,97 ,722 ,398 ,798 ,408 ,199 ,37};

String [] names = { " Chan ␣ Eason " , " Cheung ␣ Jacky " , " Winnie " ,
" Ho ␣ Denise " , " Mickey " , " Leung ␣ Gigi " , " Joey ␣ Yung " ,
" Teddy " , " Peppa " , " Tse ␣ Kay " , " Andy ␣ Lau " };

for (int i = 0; i < 11; i ++) {
	tree.insert (ids[i], new Student(ids[i], names[i]));
```

- any order in which no node is earlier than its parent

## successor in BST

- the **successor** of a node is the node whose key immediately follow it

- the **predecessor** of a node is the node whose key immediately precedes it

- if x has a right child, the minimum node of its right subtree— otherwise, on the path from the root to x, the first node on which we turn left

## finding successor/predecessor

- **the successor of node x:**
    - if x has a right child, then the successor of x is the **minimum** in the **right** subtree— follow x’s **right pointers**, then follow **left pointers** until there are no more
    - if x does not have a right child, then find the lowest ancestor of x whose left child is also an ancestor of x
        - when searching for x, record the last node whose left subtree is used

- **the predecessor of node x:**
    - if x has a left child, then the successor of x is the **maximum** in the **left** subtree— follow x’s **left pointers**, then follow **right pointers** until there are no more
    - if x does not have a left child, then find the lowest ancestor of x whose right child is also an ancestor of x
        - when searching for x, record the last node whose right subtree is used
        

## deleting a node

- to delete a leaf, set the link from its parent to it to be null— simply removing a non-leaf breaks the tree
- to delete a node x with one child y, set the link from x’s parent to x to y

| x has no children | set the child field in its parent to null  |
| --- | --- |
| x has one child | set the child field in its parent to point to its child  |
| x has two children | replace x with its successor y (minimum of x’s right subtree)  |
- easy if the right child of x has no left child (115 and 562)— otherwise we need to find some node to replace y (083 and 214)

```java
class BinarySearchTree {
	class Node {
		int key;
		Node left, right;

		Node(int item) {
			key = item;
			left = right = null;
		}
	}
	
	Node root;

	void inorder(Node root) {
		if (root != null) {
			
			inorder(root.left); //recur left nodes
			System.out.print(root.key + " "); //print key node IN BETWEEN children
			inorder(root.right); //recur right nodes 
		}
	}

	Node insert(Node node, int key) {
		
		if (node == null)
			return new Node(key); //if tree is empty, insert new node 

		if (key < node.key) //if key is less than current key 
			node.left = insert(node.left, key); //insert new node in left subtree
		else if (key > node.key)
			node.right = insert(node.right, key); //otherwise, insert node in right subtree

		return node;
	}

	Node deleteNode(Node root, int key) {
		
		if (root == null) //base case-- if key was not found in tree
			return root;

		if (root.key > key) { //key to be deleted is smaller than current key
			root.left = deleteNode(root.left, key); //recurs deleteNode on left child 
			return root;
		} else if (root.key < key) { //else if key to be deleted is greater than current key
			root.right = deleteNode(root.right, key); //recurs deleteNode on right child
			return root;
		} 
		
		//if one of the children is empty!!
		if (root.left == null) { //if node to be deleted has no left child
			Node temp = root.right; //replaces the node with its right child
			return temp;
		} else if (root.right == null) { //if node to be deleted has no right child
			Node temp = root.left; //replaces the node with its left child
			return temp;
		}

		//if both children exist!!
		else {
			Node succParent = root; //keeps track of parent of the successor node

			Node succ = root.right; //initializes variable to the right child of current node (successor is always in right subtree)
			while (succ.left != null) { //iterate down the left child of current node until it finds leftmost leaf node in right subtree
				succParent = succ; 
				succ = succ.left;
			}

			if (succParent != root) //if successor is not an immediate right child of the node to be deleted
				succParent.left = succ.right; //left pointer will point to successor's right child
			else
				succParent.right = succ.right; //else, right pointer of parent will point to successors right child

			root.key = succ.key; //key of node to be deleted is replaced with key of successor node 

			return root;
		}
	}

	// Driver Code
	public static void main(String[] args) {
		BinarySearchTree tree = new BinarySearchTree();

		/* Let us create following BST
				50
			/	 \
			30	 70
			/ \ / \
		20 40 60 80 */
		
		tree.root = tree.insert(tree.root, 50);
		tree.insert(tree.root, 30);
		tree.insert(tree.root, 20);
		tree.insert(tree.root, 40);
		tree.insert(tree.root, 70);
		tree.insert(tree.root, 60);

		System.out.print("Original BST: ");
		tree.inorder(tree.root);

		System.out.println("\n\nDelete a Leaf Node: 20");
		tree.root = tree.deleteNode(tree.root, 20);
		System.out.print("Modified BST tree after deleting Leaf Node:\n");
		tree.inorder(tree.root);

		System.out.println("\n\nDelete Node with single child: 70");
		tree.root = tree.deleteNode(tree.root, 70);
		System.out.print("Modified BST tree after deleting single child Node:\n");
		tree.inorder(tree.root);

		System.out.println("\n\nDelete Node with both child: 50");
		tree.root = tree.deleteNode(tree.root, 50);
		System.out.print("Modified BST tree after deleting both child Node:\n");
		tree.inorder(tree.root);
	}
}
```
