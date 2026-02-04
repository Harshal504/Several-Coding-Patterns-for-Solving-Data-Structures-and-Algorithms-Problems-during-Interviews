# Pattern 2: Two Pointers

In problems where we deal with sorted arrays (or <b>LinkedList</b>s) and need to find a set of elements that fulfill certain constraints, the <b>two pointers</b> approach becomes quite useful. The set of elements could be a pair, a triplet or even a subarray. For example, take a look at the following problem:

> Given an array of sorted numbers and a target sum, find a pair in the array whose sum is equal to the given target.

To solve this problem, we can consider each element one by one (pointed out by the first pointer) and iterate through the remaining elements (pointed out by the second pointer) to find a pair with the given sum. The <b>time complexity</b> of this algorithm will be `O(N^2)` where `N` is the number of elements in the input array.

Given that the input array is sorted, an efficient way would be to start with one pointer in the beginning and another pointer at the end. At every step, we will see if the numbers pointed by the <b>two pointers</b> add up to the target sum. If they do not, we will do one of two things:
1. If the sum of the two numbers pointed by the <b>two pointers</b> is greater than the target sum, this means that we need a pair with a smaller sum. So, to try more pairs, we can decrement the end-pointer.
2. If the sum of the two numbers pointed by the <b>two pointers</b> is smaller than the target sum, this means that we need a pair with a larger sum. So, to try more pairs, we can increment the start-pointer.

## 🌴 Pair with Target Sum  aka "Two Sum" (easy) 
https://leetcode.com/problems/two-sum/
> Given an array of sorted numbers and a `target` sum, find a pair in the array whose sum is equal to the given `target`.

Write a function to return the indices of the two numbers (i.e. the pair) such that they add up to the given `target`.

Since the given array is sorted, a brute-force solution could be to iterate through the array, taking one number at a time and searching for the second number through <b>Binary Search</b>. The <b>time complexity</b> of this algorithm will be `O(N*logN)`. Can we do better than this?

We can follow the <b>Two Pointers</b> approach. We will start with one pointer pointing to the beginning of the array and another pointing at the end. At every step, we will see if the numbers pointed by the <b>two pointers</b> add up to the `target` sum. If they do, we have found our pair; otherwise, we will do one of two things:
1. If the sum of the two numbers pointed by the <b>two pointers</b> is greater than the `target` sum, this means that we need a pair with a smaller sum. So, to try more pairs, we can decrement the end-pointer.
2. If the sum of the two numbers pointed by the <b>two pointers</b> is smaller than the `target` sum, this means that we need a pair with a larger sum. So, to try more pairs, we can increment the start-pointer.
### Brute Force Solution
````java
public class PairWithTargetSumBruteForce {

    public static int[] pairWithTargetSum(int[] nums, int target) {

        // pick the first number
        for (int i = 0; i < nums.length; i++) {

            // pick the second number
            for (int j = 0; j < nums.length; j++) {

                // make sure we are not using the same index twice
                if (i != j && nums[i] + nums[j] == target) {
                    return new int[]{i, j};
                }
            }
        }

        // no pair found
        return new int[]{-1, -1};
    }

    /*
     Sample Inputs:
     pairWithTargetSum([1,2,3,4,6], 6) -> [1,3]
     pairWithTargetSum([2,5,9,11], 11) -> [0,2]
    */
}
````
### Two pointer Solution
Assume the input is sorted
````java
public class PairWithTargetSumTwoPointers {

    public static int[] pairWithTargetSum(int[] arr, int target) {

        // one pointer at the beginning
        int start = 0;

        // another pointer at the end
        int end = arr.length - 1;

        // move pointers until they meet
        while (start < end) {

            int currentSum = arr[start] + arr[end];

            if (currentSum == target) {
                // found the pair
                return new int[]{start, end};
            }

            if (currentSum < target) {
                // we need a bigger sum → move start forward
                start++;
            } else {
                // we need a smaller sum → move end backward
                end--;
            }
        }

        // no pair found
        return new int[]{-1, -1};
    }

