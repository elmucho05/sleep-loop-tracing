The following directory will contain the output of what was generated with `perf` command on the imx8mp board.

Each output file contains the generated stats and the command options used to generate them.


# Generated results
```bash
perf list cache # to see the available performance counters for the board

perf stat -a -A -e L1-dcache-loads,L1-dcache-load-misses sleep 5


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




