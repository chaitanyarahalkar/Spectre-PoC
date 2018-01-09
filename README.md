# Spectre Demonstration in C 


# Code Explanation

In this code, if the compiled instructions in victim function() were executed in strict program order, the function would only read from array1[0..15] since array1 size = 16.

However, when executed speculatively, out-of-bounds reads are possible. The read memory byte() function makes several training calls to victim function() to make the branch predictor expect valid values for x, then calls
with an out-of-bounds x. 

The conditional branch mispredicts,and the ensuing speculative execution reads a secret byte using the out-of-bounds x. 
The speculative code then reads from array2[array1[x] * 512],leaking the value of array1[x] into the cache state.

To complete the attack, a simple flush+probe is used to identify which cache line in array2 was loaded, reveaing the memory contents. The attack is repeated several
times, so even if the target byte was initially uncached,the first iteration will bring it into the cache

