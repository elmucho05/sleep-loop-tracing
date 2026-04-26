# sleep-loop-tracing
This repo contains the kernel traces generated from trace-cmd in the imx8mp board. Traces are recorded while cyclictest is executing in order to identify a wake up pattern of cyclictest amongst the kernel functions. 


The traces have been divided into two directories to distinguish between the traces generated when there was no external interference from a *hmi* ([no interference](./no-interference)) and the case where there was some [interference](./interference).

All traces have been recorded from within an `imx8mp` board which runs a custom Linux Kernel version 6.6.23 with the PREEMPT_RT patch applied.








