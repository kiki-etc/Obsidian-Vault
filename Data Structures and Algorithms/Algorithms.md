An algorithm is a precise sequence of finite steps/instructions for solving a problem or performing a computation. It takes some value or set of values as input, and produces some value, or set of values, as output. They are tools for solving a well-specified computational  
problem, which specifies the desired input/output relationship.

**Methods of expressing algorithms include:**
- Natural Language
- Flowcharts
- Pseudocode
- Programming language (requires expressing low-level details that are not necessary for a high-level understanding)

| Algorithms                                              | Programs                                                 |
| ------------------------------------------------------- | -------------------------------------------------------- |
| written during the design phase                         | written during the implementation phase                  |
| person who writes should have  <br>the domain knowledge | person who writes should have  <br>programming knowledge |
| can be written in any language                          | written using programming languages                      |
| independent of platform                                 | may depend on platform                                   |
| needs analysis                                          | needs testing                                            |

>[!example]- ***Example of a pseudocode algorithm:***
>```
>// finding the largest of a sequence of n numbers
>large(s,n)
>large := s1
>
>for i is 2 to n
>	if (s[n] > large)
>		large := s[n]
>
>return large
>```

| Priori Analysis                                                 | Posteriori Testing                                 |
| --------------------------------------------------------------- | -------------------------------------------------- |
| done on algorithms                                              | done on programs                                   |
| language and platform independent                               | language and platform dependent                    |
| result is a check on correctness, some space and time functions | results is a check on desired clock time and bytes |
**Properties of Algorithms**
- Input and Output
- Unambiguous steps
- Finiteness
- Independent of programming code
- Correctly solves problems
- Generality (should apply to a set of inputs)
- Effectiveness (must not have unnecessary steps)
### Formal Analysis of Algorithms
<br>To analyze an algorithm in a formal framework, we need a model of computation.

***Random Access Memory Model of computation*** (theoretical):
- A normal computer in which instructions are executed sequentially
- Performs standard simple instructions, such as addition, multiplication, comparison, and assignment takes exactly **one time unit** to do anything
- No (complex) operations such as matrix inversion or sorting
<br>Key components of formal analysis:
- Time complexity
- Space complexity

##### Time and Space Complexity
<br>Time complexity focuses on understanding the correlation between the execution's running time and the input size.
Space complexity refers to the amount of memory space an algorithm needs to solve a problem, expressed as a function of the input size.
Both the above are expressed using notations like O, Ω, Θ.

>[!note]-
>The most important resource to analyze is the running time
>Input to algorithm greatly affects running time
><br>Three types of running time:
>- Worst case
>- Best case
>- Average case
><br>If there is more than one input, the functions may have more than one argument, e.g., T(n,k)
>Worst-case performance represents a guarantee for performance on any possible input, and hence, it is of great interest

#### General Rules
<br>**Rule 1** - For Loops
The running time for a for-loop is at most the running time of the statements inside the loop (including tests) multiplied by the number of iterations

**Rule 2** - Nested For Loops
The running time for a nested for-loop is analyzed inside out. The total running time of a statement inside a group of nested loops is the running time of the statement multiplied by the product of the size of all the loops
<br>**Rule 3** - Consecutive statements
The times are simply added. The maximum time of all is the one that counts.
<br>**Rule 4** - If-Else statements
The running time of the test condition plus the ***larger*** of the running times of the *if* part and *else* part
<br>**Other rules**
If there are other function calls, they must be analyzed first.
Recursive function calls analysis depends on the way they are used. Sometimes **simple**, and sometimes need to solve a ***recurrence relation***

#### Order of Growth
<br>The running time of an algorithm is expressed considering only the leading term, ignoring the lower order terms and constant coefficient of the leading term. This is called the ***order of growth***. It is helpful for grouping functions of the same type.
<br>We consider one algorithm to be *more efficient* than another if its worst-case running time has a *lower order of growth*.

##### Asymptotic notation
Asymptotic notation is a formal way to denote the order of growth of the running time (time complexity) or efficiency of algorithms, for large input sizes. An algorithm that is asymptotically more efficient will be the best choice.
<br>Commonly used asymptotic notations :  
– O (f(n)) (Big-oh), Θ (f(n)) (Big-theta), Ω (f(n)) (Big-omega)  
– o(f(n)) (Small-oh), θ(f(n)) (Small-theta), ω(f(n)) (Small-omega)
<br>Asymptotic notation is also used to denote other aspects of algorithms, such as the amount of space, size of variables, etc.

>[!note] O - notation
>Used to represent ***asymptotically upper bound*** on running time
><br>Definition: For a given function f(n), we write $O(g(n))$ = ${ f(n) :$ there exist positive constants $c$, $n_0$ such that $0 ≤ f(n) ≤ c g(n)$ for all $n ≥ n_0 }$
> 
>>[!Example]+
>>$4n^2+2 = O(n^2)$ (for $c = 5$, and $n_0 = 2$) because for all values of $n$ greater than 2, the result of $4n^2+2$ stays between $0$ and $5 * n^2$

>[!note] Ω -Notation
>Used to represent ***asymptotically lower bound*** on running time
><br>Definition: For a given function f(n), we write $Ω(g(n))$ = ${ f(n) :$ there exist positive constants $c$, $n_0$ such that $0 ≤ c g(n) ≤ f(n)$ for all $n ≥ n_0 }$
>.
>>[!Example]+
>>$4n^2+2 = Ω(n^2)$ (for $c = 1$, and $n_0 = 1$) because for all values of $n$ greater than 1, the magnitude of $1 * n^2$ is always less than $4n^2+2$

>[!note] Θ -Notation
>Used to represent ***asymptotically tight bound*** on running time
><br>Definition: For a given function f(n), we write $Θ(g(n))$ = ${ f(n) :$ there exist positive constants $c_1$, $c_2$, $n_0$ such that $0 ≤ c_1 g(n) ≤ f(n) ≤ c_2 g(n)$ for all $n ≥ n_0 }$
>.
>>[!Example]+
>>$\frac{1}{2}n^2-3n = Θ(n^2)$ (for $c_1 = \frac{1}{14}$,  and $n_0 = 1$) because for all values of $n$ greater than 1, the magnitude of $1 * n^2$ is always less than $4n^2+2$






