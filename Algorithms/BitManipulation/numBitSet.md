# Pass 1

To count the number of bits set in a non negative integer,
You don't need to know the size of the integer. That is, how many bits are used to represent it.
Just shift the number by one as long as it is non zero. And 'AND' it with one at every iteration to increase 
the count of bits set.

<code>
while (x) {
  count += x&1;
  x >>= 1;
}
</code>

Time complexity - O(1) per bit. Total time complexity - O(n)
