# Note: Distributed Gradient Descent
**Gradient descent** is the basic optimization concept in *machine learning*. Here is a note of *gradient descent*, *stochastic gradient descent* and how *tensorflow* deals with **gradient descent** in a distributed environment. 
### Agenda
* [Gradient Descent](#gradientdescent)
* [Stochastic Gradient Descent](#stochasticgradientdescent)
* [Asynchronous Stochastic Gradient Descent](#asynchronousstochasticgradientdescent)
* [Synchronous Stochastic Gradient Descent](#synchronousstochasticgradientdescent)

### Gradient Descent
Gradient descent is a first-order iterative optimization algorithm. To find a *local* minima of a function using gradient descent, one takes steps proportional to the negative of the gradient (or of the approximate gradient) of the function at the current point. If instead one takes steps proportional to the positive of the gradient, one approaches a *local* maxima of that function; the procedure is then known as gradient ascent. Gradient descent is widely used in machine learning according to an optimizer could be regarded as *converged* when it ahcieves the minima or maxima.<br>
Say we have a 2-D function as below, we use gradient descent to find a minima.<br>

<img src="http://cfile29.uf.tistory.com/image/243BBA4954D414872E49A2" height="240">

As shown above, we could easily find a problem of gradient descent. After we got our start point randomly on the very left side of the figure, gradient descent is used to find a minima. It's obvious for us to say the first minima is only a local minima because we have alreday seen the following curve. However, it's hard for gradient desent algorithm to figure out that. Once the gradient hits 0, which means it achieves the minima, the updates will always be zero.<br>
To solve this problem,
1. We should find a better start point;
2. We could use *Stochastic Gradient Descent* and *Simulated Annealing* randomly jumpover the local minima/maxima;
