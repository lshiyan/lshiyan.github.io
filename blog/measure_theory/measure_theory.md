---
layout: default
title: Introduction to Measure Theory
---

# Introduction to Measure Theory

This one comes with a funny story behind it. I was originally enrolled in the Math and Physics Program at McGill for my undergraduate, but I decided to switch into Computer Science in my 4th year. This tacked on an extra few years to my degree, including an internship, so naturally coming into my Master's I hadn't done any pure math in around 3 years. However, I decided that going into the AI space necessitated a foundation in probability, a branch of math that I wasn't really too familiar with from my previous courses.

For some reason, I also had a notion that probability was some sort of 'fake' math. Maybe it was because my last real exposure to pure probability was all the way back in high school, (6 years ago now almost 👴), where it was still firmly in the realm of 'What's the chances that flipping 2 coins gives you 2 heads?' This misguided notion, and a healthy bit of ego on my end, made me choose to take a graduate level probability course, MATH 587, in my first semester of my Computer Science Master's, keeping in mind my hiatus from any math courses since my degree switch. This choice could only be described as an unforced error on my part; however, it has led me to learn some very interesting concepts and ways to think about the field that I would never have been exposed to otherwise. I lay some of them out here.

## Sets and Sigma-Algebras

The foundation behind 'real' probability is a field of pure maths called measure theory. Measures are in some sense how probabilities are defined, but we won't talk about them just yet. First we need to get the boring stuff out of the way, sets and sigma-algebras.

I will assume some familiarity with basic set theory for the audience. I'll represent all sets using the notation $\Omega$. 

The power set of $\Omega$, which we will denote $2^{\Omega}$, is the set of all subsets of $\Omega$. For example, if $\Omega = \{1,2,3\}$, then:


For $\Omega = \{1,2,3\}$, the power set is:

$$
2^{\Omega} = \left\{
\emptyset, \{1\}, \{2\}, \{3\},
\{1,2\}, \{1,3\}, \{2,3\}, \{1,2,3\}
\right\}
$$

We denote it by $2^{\Omega}$ because the number of elements, or cardinality, of the power set is exactly $2^{\lvert{\Omega}\rvert}$, where $\lvert{\Omega}\rvert$ is the cardinality of $\Omega$.

A sigma-algebra ($\sigma$-algebra), denoted by $\mathcal{F}$, is a subset of $2^{\Omega}$ that satisfies the following properties:

1. $\Omega \in \mathcal{F}$
2. If $A \in \mathcal{F}$, then $A^c \in \mathcal{F}$.
3. If $A_1, A_2, \ldots \in \mathcal{F}$, then $\bigcup_{n=1}^{\infty} A_n \in \mathcal{F}$.

