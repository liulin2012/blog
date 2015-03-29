title: The prime contest
date: 2015-02-17 01:37:34
categories:
- Algorithm
tags:
- Algorithm
- C++
- multithread
---
##Overview
sum the primes from a to b, inclusive, a < b <= $10^{14}$.
Measure how long it takes your algorithm to sum the primes from 1 to $10^9$.
Determine the complexity of your algorithm and calculate how long it would take to sum for a=1, b = $10^{12}$.

##Algorithm
I have used the [Sieve_of_Eratosthenes ](http://en.wikipedia.org/wiki/Sieve_of_Eratosthenes)algorithm to delete the primes from the vector.
<!--more-->

##Multi-thread
I have used the multithread technology to speed up the program.I have test some data.You can change the thread number to check the answer and find the best thread number in this experiment.

Data|Thread number|Execution time
:----:|:-------------:|:-------------:
1~$10^9$|1000|0.6 Sec
1~$10^{10}$|5000|6.8 Sec
1~$10^{11}$|10000|90 Sec

![](http://i.imgur.com/f8dSURel.png)

![](http://i.imgur.com/0NN7rXal.png)

![](http://i.imgur.com/oNFQTwTl.png)

##Compiler optimize

	clang++ -std=c++11  -O2 main.cpp
I have use the `-O2` to optimize the compiler and then
		
	./a.out
This trick can [Maximize Speed](https://msdn.microsoft.com/en-us/library/8f8h5cxt.aspx) when compile the C++ code.

##Analyze
I have research on on the internet
>The bit complexity of the algorithm is O(n (log n) (log log n)) bit operations with a memory requirement of O(n).

My result show above when I test the data $10^{9}$,$10^{10}$,$10^{11}$.Due to the algorithm complexity,I calculate the $10^{12}$ need to use 17 minute.

##Final
If you have any question,be free to contact me.Thank you!

* Author:lin
* Email:liulin.jacob@gmail.com
* Code Address:[The prime contest](https://github.com/liulin2012/CPE593/blob/master/HW1b/main.cpp)



