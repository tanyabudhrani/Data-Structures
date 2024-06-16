# hashing

Created time: November 17, 2023 9:50 PM

- removing duplicated elements from a sorted and unsorted array
    
    ```sql
    public class RemovingDuplicateElements {
    	static class Node {
    		int data; 
    		Node next; 
    		
    		public Node(int data, Node next) {
    			this.data = data; 
    			this.next = next; 
    		}
    	}
    	
    	static Node head;
    	
    	static Node push(Node h, int newdata) {
    		Node newnode = new Node(newdata, h);
    		h = newnode;
    		return h;
    	}
    	
    	static void printList(Node node) {
    		while(node != null) {
    			System.out.print(" " + node.data);
    			node = node.next;
    		}
    	}
    	
    	static Node removeDuplicatesSorted(Node h) {
    		
    		if(h == null) { return null; } //if list is empty, do nothing
    		
    		if(h.next != null) { //traverse the list until the last node
    			if(h.data == h.next.data) { //compare head node with next node which is deleted
    				h.next = h.next.next;  
    				removeDuplicatesSorted(h);
    			} else { removeDuplicatesSorted(h.next); } //only advance if no deletion 
    		}
    		
    		return h;
    	}
    	
    	void removeDuplicatesUnsorted() {
    		Node ptr1 = head, ptr2 = null;
    		//ptr1 is used to iterate through each node in the list 
    		//ptr2 is used to compare ptr1 with the rest of the list 
    		
    		while(ptr1 != null && ptr1.next != null) { //iterate through each node in the list
    			ptr2 = ptr1; //pick elements one by one 
    			
    			while(ptr2.next != null) { //compare current node with rest of the list
    				if(ptr1.data == ptr2.next.data) { ptr2.next = ptr2.next.next; } //if duplicate is found, duplicate node is skipped
    				else { ptr2 = ptr2.next; } //otherwise, we can move on to the next node
    			}
    			ptr1 = ptr1.next; //iteration 
    		}
    	}
    	
    	public static void main(String[] args) {
    	    Node h = null;
    
    	    h = push(h, 20);
    	    h = push(h, 13);
    	    h = push(h, 13);
    	    h = push(h, 11);
    	    h = push(h, 11);
    	    h = push(h, 11);
    
    	    System.out.println("Original List:");
    	    printList(h);
    
    	    System.out.println();
    
    	    System.out.println("After removing duplicates (sorted):");
    	    head = removeDuplicatesSorted(h);
    	    printList(head);
    
    	    System.out.println();
    
    	    RemovingDuplicateElements obj = new RemovingDuplicateElements();
    	    obj.head = push(obj.head, 20);
    	    obj.head = push(obj.head, 13);
    	    obj.head = push(obj.head, 13);
    	    obj.head = push(obj.head, 11);
    	    obj.head = push(obj.head, 11);
    	    obj.head = push(obj.head, 11);
    
    	    System.out.println("Original List:");
    	    obj.printList(obj.head);
    
    	    System.out.println();
    
    	    System.out.println("After removing duplicates (unsorted):");
    	    obj.removeDuplicatesUnsorted();
    	    obj.printList(obj.head);
    	}
    
    }
    ```
    

### detecting duplicate elements

```java
boolean duplicateChar (String s) {
	boolean [] b = new boolean [256]; //create new boolean array for ascii characters
	for(int i = 0; i < s.length(); i ++) { //loop through elements of the string
		int c = s.charAt(i); //c is the character at a given index
		if (b[c]) { return true; } //if b[c] is true, that means the character has been encountered before so we continue to return true
		else {b[c] = true; } //if this is the first time encountering it, we just set b[c] to true 
	}
	return false; //otherwise return false
}

//need 65536 (2^16) for unicode characters
```

- you can improve this by adding `if(s.length() > 26) { return true; }`

### hypothetical — store computing subjects

- a naive idea is to use an array of size n
- but the department is offering less than 100 courses, so most of the space will be wasted — 100/10,000 = 1%
- how about a sorted array with no empty slot? — this could work but it would be hard to insert new values

### example

- you want to monitor the visitors to a website you’re running
    1. when a visitor is detected, check whether its a returned user
    2. add a record (if new); update the existing record (otherwise) 

