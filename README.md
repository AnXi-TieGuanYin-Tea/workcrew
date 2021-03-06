<pre>
These source files have been created using files originally part of the last in a series of 
tutorials on writing POSIX pthreads by Daniel Robbins, then of the Gentoo project. It and the other two
were presented via the IBM Developer Works website (http://www.ibm.com/developerworks/library/l-posix3/). 
This last part in the series focused on pthread condition variables which are often not well-understood
by even the most frequent users of POSIX pthreads.

In learning this myself, I have simply modified the source of the "WORKCREW" project to provide
more documentation on what has been done as well as the addition of some run-time switches that will 
hopefully make understanding the code (and thus, phtreads (espeically pthread condition queues)) more easy.

BUILDING THE CODE

The makefile is a standard GNU Make makefile; to build the code, just enter make at a command prompt (note
that you will need the pthreads library installed before building the WORKCREW code):

    examples: 

    pthreads (package) must be installed first:
    rpm -qa | grep pthread
    mingw32-pthreads-2.8.0-2mdv2010.0
    libpthread-stubs-0.3-1mdv2010.1

    make
    gcc -c -g -O0   -c -o queue.o queue.c
    gcc -c -g -O0   -c -o workcrew.o workcrew.c
    gcc -c -g -O0   -c -o control.o control.c
    gcc queue.o workcrew.o control.o -o workcrew -lpthread

To cleanup the directory files, simply enter "make clean" at a command prompt:

    make clean
    rm -f *~ *.o workcrew typescript

WORKCREW syntax:

      parrms: -w num-workers - %d if not set\n", NUM_WORKERS 
              -p num-packets - %d if not set\n", NUM_PACKETS 
              -s sleep-secs  - %d if not set\n", SLEEP_SECS  
              -d             - no debug detail if not set\n" 

      examples: 

                workcrew

                workers(-w): 4, packets(-p): 1600, sleeptime(-s): 2, debugdetail(-d): off
                created 4 worker threads
                created & queued 1600 packets
                sleeping 2 seconds ...
                deactivating work queue...
                joined 4 worker threads...


                workcrew -w50 -p2000 -s3
                
                workers(-w): 50, packets(-p): 2000, sleeptime(-s): 3, debugdetail(-d): off
                created 50 worker threads
                created & queued 2000 packets
                sleeping 3 seconds ...
                deactivating work queue...
                joined 50 worker threads...


                workcrew -w500 -p3000 -s4 -d

                workers(-w): 500, packets(-p): 3000, sleeptime(-s): 4, debugdetail(-d): on

                ... too much output to list here ...

Note: You can run this under VALGRIND to note how memory is obtained and completely freed.

      examp;e:

      valgrind workcrew
      ==29896== Memcheck, a memory error detector
      ==29896== Copyright (C) 2002-2010, and GNU GPL'd, by Julian Seward et al.
      ==29896== Using Valgrind-3.6.1 and LibVEX; rerun with -h for copyright info
      ==29896== Command: workcrew
      ==29896== 
      
      workers(-w): 4, packets(-p): 1600, sleeptime(-s): 2, debugdetail(-d): off
      created 4 worker threads
      created & queued 1600 packets
      sleeping 2 seconds ...
      deactivating work queue...
      joined 4 worker threads...
      
      ==29896== 
      ==29896== HEAP SUMMARY:
      ==29896==     in use at exit: 0 bytes in 0 blocks
      ==29896==   total heap usage: 1,608 allocs, 1,608 frees, 26,784 bytes allocated
      ==29896== 
      ==29896== All heap blocks were freed -- no leaks are possible
      ==29896== 
      ==29896== For counts of detected and suppressed errors, rerun with: -v
      ==29896== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 4 from 4)

DEBUGGING

While debugging will hopefully not be necessary, you can use gdb to step through and learn the 
code as need be.

      examples:

      gdb workcrew
      GNU gdb (GDB) 7.2
      Copyright (C) 2010 Free Software Foundation, Inc.
      License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
      This is free software: you are free to change and redistribute it.
      There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
      and "show warranty" for details.
      This GDB was configured as "x86_64-unknown-linux-gnu".
      For bug reporting instructions, please see:
      <http://www.gnu.org/software/gdb/bugs/>...
      Reading symbols from /home/marty/myprojects/pthreads/workcrew/workcrew...done.
      (gdb) b main
      Breakpoint 1 at 0x40136d: file workcrew.c, line 260.
      (gdb) run -w200 -p3000 -s3
      Starting program: /home/marty/myprojects/pthreads/workcrew/workcrew -w200 -p3000 -s3
      [Thread debugging using libthread_db enabled]
      
      Breakpoint 1, main (argc=4, argv=0x7fffffffda98) at workcrew.c:260
      260        if ( getparams( argc, argv ) )
      (gdb)       
</pre>