    /*
     Sample Inputs:
     pairWithTargetSum([1,2,3,4,6], 6) -> [1,3]
     pairWithTargetSum([2,5,9,11], 11) -> [0,2]
    */
}


````
- The <b>time complexity</b> of the above algorithm will be `O(N)`, where `N` is the total number of elements in the given array.
- The algorithm runs in constant space `O(1)`.

### ❗ Hash Table Solution 

Instead of using a two-pointer or a binary search approach, we can utilize a <b>HashTable</b> to search for the required pair. We can iterate through the array one number at a time. Let’s say during our iteration we are at number `X`, so we need to find `Y` such that `“X + Y == Target”`. We will do two things here:
1. Search for `Y` (which is equivalent to `“Target - X”`) in the HashTable. If it is there, we have found the required pair.
2. Otherwise, insert `“X”` in the HashTable, so that we can search it for the later numbers.
````java
import java.util.*;

public class PairWithTargetSumHashMap {

    public static int[] pairWithTargetSum(int[] nums, int target) {

        // map to store number → index
        Map<Integer, Integer> map = new HashMap<>();

        for (int i = 0; i < nums.length; i++) {

            int current = nums[i];
            int complement = target - current;

            // if complement already exists, we found the pair
            if (map.containsKey(complement)) {
                return new int[]{map.get(complement), i};
            }

            // otherwise store current number
            map.put(current, i);
        }

        return new int[]{-1, -1};
    }

    /*
     Sample Inputs:
     pairWithTargetSum([1,2,3,4,6], 6) -> [1,3]
     pairWithTargetSum([3,3], 6) -> [0,1]
    */
}

````

- The <b>time complexity</b> of the above algorithm will be `O(N)`, where `N` is the total number of elements in the given array.
- The <b>space complexity</b> will also be `O(N)`, as, in the worst case, we will be pushing `N` numbers in the HashTable.
## Remove Duplicates (easy)
https://leetcode.com/problems/remove-duplicates-from-sorted-array/

> Given an array of sorted numbers, <b>remove all duplicates</b> from it. You should <b>not use any extra space</b>; after removing the duplicates </i>in-place</i> return the length of the subarray that has no duplicate in it.

In this problem, we need to remove the duplicates </i>in-place</i> such that the resultant length of the array remains sorted. As the input array is sorted, therefore, one way to do this is to shift the elements left whenever we encounter duplicates. In other words, we will keep one pointer for iterating the array and one pointer for placing the next non-duplicate number. So our algorithm will be to iterate the array and whenever we see a non-duplicate number we move it next to the last non-duplicate number we’ve seen.

<b>Assume the input is sorted</b>

````java
public class RemoveDuplicates {

    public static int removeDuplicates(int[] arr) {

        // index of the next non-duplicate element
        int nextNonDuplicate = 1;

        // iterate through array starting from index 1
        for (int i = 1; i < arr.length; i++) {

            // if current number is different from the previous non-duplicate
            if (arr[nextNonDuplicate - 1] != arr[i]) {

                // move current number to nextNonDuplicate position
                arr[nextNonDuplicate] = arr[i];
                nextNonDuplicate++;
            }
        }

        // length of array without duplicates
        return nextNonDuplicate;
    }

    /*
     Sample Inputs:
     removeDuplicates([2,3,3,3,6,9,9]) -> 4  // [2,3,6,9]
     removeDuplicates([2,2,2,11]) -> 2      // [2,11]
    */
}


````
- The <b>time complexity</b> of the above algorithm will be `O(N)`, where `N` is the total number of elements in the given array.
- The algorithm runs in constant space `O(1)`.

### Remove Element
https://leetcode.com/problems/remove-element/

> Given an unsorted array of numbers and a target `key`, remove all instances of `key` </i>in-place</i> and return the new length of the array.

````java
public class RemoveElement {

