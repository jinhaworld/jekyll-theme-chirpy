---
title: Asymptotic Analysis 
categories: [Computer Science, Complexity]
tags: [algorithms]     # TAG names should always be lowercase
--- 

This guide serves as a compact summary of notes on asymptotic analysis with emphasis on notation and a complete proof of the master theorem (CLRS does not provide a complete proof). Note that parts of the guide, particularly properties, theorems, certain techniques for the proof use CLRS as a reference. However, this guide includes a complete proof.  


> karatsuba 곱셈이랑 다른 분할 정복 알고리즘 몇 개 예제로 넣을까 했는데 그냥 마스터 정리 증명까지만 했다. 미래에 코드 적을 때 시간 초과 조심하자. 

Time complexity is arguably one of the key fundamental aspects of computer science. For instance, in cp, it is often necessary to evaluate whether the code would run within a certain timeframe. Although most servers can handle at max about $c \cdot 10^8$ operations per second, some barely can handle even $10^9$ operations under 2 seconds. It would be useful to be able to vaguely have a sense of whether your code is optimal enough to pass. 

> Note that the proof does not use induction. 그냥 부등식, 빅-$O$ 와 다른 표기법 정의들로 증명함. 가만할 점은 $b/n$이 정수가 되는 보장이 없어서, (물론 $b$ 가 $n$의 제곱이면 쉽지만), 케이스별로 나누어야 한다, 즉 ($\lfloor b/n \rfloor, \lceil b/n \rceil$). 그 과정에서 비슷한 증명들이 조건만 달라지고 번복된다. 어쩔수 없으니 이해해주시고 읽기를 바래요. 

# Notation 

## $O$ 

If $f(n) = O(g(n))$, then $\exists c, n_0$ such that $\forall n_0 \leq n$, $0 \leq f(n) \leq cg(n)$ 

Intuitively, the $O$-notation notes an upper bound (although it is sometimes informally used instead of $\Theta$ in cp). Furthermore, note that $O(g(n))$ is actually a set of all $f(n)$ that satisfies the condition above. In other words, the statement $f(n) = O(g(n))$ is equivalent to $f(n) \in O(g(n))$. As such, $O(n) = O(n^2)$ while $ O(n^2) \neq O(n)$, as $O(n) \subset O(n^2)$. This is because n is defined to be arbitrarly large, so $c < n$ and $c \cdot n < n^2$. 

If we state that the runtime of a code is $O(n)$, then for the size of the input, function $g(n) \in O(n)$ is greater than the runtime for that input. Operations that take constant time such as accessing an element in an array is $O(1)$. 

## $\Omega$ 

If $f(n) = \Omega(g(n))$, then $\exists c, n_0$ such that $\forall n_0 \leq n$, $0 \leq cg(n) \leq f(n)$ 

$\Omega$-notation is similar to the $O$-notation except that it notes a lower bound. 

## $\Theta$ 

If $f(n) = \Theta(g(n))$, then $\exists c_1, c_2, n_0$ such that  $\forall n_0 \leq n, 0 \leq c_1g(n) \leq f(n) \leq c_2g(n)$. 

In other words, if $f(n) = \Theta(g(n))$, then $f(n) = O(g(n))$ and $f(n) = \Omega(g(n))$. 

## $o$ 

If $f(n) = o(g(n))$, then $\forall c > 0$, $\exists n_0 > 0$ such that $0 \leq f(n) < cg(n)$ $\forall n_0 \leq n$. 

