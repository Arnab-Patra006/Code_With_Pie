## [B. Roof Construction](https://codeforces.com/problemset/problem/1632/B)
```
in range 0 to n-1, the highest set bit starts from biggest power of 2.Let it be 2^k.And here k
is the highest set bit in n-1.So any number greater than 2^k will have smaller value if we do XOR.bcz MSB of them will be canceled out.
This way we can reduce them from n-1 to 2^k but after that when ever I do XOR of 2^k to any smaller that it will make bigger result but not 0.
So better to put 0 after that and then smaller nos till 1.  
```
## MEX and XOR :
```
- MEX :(Minimum EXcluded) of an array is the minimum non-negative integer that does not belong to the array.the shortest array with MEX a
 is the array [0,1,…,a−1], which has length a.

- XOR :if (A XOR B)=C => B=(A XOR C)
```