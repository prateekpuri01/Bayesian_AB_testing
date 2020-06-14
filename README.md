# Bayesian AB testing 


## First off, how does Frequentist AB testing work?
Frequentist AB testing is a well developed framework that is used to either accept or reject null hypothesis. Use cases are answering the following questions: Is this drug effective at treating joint soreness? Is this new website feature effetive at driving site purchases? Did this new rehab treeatment decrease athlete injury recovery time?

Frequentist AB testing, within a randomized control trial environment, works typically by assigning subjects to either control or treatment groups, computing statistics about the observed oberservations of these groups, and then drawing an inference about whether to accept/reject the null from these statistics.

## How long do we have to do this thing for?
One questions that experimental designers think about is : how many samples do I need to conduct this experiment?
Samples can after all, be expensive. Thinking about medical trials for testing drug effectiveness. Getting a person to undergo such a trial can be difficult in the first place; secondly, what if your drug actually ends up being worse than existing treatments? Every person you test on will be getting worse medical treatment than they would have otherwise. In short, you usually want to use as few samples as possible to reach a conclusion at the desired degree of significance.

In frequentist AB testing, sample size is determined *a-priori*. You estimate what you think your expected treatment effect will be, what your power requirement is, and what your desired confidence level is and *viola!* you can perform a simple sample size calculation to get your number. 

## Early stopping

The idea is that you then run your experiment with that target number of sample and see the result. The catch is that it may take a while to accumulate the results? What if you're curious about the results beforehand and want to take a peek? This is dicey grounds in frequentist AB testing. What happens if you see that your treatment group appears to be significantly better than the control group after 100 samples when your initial sample size calculation was 1000? It could be that you simply underestimated your expected effect size, and the effect is much stronger than anticipated, in which case you would like to stop the test early. Or it could mean that your are simply observing statistical fluctuations due to the low sample size and you cannot say anything about the efficacy of your treatment yet. This is known as "early stopping" problem in AB testing, and while there are sophisticated frameworks that have been developed to address it, a fundamentally different approach is also often taken, **Bayesian AB testing**

## Bayesian approaches

Bayesian AB testing is different in multiple ways. In frequetist statistics, researchers typically calculate a confidence interval that they say is likely to include the true parameter they are estimating with some degree of certainty. This can be a somewhat un-natural way to think about estimates. In the Bayesian approach, popiulation parameters are treated as random variables, with a probability distribution that has been formed by our prior beliefs. As we obtain more data from the world, we update our beliefs, and the probabiity distributions update accordingly. 

Since these probability distributions are dynamics, if you are testing for difference between two groups, you can compare their distributions after any given number of samples. Lets say A and B are our two groups P(A) and P(B) describe their probability distribtuons. You can calculate the expected loss of adopting A over B (and vice versa) as:

$L=\int \int P(A) P(B) (A-B) dA dB$ 

In Bayesian AB testing, if this expected loss is less than a pre-determined $\epsilon$, you stop the experiment. Picking $\epsilon$ takes some consideration, but once it is selected your sample size will be whatever is takes to reduce your expected loss to that amount. Did you significantly underestimate your effect size? Not a big problem, the real effect size will become evident in your data and your experiment will stop without requiring additional samples. 

The tradeoff here is that the smaller sample size comes at the expense of a higher false-positive rate i.e. if two treatment have identical effects, you may get *unlucky* and perceive one as having an effect just due to statistical fluctuations early on in your experiment. Under an AB test, these fluctuations wouldn't have affected your result since you would have collected more samples, but in a Bayesian approach it might.

## Bayesian approaches

Let's compare some simulations to see the benefits/tradeoffs of these two approaches. Let's 
