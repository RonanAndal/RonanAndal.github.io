Homework Assignment #6
Ronan Russel Andal

Question 1.1

T   CPU  NIC  Disk
00  A    I    I        
01  A    I    I                  
02  A    I    I                  
03  A    I    I                  
04  B    I    A        
05  B    I    A                
06  B    I    A                
07  B    I    I                
08  B    I    I                
09  C    B    I        
10  C    B    I                  
11  C    I    I                  
12  D    C    I        
13  D    C    I                
14  A    C    D        
15  A    C    D                
16  A    C    I                
17  A    I    I                
18  B    I    A        
19  B    I    A                
20  B    I    A                
21  B    I    I                
22  B    I    I                
23  D    B    I        
24  D    B    I                
25  C    I    D        
26  C    I    D       
27  C    I    I       
28  A    C    I       
29  A    C    I       
30  A    C    I       
31  A    C    I        
32  B    C    A        
33  B    I    A
34  B    I    A

Question 1.2

T   CPU  NIC  Disk
00  A    I    I
01  A    I    I
02  A    I    I
03  A    I    I
04  D    I    A
05  D    I    A
06  C    I    A
07  C    I    D
08  C    I    D
09  D    C    I
10  D    C    I
11  A    C    D
12  A    C    D
13  A    C    I
14  A    I    I
15  D    I    A
16  D    I    A
17  C    I    A
18  C    I    D
19  C    I    D
20  D    C    I
21  D    C    I
22  A    C    D
23  A    C    D
24  A    C    I
25  A    I    I
26  D    I    A
27  D    I    A
28  C    I    A
29  C    I    D
30  C    I    D
31  D    C    I
32  D    C    I
33  A    C    D
34  A    C    D
         
Question 1.3

T   CPU  NIC  Disk
00  A    I    I        
01  A    I    I        
02  A    I    I                
03  A    I    I

04  B    I    A                 
05  B    I    A                 
06  B    I    A        
07  B    I    I 

08  C    I    I        
09  C    I    I        
10  C    I    I 

11  D    C    I                
12  D    C    I  

13  A    C    D                
14  A    C    D                
15  A    C    I        
16  A    I    I

17  B    I    A 

18  D    B    A        
19  D    B    A 

20  C    I    D        
21  C    I    D        
22  C    I    I

23  A    C    I        
24  A    C    I        
25  A    C    I        
26  A    C    I  

27  B    C    A        
28  B    I    A                
29  B    I    A                
30  B    I    I

31  D    I    I        
32  D    I    I        
33  C    I    D

34  C    I    D


Question 2.1

CPU : AAAABCCDDDDDAAAABCCAAAABDDAAAACCBDD
NIC : IIIIIBBCCCCCCCIIIBBCCCCCCCBBIIIICCC
Disk: IIIIAAAIIIIIIIIIAAAIIIIAAAIIIIAAAII

t   CPU  NIC  Disk
00  A    I    I        
01  A    I    I        
02  A    I    I        
03  A    I    I   
04  B    I    A        
05  C    B    A        
06  C    B    A 
07  D    C    I        
08  D    C    I                
09  D    C    I                
10  D    C    I                
11  D    C    I 
12  A    C    I                
13  A    C    I                
14  A    I    I                
15  A    I    I 
16  B    I    A
17  C    B    A        
18  C    B    A
19  A    C    I        
20  A    C    I
21  A    C    I    
22  A    C    I        
23  B    C    A
24  D    C    A         
25  D    C    A                 
26  A    B    I                
27  A    B    I                 
28  A    I    I    
29  A    I    I                 
30  C    I    A                 
31  C    I    A                 
32  B    C    A 
33  A    C    I 
34  A    C    I     

Question 3.1

Under the STCF algorithm. Job B arrives at t=2 and has only 1 CPU tick, while Job A still has
2 ticks left at that moment. Therefore, when B arrives, the scheduler should preempt A and run B first.

However, in the given CPU timeline (AAAABCCCBAAAA), A keeps running at t=2-3,
and B doen't start until t=4. This violates STCF because A should have been interrupted when B arrived. As a result, the I/O sequences are also shifted:
A's NIC activity begins too early (NIC: IIIIAAAAIIIII), and B's Disk I/O occurs later than it should. (Disk: IIIIIBBIIBBII).