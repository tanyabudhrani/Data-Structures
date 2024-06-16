# recursion

Created time: October 6, 2023 2:36 AM

### using sentinels (dummy nodes)

- sentinels are nodes specially designed that do not hold or refer to any data of the data structure
    - to make insertion and deletion simple, we just need to add one sentinel node at the beginning and end of the linked list— by doing so, we no longer need to keep separate pointers for the head and tail & we also do not have to worry about updating the head and tial pointers

![Screenshot 2023-10-06 at 5.44.11 PM.png](recursion%202fb7cf81d41041b5b2958031d9ecebdd/Screenshot_2023-10-06_at_5.44.11_PM.png)

```java
class Node {
    public int data;
    public Node prev;
    public Node next;

    public Node(int data, Node prev, Node next) {
        this.data = data;
        this.prev = prev;
        this.next = next;
    }
}

class Sentinels {
    private Node senti;

    public Sentinels() { //constructor
        this.senti = new Node(0, null, null); //sets the sentinel as new node with value 0 that points to nothing
        this.senti.prev = this.senti; //then allow prev and next values of the sentinel to = itself 
        this.senti.next = this.senti; //this forms a CIRCULAR link 
    }

    public void insertAtBeginning(int data) { 
        Node newNode = new Node(data, this.senti, this.senti.next); //create a new node that points backward to senti and forward to the value after
        this.senti.next.prev = newNode; //set the link in between senti and the node after as this newnode
        this.senti.next = newNode; //senti.next is now newnode-- we do not need head anymore!
    }
    
    public void insertAtEnd(int data) {
        Node newNode = new Node(data, this.senti.prev, this.senti); //create a new node that points backwards to the value before senti and forward to senti itself
        this.senti.prev.next = newNode; //set the link between senti and the node before it as this newnode
        this.senti.prev = newNode; //the value right before senti is now newnode
    }
    
    public void deleteFromFront() {
        if (this.senti.next == this.senti) { //list is empty
            return;
        }

        Node firstNode = this.senti.next; //set temp = the value right after senti (first node)
        this.senti.next = firstNode.next; //set the value right after senti = the value after the first node
        firstNode.next.prev = this.senti; //set the link between the first and second node to now point backwards to senti
    }

    public void deleteFromEnd() {
        if (this.senti.prev == this.senti) {
            return;
        }

        Node lastNode = this.senti.prev; //set temp = the value right before senti (last node)
        lastNode.prev.next = this.senti; //set the link between the last and second to the last node to point to senti 
        this.senti.prev = lastNode.prev; //set the value right before senti = the second to the last node
    }

    public void traverse() {
        Node currentNode = this.senti.next;
        while (currentNode != this.senti) {
            System.out.println(currentNode.data);
            currentNode = currentNode.next;
        }
    }
    public static void main(String[] args) {
        Sentinels my_list = new Sentinels();
        my_list.insertAtBeginning(5);
        my_list.insertAtBeginning(10);
        my_list.insertAtBeginning(15);
        my_list.traverse();
    }
}

```

### circular doubly linked lists

![Screenshot 2023-10-06 at 5.45.30 PM.png](recursion%202fb7cf81d41041b5b2958031d9ecebdd/Screenshot_2023-10-06_at_5.45.30_PM.png)

