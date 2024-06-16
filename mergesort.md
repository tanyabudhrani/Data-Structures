# mergesort

# merge
```java
public class MergeArray {	
	static void MA(int[] a1, int[] a2, int[] a) {
		int i1 = 0, i2 = 0, i = 0;
		while (i1 < a1.length && i2 < a2.length) {
			a[i++] = (a1[i1] <= a2 [i2])? a1[i1++]: a2[i2++]; //comparing elements while simulataneously adding them into the new array 
		} //first element in a is assigned to the smaller element between a1 and a2
		
		//don’t forget the leftover!!
		while (i1 < a1.length) { 
			a[i++] = a1[i1++]; 
		}
		while (i2 < a2.length) {
			a[i++] = a2[i2++];
		}
	}
	
	public static void main(String[] args) {
		int[] a1 = {8,10,14,89};
		int[] a2 = {32,48,50,77};
		int[] merged = new int[a1.length + a1.length];
		
		MA(a1, a2, merged);
		
		for(int num : merged) {
			System.out.print(num + " ");
		}
	}
}
```

- instead of two input arrays, the two parts are in the same array

## merging two parts

```java
void mergeV1(int [] a, int low, int mid, int high) {
	int[] b = new int[mid - low + 1];
	int[] c = new int[high - mid];

	for (int i=0; i <= mid - low; i++) {
		b[i]=a[low + i];
	}
	for (int i=0; i < high - mid; i ++) {
		c[i]=a[mid +1+ i];
	}

	int i1 = 0, i2 = 0, i = low;
	while (i1 < b.length && i2 < c.length ) {
		a [i++] = (b[i1] <= c [i2])? b[i1++]: c[i2 ++];
	}
	while (i1 < b.length) {
		a[i++] = b[i1++];
	}
	while (i2 < c.length) {
		a[i++] = c[i2 ++];
	}
}
```

## recursive mergesort

```java
package merge;

public class RecursiveMS {
	void mergeV1(int [] a, int low, int mid, int high) {
		int[] b = new int[mid - low + 1]; //creates a new array to store the LEFT part of a
		int[] c = new int[high - mid]; //creates a new array to store the RIGHT part of a
		
		for (int i=0; i <= mid - low; i++) { //copies all the elements from left part of a to b
			b[i]=a[low + i];
		}
		for (int i=0; i < high - mid; i ++) { //copies all the elements from the right part of a to c
			c[i]=a[mid + 1 + i];
		}

		int i1 = 0, i2 = 0, i = low;
		
		while (i1 < b.length && i2 < c.length) { //while there are elements to compare
			a[i++] = (b[i1] <= c[i2])? b[i1++]: c[i2++]; //simultaneously compares the elements from both arrays and copies them to a  
		}
		while (i1 < b.length) { //copy leftover elements
			a[i++] = b[i1++];
		}
		while (i2 < c.length) { //copy leftover elements
			a[i ++] = c[i2 ++];
		}
	}
	
	void rMergesortV1(int []a, int low, int high) {
		if (high <= low) { //base case
			return;
		}
		int mid = low + (high - low)/2;
		rMergesortV1(a, low, mid); //left part of array 
		rMergesortV1 (a, mid + 1, high); //right part of array
		mergeV1(a, low, mid, high); //merge the sorted parts
	}
	
	public static void main(String[] args) {
		RecursiveMS rms = new RecursiveMS();
		int[] arr = {5, 1, 4, 2, 3};
		
		rms.rMergesortV1(arr, 0, arr.length-1);
		
		for(int num : arr) {
			System.out.print(num + " ");
		}
	}
}
```

- the running time of merge sort is O(n*log(n))

### question

- of these sorting algorithms, bubble, insertion, selection, and mergesort, which are good for sorting an array that is:
    1. sorted (1,2,…,n) — bubble/insertion
    2. reverse (n,n-1,…,1) 
    3. both sorted and reversely sorted (1,2,3,4,3,2,1) — bubble/insertion
    4. almost sorted (2,3,4,5,6,7,8,9,1) — insertion
    5. random — merge 
