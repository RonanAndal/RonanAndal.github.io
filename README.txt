Ronan Russel Andal

Question #1
I counted the total system calls by using

wc -l tracefile_c tracefile_java

and the C program made 43 system calls, while the Java program made 3649.
The difference is because C produces a small native binary that directly
calls a few kernel functions (time, write, exit, etc) While Java starts up the JVM, which must load classes, libraries, initializes
garbage collection, threads, and JIT compilation. That startup work triggers thousands of extra system calls before the actual
System.out.println is actually executed

Quesiton #2

To count the number of different system calls, I extracted just the syscall names from the 
strace output, removed duplicates, and counted the result. The commands I used were

awk -F '(' '{print $1}' tracefile_c | awk '{print $NF}' | sort -u | wc -l
awk -F '(' '{print $1}' tracefile_java | awk '{print $NF}' | sort -u | wc -l

The C HelloWorld program used 15 different system calls, while the Java HelloWorld program used 107 different system calls.


Question #3

I extracted unique syscall names from my Java trace and then selected two per OS area:

awk -F '(' '{print $1}' tracefile_java | awk '{print $NF}' | sort -u > java_syscalls.txt
grep -E 'clone|execve|wait4|prctl|mmap|munmap|mprotect|brk|openat|read|write|close|fstat|access|geteuid|getuid|rt_sigaction|rt_sigprocmask' java_syscalls.txt

Process Management :
execve - starts the JVM process (loads the Java launcher).
clone - creates new threads used by the JVM

Memory management :
mmap - maps files / anonymous memory for code, heap, and libraries
mprotect - changes page protections, like RX for code, RW for data

File system & I/O :
openat - opens class files and shared libraries
read - reads their contents into memory. 

Protection & Security :
access - checks file permissions/existence
rt_sigaction - installs signal handlers, with rt_sigprocmask to mask signals.

I verified each syscall appears in java_syscalls.txt with grep as shown above

EXERCISE #2 : System Call Analysis

q1. How many system calls are placed in total?
grep -v -E 'resumed|\+\+\+ exited' norvig_outfile | wc -l
result : 4541

q2. How many system calls are placed by wget before it actually starts getting the file content?
first=$(awk -F'[(),=]' '/^[[:space:]]*read\(/ && $NF+0>0 {print NR; exit}' norvig_outfile)
awk -v n="$first" 'NR<n' norvig_outfile | grep -v -E 'resumed|\+\+\+ exited' | wc -l
result : 9

q3. How many bytes does wget attempt to read from the Web server at a time typically (e.g., the buffer size)? Justify your answer by giving in your report one line of the strace output as an example.
awk -F'[(),=]' '/^[[:space:]]*read\(/ {gsub(/ /,""); print $4}' norvig_outfile \
| sort | uniq -c | sort -nr | head
result : 8192 bytes

Example line: 
read(3, "The Project Gutenberg EBook of T"..., 8192) = 8192

q4. How many times does wget attempt to read pieces of file content, in total?
awk -F'[(),=]' '/^[[:space:]]*read\(/ && $NF+0>0 {print}' norvig_outfile | wc -l
result : 1139

q5. Out of these, how many times does wget NOT receive the number of bytes it wants?
sed 's/"[^"]*"/"BUF"/g' norvig_outfile \
| awk -F'[(),=]' '/^[[:space:]]*read\(/ {req=$4+0; ret=$NF+0; if(ret>0 && req>ret)c++} END{print c}'
result : 717

q6. Do you conclude that wget typically fills its buffer or not?
sed 's/"[^"]*"/"BUF"/g' norvig_outfile \
| awk -F'[(),=]' '/^[[:space:]]*read\(/ {req=$4+0; ret=$NF+0; if(ret>0){t++; if(req==ret)f++; else if(req>ret)s++;}} END{print "total_reads="t, "full_reads="f, "short_reads="s}'
result : total_reads=1139, full_reads = 400, short_reads=717
conclusion : No, most reads are short (717 vs 400 full).

q7. What is the port and IP address of the norvig server. Include the line that contains the system call as your justification.
Found by grepping for the connect() line with the AF_INET
connect(3, {sa_family=AF_INET, sin_port=htons(80), sin_addr=inet_addr("158.106.138.13")}, 16) = 0

q8. How many times does wget attempt to read pieces of file content, in total?
sed 's/"[^"]*"/"BUF"/g' uh_outfile \
| awk -F'[(),=]' '/^[[:space:]]*read\(/ {req=$4+0; ret=$NF+0; if(ret>0){t++; if(req==ret)f++; else if(req>ret)s++;}} END{print "total_reads="t, "full_reads="f, "short_reads="s}'
result: total_reads = 831

q9. Out of these, how many times does wget NOT receive the number of bytes it wants?
sed 's/"[^"]*"/"BUF"/g' uh_outfile \
| awk -F'[(),=]' '/^[[:space:]]*read\(/ {req=$4+0; ret=$NF+0; if(ret>0 && req>ret)c++} END{print c}'
result : 38

q10. Do you conclude that wget typically fills its buffer or not?
result : total_reads = 831, full_reads = 779, short_reads = 38
Conclusion: Yes, most reads are full (779 vs 38 short)
