# arrays and stacks

# searching

- T(n): number of guesses in a range of n numbers
    - e.g. T(n) = {1 if n ≤ 3} {T(n/2) + 1 otherwise}
- linear search takes O(n) or exactly n steps
- in each iteration, binary search eliminates AT LEAST half of the elements
    - binary search works by eliminating half of its elements and focusing on either the upper or lower half— however, when the search space contains an odd number of elements, the two halves will not be equal (floor and ceiling)
- a binary search can only be applied to an ordered array— therefore, unordered arrays offer fast insertion but slow search
- arrays are always slow in deletion, sorted or not

## linear search

```sql

int linearsearch(int[] a, int k) {
	int n = a.length; 
	for(int i=0; i<n; i++) {
		if(a[i] == k) return i;
	return -1; 
}

```

## binary search

```sql
int binarysearch(int[] a, int key) {
	int n = a.length; 
	int low = 0, high = n-1; 
	while(low <= high) {
		int mid = (low+high)/2; 
		if(a[mid] == key) {
			return mid; 
		} else if(key > a[mid]) {
				low = mid + 1;
		} else if(key < a[mid]) {
				high = mid - 1;
		}
	return -1;
}
```

### demonstration of binary search

| low | high | mid | a[mid] |
| --- | --- | --- | --- |
| 0 | 31 | 15 | 25 |

| low | high | mid | a[mid] |
| --- | --- | --- | --- |
| 16 | 31 | 23 | 33 |

| low | high | mid | a[mid] |
| --- | --- | --- | --- |
| 16 | 22 | 19 | 29 |

| low | high | mid | a[mid] |
| --- | --- | --- | --- |
| 20 | 22 | 21 | 31 |
- it takes only one iteration to find 25 since it is the first mid, and two iterations to find 33 and 17
- the worst case is log(n) + 1 or 6 guesses which will occur at the extreme ends
    
    
    | first | [10, 11, 12,…,41] | 10+41/2 = 25.5 = 25 | [26, 27, 28,…,41] |
    | --- | --- | --- | --- |
    | second | [26, 27, 28,…,41] | 26+41/2 = 33.5 = 33 | [34, 35, 36,…,41] |
    | third | [34, 35, 36,…,41] | 34+41/2 = 37.5 = 37 | [38, 39, 40, 41] |
    | fourth | [37, 38, 39, 40, 41] | 37+41/2 = 39.5 = 39 | [40, 41] |
    | fifth | [40, 41] | 40+41/2 = 40.5 = 40 | [41] |
    | sixth | [41] | 41+41/2 = 41 | 41 = key  |

> log(n) is roughly the number you can divide n by 2 before the result is less than 1
> 
- exercises
    1. if you search a china ID number, we need how many steps (population ≈ 1.4 bn)
        - linear search: 1.4 billion iterations
        - if sorted, binary search log(1.4 billion) = $2^3$$^1$ approximately
    2. this subject has 330 registered students— suppose we keep your IDs in a sorted array; if we use binary search to search a student in this array, and we find her record in three steps, the possible indices for her ID in the array are
        
        
        | 0 - 329 |
        | --- |
        
        | 0 - 164 | 165 - 329  |
        | --- | --- |
        
        | 0 - 80 (because 163/2 - 1) | 82 - 163 | 165 - 246 | 248 - 329 |
        | --- | --- | --- | --- |
        | MID: 40 | MID: 122 | MID: 205 | MID: 288 |
    3. revise the recursive binary search such that it always returns the smallest index i with a[i] = key (the first occurrence of key)— for example, it should return 2 if a = [1, 2, 4, 4, 4, 5, 6, 7] and key = 4
        
        ```java
        //recursive
        
        public class int search(int arr[], int key, int low, int high) {
        	if(low > high) {
        		return -1; 
        	} 
        	int mid = low+high/2;
        	if(mid == key) {
        		if(mid > 0 && arr[mid-1] == key) 
        			return search(arr, key, low, mid-1);
        		} else {
        			return mid; 
        	} else if(arr[mid] < key) {
        		return search(arr, key, mid+1, high);
        	} else {
        		return search(arr, key, low, mid-1);
        	}
        } 
        
        //iterative 
        public class int search(int arr[], int key) {
        	int low = 0; 
        	int high = arr.length - 1; 
        
        	if(low > high) {
        		return -1; 
        	} 
        	int mid = low+high/2;
        	if(mid == key) {
        		if(mid > 0 && arr[mid - 1] == key) {
        			high = mid-1;
        		} else {
        			return mid; 
        		} 
        	} else if(arr[mid] > key) {
        			high = mid - 1;
        	} else {
        			low = mid + 1;
        	}
        	return -1;
        }
        ```
        
    4. some student claim that in each term he can guess two numbers to partition the array into three parts— in the next round, he focus on one of the three parts (for example, in searching an array of length 20, he compare key with elements at indices 6 and 13) is this algorithm better than binary search? in the worst cases, how many elements do we need to compare?
        - it is not as sufficient as binary search since
            - in binary search, after 2 guesses, **3 quarters are removed**
            - in this searching algorithm, after 2 guesses, **2 thirds are removed**

