# Sorting Algorithms in Python and C++

Bubble, insertion, selection, merge and quick sort, written in an intuitive `while`-loop style. Every function sorts in place. Each section gives the idea, the complexity, and the code in both languages.

## Complexity Summary

| Algorithm | Best | Average | Worst | Space | Stable |
|-----------|------|---------|-------|-------|--------|
| Bubble    | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Insertion | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Selection | O(n²) | O(n²) | O(n²) | O(1) | No |
| Merge     | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Quick     | O(n log n) | O(n log n) | O(n²) | O(log n) | No |

**Notes**
- Bubble sort is O(n) in the best case only because of the `swapped` flag, which stops early when a pass makes no swaps. Without it, it is O(n²) in every case.
- Selection sort always scans the whole unsorted part to find the minimum, so even sorted input costs O(n²).
- Quick sort hits O(n²) when the pivot is always the smallest or largest element. With the first element as pivot, that happens on sorted input. A random pivot makes it very unlikely.
- Stable means equal elements keep their original relative order.

## C++ Headers

Put these once at the top of your `.cpp` file. The Python code needs no imports for the sorts themselves.

```cpp
#include <iostream>
#include <vector>
#include <utility>
#include <algorithm>
#include <cstdlib>
#include <string>
using namespace std;
```

---

## 1. Bubble Sort

Repeatedly swap adjacent elements that are out of order. After each pass, the largest remaining element "bubbles" to the end.

- **Time:** Best O(n) (already sorted, thanks to the `swapped` early exit), Average O(n²), Worst O(n²)
- **Space:** O(1)
- **Stable:** Yes

### Python

```python
def bubble_sort(arr):
    n = len(arr)
    end = n - 1                       # last index still unsorted

    while end > 0:
        swapped = False
        i = 0
        while i < end:
            if arr[i] > arr[i + 1]:
                arr[i], arr[i + 1] = arr[i + 1], arr[i]
                swapped = True
            i += 1

        if not swapped:               # no swaps means already sorted
            break
        end -= 1                      # arr[end] is now in its final place
```

### C++

```cpp
void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    int end = n - 1;                  // last index still unsorted

    while (end > 0) {
        bool swapped = false;
        int i = 0;
        while (i < end) {
            if (arr[i] > arr[i + 1]) {
                swap(arr[i], arr[i + 1]);
                swapped = true;
            }
            i++;
        }

        if (!swapped) break;          // no swaps means already sorted
        end--;                        // arr[end] is now in its final place
    }
}
```

---

## 2. Insertion Sort

Take the next element and slide it left into its correct spot inside the already-sorted part, like sorting playing cards.

- **Time:** Best O(n) (already sorted), Average O(n²), Worst O(n²)
- **Space:** O(1)
- **Stable:** Yes

### Python

```python
def insertion_sort(arr):
    n = len(arr)
    i = 1                             # arr[0..i-1] is the sorted part

    while i < n:
        key = arr[i]
        j = i - 1

        # shift bigger elements one step to the right
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1

        arr[j + 1] = key              # drop key into the gap
        i += 1
```

### C++

```cpp
void insertionSort(vector<int>& arr) {
    int n = arr.size();
    int i = 1;                        // arr[0..i-1] is the sorted part

    while (i < n) {
        int key = arr[i];
        int j = i - 1;

        // shift bigger elements one step to the right
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }

        arr[j + 1] = key;             // drop key into the gap
        i++;
    }
}
```

---

## 3. Selection Sort

Find the minimum of the unsorted part and swap it to the front.

- **Time:** Best O(n²) (it always scans the full rest), Average O(n²), Worst O(n²)
- **Space:** O(1)
- **Stable:** No (the swap can reorder equal elements)

### Python

```python
def selection_sort(arr):
    n = len(arr)
    i = 0                             # arr[0..i-1] is the sorted part

    while i < n - 1:
        min_idx = i
        j = i + 1
        while j < n:
            if arr[j] < arr[min_idx]:
                min_idx = j
            j += 1

        arr[i], arr[min_idx] = arr[min_idx], arr[i]
        i += 1
```

