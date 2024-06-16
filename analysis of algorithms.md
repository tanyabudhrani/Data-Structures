# analysis of algorithms

## runtimes

1. 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15
    
    
    | bubble | no swapping— performs 1 major iteration to check the entire array and 14 comparisons to, again, check the entire array  |
    | --- | --- |
    | insertion | performs 1 major iteration because it only needs to iterate once through the entire array and 14 comparisons as it moves each element to the sorted portion |
    | selection | performs 14 major iterations because it needs to select the smallest element from the unsorted portion, and 105 comparisons as it iteratively selects the smallest element in each pass |
2. 2,3,4,5,6,7,8,9,10,11,12,13,14,15,1
    
    
    | bubble | n-1 major iterations which means 14, n-i comparisons meaning that the number of comparisons decreases in each iteration (when i=1, 14 comparisons), and there is one swap in the ith iteration (14th)  |
    | --- | --- |
    | insertion | n-1 major iterations which means 14, one comparison, and two movements in the 13th and 14th iterations |
    | selection | n-1 major iterations which means 14, comparisons are 14+13+12…+1 = 105, 14 swaps to move the smallest element to its correct position in the sorted portion |
3. 15,14,13,12,11,10,9,8,7,6,5,4,3,2,1
    
    
    | bubble | n-1 major iterations so 14, n-i comparisons meaning that the number of comparisons decreases in each iteration, requires the maximum number of swaps (15-1)*15/2 = 210 swaps  |
    | --- | --- |
    | insertion | i comparisons based on the iteration (1 iteration, 1 comparison), i+2 movements in the ith major iteration (1 iteration, 3 movements)  |
    | selection | n-1 major iterations which means 14, and 14 swaps to move the smallest element to its correct position |

```java
long startTime = System.currentTimeMillis();

// run the algorithm 

long endTime = System.currentTimeMillis();
double duration = (endTime - startTime) / 1000;
```

### why analysis?

- you cannot run your program on all possible inputs
- it is very time-consuming to test
- testing results may be impacted by factors not related to the algorithm
- your codes may contain bugs
- it may not be practical to test at all

## worst case

- for bubble sort, the worse input of length n is $n, n-1, n-2,…, 1$— n-1 major iterations with n-1 comparisons and n-i swapping in the *i*th iteration
    - **n-1 major iterations**: refers to the fact that bubble sort will need to perform n-1 passes (iterations) through the entire list to sort it
    - **with n-1 comparisons**: means that in each pass, bubble sort will compare adjacent elements n-1 times (first and second, then second and third, until the second-to-last and last)
    - **and n-i swapping in the ith iteration**: means that in the ith iteration, bubble sort will perform n-i swaps— basically, as the iterations progress, fewer swaps are needed because the largest elements are already in their correct positions
- most of the time, we consider worst-case analysis because it provides a watertight guarantee of the running time but sometimes, best-case analysis is supplementary

## time

- the running “time” of an algorithm is not time in the physical sense since different computers have different CPUs and storage systems, the same algorithm takes different time if implemented by different languages, optimization made by compilers also matters

### assumptions

| each primitive step takes constant time  | adding/multiplying, comparing two small numbers, printing a small value  |
| --- | --- |
| each CPU instruction takes constant time  | memory is large enough to store all input/output data |
| the running time of an algorithm is a function f(n) on the input size n | where f(n) is the largest number of primitive steps it takes to solve an input of size n  |
- when analyzing the performance of an algorithm, we are interested in understanding how its running time scales with the size of its input— the function f(n) characterizes this relationship and represents the worst-case scenario in terms of the maximum number of primitive steps the algorithm will take to solve an input of a given size

### example

```java
boolean sorted(int[] a) {
	int n = a.length;
	boolean answer = true; 
	for(int i=1; i<n; i++) {
		if(a[i-1] > a[i]) {
			answer = false; 
		}
	}
	return answer;
}
```

1. `int n = a.length`; 1 operation 
2. `boolean answer = true`; 1 operation 
3. `int i=1`; 1 operation
4. `i<n`; 1 operation*n-1 times
5. `i++`; 1 operation*n-1 times
6. `a[i-1]>a[i]`; 1 operation*n-1 times
7. `if a[i-1]>a[i]`; 1 operation*n-1 times 
8. `answer = false`; 1 operation*n-1 times 
9. `return answer`; 1 operation
10. $1+1+(1+1+1)(n-1)+(1+1+1+)(n-1)+1 = O(n)$ 

> 2011 = 0b111 1101 1011 = 0x7DB
- decimal form: 2011
- binary form: 0b111 1101 1011
- hexadecimal form: 0x7DB
> 

## logarithm

