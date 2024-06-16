# introduction

- codes !!
    
    ```java
    //Find the index of a peak, i.e., an element that is not smaller than its neighbor
    
    public class Peak {
        static int find(int arr[], int n) {
            if(n==1) { //if only one element, it is automatically a peak
                return 0;
            }
            if(arr[0] >= arr[1]) { //if the first element is larger than the second 
                return 0;
            }
            if(arr[n-1] >= arr[n-2]) { //if the last element is larger than the second to the last 
                return n-1;
            }
            for(int i=1; i<n-1; i++) { //all other elements 
                if(arr[i] >= arr[i-1] && arr[i] >= arr[i+1]) { //if one element is greater than its predecessor AND its successor 
                    return i;
                }
            }
            return 0;
        }
        
        public static void main(String[] args) {
            int[] arr1 = {1, 4, 7, 6, 3, 2, 9};
            int n = arr1.length; 
            System.out.println("Peak: " + find(arr1, n));
        }
    }
    ```
    
    ```java
    //Find the indices of a maximum element and a minimum element simultaneously, with the minimum number of comparisons among elements
    
    public class MM {
        public static void FMM(int arr[]) {
            int min = arr[0], max = arr[0]; 
    
            for(int i=1; i<arr.length; i++) { //for each element in the array 
                if(arr[i] > max) { //if the element is greater than its predecessor,
                    max = arr[i]; //max will be the element 
                }
                if(arr[i] < min) { //if the element is smaller than its predecessor, 
                    min = arr[i]; //min will be the element 
                }
            }
            System.out.println("Min: " + min);
            System.out.println("Max: " + max);
        }
    
        public static void main(String[] args) {
            int[] arr = {4, 53, 6, 21, 30, 42, 16, 44, 100, 97};
            FMM(arr);
        }
    }
    ```
    
    ```java
    //Find the index of a second largest element with the minimum number of comparisons among elements
    
    public class SecMax {
        public static void main(String[] args) {
            int[] arr = {4, 53, 6, 21, 30, 42, 16, 44, 100, 97};
            int max = 0, secondmax = 0; 
    
            for(int i=0; i<arr.length; i++) { //for each element in the array
                if(arr[i] > arr[max]) { //if the current element is greater than the max 
                    secondmax = max; //second max will take the value of max
                    max = i; //max will update to the new highest 
                } else if(arr[i] > arr[secondmax] && i != max ) { //otherwise, if max is already the max, then if any element is greater than the current second max AND it is not equal to the current max
                    secondmax = i; //second max will update to this number 
                }
            }
            System.out.println("Second max: " + secondmax);
        }
    }
    ```
    
    ```java
    //Find the index of the kth smallest element
    
    import java.util.Scanner;
    
    public class KthSmallest { 
        public static int finder(int arr[], int k) {
            for (int i = 0; i < arr.length; i++) { //for each element in the array
                int count = 0; //count is 0 
                for (int j = 0; j < arr.length; j++) { //compares each element with the other  
                    if (arr[j] < arr[i]) { //if the element at index j is smaller than the one at index i (recall that j will update faster than i) 
                        count++; //increase count 
                    }
                }
                if (count == k - 1) { //once count = to that element, stop the comparisons 
                    return arr[i];
                }
            }
            return 0;
        }
    
        public static void main(String[] args) {
            int[] arr = {64, 55, 12, 22, 11, 65, 27, 93, 100, 2};
            int k = 0; 
            try (Scanner sc = new Scanner(System.in)) {
                System.out.print("What is k: "); 
                k = sc.nextInt(); 
            }
            System.out.println("Kth smallest neighbor: " + finder(arr, k));
        }
    }
    ```
    
    ```java
    //Find the first element that occurs k times
    
    package introduction;
    import java.util.Scanner;
    
    public class Kthtimes {
    	public static int find(int arr[], int k) {
    		for(int i=0; i<arr.length; i++) { //for every element in arr
    			int count = 0;
    			for(int j=0; j<arr.length; j++) { //for every other element in arr (we compare) 
    				if(arr[i] == arr[j]) { //if there is a repetition
    					count++; //we increase count 
    					
    				}
    			}
    			if(count == k) { //once count = to k
    				return arr[i]; //we return that value that repeats k times
    			}
    		}
    		return -1; //if no value repeats 
    	}
    	public static void main(String[] args) {
    		int[] arr = {1, 2, 3, 2, 1, 4, 5, 8, 6, 7, 4, 2};
    		int k = 0;
    		
    		try(Scanner sc = new Scanner(System.in)) {
    			System.out.println("What is k: ");
    			k = sc.nextInt();
    		}
    		int result = find(arr, k);
    		
    		if(result != -1) {
    			System.out.println(result + " appears " + k + " times");
    		}
    	}
    }
    ```
    
    ```java
    //Are there duplicate elements within k distance from each other
    
    package introduction;
    import java.util.Scanner; 
    
    public class KthDistance {
    	public static boolean repetition(int arr[], int k) {
    		for(int i=0; i<arr.length; i++) { //for every element in arr
    			int distance = 0;
    			for(int j=i+1; j<arr.length; j++) { //we compare one element (i) to every other element in arr (j)
    				distance++; // increase distance as we traverse the array
    				if(arr[i] == arr[j]) { //as soon as we find a repetition
    					if(distance == k) { //and if the distance is equal to k
    						return true; //we return true
    					}
    					break; //break out of the loop 
    				}
    			}
    		}
    		return false; 
    	}
    	public static void main(String[] args) {
    		int[] arr = {1, 2, 3, 4, 1, 2, 3, 4};
    		int k=0; 
    		
    		try(Scanner sc = new Scanner(System.in)) {
    			System.out.println("What is the distance: ");
    			k = sc.nextInt();
    		}
    		boolean result = repetition(arr, k);
    		System.out.println(result);
    	}
    }
    ```
    
    ```java
    //Find the maximum distance between two occurrences of the same element
    package introduction;
    
    public class MaxDist {
    	public static int find(int arr[]) {
    		int maxdistance = 0;
    		for(int i=0; i<arr.length; i++) { //for every element in arr
    			for(int j=i+1; j<arr.length; j++) { //we compare one element (i) to every other element in arr (j)
    				if(arr[i] == arr[j]) { //everytime we find a repetition
    					int dist = j-i; //we find the distance between the two occurences 
    					if(dist > maxdistance) { //if this distance is greater than the current max
    						maxdistance = dist; //it will become the new max
    					}
    				}
    			}
    		}
    		return maxdistance; 
    	}
    	
    	public static void main(String[] args) {
    		int[] arr = {1, 2, 3, 4, 2, 5, 6, 2, 7, 8, 9, 2};
    		System.out.println(find(arr));
    	}
    }
    
    ```
    
    ```java
    //Are two given arrays disjoint
    package introduction;
    
    public class Disjoint {
    	public static boolean checker(int arr1[], int arr2[]) {
    		for(int i=0; i<arr1.length; i++) { //for every element in arr1
    			for(int j=0; j<arr2.length; j++) { //for every element in arr2
    				if(arr1[i] == arr2[j]) //we compare EACH element in arr1 with EVERY element in arr2-- if any of the numbers are the same
    					return false; //then this set is NOT disjoint 
    			}
    		}
    		return true; //otherwise it is disjoint 
    	}
    	
    	public static void main(String[] args) {
    		int[] arr1 = {12, 34, 11, 9, 3};
    		int[] arr2 = {2, 1, 3, 5};
    		
    		System.out.println(checker(arr1, arr2));
    	}
    }
    ```
    