- mergesort works best in all

### example

- there is an array of n elements, which is originally sorted— now, the value of one element in it is modified, but you don’t know which one; which sorting algorithm you want to use to make it sorted again?
    - insertion sort has a time complexity of O(n) when applied to an almost sorted array— or you can use merge sort to divide the array and eventually compare the modified element

## iterative mergesort

- similar to finding maximum by divide and conquer
    - sort every pair by merging two trivial parts
    - sort every quadruple by merging two pairs
    - sort a by merging two parts

- at j = 1, 2,…log(n), there are $n/2^j$ subproblems, each of size $2^j$

### process
- for a general $n$, there exists $k$ such that $2^k \leq n < 2^{k+1}$
  - the work of sorting $n$ elements is between sorting $2^k$ 

| n | logn | nlogn | $n^2$ |
| --- | --- | --- | --- |
| 2 | 1 | 2 | 4 |
| 4 | 2 | 8 | 16 |
| 8 | 3 | 24 | 64 |
| 16 | 4 | 64 | 256 |
| 32 | 5 | 160 | 1024 |
| 64 | 6 | 384 | 4096 |
| 128 | 7 | 896 | 16384 |
| 256 | 8 | 2048 | 65536 |

## standard mergesort

```java
void merge (int [] a, int low, int mid, int high) {
	int[] temp = new int[mid - low + 1];
	for(int i =0; i < temp.length; i++) {
		temp [i]= a[low +i];
	}
	int i=0, j=mid +1 , k=low;
	while (i < temp.length && j <= high) {
		a[k ++] = temp [i] <= a [j]? temp[i ++]: a[j ++];
	}
	while (i < temp.length) {
		a[k ++] = temp[i ++];
	}
}

void mergesort(int []a, int low , int high) {
	if (high < 1 + low) {
		return;
	}
	int mid = low + (high - low) / 2;
	mergesort(a, low, mid);
	mergesort(a, mid+1, high);
	merge2(a, low, mid, high);
}
```

- summary
    - merging two sorted arrays means to create a third array that contains all the elements from both arrays in sorted order
    - in mergesort, 1-element subarrays of a large array are merged into 2-element subarrays, 2-element subarrays are merged into 4-element subarrays, and so on until the entire array is sorted
    - mergesort always takes O(nlogn)time
    - mergesort requires a workspace of size n/2
        - the first sorting not in-place
    - mergesort makes two recursive calls to itself
    - to translate it to the iterative version, we take the bottom-up approach (similar as finding maximum)
    - timsort avoids the pitfalls of mergesort
    

# stability of sorting algorithms

- the relative order of two equal items (having the same key) will be preserved    

## why do we need stability?

- in a more general situation:
    - we have a lot of objects (tasks, patients) of different priority
    - when sorting them by priority, we want first come, first serve
    - if an instable sorting algorithm is used, we mess up the data

| algorithm | stability | notes |
| --- | --- | --- |
| bubble sort | stable | each time an element is moved by one position <br> it never swaps two elements of the same value |
| insertion sort | stable | we only move elements larger than the current |
| selection sort | not stable |  |
| merge sort | stable | elements in the same part never change order <br> two elements change order only when they are merged from two different parts into one part |

- of two equal elements, the one from `temp` takes precedence— `temp` is a copy of the left part
    - not stable if $\leq$ is replaced by $<$


## algorithm concepts

### stability

- two elements with equal values will appear in the same order in the sorted output as they appear in the unsorted input
    - e.g. if we wanted to sort [a, bb, c, ba] by the first letter, a stable sorting algorithm would return [a, bb, ba, c] while an unstable would maybe interchange the two
- stable algorithms are bubble and merge sort while unstable algorithms are selection

### in place

- in place algorithms can sort a list without creating an entirely new list
- examples of in place algorithms are insertion sort and quick sort— merge sort is NOT in place

### comparison

- a comparison sorting algorithm is an algorithm that only reads the list of elements through a single comparison operation to determine which of the two elements should occur first
- examples of these are bubble, selection, insertion, merge, and quick