$o(g(n))$ is a set of all $f(n)$ such that the condition above is met for any c. Note the difference between the definition of $o$-notation and $O$-notation. $3n \neq o(n)$. Also note that $f(n)$ is lesser than $c(g(n))$ not lesser than or equal to (as for $O(g(n)$). 

## $\omega$ 

If $f(n) = \omega(g(n))$, then $\forall c > 0$, $\exists n_0 > 0$ such that $0 \leq cg(n) < f(n)$ $\forall n_0 \leq n$. 

$\omega(g(n))$ is the set of all $f(n)$ such that the condition above is met for any c. Note the difference between the definition of $\omega$-notation and $\Omega$-notation. Additionally, $f(n)$ is greater than $cg(n)$ not greater than or equal to $cg(n)$ (as for $\Omega(g(n))$. 

# Properties 

### Reflexivity 
$f(n) = \Theta(f(n))$ 

$f(n) = O(f(n))$ 

$f(n) = \Omega(f(n))$ 

Note that $o$ and $\omega$ are not included in the properties for reflexivity, as by definition, 
$f(n) \neq o(f(n))$ and $f(n) \neq \omega(f(n))$. 

### Transitivity 
$f(n) = \Theta(g(n)) \textrm{ and } g(n) = \Theta(h(n)) \implies f(n) = \Theta(h(n))$ 
$f(n) = O(g(n)) \textrm{ and } g(n) = O(h(n)) \implies f(n) = O(h(n))$ 
$f(n) = \Omega(g(n)) \textrm{ and } g(n) = \Omega(h(n)) \implies f(n) = \Omega(h(n))$ 
$f(n) = o(g(n)) \textrm{ and } g(n) = o(h(n)) \implies f(n) = o(h(n))$ 
$f(n) = \omega(g(n)) \textrm{ and } g(n) = \omega(h(n)) \implies f(n) = \omega(h(n))$ 

The above properties can be proven by setting inequalities and using the above definitions. 

### Symmetry 
$f(n) = \Theta(g(n))$ iff $g(n) = \Theta(f(n))$. 

### Transpose Symmetry 
$f(n) = O(g(n))$ iff $g(n) = \Omega(f(n))$. 

$f(n) = o(g(n))$ iff $g(n) = \omega(f(n))$. 

These are again straightforward properties derived from the definitions themselves (looking at the two functions $f(n), g(n)$ in the inequality from the perspective of $f(n)$ and from $g(n)$). 

# Equations with Asymptotic Notation 

Equations may include notation(s) as in $\Theta(n^2) + 3n^2 + 3n + 1 = \Theta(n^2)$. This equation is stating that $\forall f(n) \in \Theta(n^2)$ $\exists g(n) \in \Theta(n^2)$ such that $f(n) + 3n^2 + 3n + 1 = g(n)$. Now, try verifying that $\Omega(n^2) + 3n^2 + 3n + 1 = \Theta(n^2)$. 

Does $\omega(n^2) + 3n + 1 = \Theta(n^2)$ hold true? If not, why? 

# Master Theorem 

For recurrence $T(n) = aT(n/b) + f(n)$ for function $f(n)$,$1 \leq a$ and $ 1 < b$, where $n/b$ is either $\lceil n/b \rceil$ or $\lfloor n/b \rfloor$ (for $n/b$ to be an integer), 

1. If $f(n) = O(n^{\log_b {a - \epsilon}})$ for some $\epsilon > 0$, then $T(n) = \Theta(n^{\log_{b} a})$. 

2. If $f(n) = \Theta(n^{\log_{b} a})$ then $T(n) = \Theta(n^{\log_{b} a}\log n)$. 

3. If $f(n) = \Omega(n^{\log_{b} a + \epsilon})$ for some $\epsilon > 0$, then if $af(n/b) \leq cf(n)$ for some constant $c < 1$ and all sufficiently large $n$, then $T(n) = \Theta(f(n))$. 

> Note that T(n) must be monotone while f(n) must be a polynomial. 

There are various methods of calculating the asymptotic time complexity of recursive algorithms, yet given that the function falls under the three categories, the Master Theorem is the most efficient. If it does not fall under any of the three, we must use the substitution method of choosing a reasonable approximation and proving that it is correct. Additionally, we can attempt to use a recursive tree, which is, of course, more laborious. 

# Proof of Master Theorem 
Most books, including CLRS, will often first consider the specific case of when $n = b^k$ for some integer $k$. If n is a perfect power of b, the proof of Master Theorem becomes much simpler as every $n/b$ will be an integer. However, here we will exclude that proof as it is unnecessary and instead will provide a complete proof for both when the recurrence relation is $T(n) = aT(\lceil n/b \rceil) + f(n)$ and $T(n) = aT(\lfloor n/b \rfloor) + f(n)$. 

When I read through CLRS, my first impression of its proof was that the proof was incomplete. It only considered the case of when $T(n) = aT(\lceil n/b \rceil) + f(n)$ and proved its upper bound, not its lower bound. It also did not prove the upper & lower bound of $T(n) = aT(\lfloor n/b \rfloor) + f(n)$. 

### $T(n) = aT(\lceil n/b \rceil) + f(n)$ Upper Bound 
Using a recursion tree, we can note that when $n$ is a power of $b$, is $T(n) = \sum\limits_{j=0}^{log_b n -1} a^jf(n/b^j) + \Theta(n^{\log_b a})$. This is simply because the height of the tree is $\log_b n$, and for each depth $x$, there are $a^j$ branches of $f(n/b^j)$ ($T(n) = \Theta(1)$ by definition). Now, the problem is when $n$ is not a power of $b$. We need to calculate the height of the tree. 

By definition, $\lceil n/b \rceil \leq n/b + 1$. Then it follows that $\lceil\lceil n/b \rceil/b \rceil \leq n/b^2 + 1/b + 1$. If we denote $n_0 = n$, $\lceil n/b \rceil = n_1$ and the next as $n_2$ and so on, we can deduce that

$$n_j \leq n/b^j + \sum\limits_{x=0}^{j -1} 1/b^i < n/b^j + \sum\limits_{x=0}^{\infty} 1/b^i < n/b^j + \frac{b}{b-1}$$ 

For $\lfloor log_b n \rfloor$, 
$$n_{\lfloor log_b n \rfloor} < b + \frac{b}{b-1}$$ 

As $b$ is a constant, $n_{\lfloor log_b n \rfloor} = O(1)$. Effectively, we can now consider the height of the tree to be $\lfloor log_b n \rfloor$. 

Thus, our original equation for $T(n)$ can be modified to $T(n) = \sum\limits_{j=0}^{\lfloor log_b n - 1 \rfloor} a^jf(n_j) + \Theta(n^{\log_b a})$ where $\Theta(n^{\log_b a})$ still remains unchanged. 

Lastly, let $g(n) = \sum\limits_{j=0}^{\lfloor log_b n - 1 \rfloor} a^jf(n_j)$. Now, we will prove for the upper bound for each of the three cases listed in the master theorem. 

#### Case 1 

Let us prove that $f(n_j) = O(\frac{(b^{\epsilon})^j}{a^j}n^{log_b a - \epsilon})$ for some constant $\epsilon > 0$. As $f(n) = O(n^{\log_b a - \epsilon})$, for all sufficiently large $n_j$, 

$f(n_j) \leq c(\frac{n}{b^j} + \frac{b}{b-1})^{\log_b a - \epsilon} \leq c(\frac{n}{b^j}(1 + \frac{b^j}{n} \cdot \frac{b}{b-1}))^{\log_b a - \epsilon}$ 

$f(n_j) \leq c(\frac{n^{\log_b a - \epsilon}}{(a/b^{\epsilon})^j})(1 + \frac{b^j}{n} \cdot \frac{b}{b-1})^{\log_b a - \epsilon} \leq c(\frac{n^{\log_b a - \epsilon}}{(a/b^{\epsilon})^j})(1 + \frac{b}{b-1})^{\log_b a - \epsilon}$ 

$ \implies f(n_j) = O(\frac{(b^{\epsilon})^j}{a^j} n^{\log_b a - \epsilon})$ 

as $1 + \frac{b}{b-1}$ is a constant and $\frac{1}{(a/b^{\epsilon})^j} = \frac{(b^{\epsilon})^j}{a^j}$. 


Therefore, $g(n) = \sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} a^x (n)^{\log_b a - \epsilon} = O(\sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} a^x (n/b^x)^{\log_b a - \epsilon} )$. 