![Screenshot 2023-11-18 at 2.19.40 PM.png](hashing%206d0615c66787459b98df831c3445889c/Screenshot_2023-11-18_at_2.19.40_PM.png)

- each user has one IP address, and each IP address is used by one user — **neither is correct**
    - IP addresses may be shared in public wi-fi networks
    - dynamic IP addresses are when a set of IP addresses are chosen to be reused
- every number between 0x00000000 and 0xFFFFFFFF is a valid IP address — **not quite correct**
    - some specific values are reserved (e.g. 0.0.0.0 is “unspecified”, 127.0.0.1 is the loopback address)
- user statistics of a website
    - for simplicity, we assume each IP address corresponds to a user
    - there are around 232 IP addresses in total but you can only expect a small fraction of them, say, 1 million, to visit your website
    - obviously, using a big array (of size 232) is completely out of the question (even worse for IPv6, which provides 2128 addresses)
    - what’s the problem with a (sorted) array?
        - space inefficiency, memory allocation, sparse array, dynamic scaling (as we get more users, we would have to update the entire array)
    

## the problem

- **input**: n elements with keys from the range of 1..N with N >> n
- **task**: to store them in a structure of size M, where N >> M, such that insertion, deletion, and search can be done in O(1) **average** time
    
    > N >> n indicates that N is significantly larger than n
    > 

| application | N | n |
| --- | --- | --- |
| website visitors | $2^3$$^2$ | $2^2$$^0$ |
| COMP subjects | 10000 | 100 |
| colors | >$27^2$$^0$ | ? |
| DNS | inf | ? |
- we only discuss integers but the solution applies to any data since anything in a computer is a bit-string
- the only structure that allows O(1) time access to every element is an **array**
    - **random access of arrays**: the ith entry of array A can be accessed in O(1) time, by calculating the address of A[i], which is i steps away from the starting address of A (e.g. offset + size of bit)

![Screenshot 2023-11-18 at 2.34.31 PM.png](hashing%206d0615c66787459b98df831c3445889c/Screenshot_2023-11-18_at_2.34.31_PM.png)

![Screenshot 2023-11-18 at 2.34.42 PM.png](hashing%206d0615c66787459b98df831c3445889c/Screenshot_2023-11-18_at_2.34.42_PM.png)

![Screenshot 2023-11-18 at 2.35.30 PM.png](hashing%206d0615c66787459b98df831c3445889c/Screenshot_2023-11-18_at_2.35.30_PM.png)

- the notation h[1…N] → [0…M-1] represents a **hash function h** that maps values from the range [1…N] to the range [0…M-1]
    - **[1…N]** represents the the domain of the function which consists of integers from 1 to N
    - **[0…M-1]** represents the set of possible values the function can produce
- the goal of a hash function is to distribute the inputs (n numbers) across the available buckets (M) in a way that minimizes collision (different inputs not hashing to the same value) — **h(k1) ≠ h(k2) when k1 ≠ k2**

# hashtable

- a hash table is based on an array
- the range of key values is usually **greater** than the size of the array
- a key value is hashed to an array index by a **hash function** (e.g. `object.hashcode % buckets.length`)
- hash table sizes should generally be **prime numbers**

![Screenshot 2023-11-18 at 2.37.16 PM.png](hashing%206d0615c66787459b98df831c3445889c/Screenshot_2023-11-18_at_2.37.16_PM.png)

- **M vs. n**
    
    
    | M < n | if the number of buckets is less than the number of elements, it may result in collision  |
    | --- | --- |
    | M = n | if the number of buckets is equal to the number of elements, every element can be placed in a separate bucket  |
    | M > n  | if the number of buckets is greater than the number of elements, it lessens the chance of collision but leads to the underutilization of memory  |
- the hash function *h* is good if it does not assign too many elements to one slot

![Screenshot 2023-11-19 at 10.21.56 PM.png](hashing%206d0615c66787459b98df831c3445889c/Screenshot_2023-11-19_at_10.21.56_PM.png)

### uniform hashing assumption

- for a well-designed hash-function, each key is equally likely to hash to any integer between 0 and M-1

## collision

- $k_1 ≠ k_2$ but $h(k_1) = h(k_2)$
    
    ![Screenshot 2023-11-19 at 10.22.30 PM.png](hashing%206d0615c66787459b98df831c3445889c/Screenshot_2023-11-19_at_10.22.30_PM.png)
    