### C++

```cpp
void selectionSort(vector<int>& arr) {
    int n = arr.size();
    int i = 0;                        // arr[0..i-1] is the sorted part

    while (i < n - 1) {
        int minIdx = i;
        int j = i + 1;
        while (j < n) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
            j++;
        }

        swap(arr[i], arr[minIdx]);
        i++;
    }
}
```

---

## 4. Merge Sort

Split in half, sort each half recursively, then merge the two sorted halves.

- **Time:** Best O(n log n), Average O(n log n), Worst O(n log n)
- **Space:** O(n) for the temp list
- **Stable:** Yes

### Python

```python
def merge(arr, lo, mid, hi):
    # merge sorted arr[lo..mid] and arr[mid+1..hi]
    temp = []
    i = lo                            # pointer in left half
    j = mid + 1                       # pointer in right half

    # take the smaller front element from either half
    while i <= mid and j <= hi:
        if arr[i] <= arr[j]:          # <= keeps the sort stable
            temp.append(arr[i])
            i += 1
        else:
            temp.append(arr[j])
            j += 1

    # one half is exhausted, copy whatever remains of the other
    while i <= mid:
        temp.append(arr[i])
        i += 1
    while j <= hi:
        temp.append(arr[j])
        j += 1

    # copy the merged result back into arr
    k = lo
    while k <= hi:
        arr[k] = temp[k - lo]
        k += 1


def merge_sort(arr, lo=0, hi=None):
    if hi is None:
        hi = len(arr) - 1
    if lo < hi:
        mid = (lo + hi) // 2
        merge_sort(arr, lo, mid)
        merge_sort(arr, mid + 1, hi)
        merge(arr, lo, mid, hi)
```

### C++

```cpp
void merge(vector<int>& arr, int lo, int mid, int hi) {
    // merge sorted arr[lo..mid] and arr[mid+1..hi]
    vector<int> temp;
    int i = lo;                       // pointer in left half
    int j = mid + 1;                  // pointer in right half

    // take the smaller front element from either half
    while (i <= mid && j <= hi) {
        if (arr[i] <= arr[j]) {       // <= keeps the sort stable
            temp.push_back(arr[i]);
            i++;
        } else {
            temp.push_back(arr[j]);
            j++;
        }
    }

    // one half is exhausted, copy whatever remains of the other
    while (i <= mid) {
        temp.push_back(arr[i]);
        i++;
    }
    while (j <= hi) {
        temp.push_back(arr[j]);
        j++;
    }

    // copy the merged result back into arr
    int k = lo;
    while (k <= hi) {
        arr[k] = temp[k - lo];
        k++;
    }
}

void mergeSort(vector<int>& arr, int lo, int hi) {
    if (lo < hi) {
        int mid = lo + (hi - lo) / 2;
        mergeSort(arr, lo, mid);
        mergeSort(arr, mid + 1, hi);
        merge(arr, lo, mid, hi);
    }
}
```

---

## 5. Quick Sort

Pick a pivot, move smaller elements left and bigger ones right, then sort each side recursively.

- **Time:** Best O(n log n), Average O(n log n), Worst O(n²) (e.g. sorted input with the first element as pivot)
- **Space:** O(log n) average recursion stack
- **Stable:** No

### Python

```python
def partition(arr, lo, hi):
    pivot = arr[lo]                   # first element is the pivot
    i = lo
    j = hi

    while i < j:
        # move i right until we find an element bigger than pivot
        while i < hi and arr[i] <= pivot:
            i += 1
        # move j left until we find an element smaller or equal to pivot
        while arr[j] > pivot:
            j -= 1
        # both are on the wrong side, so swap them
        if i < j:
            arr[i], arr[j] = arr[j], arr[i]

    # j is now the last position of the "<= pivot" region
    arr[lo], arr[j] = arr[j], arr[lo]
    return j                          # pivot's final sorted position


def quick_sort(arr, lo=0, hi=None):
    if hi is None:
        hi = len(arr) - 1
    if lo < hi:
        p = partition(arr, lo, hi)
        quick_sort(arr, lo, p - 1)
        quick_sort(arr, p + 1, hi)
```