(Note that point #3 also implies that $\mathcal{F}$ is also closed under infinite intersections.) I'll talk a bit more about why these specific properties are important shortly.

A tuple $(\Omega, \mathcal{F})$ is called a measurable space, meaning that it is a space that could potentially be imbued with a measure.

## Measures

A measure, denoted by $\mu$, is simply a function from some $\sigma$-algebra $\mathcal{F}$ to the non-negative real numbers, $\mu: \mathcal{F} \to [0, \infty]$. This function must also satisfy some properties: 

1. $\mu(\emptyset) = 0$
2. If $A_1, A_2, \ldots \in \mathcal{F}$ are disjoint, then $\mu\left(\bigcup_{n=1}^{\infty} A_n\right) = \sum_{n=1}^{\infty} \mu(A_n)$. (This known as countable additivity.)

The tuple $(\Omega, \mathcal{F}, \mu)$ is generally called a measure space. Note that since $\Omega \in \mathcal{F}$, we can define $\mu(\Omega)$, which is the measure of the full space. This can be arbitrary, but if we say that $\mu(\Omega) = 1$, then we have what is called a probability measure, and with it a corresponding probability space $(\Omega, \mathcal{F}, \mu)$. In this case, we'll write $\mathbb{P}$ instead of $\mu$ to denote the probability measure, so ($\Omega, \mathcal{F}, \mathbb{P}$).

That's been a whole awful lot of definitions, so let's build some intuition behind it. When we think about probability, in the naive sense, how do we think about it? Well, let's imagine rolling a dice. What are the possible outcomes of rolling a dice? Well it can be any outcome from 1 to 6. We can represent this as a set of outcomes $\Omega = \{1,2,3,4,5,6\}$. Now one can intuitively just assign a measure, or probability, of $\frac{1}{6}$ to each outcome, but remember, probabilities are a function from $\mathcal{F} \to [0, \infty]$, not from $\Omega \to [0, \infty]$. How can we reconcile this?

Instead of thinking about it in terms of outcomes, we can think about it in terms of events. An event $E$ is simply a member of $\mathcal{F}$, and can represent a given set of outcomes. For example, the $$E = \{2, 4, 6\}$$ represents the event that the outcome of my roll is even, $$E = \{1, 2, 3\}$$ represents the event that the outcome is less than 4. As such, if we take $\mathcal{F} = 2^{\Omega}$, we can define a measure $$\mathbb{P}(E) = \frac{\lvert E \rvert}{6}$$, in which case the probability of any roll from 1 to 6 occurring is simply $\frac{1}{6}$. 

Now we can see why the $\sigma$-algebra $\mathcal{F}$ needs the properties it has:

1. We need to be able to assign a measure to the entire space, so $\Omega$ must be in $\mathcal{F}$.
2. If the probability of an event $A$ happening is $p$, the probability of the event not happening, $A^c$, must be $1-p$, so the $A^c$ must be in $\mathcal{F}$.
3. If we consider any arbitrary series of events $A_1, A_2 ...$, we need to be able to assign a probability to the event that $\textit{any}$ of them occur, so $\bigcup_{n=1}^{\infty} A_n$ must be in $\mathcal{F}$.

In short, $\mathcal{F}$ determines exactly the events that we are able to assign probabilities to. One might ask why this complexity is necessary, in the dice example surely it would be intuitive to assign the probability $\frac{1}{6}$ to each outcome and then add them up right? That's a question that I myself asked when first learning about this, and my next example shows how this intuition can quite easily break.

## The Borel Sigma Algebra and Lebesgue Measure

Let's imagine now that $\Omega = [0,1]$, and I want you think about what would $$\mathbb{P}(\{0.5\})$$ be? This is similar to the dice example, but now the outcomes are infinite, uncountably so. You really only have two options: 

1. Assign $$\mathbb{P}(\{0.5\}) = 0$$. This wouldn't really make sense, since $0.5$ is no different than any other real number in the interval, which would mean that the probability of selecting any given number in the interval is 0. Surely that wouldn't be right.
2. Assign $$\mathbb{P}(\{0.5\}) = p \gt 0$$. This seems logical, but again, $0.5$ is no different than any other real number, so the probability of selecting any given number would also be $p$, which would mean that the measure of the full interval would be infinite, as there are infinitely many real numbers in $[0,1]$. Surely this can't be right either?

As it turns out, the first option is actually the correct one, and to build up to it, we first have to define the Borel $\sigma$-algebra.

### Generation

An arbitrary subset $A \subseteq 2^{\Omega}$ is not necessarily a $\sigma$-algebra, but we can talk about $\sigma(A)$, which we will define as the smallest $\sigma$-algebra that contains $A$. Going back to the dice example, if $$A = \{\{1\}\}$$, then we have: 

$$

\sigma(A) = \{\Omega, \emptyset, \{1\}, \{2,3,4,5,6\}\}

$$

One can easily check that there is no smaller $\sigma$-algebra containing the set $A$, and that $\sigma(A)$ satisifes all 3 properties to be a $\sigma$-algebra.

### Borel Sigma Algebra

Let's take the set of all half-open intervals in $\mathbb{R}$, denoted by $$\mathcal{I} = \{(a,b], a < b, a, b \in \mathbb{R}\}$$. If we take the $\sigma$-algebra generated by this set, we will have what is called the Borel $\sigma$-algebra on $\mathbb{R}$, which we will denote $\mathcal{B}(\mathbb{R})$. This is the canonical $\sigma$-algebra that we use when we talk about probability on the real numbers. What do members of this actually look like? Well, they are simply any set that can be expressed using complements, unions, and complements of half-open intervals. This might sound simple, but all sorts of strange and wacky sets can be in $\mathcal{B}(\mathbb{R})$ by this definition, see the Cantor set as an example of one.

### Lebesgue Measure

Now we can turn to the central measure that we use on $\mathcal{B}(\mathbb{R})$, called the Lebesgue Measure, which we denote by $\text{Leb}(E)$, for $E \in \mathcal{B}(\mathbb{R})$. If I were to ask you how you would evaluate $\text{Leb}((a,b])$, what would you say? If you said $b-a$, congratulations! Lebesgue himself would be proud! As it turns out, the Lebesgue measure is the unique measure on $\mathcal{B}(\mathbb{R})$ such that $\text{Leb}((a,b]) = b - a$. (This uniqueness can be proven using something called the Dynkin's Uniqueness Theorem, which we won't get into here for the sake of brevity.) 

Let's go back to the example given at the start of this section, which was to define $\mathbb{P}(0.5)$. Here, we can define $\mathbb{P} = \text{Leb}$, since the Lebesgue measure of $[0,1]$ is just $1$, this is a probability measure. Under our new Lebesgue measure, we can see that $\text{Leb}(0.5) = 0$! So the Lebesgue measure of any singleton is $0$, in fact by the countable additivity of measure, the Lebesgue measure of any countable set is $0$ as well. This implies that only uncountable sets in $\mathbb{R}$ has non-zero measure under the Lebesgue measure. 

This was a very unintuitive result for me at first, since it suggests that if our experiment was picking a number from the interval $[0,1]$, the probability of picking $\textit{any}$ number would be $1$, (obviously but also since $\text{Leb}([0,1]) = 1$), but that picking any $\textit{given}$ number from the same interval has probability $0$! (What might be even more unituitive is that the converse is not necessarily true, i.e. an uncountable set can have $0$ Lebesgue measure, see the Cantor set as an example.) If you are as confused as I was when I first learned this result, you have now been also been initiated into the world of theoretical probability, and we welcome you with open arms.

(Another way of thinking about this same result is to define $A_n = [0.5, 0.5 + \frac{1}{n}]$, note that we have $\bigcap_{n=1}^{\infty} A_n = \{0.5\}$, and that $\text{Leb}(A_n) = \frac{1}{n}$. But the measure of any specific $A_n$ is $\frac{1}{n}$, so the limit gives us $0$.)

Though I guess in a very hand-wavey sort of way this makes sense. While the interval $[0,1]$ might seem small in the grand scheme of things, it is still uncountable from a cardinality point of view. If you think about it, picking any random number from $[0,1]$ and receiving exactly $0.5$ is unlikely, unfathomably unlikely in fact. Why $0.5$ and not $0.500000000000000000000000000000000000000000000000000000000000001$, or $0.499999999999999999999999999999999999999999999999999999999999999$? In this way it does really only make sense that the measure of any singular element is $0$.

## Random Variables

This is where the real meat of this post lies. Random variables were a concept that I was familiar with from early-year math courses, but they were always presented as this sort of magical mathematical object that gives you distributions for free. In a measure-theoretic sense, they are a much more intuitive concept, which I will explain below.

### Measurability

First however, we must dive into the concept of measurability. Given a measure space $(\Omega, \mathcal{F}, \mu)$, a function $f: \Omega \to \mathbb{R}$ is measurable if for any Borel set $B$, we have that $f^{-1}(B) \in \mathcal{F}$, which we will call $\mathcal{F} / \mathcal{B}(\mathbb{R})$ measurable. Why is this the case? Well we want to be able to assign a measure, or probability, that any output of $f$ occurs, these are our events, which all lie in $\mathcal{B}(\mathbb{R})$. But we can't simply use the standard Lebesgue measure on $\mathbb{R}$, since that would lose all information about the function $f$ itself. Instead, we need to be able to assign a measure to the pre-image of any Borel set, which lies in $\mathcal{F}$. We'll talk more about that below.

A random variable $X$ is simply a measurable function from a probability space $(\Omega, \mathcal{F}, \mathbb{P})$ to $\mathbb{R}$, so $X: \Omega \to \mathbb{R}$. That's all it is, just a measurable function.

### Laws and CDFs
As I stated above, we want to be able to measure the probability that the output of a measurable function $f$, or in this case a random variable $X$, lies in any given Borel set. We define the law of $X$, denoted $\mathcal{L}_X$, as a measure on $\mathcal{B}(\mathbb{R})$ such that for any Borel set $B$, we have that $$\mathcal{L}_X(B) = \mathbb{P}(X^{-1}(B))$$. Note what I just said, the law is a $\textit{measure}$, more specifically a probability measure, on $\mathcal{B}(\mathbb{R})$. This law is what we traditionally refer to as the distribution of a random variable $X$, for example, $$X \sim \mathcal{N}(0,1)$$ if $$\mathcal{L}_x = \mathcal{L}_{\mathcal{N}(0,1)}$$. 

This was all a bit handwavey, so let's look at the dice example again. Let's say that the random variable $X$ in this case is whether or not our roll was heads, so

$$X(x) = \begin{cases}
1 & x \in \{2,4,6\} \\
0 & x \in \{1,3,5\} \\ 
\end{cases}$$. 

Now let's say that we want to know the probability that the roll is even, that would be $$\mathbb{P}(X^{-1}(\{1\})) = \mathbb{P}(\{2,4,6\}) = \frac{3}{6} = \frac{1}{2}$$. This is a more concrete example of why we can't just use the Lebesgue measure on $\mathbb{R}$, since that would give us $$\text{Leb}(\{1\}) = 0$$, which would be equivalent to saying that it is impossible to roll an even number.

The cumulative distribution function, or CDF, of a random variable $X$, denoted as $F_X(c)$, is then just the real valued function such that $F_X(c) = \mathbb{P}(X \leq c)$. Essentially it is the probability that a random variable has value $\le c$. Note that by this definition, that $F_X(c) = \mathcal{L}_X((-\infty, c])$.

## Expected Values and Probability Densities

At this point, some of you may be pointing out why I haven't mentioned anything about a probability $\textit{density}$ yet. This is usually the object that one learns about in the context of continuous random variables, where the probability density function is used to describe the distribution of a random variable. Well, it turns out that to build up to this object, we need to first explicitly define the notion of an expected value, and as a consequence, the integral of a random variable.

### Expected Value

We want to be able to define $\mathbb{E}[X] = \int_{\Omega} X d\mathbb{P}$. At face value, we don't really have a way of computing this integral, so let's build up to it. First, let's assume that the random variable $X$ is simple, i.e. it only takes on a finite number of values. In this case, we can write $X$ as: $$X = \sum_{i=1}^{n} c_i \mathbb{I}_{A_i}$$, where $c_i$ are some real numbers, $\mathbb{I}_{A_i}$ is the indicator function of the event $A_i$, and the $A_i$ are disjoint events that form a partition of $\Omega$. In this case, we can define the expected value of $X$ as: 

$$

\mathbb{E}[X] = \int_{\Omega} (\sum_{i=1}^{n} c_i \mathbb{I}_{A_i}) d\mathbb{P}
              = \sum_{i=1}^{n} c_i \int_{\Omega} \mathbb{I}_{A_i} d\mathbb{P}
              = \sum_{i=1}^{n} c_i \int_{A_i} d\mathbb{P}
              = \sum_{i=1}^{n} c_i \mathbb{P}(A_i)
$$

This is a pretty intuitive definition, since it matches with a more naive definition of expected value, which is just a weighted sum of all possible outcomes. To extend to any nonnegative $X$, we use a clever idea that any nonnegative measurable function $X$ can be expressed by an increasing sequence of simple functions $X_n$. As such, we can define the expected value of any nonnegative $X$ as:

$$

\mathbb{E}[X]
= \sup\limits_{n \geq 1} \mathbb{E}[X_n]
= \lim_{n \to \infty} \int_{\Omega} X_n \, d\mathbb{P}

$$

Essentially, we approximate $X$ by our sequence of simple functions, and then take the limit of their expected values. This is actually very similar to how a Riemann integral is defined, where we approximate an arbitrary real-valued function by rectangles under the curve, and in the case of $\Omega = [a,b]$, this is exactly what we are doing. All this definition is doing is extending the ideas used to define the Riemann integral to arbitrary measure spaces.

Obviously, not all random variables are nonnegative, so our final extension is to an arbitrary, possibly negative, random variable $X$. We can decompose $X$ into its positive and negative parts, $X^+$ and $X^-$, where $X^+ = \max(X, 0)$ and $X^- = \max(-X, 0)$. As such, we can define the expected value of any random variable $X$ as: 

$$

\mathbb{E}[X] = \mathbb{E}[X^+] - \mathbb{E}[X^-]

$$

### An Alternate Measure

Given any function $f: \Omega \to \mathbb{R}$, we can define a new measure $\mu_f$ on $\mathcal{F}$ as:

$$

\mu_f(A) = \int_A f d\mathbb{P} = \mathbb{E}[\mathbb{I}_A f]

$$

Essentially this the expected value of $f$ on the event $A$. One can check that this satifies the properties to be a measure. We can think about this in a more subtle way too. If we write out the integrals explicitly, we have:

$$

\mu_f(A) = \int_A f d\mathbb{P} = \int_A d{\mu_f}

$$

In this way, we can write that $d{\mu_f} = f d\mathbb{P}$. They must be equal of course since they integrate to the same thing over all events $A$. What does this remind you of? To me it reminded me of a derivative $dy = f'(x) dx$. Turns out that is exactly what it is, in a slightly different sense.

### Absolute Continuity and the Radon-Nikodym Derivative

Turns out, the whole idea of a probability density is essentially just an alias for what is more formally known as a Radon-Nikodym derivative. First, one more small definition. 

Given two measures $\nu$ and $\mu$ on the same measurable space $(\Omega, \mathcal{F}$), $\nu$ is said to be absolutely continuous w.r.t. $\mu$, denoted $\nu \lt \lt \mu$, if given a set $A \in \mathcal{F}$, $\mu(A) = 0 \implies \nu(A)=0$. 

The Radon-Nikodym theorem says that if $\nu$ and $\mu$ are as above, with $\nu \lt \lt \mu$, that there exists a nonnegative, measurable function $f: \Omega \rightarrow \mathbb{R}$ such that $A \in \mathcal{F}$, $\nu(A) = \int_A f d\mu$. (We can also write $d\nu = f d\mu$.) This is why I talked about the derivative angle before, since we can now write $f = \frac{d\nu}{d\mu}$. (This is not an actual derivative per-se, but a lot of the literature denotes it like this to due to its analogous role of being a rate of change between measures.)

It might also now be clear why the absolute continuity of these measures is important. If this were not the case, then integrating over a set $A$ such that $\mu(A) = 0$ would violate the relation $\nu(A) = \int_A f d\mu$, since $\nu(A)$ can be positive. In a more intuitive sense, we want the two measures to approach zero at a similar rate, otherwise the "derivative" $\frac{d\nu}{d\mu}$ could go to infinity. (It is the case that densities can be infinite, but this is more talking about it in a less rigorous sense.)

### Probability Densities

As stated in the sections above, the law of a random variable $X$ is a probability measure on $\mathcal{B}(\mathbb{R})$. This means that if $\mathcal{L}_X \lt \lt \text{Leb}$, then by Radon-Nikodym, there exists a unique function $f_X$ such that $d\mathcal{L}_X = f_X  d\text{(Leb)}$. This $f_X$ is precisely the traditional probability density that one might be familiar with, and it exactly matches our prior idea of a density. Given a set $B = (a,b]$, which is a Borel set, and writing $dx$ meaning integration w.r.t the Lebesgue measure:

$$

\int_B d\mathcal{L}_x = \mathbb{P} (X \in B) = \int_B f_X dx = \int_a^b f_X(x) dx

$$

And we've finally come full circle.

### A Quick Sidebar

When working with random variables in the past, one might have not ever thought about inputs to a random variable. The way that they were taught, at least to me, was always as a standalone object that spits out a number in line with a certain distribution. The way that I reconcile this is the following. When you use a random variable $X$, think about it like you're actually picking an outcome from an underlying, latent abstract set $\Omega$. You don't know anything about $\Omega$ itself, it can contain any elements that you can dream of. All you know is that you pick a random element $\omega$ from $\Omega$, and receive the corresponding output $X(\omega)$, which matches the distribution. In this way, sampling from a distribution is the exact same as randomly picking an element from the underlying $\Omega$.

## Conclusion

The material in this post has been only around a half of what my course teaches, and heavily truncated and simplified at that. I decided to stop around here since it covers most of the concepts that I, and many of you, may have learned in earlier-year probability or statistics courses, (probabilities, random variables, distributions, CDFS, densities), but from a measure-theoretic point of view. However, I think it goes to show one of the reasons why Maths always intrigued me. No matter how much I think I know, how 'fake' I believe probability to be, there's always a higher level abstraction that really formalizes the basic ideas that I've been taught and challenges the way that I thought about them. 

I guess that's the main takeaway I wanted to give from this article; that there's always something more to learn. Ideas and concepts that you thought you understood at face value might always have something deeper lying below the surface. It might not always be necessary to fully delve into the depths, but often times it is just a bit more fulfilling than wading in the shallow end.

Funnily enough, as I write this post I just completely bottled my second midterm for this course, so maybe the real takeaway is to not ego-enroll in a graduate level math course after 3 years off from doing pure maths :P