## two-dimensional arrays

- there are three ways to store
    
    > 0 1 2 3
    4 5 6 7 
    8 9 10 11
    > 
    
    | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
    | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
    
    | 0 | 4 | 8 | 1 | 5 | 9 | 2 | 6 | 10 | 3 | 7 | 11 |
    | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
    
    | 8 | 9 | 10 | 11 |  | 0 | 1 | 2 | 3 |  | 4 | 5 | 6 | 7 |
    | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |

# data structures

- ways to organize data/information
    - simple variables— primitive types
    - objects— collection of data items of various types
    - arrays— collection of data items of the same type
    - linked lists— sequence of data items

> an algorithm is like a recipe which you can express with plain words, java code, or pseudocode
> 

### problem solving with a computer

1. model the problem as a computational problem
2. design an algorithm
3. implement the algorithm as a program

```java
int find(int key, int low, int high) {
	int mid = (low+high)/2; //using binary search 
	if(a[mid] == key) //if the middle value is the correct value
		return mid; //simply return the value 
	if(low == high) //if there is only one value in the array
		return -1; //return -1 
	if(a[mid] < key) //if the middle value is less than the key 
		return find(key, mid, high); //keep calling until it is higher than the key 
}

int find(int key) {
	if(length == 0) 
		return -1;
	return find(key, 0, length-1);
}
```