```java
public class CircularDLL {

    private Node head;
    private Node tail;
    private int length;

    private class Node {
        private int data;
        private Node next;
        private Node prev;

        public Node(int data) {
            this.data = data;
        }
    }

    public CircularDLL() {
        this.head = null;
        this.tail = null;
        this.length = 0;
    }

    public boolean isEmpty() {
        return length == 0; // head == null
    }

    public int length() {
        return length;
    }

    public void insertfirst(int value) {
        Node newNode = new Node(value);

        if (isEmpty()) {
            head = newNode; //if is empty, both head and tail will point to the newnode
            tail = newNode;
            head.next = tail; //head.next will point to tail
            tail.prev = head; //tail.prev will point to head
        } else {
            newNode.next = head; 
            head.prev = newNode;
            head = newNode; //set head = new node
            head.prev = tail; //set head to point backwards to tail
            tail.next = head; //set tail to point forwards to head (CIRCULAR)
        }
        length++;
    }

    public void insertlast(int value) {
        Node newNode = new Node(value);

        if (isEmpty()) {
           insertfirst(value);
        } else {
            tail.next = newNode; 
            newNode.prev = tail;
            tail = newNode; //set tail = new node
            head.prev = tail; //set head to point backwards to tail
            tail.next = head; //set tail to point forwards to head
        }
        length++;
    }

    public Node deletefirst() {
    	
        if (isEmpty()) {
            System.out.println("Empty!");
            return null;
        }

        Node temp = head;
        if (head == tail) { //if now, set both head and tail = null
            head = null;
            tail = null;
        } else {
            head = head.next; //else, set head to point to its next node
            head.prev = tail; //set new head to point backwards to tail
            tail.next = head; //and tail to point forwards to new head
        }
        temp.next = null; //break the link between the old head's next and prev pointers
        temp.prev = null;
        length--; //reclaim memory 
        return temp; //return the deleted value
    }

    public Node deletelast() {
    	
        if (isEmpty()) {
            System.out.println("Empty!");
            return null;
        }
        Node temp = tail;
        if (head == tail) { //if now empty, set both head and tail = null
            head = null;
            tail = null;
        } else {
            tail = tail.prev; //else set tail = its previous value
            tail.next = head; //set new tail to point forwards to head
            head.prev = tail; //set head to point backwards to new tail
        }
        temp.next = null; //break the link between the old tail's next and prev pointers
        temp.prev = null;
        length--; //reclaim memory
        return temp; //return deleted value
    }

    public void displayForward() {
    	
        if (head == null) { //empty
            return;
        }
        Node temp = head;
        do {
            System.out.print(temp.data + " ");
            temp = temp.next;
        } while (temp != head); //while temp does not equal head instead of while temp.next != null!!
    }

    public void displayBackward() {
    	
        if (tail == null) { //empty
            return;
        }
        Node temp = tail;
        do {
            System.out.print(temp.data + " ");
            temp = temp.prev;
        } while (temp != tail); //while temp does not equal tail!!
    }

    public static void main(String[] args) {
    	CircularDLL dll = new CircularDLL();
        dll.insertlast(11);
        dll.insertlast(20);
        dll.insertlast(15);
        dll.insertlast(2);
        dll.displayForward();
        System.out.println();
        dll.displayBackward();
    }
}
```

---

# recursion

- a method that calls itself
    
    ```java
    public class int fibonacci(int f) {
    	if(f <= 1) {
    		return f;
    	} else {
    		return fibonacci(f-1) + fibonacci(f-2);
    	}
    }
    ```
    
- examples
    - what do these methods do?
        
        ```java
        long sum(int n) {
        	if(n<=1) {
        		return n;
        	} else {
        		return sum(n-1) + n;
        } 
        
        //finds the sum of all numbers from 1-n
        
        void mystery1(String s) {
        	if(s.length() == 0) {
        		return;
        	} else {
        		System.out.println(s.charAt(0)); //prints the first character of the string
        		mystery1(s.substring(1)); //recur with the remaining substring
        	}
        }
        //prints each character in a new line
        void mystery2(String s) {
        	if(s.length() == 0) {
        		return;
        	} else {
        		mystery2(s.substring(1)); //calls itself with the remaining substring 
        		System.out.println(s.charAt(0)); //prints the character
        	}
        }
        
        //if you were to interchange mystery 1 and mystery 2
        //it will print the odd characters then the even characters
        ```
        

### GCD