- the inverse function to exponentiation
    
    
    | log functions | example |
    | --- | --- |
    | $log_a1=0$ | $log_21=0$ because any non-zero number raised to the power of 0 is 1  |
    | $log_axy=log_ax+log_ay$ | $log_2(4)=log_2(2)+log_2(2)$ because 4 is the product of 2 and 2 |
    | $log_a(x^y)=ylog_ax$ | $log_2(8)=3*log_2(2)$ because 8 is 2 raised to the power of 3 |
    | $log_ab=1/log_ba$ | $log_2(8)=1/log_8(2)$ because $log_8(2)$ is the reciprocal of $log_2(8)$ |
    | $log_aa=1$ | $log_22=1$ because any number raised to the power of 1 is itself |
    | $log_a(x/y)=log_ax-log_ay$ | $log_1$$_0(100)=log_1$$_0(1000)-log_1$$_0(10)$ because 100 is 1000 divided by 10  |
    | $log_a(a^x)=x$ | $log_5(5^3)=3 $ because 5$^3$ equals 125 |
    | $loglogn=log(logn)$ | $loglog(1000)=log(3)$ because 10*10*10  |

## floor and ceiling

- the floor function gives the greatest integer less than or equal to a real number x— $[x] = min${$m∈Z|m≤x$}, where min denotes the minimum value in a set, {$m∈Z|m≥x$} represents the set of all integers (Z) such that each integer m in the set is less than or equal to x
    - [3.2] = 3 because 3 is the largest integer that is less than or equal to 3.2

- the ceiling function gives the least integer greater than or equal to a real number x— $[x] = max${$m∈Z|m≥x$} where max denotes the maximum value in a set, {$m∈Z|m≥x$} represents the set of all integers (Z) such that each integer m in the set is greater than or equal to x
    - [3.2] = 4 because 4 is the smallest integer that is greater than or equal to 3.2
- the use of floor and ceiling functions is ubiquitous and mostly implicit
    - by definition, the running time is # primitive steps, hence an integer— if the running time of an algorithm is log n, we mean [log n]
    - we frequently say that we partition an array into two equal parts, hence size n/2 but this is impossible when n is odd— what we mean is to separate it into two parts of size n/2 and n/2 respectively

### example

- which grows faster? $n^2/1024$ vs. $1024nlogn$
    - comparing the two, $n^2/102$4 grows faster as a quadratic equation will encapsulate nlogn
        - they will cross at about 30,000,000

# big-oh notation
- **asymptotic upper bound**: helps describe the growth rate of a function as the input size approaches infinity
    - $1024nlogn$ grows in the order of $nlogn$
    - $n^2/1024$ grows in the order of **$n^2$**
    - $7n-1$ grows in the order of $n$
- mathematically, there exists constants ‘C’ and ‘k’ such that for all ‘n ≥ k’, ‘f(n) ≤ C*g(n)’— in summary, f(n) can bound T(n) if and only if T(n) is less than or equal to some constant multiplied by f(n) for all n, meaning that we need to choose a C such that T(n) is always bounded by f(n)

- we ignore multiplicative constants
    - ex: 73 = O(1), 3n = O(n)

- precision does not matter much

- discard the lower order terms
    - ex: $5n^2 + 3n = O(n^2)$
    - as n gets larger, **only the largest term matters**

> the reason we drop constants is because the very definition of asymptotic is to modulate constants to achieve a behavior
> 

### simple sorting algorithms

- all the simple sorting algorithms run in $O(n^2)$ time which means that their time quadruples when n doubles
    - in best case, bubble and insertion only need $O(n)$  time
- their differences are in the constant, but its significant when n is small— still their significance cannot be shown by experiments
- insertion sort is the best for small n
- the space is O(1)— such algorithms are called **in-place (in situ)**
    
    > since selection sort is an in-place sorting algorithm, it does not require additional storage and it sorts the elements within the original structure, without creating a new data structure to hold the sorted elements
    > 

- exercises
    1. $2011n + n^2/2011 = O(n)$
        - false
    2. $2011n = O(n^2/2011)$
        - true
    3. $2011n + n^2/2011 = O(nlogn)$
        - false
    4. $5nlogn = O(n)$
        - false
    5. $5nlogn = O(n^2)$
        - true
    6. $2011n = O(nlogn/2011)$
        - true
    7. $nlogn/2011 = O(2011n)$
        - false
    8. $2011n + n(logn)^2$$^0$$^1$$^1/2011 = O(n)$
        - false
    9. $2011n + n(logn)^2$$^0$$^1$$^1/2011 = O(n^2)$
        - true
    

> emphasizing that linear search’s time complexity is O(n)— in the worst-case scenario, it may have to examine every element in the list before finding the target, which leads to a linear relationship between input size and number of operations required
> 

# big omega

- **asymptotic lower bound**: describes the lower limit of a growth rate, often represented by big omega (best case)
- mathematically, there exists constants ‘C’ and ‘k’ such that for all ‘n≥k’, ‘f(n) ≥ C*g(n)’

- linear search takes $Ω(n)$ time, selection sort takes $Ω(n^2)$ time, and bubble sort takes $Ω(n^2)$ time in the worst cases