    public static int removeElement(int[] arr, int key) {

        // index where next valid element will be placed
        int nextElement = 0;

        for (int i = 0; i < arr.length; i++) {

            // copy only elements that are NOT equal to key
            if (arr[i] != key) {
                arr[nextElement] = arr[i];
                nextElement++;
            }
        }

        return nextElement;
    }

    /*
     Sample Inputs:
     removeElement([3,2,3,6,3,10,9,3], 3) -> 4
     removeElement([2,11,2,2,1], 2) -> 2
    */
}


````
- The <b>time complexity</b> of the above algorithm will be `O(N)`, where `N` is the total number of elements in the given array.
- The algorithm runs in constant space `O(1)`.

## Squaring a Sorted Array (easy)
https://leetcode.com/problems/squares-of-a-sorted-array/

This is a straightforward question. The only trick is that we can have negative numbers in the input array, which will make it a bit difficult to generate the output array with squares in sorted order.

An easier approach could be to first find the index of the first non-negative number in the array. After that, we can use <b>Two Pointers</b> to iterate the array. One pointer will move forward to iterate the non-negative numbers, and the other pointer will move backward to iterate the negative numbers. At any step, whichever number gives us a bigger square will be added to the output array. 

Since the numbers at both ends can give us the largest square, an alternate approach could be to use <b>two pointers</b> starting at both ends of the input array. At any step, whichever pointer gives us the bigger square, we add it to the result array and move to the next/previous number according to the pointer. 

````java
public class SortedArraySquares {

    public static int[] makeSquares(int[] arr) {

        int n = arr.length;
        int[] squares = new int[n];

        // highest index to fill in result
        int highestSquareIndex = n - 1;

        int start = 0;
        int end = n - 1;

        // compare squares from both ends
        while (start <= end) {

            int startSquare = arr[start] * arr[start];
            int endSquare = arr[end] * arr[end];

            if (startSquare > endSquare) {
                squares[highestSquareIndex] = startSquare;
                start++;
            } else {
                squares[highestSquareIndex] = endSquare;
                end--;
            }

            highestSquareIndex--;
        }

        return squares;
    }

    /*
     Sample Inputs:
     makeSquares([-2,-1,0,2,3]) -> [0,1,4,4,9]
     makeSquares([-3,-1,0,1,2]) -> [0,1,1,4,9]
    */
}


````
- The above algorithm’s <b>time complexity</b> will be `O(N)` as we are iterating the input array only once.
- The above algorithm’s <b>space complexity</b> will also be `O(N)`; this space will be used for the output array.

## 🌟 Triplet Sum to Zero (medium)
https://leetcode.com/problems/3sum/

> Given an array of unsorted numbers, find all unique triplets in it that add up to zero.

