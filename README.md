# Dynamic Stream Statistics Calculator

This repository contains a C++ implementation of a custom data structure (`data_base`) designed to maintain and query statistical metrics—mean, median, mode, and variance—over a dynamic stream of integers. It supports efficient insertions and deletions while maintaining running calculations.

## Features & Time Complexities

* **Insert/Remove Elements:** $\mathcal{O}(\log N)$
* **Get Mean:** $\mathcal{O}(1)$
* **Get Median:** $\mathcal{O}(1)$
* **Get Mode:** $\mathcal{O}(1)$
* **Get Variance:** $\mathcal{O}(1)$

## Algorithmic Approach

### 1. Mean and Variance (Welford's Algorithm)
To avoid floating-point precision issues and catastrophic cancellation that often occur when keeping a running sum of squares, this implementation uses **Welford's Online Algorithm**. 

For a new value $x_k$, the running mean $M_k$ and the sum of squared differences $S_k$ are updated as follows:
* $M_k = M_{k-1} + \frac{x_k - M_{k-1}}{k}$
* $S_k = S_{k-1} + (x_k - M_{k-1})(x_k - M_k)$

The sample variance is then retrieved in $\mathcal{O}(1)$ time by computing $\frac{S_k}{k - 1}$.

### 2. Median (Two-Heap Method)
The median is maintained using two `std::multiset` containers to simulate a max-heap (`low`) and a min-heap (`high`):
* `low` stores the smaller half of the elements.
* `high` stores the larger half.
* The sets are rebalanced after every insertion or deletion to ensure their sizes differ by no more than 1. 
* If the total count is odd, the median is the largest element in `low`. If even, it is the average of the largest in `low` and the smallest in `high`.

### 3. Mode (Frequency Map & Ordered Set)
The mode is tracked using a combination of `std::map` (to store frequencies) and a `std::set` of pairs (to keep the frequencies sorted).
* When an element is inserted or removed, its frequency is updated in the map.
* The set is updated with the new `{frequency, -value}` pair. We store the negative value so that in the event of a tie in frequency, the largest value can be easily retrieved from the end of the set.

## How to Run

Compile the code using a standard C++ compiler (C++11 or higher recommended):

```bash
g++ -O2 StreamStatistics.cpp -o StreamStatistics
./StreamStatistics