$ = O((n)^{\log_b a - \epsilon} \sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} (\frac{ab^{\epsilon}}{b^{\log_b a}}))^j$

$ = O((n)^{\log_b a - \epsilon} \sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} (b^{\epsilon})^j$

$ = O((n)^{\log_b a - \epsilon} \frac{(b^{\epsilon \log_b n} -1}{b^{\epsilon}-1}$ 

$ = O(((n)^{\log_b a - \epsilon}) (\frac{n^{\epsilon} - 1}{b^{\epsilon} - 1})) = O(n^{\log_b a})$. 

Lastly, 

$T(n) = \Theta(n^{\log_b a}) + O(n^{\log_b a}) = \Theta(n^ {\log_b a})$.  

#### Case 2 

We will prove that $f(n_j) = O(n^{log_b a})$. As $f(n) = O(n^{\log_b a})$, for all sufficiently large $n_j$, 

$f(n_j) \leq c(\frac{n}{b^j} + \frac{b}{b-1})^{\log_b a} \leq c(\frac{n}{b^j}(1 + \frac{b^j}{n} \cdot \frac{b}{b-1}))^{\log_b a }$ 

$f(n_j) \leq c(\frac{n^{log_b a}}{a^j}\frac{b}{b-1}))^{\log_b a}$. 


This implies that $f(n_j) = O(\frac{n^{\log_b a}}{a^j})$ as $c \cdot (1 + \frac{b}{b-1})^{\log_b a}$ is a constant. $f(n_j) = O((n/b^j)^{\log_b a})$. 

Therefore, $g(n) =\sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} a^x f(n_x) = O(\sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} a^x (\frac{n}{b^j})^{\log_b a})$. 

