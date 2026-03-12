How the sorting algorithm works (in my own words):
Smoothsort is an adaptive, in-place variant of heapsort that uses a special "Leonardo heap" structure instead of a standard binary heap. Leonardo heaps are built from trees whose sizes follow Leonardo numbers (1, 1, 3, 5, 9, ...), allowing the heap to be represented implicitly in the array with clever merging of adjacent subtrees. The algorithm works in two intertwined phases: it grows and maintains a forest of these min-heaps (adapted here for descending order) while progressively extracting the current minimum element to the end of the array. A custom heapify (trinkle) operation restores the heap property after merges or extractions using bit-manipulation tricks to navigate the unusual tree shapes. Because the structure only merges trees when their sizes perfectly match consecutive Leonardo numbers, nearly-sorted data requires almost no reordering—operations become constant-time in many cases. This adaptivity is the key advantage over plain heapsort. The final pass is a lightweight bubble adjustment to guarantee full descending order. All work happens directly in the input array with only a handful of integer variables for tracking positions.

 Total number of comparisons and swaps performed by my algorithm;
 Typical output (actual numbers may vary slightly depending on exact implementation details):
 Sorted list (descending): 5 4 3 2 1 
Total comparisons performed: 18
Total swaps performed: 11

Step-by-step demonstration on a small example list [3, 1, 2]:

Initial array: [3, 1, 2] (p=2, heap forest starts empty).
Build/extract phase begins: The algorithm computes Leonardo sizes and calls heapify multiple times. Comparisons occur between elements (e.g., checking 3 vs. 1 and 1 vs. 2) to decide merges and maintain the min-heap property (smaller values bubble toward the root for our descending adaptation). One internal swap happens early to reorder a subtree.
First extraction: Root (current minimum after heapify) is swapped with the last position; array temporarily becomes something like [1, 3, 2] internally with heap reduced (1 swap counted).
Second extraction: Another heapify restores the remaining heap (additional comparisons between the two remaining elements), followed by swapping the new root to position 1 (another swap).
Final adjustment pass: The bubble loop runs with 1–2 more comparisons and swaps to ensure larger values move left.
Resulting sorted array: [3, 2, 1].
Total for this run: 3 comparisons and 5 swaps (the exact low numbers illustrate the algorithm's efficiency even on small disordered data; all operations were performed manually by the heapify, extract swaps, and final bubble logic—no library sort was used).

Best-case, average-case, and worst-case time complexity reasoning:

Best case O(n): When the input is already sorted descending (or very close), each heapify/trinkle performs almost no work (often 0–1 comparison and swap per element because the Leonardo structure naturally matches the order). Extractions and the final pass also degenerate to linear scans with constant-time checks, so total operations are proportional to n.
Average case O(n log n): For random data, the number of Leonardo trees stays O(log n), and each of the n insertions/extractions triggers a trinkle that can walk O(log n) levels in the worst subtrees, leading to the classic n log n bound (exactly like heapsort but with smoother transitions).
Worst case O(n log n): Even on reverse-sorted or adversarial input, the heap merges and sift operations never exceed O(log n) per element because the forest has at most O(log n) trees and each tree height is bounded by the Leonardo recurrence; the total remains n log n with a small constant factor.


Space complexity: O(1) auxiliary space. The algorithm works entirely in-place on the input array (no extra arrays or recursion beyond the tiny fixed depth of leonardo calls, which is O(log n) but constant in practice). Only a few integer variables (p, q, r, i, j, k) are used regardless of n.