> f(n) = Ω(g(n)) if g(n) = O(f(n)) — this states that, if f(n) grows as fast as g(n) then g(n) can be bounded by f(n)
> 

# big theta
- **asymptotic tight bound**: describes both upper and lower bound on the growth rate of a function to capture the exact rate without over or underestimating
- mathematically, there exists positive constants ‘C1’, ‘C2’, and ‘k’ such that for all ‘n >= k, C1 * g(n) <= f(n) <= C2 * g(n)’

- determine if $(5n^2 +3n) =Θ(n^2)$ is true
    - $C1*n^2≤5n^2+3n≤C2*n^2$
        - **lower bound**: we need to show that there exists a constant C1 such that $C1*n^2$ is a lower bound for the expression— let’s take C1 = 1— now, for all n greater than or equal to some value k (let’s say k=1), **the expression $n^2$ is smaller than $5n^2+3n$ which satisfies the lower bound condition**
        - upper bound: we need to show that there exists a constant C2 such that $5n^2 + 3n$ is an upper bound for $C2 * n^2$— let’s take C2 = 8 (since 5+3 = 8)— now, for all n greater than or equal to the same value of k (k=1), **the expression $5n^2 + 3n$ is clearly smaller than $8n^2$ which satisfies the upper bound condition**

- determine if $(5n^2 +3n) =Θ(n)$ is false
    - the expression $5n^2 +3$ cannot be Θ(n) because $5n^2 +3n$ grows at a quadratic rate ($n^2$) while Θ(n) represent a linear growth rate
    

- challenging questions one
    - assume all functions are positive, and f1(n) > f2(n) and g1(n) > g2(n)
        - if f(n) = O(g(n)) and g(n) = O(h(n)), then f(n) = O(h(n))
            - this is true— if f(n) is bounded by a constant multiple of g(n), and g(n) is bounded by a constant multiple of h(n), then f(n) is bounded by a constant multiple of h(n) as well
        - if f1(n) = O(g1(n)) and f2(n) = O(g2(n)), then f1(n)+f2(n) = O(g1(n)+g2(n))
            - this is true— if f1(n) is bounded by a constant multiple of g1(n), and f2(n) is bounded by a constant multiple of g2(n), then the sum f1(n) + f2(n) is bounded by the constant multiple of the sum g1(n) + g2(n)
        - if f1(n) = O(g1(n)) and f2(n) = O(g2(n)), then f1(n)−f2(n) = O(g1(n)−g2(n))
            - false!
        - max{f(n),g(n)} = O(f(n)+g(n))
            - this is true— the maximum of two functions is ALWAYS less than or equal to their sum, therefore they are always bounded by the latter
    
- challenging questions two
    - $log(n!) = O(nlogn)$
        - false
    - $log(n!) = Θ(nlogn)$
        - since only upper bound is true, theta will be false
    - $log(n!) = Ω(nlogn)$
        - according to the definition of big-omega, nlogn grows much slower than log(n!)— so this is true

## code snippets for each complexity

```java
//O(1) 
void constantTimeExample() {
    int x = 5; // This assignment is constant time.
    System.out.println(x);
}

//O(logn) 
int binarySearch(int[] arr, int target) {
    int left = 0;
    int right = arr.length - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    return -1;
}

//O(n)
void linearTimeExample(int[] arr) {
    for (int i = 0; i < arr.length; i++) {
        System.out.println(arr[i]);
    }
}

//O(nlogn)
import java.util.Arrays;

void mergeSort(int[] arr) {
    if (arr.length <= 1) {
        return;
    }
    int mid = arr.length / 2;
    int[] left = Arrays.copyOfRange(arr, 0, mid);
    int[] right = Arrays.copyOfRange(arr, mid, arr.length);
    mergeSort(left);
    mergeSort(right);
    merge(arr, left, right);
}

void merge(int[] arr, int[] left, int[] right) {
    int i = 0, j = 0, k = 0;
    while (i < left.length && j < right.length) {
        if (left[i] < right[j]) {
            arr[k++] = left[i++];
        } else {
            arr[k++] = right[j++];
        }
    }
    while (i < left.length) {
        arr[k++] = left[i++];
    }
    while (j < right.length) {
        arr[k++] = right[j++];
    }
}

//O(n^2)
void quadraticTimeExample(int[] arr) {
    int n = arr.length;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            System.out.println(arr[i] + " " + arr[j]);
        }
    }
}

//O(n^3) 
void cubicTimeExample(int[][] matrix) {
    int n = matrix.length;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            for (int k = 0; k < n; k++) {
                System.out.println(matrix[i][j] + " " + matrix[i][k]);
            }
        }
    }
}

//O(2^n)
int fibonacci(int n) {
    if (n <= 1) {
        return n;
    } else {
        return fibonacci(n - 1) + fibonacci(n - 2);
    }
} 
```