### C++

```cpp
int partition(vector<int>& arr, int lo, int hi) {
    int pivot = arr[lo];              // first element is the pivot
    int i = lo;
    int j = hi;

    while (i < j) {
        // move i right until we find an element bigger than pivot
        while (i < hi && arr[i] <= pivot) {
            i++;
        }
        // move j left until we find an element smaller or equal to pivot
        while (arr[j] > pivot) {
            j--;
        }
        // both are on the wrong side, so swap them
        if (i < j) {
            swap(arr[i], arr[j]);
        }
    }

    // j is now the last position of the "<= pivot" region
    swap(arr[lo], arr[j]);
    return j;                         // pivot's final sorted position
}

void quickSort(vector<int>& arr, int lo, int hi) {
    if (lo < hi) {
        int p = partition(arr, lo, hi);
        quickSort(arr, lo, p - 1);
        quickSort(arr, p + 1, hi);
    }
}
```

---

## Test Harness

Put this after the sorting functions to check all five against the built-in sort.

### Python (`python3 sorting.py`)

```python
if __name__ == "__main__":
    import random

    algorithms = [
        ("Bubble",    bubble_sort),
        ("Insertion", insertion_sort),
        ("Selection", selection_sort),
        ("Merge",     merge_sort),
        ("Quick",     quick_sort),
    ]

    sample = [38, 27, 43, 3, 9, 82, 10]
    print("Input:", sample)
    for name, sort_fn in algorithms:
        data = sample[:]              # copy so each algorithm gets the original
        sort_fn(data)
        print(f"{name:<10}: {data}")

    # random tests, including empty lists and duplicates
    for name, sort_fn in algorithms:
        for _ in range(500):
            size = random.randint(0, 30)
            data = [random.randint(-20, 20) for _ in range(size)]
            expected = sorted(data)
            sort_fn(data)
            assert data == expected, f"{name} failed"
    print("\nAll random tests passed.")
```

### C++ (`g++ -std=c++17 sorting.cpp -o sorting && ./sorting`)

```cpp
void printVector(const string& name, const vector<int>& arr) {
    cout << name;
    for (int x : arr) cout << " " << x;
    cout << endl;
}

int main() {
    vector<int> sample = {38, 27, 43, 3, 9, 82, 10};
    printVector("Input:     ", sample);

    vector<int> a;

    a = sample; bubbleSort(a);                       printVector("Bubble:    ", a);
    a = sample; insertionSort(a);                    printVector("Insertion: ", a);
    a = sample; selectionSort(a);                    printVector("Selection: ", a);
    a = sample; mergeSort(a, 0, (int)a.size() - 1);  printVector("Merge:     ", a);
    a = sample; quickSort(a, 0, (int)a.size() - 1);  printVector("Quick:     ", a);

    // random tests, including empty vectors and duplicates
    for (int t = 0; t < 2000; t++) {
        int size = rand() % 31;
        vector<int> data(size);
        for (int& x : data) x = rand() % 41 - 20;

        vector<int> expected = data;
        sort(expected.begin(), expected.end());

        vector<int> b = data; bubbleSort(b);
        vector<int> ins = data; insertionSort(ins);
        vector<int> sel = data; selectionSort(sel);
        vector<int> mg = data; mergeSort(mg, 0, (int)mg.size() - 1);
        vector<int> qk = data; quickSort(qk, 0, (int)qk.size() - 1);

        if (b != expected || ins != expected || sel != expected ||
            mg != expected || qk != expected) {
            cout << "A test failed!" << endl;
            return 1;
        }
    }
    cout << "\nAll random tests passed." << endl;
    return 0;
}
```