## trade-offs

- time vs. space
- time of one operations vs. time of another

# arrays

- a collection of elements of the same type stored consecutively (no gaps) in the memory
- the simplest and most used data structure, built into all programming languages
- operations an array should provide:
    - adding an element
    - deleting an element
    - searching for an element
    - finding the min/max
- all strengths and weaknesses of arrays come from the consecutive storage

## insertion and deletion

- to insert into an unsorted array, simply put it at the end
- to insert into a sorted array, we need to find the correct location for the new element which is exactly insertion sort
- if we were to delete a value, for the unsorted array, we can simply move the values on the right to that empty slot, however, for the sorted array, we need to move all values on the right one space to the left
    - for unsorted: we use linear search, then move the last element to the empty slot → O(n)
    - for sorted: we use binary search, then we move all the elements to the left → O(n)*O(log(n)) = O(n)

### references

> if a[0] is stored at address is 0x5CA881B5, then what is the address of a[9]?
- 0x5CA881B5 + (9 * sizeOfN) — let’s just say we are dealing with integers — 0x5CA881B5 + (9 * 4) = 0x5CA881B5 + 0x24 = **5CA881D9**
> 

- the difference between `int count` and `int[] students`:
    - **objects** and **variables of primitive types** are declared differently
    
    - **int count**— the name ‘count’ is associated with the address of the four bytes allocated on the stack
    - **int[] students**— the name ‘students’ is associated with the address of the eight bytes allowed on the stack
- we need to call a constructor (a special method) to create an object using the keyword `new`
    
    - the meaning of assignment is different for objects than it is for primitive data types

- after `int[] a = new int[10]` which is wrong?
    
    ```java
    a[10]++; //WRONG because there are only 9 elements
    a = new int[20];
    a = new float[10]; //WRONG because this changes the memory address
    ```
    

## adjustable arrays

- an array has a fixed-size that needs to be specified during initialization

### balanced parentheses

1. ()
2. ())(
3. (()(()))
4. (()))()
- the empty sequence is balanced
    - if S and T balanced, then so are ST and (S)
    - it can be made empty by removing adjacent ()
    
    ```java
    boolean isBalanced(String s) {
    	int count = 0;
    	for(int i=0; i<s.length; i++) {
    		if(s.charAt(i) == '(') {
    			count++;
    		}
    		if(s.charAt(i) == ')') {
    			count--;
    		}
    		if(count < 0) {
    			return false;
    		}
    	return count = 0; //return true 
    }
    ```
    
- adapting the previous algorithm to check multiple kinds of parentheses
    
    ```java
    package staacks;
    
    import java.util.*;
    
    public class BracketBalance {
    	static boolean balanced(String exp) {
    		Stack<Character> s1 = new Stack<>(); //creating a stack 
    		for(int i=0; i<exp.length(); i++) { //iterates through the length of the string
    			char x = exp.charAt(i); //assigns x as each character of the string
    			if(x == '(' || x == '{' || x == '[') { //if the char is equal to any of the opening brackets, 
    				s1.push(x); //we push the value into the stack 
    				continue; //we continue
    			} else {
    				if(s1.isEmpty()) { //otherwise, we check if the stack is empty (i.e. there MUST be at least one opening bracket) 
    					return false;
    				}
    			}
    			
    			char check = s1.pop(); //assign check as the value popped from the stack
    			if(x == ')' && check != '(' || x == '}' && check != '{' || x == ']' && check != '[') { //if the brackets don't match
    				return false; //return false 
    			}
    			
    		}
    		return s1.isEmpty();
    	}
    	public static void main(String[] args) {
    		String exp = "{[(]]}";
    		
    		if(balanced(exp)) {
    			System.out.println("Balanced!");
    		} else {
    			System.out.println("Not balanced");
    		}
    	}
    }
    ```
    

# stacks