$O(\sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} a^x (\frac{n}{b^x})^{\log_b a}) = O(n^{\log_b a} \sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} 1) = O(n^{\log_b a}\log_b n)$. 

Lastly, 

$T(n) = \Theta(n^{\log_b a}) + O(n^{\log_b a}\log_b n) = O(n^{\log_b a}\log_b n)$. 


#### Case 3 

 $af(\lceil n/b \rceil) \leq cf(n)$ for $b + b/(b-1) < n \implies a^jf(n_j) \leq c^jf(n)$. 

 Thus, $g(n) = \sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} a^x f(n_x) \leq f(n) \sum\limits_{x=0}^{\infty} c^x + O(1) \leq f(n)(\frac{1}{1-c}) + O(1) = O(f(n))$. 

 Furthermore, since no $f(n_j)$ is negative, and $g(n)$ includes $f(n_0)$ or $f(n)$, $g(n) = \Omega(f(n))$. 

 Thus, $g(n) = \Theta(f(n))$. 

Lastly, $T(n) = \Theta(n^{\log_b a}) + \Theta(f(n)) = \Theta(f(n))$ as
it is given as a condition that $f(n) = \Omega(n^{\log_b a + \epsilon})$. 


### $T(n) = aT(\lceil n/b \rceil) + f(n)$ Lower Bound  

#### Case 1

The proof above for case 1) suffice, as only an upper bound is necessary to prove case 1, specifically, $T(n) = \Theta(n^{\log_b a}) + O(n^{\log_b a}) = \Theta(n^ {\log_b a})$.

#### Case 2

We will prove that $g(n) = \Omega(n^{\log_b a}log_b n)$. 

As $f(n) = \Omega(n^{\log_b a})$, given as a condition stated in the theorem, for all sufficiently large $n_j$, 

$f(n_j) \geq c(\frac{n}{b^j} + \frac{b}{b-1})^{\log_b a} \geq c(\frac{n}{b^j}(1 + \frac{b^j}{n} \cdot \frac{b}{b-1}))^{\log_b a }$ 

$f(n_j) \geq c(\frac{n^{log_b a}}{a^j}\frac{b}{b-1}))^{\log_b a}$. 


This implies that $f(n_j) = \Omega(\frac{n^{\log_b a}}{a^j})$ as $c \cdot (1 + \frac{b}{b-1})^{\log_b a}$ is a constant. $f(n_j) = \Omega((n/b^j)^{\log_b a})$. 

Therefore, $g(n) =\sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} a^x f(n_x) = \Omega(\sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} a^x (\frac{n}{b^j})^{\log_b a})$. 

$\Omega(\sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} a^x (\frac{n}{b^x})^{\log_b a}) = \Omega(n^{\log_b a} \sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} 1) = \Omega(n^{\log_b a}\log_b n)$. 

Lastly, 

$T(n) = \Theta(n^{\log_b a}) + \Omega(n^{\log_b a}\log_b n) = \Omega(n^{\log_b a})$. 

Thus, we have proven both that $T(n) = \Omega(n^{\log_b a})$ and $T(n) = O(n^{\log_b a})$

$\implies T(n) = \Theta(n^{\log_b a})$.

#### Case 3 

In the proof above for case 3, we already proved that $g(n) = \Theta(f(n))$, thus, the lower bound of $T(n) = g(n) + \Theta(n^{\log_b a})$ which is $\Theta(f(n))$, as it is given as a condition that $f(n) = \Omega(n^{\log_b a + \epsilon})$. 