- N >> n and N >> M, but the relation between n and M have not been specified
    - the total number of keys (N) is significantly larger than the number of actual elements (n) and the number of buckets (M)
- the **load factor** of a hash table measures how full the table is: **ʎ =** **n/M**

## separate chaining

- technique used in hash tables to handle situations where two or more keys hash to the same index (e.g. 57, 31, and 44) — in separate chaining, each bucket in the hash table contains a **linked list** that stores all the keys to that particular index

![Screenshot 2023-11-18 at 2.42.42 PM.png](hashing%206d0615c66787459b98df831c3445889c/Screenshot_2023-11-18_at_2.42.42_PM.png)

- with separate chaining, load factor is the **average length of each linked list**
    - to search for an element, we would need to traverse the entire list at the generated index
- experiments and average-case analyses suggest that we should maintain a load balance of less than 0.9
    - by default, java uses a load balance of **less than 0.75**
- a major disadvantage however, is that it wastes a lot of space — you can change the implementation such that an auxiliary data (dynamic array or another hash table) structure is only required when there are collisions, but it becomes more complicated

### example

- suppose our table has a length of 6, and we want to insert the following {40, 310, 74, 9, 2}
    
    
    | 40%6 = 1 — currently, there are no elements at index 1 so we create a new linked list with a single element 40  |
    | --- |
    | 310%6 = 3 — currently, there are no elements at index 3 so we create a new linked list with a single element 310 |
    | 74%6 = 4 — currently, there are no elements at index 4 so we create a new linked list with a single element 74 |
    | 9%6 = 5 — currently, there are no elements at index 5 so we create a new linked list with a single element 9 |
    | 2%6 = 3 — inserting 2 at index 3 creates a collision since we already have 310 inserted — in this case, separate chaining inserts the new key at the beginning of the created list |

![Screenshot 2023-11-19 at 10.40.16 PM.png](hashing%206d0615c66787459b98df831c3445889c/Screenshot_2023-11-19_at_10.40.16_PM.png)

```java
import java.util.ArrayList;

public class SeprateChaining {
	private final int bucket; //number of buckets
	private final ArrayList<Integer>[] table; //creating a hashtable using dynamic array 

	public SeprateChaining(int bucket) {
		this.bucket = bucket;
		this.table = new ArrayList[bucket]; //initialize a new arrayList with the size of the number of buckets (M) 
		
		for (int i = 0; i < bucket; i++) { //for each bucket index
			table[i] = new ArrayList<>(); //create an empty arrayList to store the values hashed to each index
		}
	}

	public int hashFunction(int key) {
		return (key % bucket); //hashes values!
	}

	public void insertItem(int key) {
		int index = hashFunction(key); //gets the hashcode of the key and assigns it to index
		table[index].add(key); //inserts the key into the hashtable at that specified index
	}

	public void deleteItem(int key) {
		int index = hashFunction(key); //gets the hashcode of the key and assigns it to index

		if (!table[index].contains(key)) { //checks if the key is present at that specific index 
			return;
		}

		table[index].remove(Integer.valueOf(key)); //deletes the key 
	}

	// function to display hash table
	public void displayHash() {
		for (int i = 0; i < bucket; i++) {
			System.out.print(i);
			for (int x : table[i]) {
				System.out.print(" --> " + x);
			}
			System.out.println();
		}
	}

	public static void main(String[] args) {
		int[] a = {15, 11, 27, 8, 12};
		SeprateChaining h = new SeprateChaining(7); //7 buckets

		for (int x : a) {
			h.insertItem(x);
		}
		
		h.displayHash();
		System.out.println();
		
		h.deleteItem(12);
		h.displayHash();
	}
}
```

## open addressing

- in open addressing, all the values are stored in the table itself, so, at any given point, size of the table must always be larger than the number of keys

![Screenshot 2023-11-19 at 10.43.38 PM.png](hashing%206d0615c66787459b98df831c3445889c/Screenshot_2023-11-19_at_10.43.38_PM.png)

![Screenshot 2023-11-19 at 10.43.55 PM.png](hashing%206d0615c66787459b98df831c3445889c/Screenshot_2023-11-19_at_10.43.55_PM.png)