```java
//iterative 
static int iterativeGCD(int a, int b) {
	while(a!=b) {
		if(a>b) {
			a-=b;
		} else {
			b-=a;
		}
	}
	return a;
}

//recursion 
static int recursiveGCD(int a, int b) {
	if(b!=0) {
		return recursiveGCD(b, a%b);
	} else {
		return a;
}
```

- alternative code
    
    ![Screenshot 2023-09-29 at 10.59.21 PM.png](recursion%202fb7cf81d41041b5b2958031d9ecebdd/Screenshot_2023-09-29_at_10.59.21_PM.png)
    

## tower of hanoi

- three rods and a number of disks of different diameters
- initially, all disks are in ascending order of size on one rod, smallest at the top
- the objective is to move the entire stack to another rod
    - only one disk can be moved at a time
    - only the uppermost disk on a stack can be moved
    - no disk may be placed on top of a smaller disk

### to move n disk from left to right

- move the top n-1 disks from the left to center
- move the largest disk from the left to the right
- move the n-1 disk from the center to the right

![Screenshot 2023-10-06 at 5.49.49 PM.png](recursion%202fb7cf81d41041b5b2958031d9ecebdd/Screenshot_2023-10-06_at_5.49.49_PM.png)

```java
public class TowerOfHanoi {
	static void solveHanoi(int n, char S, char T, char R) {
		if(n==0) { //base case
			return;
		}
		solveHanoi(n-1, S, R, T);
		System.out.println("Moved " + n + " from " + S + " to " + T);
		solveHanoi(n-1, R, T, S);
	}
	
	public static void main(String[] args) {
		int n = 3;
		solveHanoi(n, 'A', 'B', 'C');
	}
}
```

- summary of recursion
    - a recursive method calls itself repeatedly, with different argument values each time— although, some arguments don’t need to call themselves when they want to return (i.e. base case)
    - when the innermost instance of a recursive method returns, the process unwinds by completing pending instances of the method
    - a binary search can be carried out recursively by checking which half of a sorted range the search key is in, then doing the same thing with that half
        
        ```java
        package recursion;
        
        class RecursiveBinary {
        	
        	int binarySearch(int arr[], int low, int high, int x) {
        	    if (high >= low) {
        	        int mid = (low + high) / 2;
        
        	        if (arr[mid] == x) {
        	            return mid;
        	        } else if (arr[mid] > x) {
        	            return binarySearch(arr, low, mid - 1, x);
        	        } else {
        	            return binarySearch(arr, mid + 1, high, x);
        	        }
        	    }
        	    return -1;
        	}
        
            public static void main(String args[])
            {
            	RecursiveBinary ob = new RecursiveBinary();
                int arr[] = {2, 3, 4, 10, 40};
                int n = arr.length;
                int x = 10;
                
                int result = ob.binarySearch(arr, 0, n - 1, x);
                
                if (result == -1)
                    System.out.println(
                        "Element is not present in array");
                else
                    System.out.println(
                        "Element is present at index " + result);
            }
        }
        ```
        

## recursion vs. iteration

- sometimes it is easier to rewrite a recursive algorithm into an iterative one— however, some problems and procedures are extremely difficult to be expressed non-recursively
- which of the three sorting algorithms can be written as recursion?

### selection

```java
package doublylinked;

public class RecursiveSorting {
    void swap(int[] a, int i, int j) {
        int temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }
    void recursiveSelection(int[] a, int j) {
        if (j == a.length - 1) { //if already sorted, just return nothing
            return;
        }
        int n = a.length;
        int min = j;

        for (int i = j + 1; i < n; i++) { //iterates through the array to find the minimum
            if (a[min] > a[i]) {
                min = i;
            }
        }
        swap(a, j, min); //swaps minimum with first index
        recursiveSelection(a, j + 1); //updates the index and finds the next minimum
    }

	public static void main(String[] args) {
        RecursiveSorting sorter = new RecursiveSorting();
        int[] array1 = {5, 2, 9, 1, 5};
        int[] array2 = {8, 7, 6, 5, 4};

        sorter.recursiveSelection(array1, 0);

        System.out.print("Sorted array using Selection Sort: ");
        for (int num : array1) {
            System.out.print(num + " ");
        }
    }
}
```

