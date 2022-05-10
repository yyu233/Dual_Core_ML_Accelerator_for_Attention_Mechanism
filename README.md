Part1. Single core synthesis & PnR (10pt)

     - Single core matrix multiplier is provided in "public/ece260_project" directory.

     - Note: currently the output of core is not connected yet. So, you need to connect based on your design, and then need to run synthesis. Otherwise, you will get zero power and zero delay.

     - You are allowed to change any part of verilog code, like SRAM size, FIFO depth, and others.

     - You are supposed to compute [Q0, Q1, ... ,Q7]T * [K0, K1, ..., K7], where Qi and Ki are vectors with 16 elements. (Note in pdf, figure was drawn by assuming 8 elements for simplicity.) Each element is 8bit integer.

     - Current code shows the estimated result and simulated results,

       e.g.,  [Q0K0, Q0K1, ..., Q0K7] @ cyc0,

                [Q1K0, Q1K1, ..., Q1K7] @ cyc1, 

                [Q2K0, Q2K1, ..., Q2K7] @ cyc1, ...

     - synthesize and run PnR to get the WNS, power, and area report by targeting 1GHz

     - It is okay not to meet the 0 WNS.

     - Input ports are from west (typo corrected. it was north before, but changed to west) whereas output ports are at south

 

Part2. Output normalization (30pt)

     - Your outputs are stored in the partial sum memory (pmem)

       e.g.,  [Q0K0, Q0K1, ..., Q0K7] @ add0,

                [Q1K0, Q1K1, ..., Q1K7] @ add1,

                [Q2K0, Q2K1, ..., Q2K7] @ add2, ...

     - Fetch one by one and compute the summation to normalize it, then write back to the memory

       e.g.,  [Q0K0, Q0K1, ..., Q0K7]  / sum(  abs(Q0K0), abs(Q0K1), ..., abs(Q0K7)  )

                [Q1K0, Q1K1, ..., Q1K7]  / sum(  abs(Q1K0), abs(Q1K1), ..., abs(Q1K7)  ) ...

     - Any method is fine, e.g., fifo, register, ...

 

Part3. Dual core implementation (40pt)

     - Ideally, you want to process 16 vectors,

       e.g.,  [Q0K0, Q0K1, ..., Q0K15] @ cyc0,

                [Q1K0, Q1K1, ..., Q1K15] @ cyc1, ...

     - But, your core only support up to 8 vectors.

     - Process in parallel with dual cores

     - But, the sum of vector elements should be merged for normalization,  

       e.g.,  sum(  abs(Q0K0), abs(Q0K1), ..., abs(Q0K7)  ) from core0

             + sum(  abs(Q0K8), abs(Q0K9), ..., abs(Q0K15)  ) from core1

     - Two cores need to exchange their sums, but note the core0 and core1 operate on separate CLK domains.

     - Any asynchronous protocol is fine if it is technically correct.

 

Part4. Optimization (bonus credit upto 10pt depending on your implementation. used to be 30pt, but we decided to assign 10pt after discussion)

     - Apply techniques for further boost performance while reducing power that you learned from this class

       e.g., pipeline, multi-cycle path, memory double-buffering, clock gating, loop unrolling, more # of cores, any other creative idea

     - A condition 1 GHz freq (if you already met it, push further) with IO_delay 0.2ns.

     - Async interface handling (synchronizer, hand shaking, ….)

     - Maximize parallelism between processing stages

     - Design your own controller

     - Better verification (with more input vector coverage)

     - Minimize WNS and DRC errors

     - Similar to your previous home works, try to achieve 0 WNS for setup time with Typ corner while Fast corner for hold time. 

     - You do not need to apply the optimization for all the steps, e.g., single core's optimized version + dual core's optimized version -> optimize only the final dual core version

     - Note: This project is not spec competition. Your logical approach and idea are more important

  

Part5. Report (20pt)

     - Prove the efficacy of your idea (why does it work? How does it work? How much performance/improvement do you see?)

     - Measure power and check functionality with VCD

       (SDF is not required, but it will be plus alpha)

     - Clear comparison before vs. after applying your technique

     - Clear description in the report regarding your design choice

       e.g., why you chose the decision, why you cut the pipeline boundary, ….

     - Your report should be no more than 4 pages (fewer is fine; not required to fill) with roughly these sections:

         - Introduction (roughly introduce what you're doing; can be simply one paragraph)

         - Sections for parts 1, 2, 3, and 4 + a performance analysis section (or subsection)

         - Conclusion

         - You can also add other sections you think necessary

     - Note: Report is VERY important
