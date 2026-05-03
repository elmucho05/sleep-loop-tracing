The following directory will contain the output of what was generated with `perf` command on the imx8mp board.

Each output file contains the generated stats and the command options used to generate them.


# Tests and results

#### Generic tests with L1 cache
```bash
$ perf list cache # to see the available performance counters for the board

$ perf stat -a -A -e L1-dcache-loads,L1-dcache-load-misses sleep 5


# OUTPUT:
Performance counter stats for 'system wide':

CPU0                  3623445      L1-dcache-loads                                             
CPU1                   403420      L1-dcache-loads                                             
CPU2                   868480      L1-dcache-loads                                             
CPU3                   454977      L1-dcache-loads                                             
CPU0                   139335      L1-dcache-load-misses     #    3.85% of all L1-dcache accesses
CPU1                    28738      L1-dcache-load-misses     #    7.12% of all L1-dcache accesses
CPU2                    38069      L1-dcache-load-misses     #    4.38% of all L1-dcache accesses
CPU3                    28803      L1-dcache-load-misses     #    6.33% of all L1-dcache accesses

       5.004551106 seconds time elapsed

```


```bash
$ perf stat -e L1-dcache-loads,L1-dcache-load-misses sleep 5

 Performance counter stats for 'sleep 5':

            254268      L1-dcache-loads                                             
              7994      L1-dcache-load-misses     #    3.14% of all L1-dcache accesses

       5.003847029 seconds time elapsed

       0.000000000 seconds user
       0.003517000 seconds sys
```



```bash
$ perf stat -e L1-dcache-loads,L1-dcache-load-misses sleep 5

 Performance counter stats for 'sleep 5':

            254268      L1-dcache-loads                                             
              7994      L1-dcache-load-misses     #    3.14% of all L1-dcache accesses

       5.003847029 seconds time elapsed

       0.000000000 seconds user
       0.003517000 seconds sys
```


```bash
$ perf stat -e L1-dcache-loads,L1-dcache-load-misses cyclictest -t 1 -p 99 -i 1000 -D 10 -q
# /dev/cpu_dma_latency set to 0us
T: 0 ( 1934) P:99 I:1000 C:  10000 Min:     14 Act:   53 Avg:   55 Max:      90

 Performance counter stats for 'cyclictest -t 1 -p 99 -i 1000 -D 10 -q':

          19902246      L1-dcache-loads                                             
            435660      L1-dcache-load-misses     #    2.19% of all L1-dcache accesses

      10.063255753 seconds time elapsed

       0.001551000 seconds user
       0.409945000 seconds sys

#######
# WE REPEAT THE SAME TEST with a hot cache
$ perf stat -e L1-dcache-loads,L1-dcache-load-misses cyclictest -t 1 -p 99 -i 1000 -D 10 -q
# /dev/cpu_dma_latency set to 0us
T: 0 ( 1937) P:99 I:1000 C:  10000 Min:     11 Act:   14 Avg:   13 Max:      31

 Performance counter stats for 'cyclictest -t 1 -p 99 -i 1000 -D 10 -q':

          15702486      L1-dcache-loads                                             
            286504      L1-dcache-load-misses     #    1.82% of all L1-dcache accesses

      10.058175679 seconds time elapsed

       0.000000000 seconds user
       0.117904000 seconds sys
```



#### Generic tests with L2 cache
```bash
# Depending on the board, there may be performance counter which offer more information. 
# This particular board has only the following counters regarding the l2 cache:

# to check thoroughly for your available counters
$ perf list 

# then launch the test 

$ perf stat -e l2d_cache,l2d_cache_refill cyclictest -t 1 -p 99 -i 1000 -D 10 -q
# /dev/cpu_dma_latency set to 0us
T: 0 ( 1945) P:99 I:1000 C:  10000 Min:     11 Act:   13 Avg:   13 Max:      28

 Performance counter stats for 'cyclictest -t 1 -p 99 -i 1000 -D 10 -q':

           4403493      l2d_cache                                                   
             17033      l2d_cache_refill                                            

      10.058208585 seconds time elapsed

       0.014821000 seconds user
       0.103721000 seconds sys
       
       
# same test with hot cache
$ perf stat -e l2d_cache,l2d_cache_refill cyclictest -t 1 -p 99 -i 1000 -D 10 -q
# /dev/cpu_dma_latency set to 0us
T: 0 ( 1950) P:99 I:1000 C:  10000 Min:     11 Act:   14 Avg:   13 Max:      33

 Performance counter stats for 'cyclictest -t 1 -p 99 -i 1000 -D 10 -q':

           4333337      l2d_cache                                                   
             16710      l2d_cache_refill                                            

      10.057515254 seconds time elapsed

       0.016664000 seconds user
       0.099872000 seconds sys
```



### Generic test L1 and L2 not aggregated per CPU
```bash
$ perf stat -a -A -e L1-dcache-loads,L1-dcache-load-misses,l2d_cache,l2d_cache_refill sleep 5

 Performance counter stats for 'system wide':

CPU0                  3690726      L1-dcache-loads                                             
CPU1                   171059      L1-dcache-loads                                             
CPU2                   908098      L1-dcache-loads                                             
CPU3                   702486      L1-dcache-loads                                             
CPU0                   142704      L1-dcache-load-misses     #    3.87% of all L1-dcache accesses
CPU1                    11398      L1-dcache-load-misses     #    6.66% of all L1-dcache accesses
CPU2                    41446      L1-dcache-load-misses     #    4.56% of all L1-dcache accesses
CPU3                    45661      L1-dcache-load-misses     #    6.50% of all L1-dcache accesses
CPU0                   930711      l2d_cache                                                   
CPU1                    59592      l2d_cache                                                   
CPU2                   204341      l2d_cache                                                   
CPU3                   237738      l2d_cache                                                   
CPU0                    10362      l2d_cache_refill                                            
CPU1                     6991      l2d_cache_refill                                            
CPU2                    24138      l2d_cache_refill                                            
CPU3                    15338      l2d_cache_refill                                            

       5.003996796 seconds time elapsed
```


### Stress tests to check `cache refill`