### bubble

```java
public class RecursiveSorting {
    void swap(int[] a, int i, int j) {
        int temp = a[i];
        a[i] = a[j];
        a[j] = temp;
    }
		
		void recursiveBubble(int[] a, int n) {
        if (n == 1) { //if length == 1, just return
            return;
        }
        for (int i = 0; i < n - 1; i++) { //iterate through the list and swap adjacent elements
            if (a[i] > a[i + 1]) {
                swap(a, i, i + 1);
            }
        }
        recursiveBubble(a, n - 1); //you start from the end of the list and n--
    }

	public static void main(String[] args) {
        RecursiveSorting sorter = new RecursiveSorting();
        int[] array2 = {8, 7, 6, 5, 4};

        sorter.recursiveBubble(array2, array2.length);

        System.out.print("\nSorted array using Bubble Sort: ");
        for (int num : array2) {
            System.out.print(num + " ");
        }
    }
}
		
```

### insertion

```java
public class RecursiveSorting {
	void recursiveInsertion(int[] a, int n) {
	    	if (n <= 1) { //if empty, return
	            return;
	        }
	
	    	recursiveInsertion(a, n - 1); //keep calling itself with n-1 to sort the smaller subarrays to sort from the FIRST element to the second to the last
	
	        int lastElement = a[n - 1]; //last element is placed in its correct position in the already sorted array 
	        int j = n - 2;
	
	        while (j >= 0 && a[j] > lastElement) { //while j is greater than the last element
	            a[j + 1] = a[j]; //we swap
	            j--;
	        }
	
	        a[j + 1] = lastElement;
	    }
	}
```

## implementation of recursion

- when method A (caller) calls method B (callee), the return address of A is pushed onto the stack, and control is passed to B— when B finishes, the return address is popped off the stack and A resumes control
- recursive calls make the stack grow vary fast
- a recursion can always be mechanically simulated by manually maintaining the call stack— but it is too error-prone

### recursive calls eat space

```java
void iter(int n) {
	while(true) {
		n++;
	}
}

void rec(int n) {
	rec(n++);
}
```

### data structures are intrinsically recursive

```java
class Node <T > {
	T element ;
	Node <T> next ;
}
```

# divide and conquer

## finding a peak

- an array element that is not smaller than its neighbor
- every nonempty array has a peak
- if a[0] is not a peak then a[0] < a[1]; if a[1] isn’t, then a[1] < a[2]; then a[n-1]
    - for any a[i] that is not a peak: if a[i] < a[i-1], then there is a peak to the left of a[i], otherwise, a[i] < a[i+1], and there is a peak to the right of a[i]
    

```java
class RecursivePeak { 
	static int finder(int arr[], int low, int high, int n) { 

		int mid = (low + high) / 2; 

		if ((mid == 0 || arr[mid - 1] <= arr[mid]) && (mid == n - 1 || arr[mid + 1] <= arr[mid])) { //comparing mid with its neighbors IF mid is either the first or last element
			return mid; 
		} else if (mid > 0 && arr[mid - 1] > arr[mid]) { //if mid is not peak, but its left neighbor is greater than it, then its left half must have a peak element
			return finder(arr, low, (mid - 1), n); 
		} else
			return finder(arr, (mid + 1), high, n);  //if mid is not peak, but its right neighbor is greater than it, then its right half must have a peak element
		} 

	public static void main(String[] args) { 
		int arr[] = {1, 3, 20, 4, 1, 0}; 
		int n = arr.length; 
		System.out.println( 
			"Index of a peak point is " + finder(arr, 0, n-1, n)); 
	} 
}
```

## comparison of elements

- for loop conditions, we compare indices, but inside the loop, we compare elements