### $T(n) = aT(\lfloor n/b \rfloor) + f(n)$ Lower Bound 

By definition, $\lfloor n/b \rfloor \geq n/b - 1$. Then it follows that $\lfloor\lfloor n/b \rfloor/b \rfloor \leq n/b^2 - 1/b - 1$. If we denote $n_0 = n$, $\lfloor n/b \rfloor = n_1$ and the next as $n_2$ and so on, we can deduce that

$$n_j \leq n/b^j - \sum\limits_{x=0}^{j -1} 1/b^i < n/b^j + \sum\limits_{x=0}^{\infty} 1/b^i < n/b^j + \frac{b}{b-1}$$ 

For $\lfloor log_b n \rfloor$, 
$n_{\lfloor log_b n \rfloor} < b + \frac{b}{b-1}$.

As $b$ is a constant, $n_{\lfloor log_b n \rfloor} = O(1)$. Effectively, we can now consider the height of the tree to be $\lfloor log_b n \rfloor$. 

Thus, our original equation for $T(n)$ can be modified to $T(n) = \sum\limits_{j=0}^{\lfloor log_b n - 1 \rfloor} a^jf(n_j) + \Theta(n^{\log_b a})$ where $\Theta(n^{\log_b a})$ still remains unchanged. 

Lastly, let $g(n) = \sum\limits_{j=0}^{\lfloor log_b n - 1 \rfloor} a^jf(n_j)$. Now, we will prove each of the three cases listed in the master theorem. 

#### Case 1 

Let us prove that $f(n_j) = O(\frac{(b^{\epsilon})^j}{a^j}n^{log_b a - \epsilon})$ for some constant $\epsilon > 0$. As $f(n) = O(n^{\log_b a - \epsilon})$, for all sufficiently large $n_j$, 

$f(n_j) \leq c(\frac{n}{b^j} + \frac{b}{b-1})^{\log_b a - \epsilon} \leq c(\frac{n}{b^j}(1 + \frac{b^j}{n} \cdot \frac{b}{b-1}))^{\log_b a - \epsilon}$ 

$f(n_j) \leq c(\frac{n^{\log_b a - \epsilon}}{(a/b^{\epsilon})^j})(1 + \frac{b^j}{n} \cdot \frac{b}{b-1})^{\log_b a - \epsilon} \leq c(\frac{n^{\log_b a - \epsilon}}{(a/b^{\epsilon})^j})(1 + \frac{b}{b-1})^{\log_b a - \epsilon}$ 

$ \implies f(n_j) = O(\frac{(b^{\epsilon})^j}{a^j} n^{\log_b a - \epsilon})$ 

as $1 + \frac{b}{b-1}$ is a constant and $\frac{1}{(a/b^{\epsilon})^j} = \frac{(b^{\epsilon})^j}{a^j}$. 


Therefore, $g(n) = \sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} a^x (n)^{\log_b a - \epsilon} = O(\sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} a^x (n/b^x)^{\log_b a - \epsilon} )$. 

$ = O((n)^{\log_b a - \epsilon} \sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} (\frac{ab^{\epsilon}}{b^{\log_b a}}))^j$

$ = O((n)^{\log_b a - \epsilon} \sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} (b^{\epsilon})^j$

$ = O((n)^{\log_b a - \epsilon} \frac{(b^{\epsilon \log_b n} -1}{b^{\epsilon}-1}$ 

$ = O(((n)^{\log_b a - \epsilon}) (\frac{n^{\epsilon} - 1}{b^{\epsilon} - 1})) = O(n^{\log_b a})$. 

Lastly, 

$T(n) = \Theta(n^{\log_b a}) + O(n^{\log_b a}) = \Theta(n^ {\log_b a})$.  

#### Case 2 

Let us prove that $f(n_j) = O(n^{log_b a})$. As $f(n) = O(n^{\log_b a})$, for all sufficiently large $n_j$, 

$f(n_j) \leq c(\frac{n}{b^j} + \frac{b}{b-1})^{\log_b a} \leq c(\frac{n}{b^j}(1 + \frac{b^j}{n} \cdot \frac{b}{b-1}))^{\log_b a }$ 

$f(n_j) \leq c(\frac{n^{log_b a}}{a^j}\frac{b}{b-1}))^{\log_b a}$. 


This implies that $f(n_j) = O(\frac{n^{\log_b a}}{a^j})$ as $c \cdot (1 + \frac{b}{b-1})^{\log_b a}$ is a constant. $f(n_j) = O((n/b^j)^{\log_b a})$. 

Therefore, $g(n) =\sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} a^x f(n_x) = O(\sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} a^x (\frac{n}{b^j})^{\log_b a})$. 