# bubble sort

- works by repeatedly swapping the adjacent elements if they are in the wrong order
    - traverse from left and compare adjacent elements so the higher one is placed at the right side
    - in this way, the largest element is moved to the rightmost end at first

```java
static void bubbleSort(int arr[], int n) {
	int i, j, temp; 
	boolean swapped; 
	for(i=0; i<n-1; i++) { //for every value in the array 
		swapped = false; 
		for(j=0; j<n-i-1; j++) { //compare the elements to one another  
			if(arr[j] > arr[j+1]) { //if the previous element is greater than the proceeding element 
				temp = arr[j]; //swapping algorithm
				arr[j] = arr[j+1];
				arr[j+1] = temp;
				swapped = true; //swapped at least once
			}
		}

		if(swapped = false) {
			break; //if no two elements were swapped, break
		}
	}
}
```

- worst case is O($n^2$) which can be extremely time consuming

# selection sort

- works by repeatedly selecting the smallest element from the unsorted portion of the list and moving it to the sorted portion
    - passes
        1. for the first position in the sorted array, the whole array is traversed from the first index to the last sequentially— the minimum value is then placed at the very beginning 
        2. for the second position, the array is traversed in a sequential manner to find the second lowest value and swap it with the number in the second position
        3. for the third place, again the array is traversed to find the next minimum value and it is swapped with the number in third place 
        4. do the same step for the fourth and any remaining positions until the array is fully sorted

```java
void sort(int arr[]) {
	int n = arr.length;

	for(int i=0; i<n-1; i++) {
		int min_idx = i; 
		for(int j=i+1; j<n; j++) {
			if(arr[j] < arr[min_idx]) {
				min_idx = j;
			}
		}
		int temp = arr[min_idx];
		arr[min_idx] = arr[i];
		arr[i] = temp; 
	}
}
```

- one loop to select an element one by one * one loop to compare that element with every other element— $O(N)*O(N) = O(N^2)$

# insertion sort

- works by splitting an array into a sorted and unsorted part— then values from the unsorted part are picked and placed at the correct position in the sorted part
    - to sort an array of size N in ascending order, iterate over the array and compare the current element to is predecessor— if the key element is smaller than its predecessor, compare it to the elements before. then move the greater elements up one position to make space for the swapped element
    - passes
        1. initially, the first two elements of the array are compared 
            
            
            | 12 | 11 | 13 | 5 | 6 |
            | --- | --- | --- | --- | --- |
        2. here, the first and second values are not in ascending order, therefore they swap and the smaller value is placed in the sorted sub array 
            
            
            | 11 | 12 | 13 | 5 | 6 |
            | --- | --- | --- | --- | --- |
        3. then, we compare the next two elements— since the third element is greater than the second, no swapping will occur, and the minimum will be placed in the sorted sub array
        4. then, we compare the next two elements— since the third element is greater than the fourth, we swap 
            
            
            | 11 | 12 | 5 | 13 | 6 |
            | --- | --- | --- | --- | --- |
            - but, now the second and third are not in the right place so we swap
            
            | 11 | 5 | 12 | 13 | 6 |
            | --- | --- | --- | --- | --- |
            - now the first and second are not in the right place so we swap again
            
            | 5 | 11 | 12 | 13 | 6 |
            | --- | --- | --- | --- | --- |
        5. now the 1st element up until the 3rd are placed in the sub array— then we compare the fourth and fifth element which are not in the correct place so we swap 
            
            
            | 5 | 11 | 12 | 6 | 13 |
            | --- | --- | --- | --- | --- |
            - again, the same problem:
            
            | 5 | 11 | 6 | 12 | 13 |
            | --- | --- | --- | --- | --- |
            
            | 5 | 6 | 11 | 12 | 13 |
            | --- | --- | --- | --- | --- |
            - finally, the array is sorted

```java
public class insertionsort {
	void sort(int arr[]) {
		int n = arr.length;
		for(int i=1; i<n; ++i) {
			int key = arr[i];
			int j = i-1; //j is the preceding element

		/* move elements of arr[0..i-1], that are 
		greater than key, to one position up */ 
		
			while(j>=0 && arr[j]>key) {
				arr[j+1] = arr[j];
				j=j-1;
			}
			arr[j+1] = key;
		}
	} 
}
```

- best case is O(N) but worst case is O(N$^2$)