This problem follows the <b>Two Pointers</b> pattern and shares similarities with [Pair with Target Sum](#🌴-pair-with-target-sum-aka-"two-sum"-easy). A couple of differences are that the input array is not sorted and instead of a pair we need to find triplets with a target sum of zero.

To follow a similar approach, first, we will sort the array and then iterate through it taking one number at a time. Let’s say during our iteration we are at number `X`, so we need to find `Y` and `Z` such that `X + Y + Z == 0`. At this stage, our problem translates into finding a pair whose sum is equal to `-X` (as from the above equation `Y + Z == -X`).

Another difference from <b>[Pair with Target Sum](#🌴-pair-with-target-sum-aka-"two-sum"-easy)</b> is that we need to find all the unique triplets. To handle this, we have to skip any duplicate number. Since we will be sorting the array, so all the duplicate numbers will be next to each other and are easier to skip.

````java
import java.util.*;

public class TripletSumToZero {

    public static List<List<Integer>> searchTriplets(int[] arr) {

        // sort array to apply two pointers
        Arrays.sort(arr);
        List<List<Integer>> triplets = new ArrayList<>();

        // fix one number and find pairs for remaining sum
        for (int i = 0; i < arr.length; i++) {

            // skip duplicates
            if (i > 0 && arr[i] == arr[i - 1]) continue;

            searchPair(arr, -arr[i], i + 1, triplets);
        }

        return triplets;
    }

    private static void searchPair(int[] arr, int targetSum,
                                   int left, List<List<Integer>> triplets) {

        int right = arr.length - 1;

        while (left < right) {

            int currentSum = arr[left] + arr[right];

            if (currentSum == targetSum) {
                triplets.add(Arrays.asList(-targetSum, arr[left], arr[right]));
                left++;
                right--;

                // skip duplicates
                while (left < right && arr[left] == arr[left - 1]) left++;
                while (left < right && arr[right] == arr[right + 1]) right--;

            } else if (currentSum < targetSum) {
                // need bigger sum
                left++;
            } else {
                // need smaller sum
                right--;
            }
        }
    }

    /*
     Sample Input:
     searchTriplets([-3,0,1,2,-1,1,-2])
     -> [[-3,1,2],[-2,0,2],[-2,1,1],[-1,0,1]]
    */
}

````
- Sorting the array will take `O(N * logN)`. The `searchPair()` function will take `O(N)`. As we are calling `searchPair()` for every number in the input array, this means that overall `searchTriplets()` will take `O(N * logN + N^2)`, which is asymptotically equivalent to `O(N^2)`.
- Ignoring the space required for the output array, the <b>space complexity</b> of the above algorithm will be `O(N)` which is required for sorting.

## Triplet Sum Close to Target (medium)
https://leetcode.com/problems/3sum-closest/

> Given an array of unsorted numbers and a `targetSum`, find a <b>triplet in the array whose sum is as close to the `targetSum` as possible</b>, return the sum of the triplet. If there are more than one such triplet, return the sum of the triplet with the smallest sum.

This problem follows the <b>Two Pointers</b> pattern and is quite similar to <b>[Triplet Sum to Zero](#🌟-triplet-sum-to-zero-medium)</b>.

We can follow a similar approach to iterate through the array, taking one number at a time. At every step, we will save the difference between the triplet and the `targetSum`, so that in the end, we can return the triplet with the closest sum.

````java
import java.util.*;

public class TripletSumCloseToTarget {

    public static int tripletSumCloseToTarget(int[] arr, int targetSum) {
        // sort the array
        Arrays.sort(arr);

        // this will store the smallest difference between
        // targetSum and the sum of a triplet
        int smallestDifference = Integer.MAX_VALUE;

        // iterate through the array, taking one number at a time
        for (int i = 0; i < arr.length - 2; i++) {

            int start = i + 1;
            int end = arr.length - 1;

            // search for the best pair for arr[i]
            while (start < end) {

                // difference between target and current triplet sum
                int targetDifference =
                        targetSum - arr[i] - arr[start] - arr[end];

                if (targetDifference == 0) {
                    // we've found a triplet with an exact sum
                    // so return the sum of all the numbers
                    return targetSum - targetDifference;
                }

                // save the closest difference
                if (Math.abs(targetDifference) < Math.abs(smallestDifference)) {
                    smallestDifference = targetDifference;
                }

                // the second part of the following 'if' is to handle
                // the smallest sum when we have more than one solution
                if (Math.abs(targetDifference) == Math.abs(smallestDifference)
                        && targetDifference > smallestDifference) {
                    // save the closest and the biggest difference
                    smallestDifference = targetDifference;
                }

                if (targetDifference > 0) {
                    // we need a triplet with a bigger sum
                    start++;
                } else {
                    // we need a triplet with a smaller sum
                    end--;
                }
            }
        }

        // return the sum closest to the target
        return targetSum - smallestDifference;
    }

    /*
     Sample Inputs:
     tripletSumCloseToTarget([-2, 0, 1, 2], 2)
     -> 1   // The triplet [-2, 1, 2] has the closest sum to the target.

     tripletSumCloseToTarget([-3, -1, 1, 2], 1)
     -> 0   // The triplet [-3, 1, 2] has the closest sum to the target.

     tripletSumCloseToTarget([1, 0, 1, 1], 100)
     -> 3   // The triplet [1, 1, 1] has the closest sum to the target.

     tripletSumCloseToTarget([-1, 2, 1, -4], 1)
     -> 2   // (-1 + 2 + 1 = 2) is closest to target 1.
    */
}

````
- Sorting the array will take `O(N* logN)`. Overall, the function will take `O(N * logN + N^2)`, which is asymptotically equivalent to `O(N^2)`
- The above algorithm’s <b>space complexity</b> will be `O(N)`, which is required for sorting.
## Triplets with Smaller Sum (medium)
https://leetcode.com/problems/3sum-smaller/

> Given an array `arr` of unsorted numbers and a `target` sum, count all triplets in it such that `arr[i] + arr[j] + arr[k] < target` where `i`, `j`, and `k` are three different indices. Write a function to return the count of such triplets.

This problem follows the <b>Two Pointers pattern</b> and shares similarities with <b>Triplet Sum to Zero</b>. The only difference is that, in this problem, we need to find the triplets whose sum is less than the given target. To meet the condition `i != j != k` we need to make sure that each number is not used more than once.

Following a similar approach, first, we can sort the array and then iterate through it, taking one number at a time. Let’s say during our iteration we are at number `X`, so we need to find `Y` and `Z` such that `X + Y + Z < target`. At this stage, our problem translates into finding a pair whose sum is less than `“target - X”` (as from the above equation `Y + Z == target - X`). We can use a similar approach as discussed in <b>Triplet Sum to Zero</b>.

````java
import java.util.*;

public class TripletsWithSmallerSum {

    public static int tripletWithSmallerSum(int[] arr, int target) {

        // sort array to use two pointers
        Arrays.sort(arr);
        int count = 0;

        // fix first number
        for (int i = 0; i < arr.length - 2; i++) {
            count += searchPair(arr, target - arr[i], i);
        }

        return count;
    }

    private static int searchPair(int[] arr, int targetSum, int first) {

        int count = 0;
        int start = first + 1;
        int end = arr.length - 1;

        while (start < end) {

            if (arr[start] + arr[end] < targetSum) {
                // found valid triplets
                // all numbers between start and end will work
                count += end - start;
                start++;
            } else {
                // need a smaller sum
                end--;
            }
        }
        return count;
    }

    /*
     Sample Inputs:
     tripletWithSmallerSum([-1,0,2,3], 3) -> 2
     tripletWithSmallerSum([-1,4,2,1,3], 5) -> 4
     tripletWithSmallerSum([-2,0,1,3], 2) -> 2
    */
}

````
- Sorting the array will take `O(N * logN)`. The `searchPair()` will take `O(N)`. So, overall `searchTriplets()` will take `O(N * logN + N^2)`, which is asymptotically equivalent to `O(N^2)`.
- The <b>space complexity</b> of the above algorithm will be `O(N)` which is required for sorting if we are not using an </i>in-place</i> sorting algorithm.

> Write a function to return the list of all such triplets instead of the count. How will the <b>time complexity</b> change in this case?

````java
import java.util.*;

public class TripletsWithSmallerSumList {

    public static List<List<Integer>> tripletWithSmallerSum(
            int[] arr, int target) {

        Arrays.sort(arr);
        List<List<Integer>> triplets = new ArrayList<>();

        for (int i = 0; i < arr.length - 2; i++) {
            searchPair(arr, target - arr[i], i, triplets);
        }

        return triplets;
    }

    private static void searchPair(
            int[] arr,
            int targetSum,
            int first,
            List<List<Integer>> triplets) {

        int start = first + 1;
        int end = arr.length - 1;

        while (start < end) {

            if (arr[start] + arr[end] < targetSum) {
                // every index between start and end is valid
                for (int i = end; i > start; i--) {
                    triplets.add(Arrays.asList(
                            arr[first],
                            arr[start],
                            arr[i]
                    ));
                }
                start++;
            } else {
                // need smaller sum
                end--;
            }
        }
    }
}

````
- Sorting the array will take `O(N * logN)`. The `searchPair()`, in this case, will take `O(N^2)`; the main while loop will run in `O(N)` but the nested for loop can also take `O(N)` - this will happen when the target sum is bigger than every triplet in the array.  So, overall `searchTriplets()` will take `O(N * logN + N^3)`, which is asymptotically equivalent to `O(N^3)`.
- Ignoring the space required for the output array, the <b>space complexity</b> of the above algorithm will be `O(N)` which is required for sorting.
## 🌟 Subarrays with Product Less than a Target (medium)
https://leetcode.com/problems/subarray-product-less-than-k/

> Given an array with positive numbers and a `targetSum`, find all of its contiguous subarrays whose <b>product is less than the `targetSum`</b>.

This problem follows the <b>Sliding Window</b> and the <b>Two Pointers</b> pattern and shares similarities with <b>[Triplets with Smaller Sum](#triplets-with-smaller-sum-medium)</b> with two differences:
1. In this problem, the input array is not sorted.
2. Instead of finding triplets with sum less than a target, we need to find all subarrays having a product less than the target.
The implementation will be quite similar to <b>[Triplets with Smaller Sum](#triplets-with-smaller-sum-medium)</b>.

````java
import java.util.*;

public class SubarraysWithProductLessThanTarget {

    public static List<List<Integer>> findSubarrays(int[] arr, int target) {

        List<List<Integer>> result = new ArrayList<>();
        int product = 1;
        int start = 0;

        for (int end = 0; end < arr.length; end++) {

            product *= arr[end];

            // shrink window until product < target
            while (product >= target && start < arr.length) {
                product /= arr[start];
                start++;
            }

            // build subarrays ending at index end
            List<Integer> tempList = new ArrayList<>();
            for (int i = end; i >= start; i--) {
                tempList.add(0, arr[i]);
                result.add(new ArrayList<>(tempList));
            }
        }

        return result;
    }

    /*
     Sample Inputs:
     findSubarrays([2,5,3,10], 30)
     findSubarrays([8,2,6,5], 50)
    */
}

````

- The main for-loop managing the sliding window takes `O(N)` but creating subarrays can take up to `O(N^2)` in the worst case. Therefore overall, our algorithm will take `O(N^3)`.
- Ignoring the space required for the output list, the algorithm runs in `O(N)` space which is used for the temp list.

## Dutch National Flag Problem (medium)
https://leetcode.com/problems/sort-colors/

> Given an array containing `0`s, `1`s and `2`s, sort the array </i>in-place</i>. You should treat numbers of the array as objects, hence, we can’t count `0`s, `1`s, and `2`s to recreate the array.

The flag of the Netherlands consists of three colors: red, white and blue; and since our input array also consists of three different numbers that is why it is called <b>Dutch National Flag problem</b>.

The brute force solution will be to use an <i>in-place</i> sorting algorithm like <i>Heapsort</i> which will take `O(N*logN)`. Can we do better than this? Is it possible to sort the array in one iteration?

We can use a <b>Two Pointers</b> approach while iterating through the array. Let’s say the <b>two pointers</b> are called `low` and `high` which are pointing to the first and the last element of the array respectively. So while iterating, we will move all `0`s before `low` and all `2`s after `high` so that in the end, all `1`s will be between `low` and `high`.

````java
public class DutchNationalFlag {

    public static void sortColors(int[] arr) {

        // all elements < low are 0
        // all elements > high are 2
        int low = 0;
        int high = arr.length - 1;
        int i = 0;

        while (i <= high) {

            if (arr[i] == 0) {
                swap(arr, i, low);
                i++;
                low++;
            } else if (arr[i] == 1) {
                i++;
            } else {
                swap(arr, i, high);
                high--;
            }
        }
    }

    private static void swap(int[] arr, int i, int j) {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }

    /*
     Sample Inputs:
     sortColors([1,0,2,1,0]) -> [0,0,1,1,2]
     sortColors([2,2,0,1,2,0]) -> [0,0,1,2,2,2]
    */
}

````
- The <b>time complexity</b> of the above algorithm will be `O(N)` as we are iterating the input array only once.
- The algorithm runs in constant space `O(1)`.

## 🌟 Quadruple Sum to Target (medium)
https://leetcode.com/problems/4sum/

> Given an array of unsorted numbers and a `targetSum`, find all <b>unique quadruplets</b> in it, whose <b>sum is equal to the `targetSum`</b>.

This problem follows the <b>Two Pointers</b> pattern and shares similarities with <b>[Triplet Sum to Zero](#🌟-triplet-sum-to-zero-medium)</b>.

We can follow a similar approach to iterate through the array, taking one number at a time. At every step during the iteration, we will search for the quadruplets similar to <b>Triplet Sum to Zero</b> whose sum is equal to the given `target`.
````java
public class QuadrupleSumToTarget {

    public static List<List<Integer>> searchQuads(int[] arr, int target) {

        // sort the array to use two pointers and handle duplicates
        Arrays.sort(arr);

        List<List<Integer>> quads = new ArrayList<>();

        // pick the first number
        for (int i = 0; i < arr.length - 3; i++) {

            // skip duplicate first numbers
            if (i > 0 && arr[i] == arr[i - 1]) {
                continue;
            }

            // pick the second number
            for (int j = i + 1; j < arr.length - 2; j++) {

                // skip duplicate second numbers
                if (j > i + 1 && arr[j] == arr[j - 1]) {
                    continue;
                }

                // search remaining two numbers using two pointers
                searchPairs(arr, target, i, j, quads);
            }
        }
        return quads;
    }

    private static void searchPairs(
            int[] arr,
            int targetSum,
            int first,
            int second,
            List<List<Integer>> quads) {

        int start = second + 1;
        int end = arr.length - 1;

        while (start < end) {

            int sum = arr[first] + arr[second] + arr[start] + arr[end];

            if (sum == targetSum) {
                // found a valid quadruplet
                quads.add(Arrays.asList(
                        arr[first],
                        arr[second],
                        arr[start],
                        arr[end]
                ));

                start++;
                end--;

                // skip duplicates for third number
                while (start < end && arr[start] == arr[start - 1]) {
                    start++;
                }

                // skip duplicates for fourth number
                while (start < end && arr[end] == arr[end + 1]) {
                    end--;
                }

            } else if (sum < targetSum) {
                // we need a bigger sum
                start++;
            } else {
                // we need a smaller sum
                end--;
            }
        }
    }

    /*
     Sample Inputs:
     searchQuads([4,1,2,-1,1,-3], 1)
     -> [[-3,-1,1,4], [-3,1,1,2]]

     searchQuads([2,0,-1,1,-2,2], 2)
     -> [[-2,0,2,2], [-1,0,1,2]]
    */
}
````
- Sorting the array will take `O(N*logN)`. Overall `searchQuads()` will take `O(N * logN + N³)`, which is asymptotically equivalent to `O(N³)`.
- The <b>space complexity</b> of the above algorithm will be `O(N)` which is required for sorting.
## 🌟 Comparing Strings containing Backspaces (medium)
https://leetcode.com/problems/backspace-string-compare/
>Given two strings containing backspaces (identified by the character `#`), check if the two strings are equal.

To compare the given strings, first, we need to apply the backspaces. An efficient way to do this would be from the end of both the strings. We can have separate pointers, pointing to the last element of the given strings. We can start comparing the characters pointed out by both the pointers to see if the strings are equal. If, at any stage, the character pointed out by any of the pointers is a backspace (`#`), we will skip and apply the backspace until we have a valid character available for comparison.

````java
public class BackspaceStringCompare {

    public static boolean backspaceCompare(String str1, String str2) {

        int i = str1.length() - 1;
        int j = str2.length() - 1;

        while (i >= 0 || j >= 0) {

            i = getNextValidIndex(str1, i);
            j = getNextValidIndex(str2, j);

            if (i < 0 && j < 0) return true;
            if (i < 0 || j < 0) return false;

            if (str1.charAt(i) != str2.charAt(j)) return false;

            i--;
            j--;
        }
        return true;
    }

    private static int getNextValidIndex(String str, int index) {

        int backspaceCount = 0;

        while (index >= 0) {
            if (str.charAt(index) == '#') {
                backspaceCount++;
            } else if (backspaceCount > 0) {
                backspaceCount--;
            } else {
                break;
            }
            index--;
        }
        return index;
    }

    /*
     Sample Inputs:
     backspaceCompare("xy#z","xzz#") -> true
     backspaceCompare("xy#z","xyz#") -> false
    */
}

````
- The <b>time complexity</b> of the above algorithm will be `O(M+N)` where `M` and `N` are the lengths of the two input strings respectively.
- The algorithm runs in constant space `O(1)`.

## 🌟 Minimum Window Sort (medium)
https://leetcode.com/problems/shortest-subarray-to-be-removed-to-make-array-sorted/

> Given an array, find the length of the smallest subarray in it which when sorted will sort the whole array.

As we know, once an array is sorted (in ascending order), the smallest number is at the beginning and the largest number is at the end of the array. So if we start from the beginning of the array to find the first element which is out of sorting order i.e., which is smaller than its previous element, and similarly from the end of array to find the first element which is bigger than its previous element, will sorting the subarray between these two numbers result in the whole array being sorted?

Let’s try to understand this with the Example mentioned above. In the following array, what are the first numbers out of sorting order from the beginning and the end of the array:

````
[1, 3, 2, 0, -1, 7, 10]
````
Starting from the beginning of the array the first number out of the sorting order is `2` as it is smaller than its previous element which is `3`.
Starting from the end of the array the first number out of the sorting order is `0` as it is bigger than its previous element which is `-1`

As you can see, sorting the numbers between `3` and `-1` will not sort the whole array. To see this, the following will be our original array after the sorted subarray:
````
[1, -1, 0, 2, 3, 7, 10]
````
The problem here is that the smallest number of our subarray is `-1` which dictates that we need to include more numbers from the beginning of the array to make the whole array sorted. We will have a similar problem if the maximum of the subarray is bigger than some elements at the end of the array. To sort the whole array we need to include all such elements that are smaller than the biggest element of the subarray. So our final algorithm will look like:
1. From the beginning and end of the array, find the first elements that are out of the sorting order. The two elements will be our candidate subarray.
2. Find the maximum and minimum of this subarray.
3. Extend the subarray from beginning to include any number which is bigger than the minimum of the subarray.
4. Similarly, extend the subarray from the end to include any number which is smaller than the maximum of the subarray.

````java
public class MinimumWindowSort {

    public static int shortestWindowSort(int[] arr) {

        int low = 0;
        int high = arr.length - 1;

        // find first out-of-order element from start
        while (low < arr.length - 1 && arr[low] <= arr[low + 1]) {
            low++;
        }

        if (low == arr.length - 1) return 0;

        // find first out-of-order element from end
        while (high > 0 && arr[high] >= arr[high - 1]) {
            high--;
        }

        int subArrayMin = Integer.MAX_VALUE;
        int subArrayMax = Integer.MIN_VALUE;

        for (int k = low; k <= high; k++) {
            subArrayMin = Math.min(subArrayMin, arr[k]);
            subArrayMax = Math.max(subArrayMax, arr[k]);
        }

        // extend window
        while (low > 0 && arr[low - 1] > subArrayMin) low--;
        while (high < arr.length - 1 && arr[high + 1] < subArrayMax) high++;

        return high - low + 1;
    }

    /*
     Sample Inputs:
     shortestWindowSort([1,2,5,3,7,10,9,12]) -> 5
     shortestWindowSort([1,3,2,0,-1,7,10]) -> 5
     shortestWindowSort([1,2,3]) -> 0
     shortestWindowSort([3,2,1]) -> 3
    */
}

````
- The <b>time complexity</b> of the above algorithm will be `O(N)`.
- The algorithm runs in constant space `O(1)`.