- a comparison of two indices (not too large numbers) is considered a primitive step, but the comparison of two objects usually needs a lot of operations
- to make class comparable, we need to implement the comparable interface
    
    ```java
    class Student implements Comparable < Student >{
    	String name;
    	String id;
    	String major;
    	String email;
    	public int compareTo(Student s2) {
    		return name.compareTo(s2.name);
    	}
    }
    ```
    

## finding maximum by divide and conquer

- an array can have many maximum elements, but they must have the same value (e.g. {11, 19, 23, 4, 12, 23, 7}, both a[2] and a[5] are maximum elements)
- each iteration of selection sort finds a maximum element in a range which takes n-1 comparisons— therefore, we should solve it using divide and conquer:
    - partition array into two parts
    - find the maximum element from each part
    - compare the two maximum elements and return larger
    - base case: only one element

![Screenshot 2023-10-06 at 6.11.59 PM.png](recursion%202fb7cf81d41041b5b2958031d9ecebdd/Screenshot_2023-10-06_at_6.11.59_PM.png)

![Screenshot 2023-10-06 at 6.12.09 PM.png](recursion%202fb7cf81d41041b5b2958031d9ecebdd/Screenshot_2023-10-06_at_6.12.09_PM.png)

![Screenshot 2023-10-06 at 6.12.32 PM.png](recursion%202fb7cf81d41041b5b2958031d9ecebdd/Screenshot_2023-10-06_at_6.12.32_PM.png)

```java
public class RecursiveMax {
	static int max(int[] a, int low, int high){
		if (low >= high) {
			return a[low];
		}
		
		int mid = (low+high)/2;
		
		int m1 = max(a , low , mid);
		int m2 = max(a , mid +1 , high);
		return (m1 > m2)? m1 : m2;
	}
	
	public static void main(String[] args) {
		int[] arr = {2, 5, 6, 8, 10};
		System.out.println(max(arr, 0, arr.length-1));
	}

}
```

### iterative implementation

![Screenshot 2023-10-06 at 6.15.35 PM.png](recursion%202fb7cf81d41041b5b2958031d9ecebdd/Screenshot_2023-10-06_at_6.15.35_PM.png)

```java
static int IterativeMax(int[] a) {
		int n = a.length ;
		int[] b = new int[n]; //creates a new array with the same size as the original
		for(int i = 0; i < n; i ++) {
			b[i] = a[i]; //copies all the values of the original array into the new one
		}
		while (n > 1) { //as long as the array isn't empty
			for (int j = 0; j < n/2; j ++) { //we iterate through HALF of the array 
				b[j] = (b[j*2] > b[j*2+1])? b[j*2]: b[j*2+1]; //compares the values at b[j*2] and b[j*2+1] (SECOND HALF) then replaces b[j] with the maximum of the comparison 
			}
			if (n != n/2*2) { //checks if the number of elements in the array is odd
				b[n/2] = b[n-1]; //if yes, it assigns the middle number as the element that has not been compared yet
			}
			n = (n+1)/2; //reduces the array size for the next iteration
		}
		return b[0];
	}
```

- we know how to find the maximum and how to find the minimum— now the question is how to find both
    - we can find the maximum first, then a minimum with how many comparisons?
        - this will take 2n-2 comparisons (n-1 for max, then n-1 for min)

![Screenshot 2023-10-06 at 6.22.32 PM.png](recursion%202fb7cf81d41041b5b2958031d9ecebdd/Screenshot_2023-10-06_at_6.22.32_PM.png)

- the operations at the lowest level are the same
    - there are two comparisons for two parts
        - compare the left max and right max
        - compare the left min and right min
    - but only one comparison in the base case

| n | comparisons |
| --- | --- |
| 2 | 1 |
| 4 | 4 |
| 8 | 10 |
| 16 | 22 |
- f(2n) = 2f(n) + 2 → f(n) = 1.5n - 2