![Screenshot 2023-11-19 at 10.44.16 PM.png](hashing%206d0615c66787459b98df831c3445889c/Screenshot_2023-11-19_at_10.44.16_PM.png)

![Screenshot 2023-11-19 at 10.44.21 PM.png](hashing%206d0615c66787459b98df831c3445889c/Screenshot_2023-11-19_at_10.44.21_PM.png)

- all data is stored in the array, so load balance is less than or equal to 1 — but it’s better to **keep it below 0.5**, otherwise large clusters form and the performance degrades quickly

## linear probing

- suppose i = h(k)
    - **insert**: put a table index i if available, **otherwise, try i+1, i+2,…**
    - **search**: search table index i; if occupied but no match, try i+1, i+2,… until an unoccupied slot

![Screenshot 2023-11-19 at 10.45.03 PM.png](hashing%206d0615c66787459b98df831c3445889c/Screenshot_2023-11-19_at_10.45.03_PM.png)

> let **hash(x)** be the slot index computed using a hash function and **S** be the table size
if slot hash(x) % S is full, then we try (hash(x) + 1) % S
if (hash(x) + 1) % S is also full, then we try (hash(x) + 2) % S
if (hash(x) + 2) % S is also full, then we try (hash(x) + 3) % S
> 

## deletion

![Screenshot 2023-11-18 at 2.50.19 PM.png](hashing%206d0615c66787459b98df831c3445889c/Screenshot_2023-11-18_at_2.50.19_PM.png)

![Screenshot 2023-11-18 at 2.50.25 PM.png](hashing%206d0615c66787459b98df831c3445889c/Screenshot_2023-11-18_at_2.50.25_PM.png)

![Screenshot 2023-11-18 at 2.50.32 PM.png](hashing%206d0615c66787459b98df831c3445889c/Screenshot_2023-11-18_at_2.50.32_PM.png)

![Screenshot 2023-11-18 at 2.50.38 PM.png](hashing%206d0615c66787459b98df831c3445889c/Screenshot_2023-11-18_at_2.50.38_PM.png)

- deleting 4146, we replace 73 with 4435

> **delete**: establish a tombstone to mark this position was occupied 
**insert**: a slot with a tombstone is available
**search**: stop only at an unoccupied slot without a tombstone
> 

```java
class HashNode { //creating a hashNode class
	int key;
	int value;

	public HashNode(int key, int value) {
		this.key = key;
		this.value = value;
	}
}

class HashMap {
	int capacity;
	int size;
	HashNode[] arr; //array to store the hashNodes
	HashNode dummy;

	public HashMap() {
		this.capacity = 20; //default capacity 
		this.size = 0;
		this.arr = new HashNode[this.capacity]; //initialize array as a new array of hash nodes 
		this.dummy = new HashNode(-1, -1); //initialize dummy node as a new hashnode with -1 as key and -1 as value -- null 
	}

	public int hashCode(int key) {
		return key % this.capacity; //returns the hash code for a key
	}

	public void insertNode(int key, int value) {
		HashNode temp = new HashNode(key, value); //new hashnode is created with given key and value
		int hashIndex = hashCode(key); //apply hash function to find index for given key
		
		//LINEAR PROBING
		//this.arr[hashIndex] != null and this.arr[hashIndex].key != -1 checks if there is already a node at that particular index
		//this.arr[hashIndex].key != key ensures that, if there is a node at that particular index, that it is not the same as the key 
		while (this.arr[hashIndex] != null && this.arr[hashIndex].key != key && this.arr[hashIndex].key != -1) {
			hashIndex++; //we keep iterating though the list 
			hashIndex %= this.capacity; //updating the hash index by the modulo of the capacity
		}
		
		/*
		 * the while loop continues probing until one of the following conditions is met:
		 * the slot is null, meaning that the index is empty 
		 * the key being inserted is already inserted 
		 * the key of the node at the current slot is -1 -- meaning that it has been deleted
		 */

		if (this.arr[hashIndex] == null || this.arr[hashIndex].key == -1) { //if there is an open slot
			this.size++; //a new node is being inserted so we increment size by 1
		}
		this.arr[hashIndex] = temp; //then finally insert the new node at the index 
	}

	// function to delete a key value pair
	public int deleteNode(int key) {
		int hashIndex = hashCode(key);
		
		while (this.arr[hashIndex] != null) { //while there is no empty slots
			if (this.arr[hashIndex].key == key) { //if the key is found at its index
				HashNode temp = this.arr[hashIndex]; //assign it to temp node
				this.arr[hashIndex] = this.dummy; //insert a dummy node -- -1 indicates that node has been deleted
				this.size--; //reduce size
				return temp.value; //return value of deleted node
			}
			hashIndex++; //if not found, keep searching
			hashIndex %= this.capacity; //search next available slot where a collision occurs 
		}
		//if not found return -1
		return -1;
	}

	public int get(int key) {
		int hashIndex = hashCode(key);
		int counter = 0;
	
		while (this.arr[hashIndex] != null) { //search through table
			if (counter > this.capacity) { return -1; } //fail
			if (this.arr[hashIndex].key == key) { return this.arr[hashIndex].value; } //if node is found, return value
			hashIndex++; //otherwise, keep searching
			hashIndex %= this.capacity; //search next available slot where a collision occurs 
			counter++; //increment counter 
			
			//counter is used to prevent searching the entire table infinitely -- it will exit when it reaches the capacity
		}
		// if not found return 0
		return 0;
	}

	public void display() {
		for (int i = 0; i < this.capacity; i++) {
			if (this.arr[i] != null && this.arr[i].key != -1) {
				System.out.println("key = " + this.arr[i].key + " value = " + this.arr[i].value);
			}
		}
	}
}

public class OpenAddressing {
	public static void main(String[] args) {
		HashMap h = new HashMap();
		h.insertNode(1, 1);
		h.insertNode(2, 2);
		h.insertNode(2, 3);
		h.display();
		System.out.println(h.deleteNode(2));
		System.out.println(h.get(2));
	}
}
```