- **recursion**: a method that calls itself
    
    ```java
    int f(int a) {
    	if(a<=1) {
    		return 1;
    	} else {
    		return a * f(a-1);
    	}
    
    //f(4) 
    //f(4) f(3)
    //f(4) f(3) f(2) f(1) -> f(1) = 1
    //f(4) f(3) f(2) -> f(2) = 2*1 = 2
    //f(4) f(3) -> f(3) = 3*2 = 6
    //f(4) -> f(4) = 4*6 = 24 
    ```
    

### examples of stacks

- the back button of a web browser
- the undo button of a text editor
- the most recent pending method/function call is the current one to execute (LIFO)
- evaluation of arithmetic expression (including checking the balancedness of parentheses)
- validation of XML documents

## specification of the stack

- a stack’s state is modeled as a sequence of elements, initially empty
 
- a `push(x)` operation appends x to the end of the sequence

- a `pop()` operation deletes the last element of the sequence and returns the element deleted

- an `isEmpty()` operation tests if the stack is empty

- a `peek()` operation simply looks at the object at the top
- write the output of the following operations
    1. push(2021), push(9), push(13), push(2011), pop, pop
        - returns 2011, then 13
        - final stack is [2021, 9]
    2. push(2021), pop, push(9), pop, push(13), push(2011)
        - returns 2021, then 9
        - final stack is [13, 2011]
    3. push(2021), push(9), pop, push(13), pop, push(2011)
        - returns 9, then 13
        - final stack is [2021, 2011]
    4. push(2021), push(9), push(13), pop, pop, push(2011)
        - returns 13, then 9
        - final stack is [2021, 2011]
    5. pop, push(2021), push(9), push(13), push(2011), pop
        - error
        - final stack is [2021, 9, 13]
- how many different outputs can we get
    - push(1), push(2), push(3), push(4), push(5), push(6), pop(), pop(), pop(), pop(), pop(), pop()
        - not 6! because we cannot output all possible combinations (e.g. 654312, 312456)

### exceptions

- popping from an empty stack cannot be done— an error/exception is reported
    
    ```java
    class charStack {
    	private char[] data;
    
    	char pop() {
    		if(isEmpty()) {
    			System.out.println("oops...");
    			return 0;
    		}
    	}
    }
    ```
    

## expressions

- **postfix notation**, also known as **reverse polish notation**— the two operands come first, then the operator
    - e.g. 3+4 → 34+
    3-2-1 → 32-1-
    3-(2-1) → 321--
- translation exercises
    1. 1-2-5+6/5 → 12-5-65/+
    2. (22/7+4)(6-2) → 227/4+62-*
    3. 7-(2*3+5)(8-4/2) → 723*5+842/-*-
    
- post fix notation in java
    
    ```java
    import java.util.*;
    
    public class PostFix {
    	static int reversepolish(String expr) {
    		Stack<String> s1 = new Stack<>();
    		for(int i=0; i<expr.length(); i++) {
    			char x = expr.charAt(i);
    			if(Character.isDigit(x)) {
    				s1.push(Character.toString(x));
    			} else if(x == '+' || x == '-' || x == '*' || x == '/') {
    				
    				int y = Integer.parseInt(s1.pop());
    				int j = Integer.parseInt(s1.pop());
    				int res = 0;
    				
    				switch(x) {
    				case '+':
    					res = j+y;
    					break;
    				case '-':
    					res = j-y; //reverse the operations since stack is LIFO
    					break;
    				case '*':
    					res = j*y;
    					break;
    				case '/':
    					res = j/y;
    					break;
    				}
    				
    				s1.push(Integer.toString(res));
    			}
    			
    		}
    		return Integer.parseInt(s1.pop());
    	}
    	
    	public static void main(String[] args) {
    		String value = "234*+";
    		System.out.println("234*+ = " + reversepolish(value));
    	}
    }
    ```
    

## generics and abstraction

- add stability to your code by making bugs detectable at compile time
    
    ```java
    Stack stack = new Stack();
    stack.add("hello");
    String s = (String) stack.pop();
    
    Stack<String> stack = new Stack<String>();
    stack.add("hello");
    String s = stack.pop();
    ```
    

## objects

- an object is an entity that has a state (variables) and behavior (methods)
- a class is the model, or pattern, from which objects are created— many objects can be created from the same class
- the class concept supports data abstraction
    - grouping data that belongs together
    - grouping data together with accompanying behavior
    - separate the issue of how behavior is implemented from the issue of how it is used

> **specification vs. implementation**
>