$O(\sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} a^x (\frac{n}{b^x})^{\log_b a}) = O(n^{\log_b a} \sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} 1) = O(n^{\log_b a}\log_b n)$. 

Lastly, 

$T(n) = \Theta(n^{\log_b a}) + O(n^{\log_b a}\log_b n) = O(n^{\log_b a}\log_b n)$. 

#### Case 3  

 $af(\lceil n/b \rceil) \leq cf(n)$ for $b + b/(b-1) < n \implies a^jf(n_j) \leq c^jf(n)$. 

 Thus, $g(n) = \sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} a^x f(n_x) \leq f(n) \sum\limits_{x=0}^{\infty} c^x + O(1) \leq f(n)(\frac{1}{1-c}) + O(1) = O(f(n))$. 

 Furthermore, since no $f(n_j)$ is negative, and $g(n)$ includes $f(n_0)$ or $f(n)$, $g(n) = \Omega(f(n))$. 

 Thus, $g(n) = \Theta(f(n))$. 

Lastly, $T(n) = \Theta(n^{\log_b a}) + \Theta(f(n)) = \Theta(f(n))$ as
it is given as a condition that $f(n) = \Omega(n^{\log_b a + \epsilon})$. 

### $T(n) = aT(\lfloor n/b \rfloor) + f(n)$ Upper Bound 

#### Case 1

The proof above for case 1) suffice, as only an upper bound is necessary to prove case 1, specifically, $T(n) = \Theta(n^{\log_b a}) + O(n^{\log_b a}) = \Theta(n^ {\log_b a})$.

#### Case 2 

We will prove that $g(n) = \Omega(n^{\log_b a}log_b n)$.  

As $f(n) = \Omega(n^{\log_b a})$, given as a condition stated in the theorem, for all sufficiently large $n_j$, 

$f(n_j) \geq c(\frac{n}{b^j} + \frac{b}{b-1})^{\log_b a} \geq c(\frac{n}{b^j}(1 + \frac{b^j}{n} \cdot \frac{b}{b-1}))^{\log_b a }$ 

$f(n_j) \geq c(\frac{n^{log_b a}}{a^j}\frac{b}{b-1}))^{\log_b a}$. 


This implies that $f(n_j) = \Omega(\frac{n^{\log_b a}}{a^j})$ as $c \cdot (1 + \frac{b}{b-1})^{\log_b a}$ is a constant. $f(n_j) = \Omega((n/b^j)^{\log_b a})$. 

Therefore, $g(n) =\sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} a^x f(n_x) = \Omega(\sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} a^x (\frac{n}{b^j})^{\log_b a})$. 

$\Omega(\sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} a^x (\frac{n}{b^x})^{\log_b a}) = \Omega(n^{\log_b a} \sum\limits_{x=0}^{\lfloor log_b n - 1 \rfloor} 1) = \Omega(n^{\log_b a}\log_b n)$. 

Lastly, 

$T(n) = \Theta(n^{\log_b a}) + \Omega(n^{\log_b a}\log_b n) = \Omega(n^{\log_b a})$. 

Thus, we have proven both that $T(n) = \Omega(n^{\log_b a})$ and $T(n) = O(n^{\log_b a})$

$\implies T(n) = \Theta(n^{\log_b a})$.


#### Case 3 

In the proof above for case 3, we already proved that $g(n) = \Theta(f(n))$, thus, the lower bound of $T(n) = g(n) + \Theta(n^{\log_b a})$ which is $\Theta(f(n))$, as it is given as a condition that $f(n) = \Omega(n^{\log_b a + \epsilon})$. 

# Final Words

This is the end of the guide. The master theorem is used extensively for recursive algorithms and the notation defined above are used as standard. Try using the master theorem to find the asymptotic complexity of karatsuba multiplication or matrix multiplication. 