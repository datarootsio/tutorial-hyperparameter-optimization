# rootlabs-hyperparameter-optimization

You all know that at dataroots, we are excellent data athletes. Compared to mainstream Olympics athletes, who train <u>with</u> weights, we evolved past that mere display of physical strength and started training <u>the</u> weights. In this way, Machine Learning Engineers train our models to achieve optimal performance on any task given to us. That’s how we shine! 

<img src="https://media.giphy.com/media/KDQ25pahVfwGRvvT9X/giphy.gif?cid=ecf05e47z4pp5k1wkdxdbvvvev66kbx8qag6z8b2ml0vzyg2&rid=giphy.gif" height=200>

Behind the shine and bling-bling, we have to admit that the performance of our models depends largely on how our model looks like, call it model infrastructure. This infrastructure is governed by a set of ‘hyperparameters’. Think about the depth of a tree, the number of layers and nodes in a neural network, weight regularization terms and even distributions, activation functions, optimizers, learning rates, and dropout probabilities. All of these choices need to be made before we can even start training our weights and these choices cannot be trained by the model itself. Therefore, choosing the best hyperparameters is one of the key challenges of many Machine Learning Engineers.

In the days of yore, we chose not to tune hyperparameters at all and rely on vanilla models or use gut-feel and experience to pick the best hyperparameters. We can do better.
We only need a good algorithm to learn good values for these hyperparameters on top of our machine learning model (which is in itself also an algorithm).  
We like these algorithms to be FRESH;
- Flexible; they need to be able to take many configurations into account,
- Robust; they need to be applicable to various problems and model architectures,
- Efficient; they need to find **good** hyperparameters **as fast as possible**,
- Scalable; they need to be scalable in runtime, and
- Helpful; they need to be easy to apply by means of a maintained and open-source project.

By now, we are quite familiar with the *good ol’* grid search, random search and some of us might have heard about evolutionary algorithms using heuristics (based on birds (Particle Swarm) and ants (Ant Colony)) or genetic approaches (based on #NSFW and Natural Selection). The problem with these algorithms are that (1) they take a long time as a whole new model needs to be trained for every possible combination of hyperparameters, (2) they do not ensure optimal hyperparameters as we can only hope to randomly pick the best, and (3) these algorithms do not really *learn* from previous trials as we can only take blind steps in the direction of where we hope the grass is greener. 

<img src="https://media.giphy.com/media/A03pTCglKdaRG/giphy.gif?cid=ecf05e47zdbs5xfg8e3hy4ssl3ykc7chfptrevb6wt57azc6&rid=giphy.gif&ct=g" height=150>

Before we can prance through green meadows with flowers in our hair, we need to come up with an approach that gives us a flexible and robust approach to effectively and efficiently find the best hyperparameters in the configuration space… in a fast and preferably cheap way. When confronted with a seemingly impossible problem, we all know that one guy that says: “Let’s go Bayesian”. 

<img src="https://media.giphy.com/media/l378BzHA5FwWFXVSg/giphy.gif?cid=ecf05e47t6z632ky9je9iafd6s0v9xs7md2qsuovl7sat2gl&rid=giphy.gif&ct=g" height=150>

Honoring the true Bayesian spirit, allow me to make things easy for you:

* First, we just have to try training the full model a few times, each time using distinct hyperparameters. We log the hyperparameters and the resulting loss metric of our trials. We call this *'evaluation of the objective function'*. Luckily for us, about 3 runs will do the trick.
* After this, we infer a **surrogate model** –call it a meta-model– which maps the hyperparameters to our loss metric (in the test set). A good surrogate model predicts how our loss is expected to evolve when we change our hyperparameters and incorporates our uncertainty. The key takeaways for a surrogate model are:
  * It is very easy to build and evaluate, which means that we can do thorough and fast optimization!
  * You can make this model as kick-ass as you would like, but currently 'they' explored (i) Gaussian processes, (ii) random forests (SMAC), (iii) Tree-Parzen Estimators and (iii) Bayesian Neural Nets. 
  * After every trial, we can update this model (*learn better and reduce uncertainty*). 
    
* We also define an **acquisition function** to help us choose the next hyperparameters to be tried. This acquisition function balances exploitation (*rely on the current best*) and exploration (*keep exploring better possibilities*) of our configuration space. We would be wise to move in the direction we expect the next trial to be much better than the best result found so far, call this Expected Improvement. 

* Repeat this process a few times leads you to the best set of hyperparameters quite fast and with as few iterations as possible.

Starts to sound insanely -Bayesian-y- simple, doesn’t it?

Remember I started of with talking about athletics (if not, scroll up)? Well, in hyperparameter optimization, we are allowed to play dirty! We can actually put our meta-models on **steroids**, which resulted in the creation of our good friend [BOHB](https://tenor.com/view/spongebob-muscles-ripped-gif-9019709.gif), the current state-of-the-art in hyperparameter optimization. This is basically where most AutoML libraries, both commercial and open-source, are thriving on.

I explain all of this, including a tutorial using the Optuna package, in our [Rootlabs@Lunch on Practical Hyperparameter Optimisation](https://www.youtube.com/watch?v=hboCNMhUb4g).

You can also try this notebook in Google Colab <a href="https://colab.research.google.com/drive/1fNzrF96E-Uhexdd0mFITsp-YpWZ2Mzwa/" target="_blank" rel="noopener noreferrer"><img src="https://colab.research.google.com/assets/colab-badge.svg"></a>.

You can find much more information on the [Optuna](https://optuna.readthedocs.io/en/stable/) package.

<img src="https://raw.githubusercontent.com/optuna/optuna/master/docs/image/optuna-logo.png" width=500>