- code for simultaneously finding the min and max
    
    ```java
    public class SimultaneousMM {
    	static class Pair {
    		int min;
    		int max;
    	}
    	static Pair MinMax(int[] a, int low, int high) {
    		Pair result = new Pair();
    		
    		if(low == high) {
    			result.min = a[low];
    			result.max = a[high];
    			return result;
    		}
    		
    		if(high - low == 1) {
    			if(a[low] < a[high]) {
    				result.min = a[low];
    				result.max = a[high];
    			} else {
    				result.min = a[high];
    				result.max = a[low];
    			}
    			return result;
    		}
    
    		int mid = (low+high)/2; 
    		Pair left = MinMax(a, low, mid);
    		Pair right = MinMax(a, mid+1, high);
    		
    		result.min = (left.min > right.min) ? right.min: left.min;
    		result.max = (left.max > right.max) ? left.max: right.max;
    		
    		return result;
    	}
    	
    	public static void main(String[] args) {
    		int[] arr = {1, 6, 3, 4, 2, 9, 0};
    		Pair p1 = MinMax(arr, 0, arr.length-1);
    		
    		System.out.println("Minimum: " + p1.min);
    		System.out.println("Maximum: " + p1.max);
    	}
    }
    ```
    

## finding the second largest element

- if the array has two largest elements, then we return one of them
    - 9, 9, 6: return 0 or 1, the index of one of the 9
    If the array has a unique maximum element, then we return an element with the second largest value
    - 9, 6, 6: return 1 or 2, the index of one of the 6’s
- although, we are only explicitly looking for a second largest, we have to find a largest and a second largest elements
    
    ![Screenshot 2023-10-06 at 6.55.10 PM.png](recursion%202fb7cf81d41041b5b2958031d9ecebdd/Screenshot_2023-10-06_at_6.55.10_PM.png)
    
- this problem can be solved using only n + logn comparisons ($n + logn = 2^3$$^0 + 30$ for $n = 2^3$$^0$)— we can achieve this by only calculating the maximum for each recursive step
    
    ![Screenshot 2023-10-06 at 6.56.59 PM.png](recursion%202fb7cf81d41041b5b2958031d9ecebdd/Screenshot_2023-10-06_at_6.56.59_PM.png)
    
    - either the largest of the left or the second of the right: $n-1 + ⌈log n⌉ − 1 = n + ⌈log n⌉ − 2$
    
    ```java
    public class SecondMax {
    	static class Pair {
    		int max; 
    		int secondmax;
    	}
    	
    	static Pair finder(int[] a, int low, int high) {
    		Pair res = new Pair();
    		
    		if(low == high) {
    			res.secondmax = a[low];
    			res.max = a[high];
    			return res;
    		}
    		
    		if(high - low == 1) {
    			if(a[low] < a[high]) {
    				res.secondmax = a[low];
    				res.max = a[high];
    			} else {
    				res.secondmax = a[high];
    				res.max = a[low];
    			}
    			return res;
    		}
    		
    		int mid = (low+high)/2;
    		Pair left = finder(a, low, mid);
    		Pair right = finder(a, mid+1, high);
    		
    		res.max = (left.max > right.max) ? left.max: right.max;
    		res.secondmax = findsecondmax(left.max, right.max, left.secondmax, right.secondmax);
    		
    		return res;
    	}
    	
    	static int findsecondmax(int m1, int m2, int s1, int s2) {
    		if(m1 > m2) {
    			if(m2 > s1) {
    				return m2;
    			} else {
    				return s1;
    			}
    		} else {
    			if(m1 > s2) {
    				return m1; 
    			} else {
    				return s2;
    			}
    		}
    	}
    	
    	public static void main(String[] args) {
    		int[] arr = {1, 6, 3, 4, 2, 9, 0};
    		Pair p1 = finder(arr, 0, arr.length-1);
    		
    		System.out.println("Second max: " + p1.secondmax);
    		System.out.println("Maximum: " + p1.max);
    	}
    }
    ```