---
num: "Lecture 6"
desc: "Hashing"
ready: true
lecture_date: 2020-01-27 12:30:00.00-7:00
---

# Hashing
* The ability to address unique key values in an array whose size may be smaller than the set of possible key values.
* Generally, keys are unique values that identify some record of information.
	* In order to put data (value) in the appropriate place in the collection, a hash function is required.
		* The hash function outputs a position in the collection based on the key value
		* Hash function outputs should be uniformly distributed
			* Or else all data would try to be stored in the same index.
		* For example, if you have array of 100 elements, a simple hash function could be:
			* key % 100 = [0-99]

![Hash Table](hashTable.png)

* Hashing is very efficient for searching for data in an array.
	* Recall binary search: O(log n) search time (if elements are sorted).
	* Linear search: O(n) search time.
	* Hash Table search: O(1) average search time in unsorted order.
		* Hash Table searching provides instant access to an element in an array since the hash function computes the index where the data is stored.

## Collisions
* It's possible that two elements may be indexed to the same location.
	* This is known as collisions

## Open-address Hashing
* Collisions are resolved by placing the item in the next open spot in the array.
* For example, if a record is hashed to position i and data already exists in that index, then check the next available spot i+1, i+2, etc.
	* Known as linear probing
	* What is a problem with this mechanism?
		* Delayed Insertion – inserting an item in a crowded hash table takes time since it must look for an empty spot.
			* Elements are inserted farther away from their actual hashed index.
		* Clustering – When different keys are hashed to the same index, there may be groups of data records grouped in the same place.

![Open Address](openAddress.png)

## Double Hashing
* Technique that uses a 2nd hash function when resolving a collision.
	* If a hash function index results in a collision, then use the 2nd hash function to determine how far to step in the array to look for an empty slot.
	* Helps reduce the clustering effect.
* Problems
	* If hash2 function is large, there is a possibility that we will go out of bounds.
	* Depending on the table size and hash2, it is possible that the index won’t be uniformly distributed.

![Double Hashing](doubleHashing.png)

## Chained Hashing
* The biggest problem to open-address hashing is 
	* If the table is full, no more elements can be added.
	* Similar to a vector, it could expand the capacity “under-the-hood” when needed, but…
		* All elements will probably have to be rehashed
		* New capacity shouldn’t be wasteful (too big) or too small
* Chained hashing (chaining)
	* If a collision occurs, then we store a series of data records in a list that the index in the hash table references.
	* Linked Lists are a common collection to store collided data records.

![Chaining](chaining.png)

## `std::map` vs `std::unordered_map`

* The underlying structure of an std::map is implemented with a balanced tree structure.
    * Useful if you care about the ordering of keys.
* `std::unordered_map` is used similarly to std::map, but the `std::unordered_map` is implemented with hash tables.
    * Notice the `std::unsorted_map`'s key / value pairs are not printed in sorted order by keys, but `std::map`'s keys are.
    * Hash Tables have a better average case performance (O(1)), but data order is not guaranteed when traversing the structure with iterators.

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{font-family:Arial, sans-serif;font-size:14px;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-family:Arial, sans-serif;font-size:14px;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-88nc{font-weight:bold;border-color:inherit;text-align:center}
.tg .tg-pq3e{font-style:italic;border-color:inherit;text-align:left}
.tg .tg-kiyi{font-weight:bold;border-color:inherit;text-align:left}
.tg .tg-uys7{border-color:inherit;text-align:center}
.tg .tg-xldj{border-color:inherit;text-align:left}
</style>
<table class="tg">
  <tr>
    <th class="tg-xldj"></th>
    <th class="tg-88nc" colspan="2">unordered_map</th>
    <th class="tg-88nc" colspan="2">map</th>
  </tr>
  <tr>
    <td class="tg-xldj"></td>
    <td class="tg-pq3e">Average</td>
    <td class="tg-pq3e">Worst-Case</td>
    <td class="tg-pq3e">Average</td>
    <td class="tg-pq3e">Worst-Case</td>
  </tr>
  <tr>
    <td class="tg-kiyi">Lookup</td>
    <td class="tg-uys7">O(1)</td>
    <td class="tg-uys7">O(n)</td>
    <td class="tg-uys7">O(log n)</td>
    <td class="tg-uys7">O(log n)</td>
  </tr>
  <tr>
    <td class="tg-kiyi">Deletion</td>
    <td class="tg-uys7">O(1)</td>
    <td class="tg-uys7">O(n)</td>
    <td class="tg-uys7">O(log n)</td>
    <td class="tg-uys7">O(log n)</td>
  </tr>
</table>