## quadratic probing

- $h(k) → (h(k) + 1^2)$%$M → (h(k) + 2^2)$%$M → (h(k) + 3^2 )$%M →
    - avoids the clustering patterns that occur with linear probing, but creates its own kind of clustering, **secondary clustering**
    
    ```java
    public int get(int key) {
    	int index = hashCode(key); 
    	int counter = 0; 
    
    	while(counter < this.capacity) {
    		if(arr[index].key == key) { return arr[index].value; } 
    		index = (index + counter * counter) % this.capacity;
    		counter++;
    	} 
    	return 0;
    	}
    ```
    

- **double hashing**: $h(k) → (h(k) + h′(k))$%$M → (h(k) + 2h′(k))$$M → (h(k) +3h′(k))$%M →, where h’ is a secondary hash function
    - a common choice is $h’(k) = q - (k$%$q)$, for some prime number q < M
    
    ```java
    public int get(int key) {
    	int index = hashCode(key); 
    	int counter = 0; 
    
    	while(counter < this.capacity) {
    		if(arr[index].key == key) { return arr[index].value; } 
    		index = (index + (counter * secondHashCode(key))) % this.capacity;
    		counter++;
    	} 
    	return 0;
    	}
    ```
    

### clustering

- **primary**: many consecutive elements form groups, and it starts taking time to find a free spot to insert elements (linear)
- **secondary**: two records have the same collision chain only if their initial position is the same (quadratic)

### example — quadratic probing

- suppose we have a hash table of size 7 and we want to insert the values 22, 30, and 50
    
    
    | 22%7 = 1 — since the cell at index 1 is empty, we can insert 22 |
    | --- |
    | 30%7 = 2 — since the cell at index 2 is empty, we can insert 30  |
    | 50%7 = 1 — cell at index 1 is already occupied, so we search for slot $1+1^2$ which is 2, but 2 is already occupied, so we search for slot $1+2^2$ which is 5, which is not occupied, so we can insert 50  |

### example — double hashing

- suppose we have a hash table of size 7 and we want to insert the values 27, 43, 692, 72
    - assume that **h1(k) = k mod 7** and **h2(k) = 1 + (k mod 5)**
    
    | 27%7 = 6 — since the cell at index 6 is empty, we can insert 27 |
    | --- |
    | 43%7 = 1 — since the cell at index 1 is empty, we can insert 43  |
    | 692%7 = 6 — cell at index 6 is already occupied
    
    h’ = [h1(692) + i*[h2(692)]%7 
    h’ = [6 + 1 * (1 + 692%5)]%7 = 9%7 = 2  |
    | 72%7 = 2 — since 2 is now occupied, we need to do double hashing again 
    
    h’ = [2 + 1 * (1 + 72%5)]%7 = 5%7 = 5 |
    

### exercise

- we use an 11-entry hash table, with h(i) = (3i+5)%11 to hash the keys— write down the results after inserting 12, 44, 13, 88, 23, 94, 11, 39, 20, 16, 5
    
    
    | 0 | 11 → 88 → 44 |
    | --- | --- |
    | 1 | 23 → 12 |
    | 2 | 13 |
    | 3 |  |
    | 4 |  |
    | 5 | 5 → 16 |
    | 6 | 39 → 94 |
    | 7 |  |
    | 8 |  |
    | 9 | 20 |
    | 10 |  |
    1. assuming collisions are handled by separate chaining
        
        
        | 0 | 13 |
        | --- | --- |
        | 1 | 39 → 94 |
        | 2 |  |
        | 3 |  |
        | 4 |  |
        | 5 | 11 → 88 → 44 |
        | 6 |  |
        | 7 |  |
        | 8 | 23 → 12 |
        | 9 | 5 → 16 |
        | 10 | 20  |
    2. assuming collisions are handled by linear probing
        
        
        | 0 | 13 |
        | --- | --- |
        | 1 | 94 |
        | 2 | 39 |
        | 3 | 16 |
        | 4 | 5 |
        | 5 | 44 |
        | 6 | 88 |
        | 7 | 11 |
        | 8 | 12 |
        | 9 | 13 |
        | 10 | 20 |
    3. assuming collisions are handled by quadratic probing
        
        
        |  |  |
        | --- | --- |
        | 0 | 13 |
        | 1 | 94 |
        | 2 | 11 — (5+3$^2$)%7 |
        | 3 | 5 |
        | 4 | 44 |
        | 5 | 88 |
        | 6 | 16 |
        | 7 | 12 |
        | 8 | 23 |
        | 9 | 39 |
        | 10 | 20 |
    4. assuming collisions are handled by double hashing using the secondary hash function $h′(k) = 7 − (k$%$7)$?
        
        
        | 0 | 13 |
        | --- | --- |
        | 1 | 94 |
        | 2 | 39 |
        | 3 | 23 |
        | 4 | 5 |
        | 5 | 44 |
        | 6 | 10 |
        | 7 | 88 |
        | 8 | 12 |
        | 9 | 11 |
        | 10 | 20 |
        

|  | hash table average | hash table worst | array unsorted | array sorted |
| --- | --- | --- | --- | --- |
| insert | O(1) | O(n) | O(1) | O(n) |
| delete | O(1) | O(n) | O(n) | O(n) |
| search | O(1) | O(n)  | O(n)  | O(log n)  |
- hash tables with separate chaining and hash tables with open addressing have the same average and worst-case running time, but there are subtle difference

- separate chaining is easier to implement, and less sensitive to load factor

- open addressing is more efficient, but degrades quickly with load factoring → 1

### summary

- the hashing of a key to an already-occupied array cell is called a **collision**
- two major ways to handle collisions: **separate chaining an open addressing**
    - in separate chaining, each array element consists of a linked list
    - in open addressing, an item doesn’t need to be put at index of its hash value
- the load factor is the ratio of data items to array size (n/M)
    - **good practices**: load balance < 0.75 (separate chaining) and load balance < 0.5 (open addressing)
- in linear probing,
    - a **cluster** is a contiguous sequence of occupied slots
    - sizes of clusters increase with load factor
    - large clusters need more steps for each operation
- alternative strategies of open addressing: quadratic probing and double hashing

### compromises for a hashmap

- operations min, max, predecessor, and successor take O(n) time
- it cannot be sorted internally
- performance may degrade when table is too full or too sparse

## applications

### first element occurring k times

- [1,2,3,2,1,4,5,8,6,7,4,2] → 1(k = 2), 2(k = 3)
    - use a hash-table to store the counts
    
    ```java
    /* 
    traverse the array of elements from left to right while traversing increment their count in the hash table
    again traverse the array from left to right and  check which element has a count equal to k
    print that element and stop if no element has a count equal to k, print -1
    */
    
    import java.util.HashMap;
     
    class KTimes {
     
    // function to find the first element
    // occurring k number of times
        static int firstElement(int arr[], int n, int k) {
     
            HashMap<Integer, Integer> count_map = new HashMap<>(); //store the frequency of each element 
    
            for (int i = 0; i < n; i++) {
                int a = 0; 
                if(count_map.get(arr[i])!=null) { //checks if current element is present in the hashmap
                    a = count_map.get(arr[i]); //if so, retrieve its count 
                }
                count_map.put(arr[i], a+1); //increment and store the new count 
            }
     
            for (int i = 0; i < n; i++) { // if count of element == k, then it is the required first element 
                if (count_map.get(arr[i]) == k) {
                    return arr[i];
                }
            }
            // no element occurs k times
            return -1;
        }
    
    	public static void main(String[] args) {
            int arr[] = {1, 7, 4, 3, 4, 8, 7};
            int n = arr.length;
            int k = 2;
            System.out.println(firstElement(arr, n, k));
        }
    }
    ```
    

### duplicate elements within k distance

- [1,2,3,4,1,2,3,4] → true(k = 4), false(k = 3)
    - use a hash-table to store the index of the previous occurrence of each element
    
    ```java
    /*
    create an empty hashtable
    traverse all elements from left to right and let the current element be ‘arr[i]’ 
    if the current element ‘arr[i]’ is present in a hashtable, then return true 
    else add arr[i] to hash and remove arr[i-k] from hash if i is greater than or equal to k
    */
    
    import java.util.*;
     
    class DuplicaateElements {
        static boolean checkDuplicatesWithinK(int arr[], int k) {
            // creates an empty hashset
            HashSet<Integer> set = new HashSet<>();
     
            // traverse the input array
            for (int i=0; i<arr.length; i++) {
            	
                if (set.contains(arr[i])) { //if the element is already in the set, that means that we have found a duplicate within k distance 
                   return true;
                }
    
                set.add(arr[i]); //otherwise we add the element to the hashset
                if (i >= k) { //of index i >= k, that means that it is outside the range of the distance we want, 
                  set.remove(arr[i-k]); //so we can remove the element at i-k 
                }
            }
            return false;
        }
    
    	public static void main (String[] args) {
            int arr[] = {10, 5, 3, 4, 3, 5, 6};
            if (checkDuplicatesWithinK(arr, 3))
               System.out.println("Yes");
            else
               System.out.println("No");
        }
    }
    ```
    

### max distance between two of the same element

- [3,2,1,2,1,4,5,8,6,7,4,2] → 10
    - use a hash-table to store the index of the first occurrence of each element
    
    ```java
    /*
    the idea is to traverse the input array and store the index of the first occurrence in a hash map
    for every other occurrence, find the difference between the index and the first index stored in the hash map
    if the difference is more than the result so far, then update the result
    */
    
    import java.util.HashMap;
      
    class MaxDistance { 
     
        static int maxDistance(int[] arr, int n) { 
            HashMap<Integer, Integer> map = new HashMap<>(); 
            int max_dist = 0; 
      
            for (int i = 0; i < n; i++) { //traverse the array 
                // if the element is not present in the hash map, this is its first occurrence,
                if (!map.containsKey(arr[i])) {
                    map.put(arr[i], i); // we add the element to the hash map with its first occurrence index as its value
            	} else { //if the element is present, that means it is not the first occurrence
                    max_dist = Math.max(max_dist, i - map.get(arr[i])); //we find the distance by subtracting its first occurrence index from its current index
            	} //then we check to see if distance is greater than max, if so we update current max distance 
            }
      
            return max_dist; 
    	}
    	public static void main(String args[]) { 
        int[] arr = {3, 2, 1, 2, 1, 4, 5, 8, 6, 7, 4, 2}; 
        int n = arr.length; 
        System.out.println(maxDistance(arr, n)); 
    	} 
    }
    ```
    

### group elements, ordered by first occurrence

- [4,6,9,2,3,4,9,6,10,4] → [4,4,4,6,6,9,9,2,3,10]
- [5,3,5,1,3,3] → [5,5,3,3,3,1]

- you can sort the array, get the counts for each element, then build an array of counts (e.g. [(1, 1), (3, 3), (5, 2)]) traverse the input array and do binary search in the array of counts and change to 0 after its done, in O(n log n)

- or, you can use a hash-table to store the count of each element

```java
import java.util.HashMap;

class OrderedFirstOccurrence {
    static void orderedGroup(int arr[]) {
        HashMap<Integer, Integer> hM = new HashMap<Integer, Integer>();

        for (int i=0; i<arr.length; i++) {
           Integer prevCount = hM.get(arr[i]); //retrieves current count of the element 
           if (prevCount == null) //if count is null, this is the first occcurrence of element arr[i]
                prevCount = 0; //so we set its count to 0 
            
           hM.put(arr[i], prevCount + 1); //otherwise, we increment the count of the element arr[i] 
        }
        
        for (int i=0; i<arr.length; i++) { 
            Integer count =  hM.get(arr[i]); //retrieve the current count of the element   
            if (count != null) { //if this is not its first occcurrence,
                for (int j=0; j<count; j++) { System.out.print(arr[i] + " "); } //print element count times 
                hM.remove(arr[i]); //then remove the element from the hash map
            }
        }
    }
 
    public static void main (String[] args) {
        int arr[] = {10, 5, 3, 10, 10, 4, 1, 3};
        orderedGroup(arr);
    }
}
```

### are the two given sets A and B disjoint?

- [12,34,11,9,3], [2,1,3,5] → false
    - |A| = m, |B| = n

- you can sort both arrays and check in order in O(n log n + m log m) or sort one array, and for each element of the other array, do binary search in the sorted one

- or, you can use a hash-table to store elements of the smaller set

```java
/* 
create an empty hashtable 
iterate through the first set and store every element in the hashtable 
iterate through the second set and check if any element is present in the hash table
if present, return false, ignore everything else-- otherwise return true
*/ 

import java.util.*;

class Main {
    static boolean areDisjoint(int set1[], int set2[]) {
        HashSet<Integer> set = new HashSet<>();
 
        for (int i=0; i<set1.length; i++) { set.add(set1[i]); } 
        //traverse the first array and add all its elements to the hashset
				 for (int i=0; i<set2.length; i++) { //traverse the second array 
            if (set.contains(set2[i])) { return false; }//for each element, check if it is contained in the hash set
	       }
        return true; //otherwise return true
    }

    public static void main (String[] args) {
        int set1[] = {10, 5, 3, 4, 6};
        int set2[] = {8, 7, 9, 3};
        if (areDisjoint(set1, set2))
            System.out.println("Yes");
        else
            System.out.println("No");
    }
}
```

### find symmetric pairs

- [(11,20), (30,40), (5,10), (40,30), (10,5)] → (30,40), (5,10)

- you can sort the pairs by the first coordinate, then search, in O(n log n)

- or, you can use a hash-table, with x as the key and y as the value

```java
/* 
the first element of the pair is used as the key and the second element is used as the value
the idea is to traverse all pairs one by one
for every pair, check if its second element is in the hash table
if yes, then compare the first element with the value of the matched entry of the hash table
if the value and the first element match then we found symmetric pairs
else, insert the first element as a key and the second element as a value
*/ 

import java.util.HashMap;
  
class SymmetricPairs {

    static void findSymPairs(int arr[][]) {
        HashMap<Integer, Integer> hM = new HashMap<Integer, Integer>();
        for (int i = 0; i < arr.length; i++) {
            int first = arr[i][0]; //key
            int sec   = arr[i][1]; //value
            Integer val = hM.get(sec); //retrieves second element in pair from hash table
  
            if (val != null && val == first) { System.out.println("(" + sec + ", " + first + ")"); } 
            //if found, and value in hash matches with key, then this is a symmetric pair
                
            else { hM.put(first, sec); } //else add the pair to the hash table
        }
    }
    
	public static void main(String arg[]) {
        int arr[][] = new int[5][2];
        arr[0][0] = 11; arr[0][1] = 20;
        arr[1][0] = 30; arr[1][1] = 40;
        arr[2][0] = 5;  arr[2][1] = 10;
        arr[3][0] = 40;  arr[3][1] = 30;
        arr[4][0] = 10;  arr[4][1] = 5;
        findSymPairs(arr);
    }
}

```