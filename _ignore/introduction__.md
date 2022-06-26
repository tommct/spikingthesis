# Introduction

## Putting popular notions of how the brain works into AI systems.

In 2007, as a graduate student in Computational Bioscience and with interests in Machine Learning and Computational Neuroscience, I was fortunate to attend the [Neural Information Processing Systems (NeurIPS) conference](https://neurips.cc/Conferences/2007).
A neural information processing system is a network of computational units, called *neurons*, that pass signals between each other, encoding and decoding those signals as information.
Of course, the brain is the most obvious neural information processing system, so interpreting and modeling how the brain works has widespread implications for interfaces with the brain and the field of artificial intelligence (AI).
This conference brought together an interdisciplinary cadre of neuroscientists, AI and machine learning experts, statisticians, and many other researchers all with the goal of presenting pieces of the puzzle of how networks of computational units can encode, process, and even learn information.

While I felt an amazing energy and inspiration, I did not fully realize how special and particular the moment was.
[Geoff Hinton](https://en.wikipedia.org/wiki/Geoffrey_Hinton), [Yoshua Bengio](https://en.wikipedia.org/wiki/Yoshua_Bengio), and [Yann LeCun](https://en.wikipedia.org/wiki/Yann_LeCun) were advocating [deep neural networks](https://en.wikipedia.org/wiki/Deep_learning) where "deep" simply meant anything more than one hidden layer in an [*artificial neural network*](https://en.wikipedia.org/wiki/Artificial_neural_network)---the most popular neural information processing system design in use before that conference and since.
We will describe these networks and their hidden layers shortly after we discuss neurons, but long story short, the deep architectures these researchers were promoting later earned them the 2018 [Turing Award](https://amturing.acm.org/), computer science's Nobel Prize.
Their ideas lanched the AI component of the [Fourth Industrial Revolution](https://en.wikipedia.org/wiki/Fourth_Industrial_Revolution).
Nearly any application of AI you experience today is likely to have a deep neural net behind it.

At that 2007 conference, [Geoff Hinton](https://en.wikipedia.org/wiki/Geoffrey_Hinton) gave an unofficial, impromptu talk where we sat on the floor to hear it.
He told us he had figured out ["how the brain works"](https://www.quantamagazine.org/artificial-neural-nets-finally-yield-clues-to-how-brains-learn-20210218/).
Central to his argument is a generic model of how neurons in the brain behave.
It is known that they communicate with each other via abrupt electrochemical events known as *action potentials*.
And because these events look like abrupt ticks in electrophysiology recordings, they are also called *spikes*.
A neuron fires an action potential, and nearly instantaneously, that spike radiates down its axon and activates the synapses of its target neurons.
If the neuron doing the spiking is excitatory, then it promotes the target neurons to fire their own action potentials; and if it is inhibitory, it will impede them from spiking.
In a network context, any one neuron is stimulated by many, even thousands, of other neurons and may have as many targets, so each neuron of the brain is constantly awash in a barrage of inhibitory and excitatory signals.
<!-- The mechanisms to set up the electrochemical environment for neurons to spike requires energy in the form of burning ATP. -->
<!-- With around 100 billion neurons and 100 trillion synapses, the human brain consumes 20% of our energy {cite}`raichleAppraisingBrainEnergy2002`. -->

### The Rate-Coding Neuron

Loosely speaking, for a neuron of the brain to spike, it has to be induced through the activation of many excitatory synapses.
When an excitatory threshold is reached, it will spike.
In electrophysiology recordings, it is possible to observe a neuron spiking rapidly to some stimuli and inhibited or unresponsive to other stimuli.
Hubel and Wiesel's extraordinary work to illuminate receptive fields and columns of the visual cortex showed that neurons were particularly sensitive to specific patterns {cite}`hubelReceptiveFieldsSingle1959, hubelReceptiveFieldsFunctional1968`.
(See David Hubel's [Nobel lecture video](https://www.nobelprize.org/prizes/medicine/1981/hubel/lecture/), especially 16 minutes in. One can hear the rapid spikes of the neuron being recorded from.)
{numref}`fig-hubel_wiesel_dayan_abbott_tuningcurve`**a** shows spikes Hubel and Wiesel recorded from a neuron of a monkey in response to a bar of light moving across its visual field at particular angles. Panel **b** shows the corresponding "tuning curve" of firing rates with respect to deviations from the neuron's most sensitive pattern.
It has also been found that the sensitivity of the tuning curve can be modulated with attention {cite}`treueNeuralCorrelatesAttention2001`. 

```{figure} ./figs/hubel_wiesel_dayan_abbott_tuningcurve.png
---
width: 100%
name: fig-hubel_wiesel_dayan_abbott_tuningcurve
---
**a** Example responses from a neuron of a monkey in the primary visual cortex to bars of light moving across its visual field at different angles from {cite:p}`hubelReceptiveFieldsFunctional1968`. **b** Corresponding firing rates of Hubel and Wiesel's spike traces by deviations from the most sensitive orientation as translated by {cite:p}`henryOrientationSpecificityCells1974`. Reprinted from Figure 1.5 from {cite:p}`abbottTheoreticalNeuroscienceComputational2001`.
```

Given the broad understanding of the excitatory and inhibitory mechanisms that drive neurons to fire rapidly or slowly, it is easily accepted that information can be conveyed by a neuron's firing rate.
Indeed, firing rate *does* convey information and it is this interpretation that drives deep learning systems.
But is this the most efficient or best way to convey information in intelligent systems?

<!-- Given all of the stimulus-response profiles of neural activity captured by neuroscientists, it seems rather obvious *to us* that a neuron signals its recognition of a pattern it is sensitive to through its firing rate. -->
<!-- (Spoiler alert: we will challenge that this is the principal mechanism for neurons to convey information.) -->

### The Deep Learning Neuron

Sitting cross-legged in Hinton's impromptu NeurIPS talk, I listened as he took this notion of a neuron conveying information via its firing rate and presented a neuron model somewhat like the one depicted in {numref}`fig-ann_neuron`, which remains the central portrayal of a neuron in deep learning systems.

```{figure} ./figs/ann_neuron.png
---
width: 100%
name: fig-ann_neuron
---
Cartoon of a neuron and its input and output neurons in an [artificial neural network](https://en.wikipedia.org/wiki/Artificial_neural_network).
For aid in understanding, we illustrate with neurons that have a [Rectified Linear Unit (ReLu)](https://en.wikipedia.org/wiki/Rectifier_(neural_networks)) activation function, the "elbow" function that is 0 for negative values, but is then the value of the summation if that summation is positive.
Therefore, each neuron emits its "firing rate", the sum of inputs driving it, if the sum of those inputs is positive.
The width of arrows indicate the magnitude or weight of influence each neuron has on its target.
Red indicates negative weights while black indicates positive weights.
The value emitted by the neuron is multiplied by this weight.
For example, when the second neuron from the top emits a positive value, it will have a negative effect on the central neuron.
The central neuron then sums its inputs.
If the sum is near zero, either because the positive and negative signals cancel each other out or because they themselves are weak and emit near-zero values, this neuron will also emit a value near 0.5.
However, if the sum is more positive, there is a strong drive for it to emit a value closer to +1.
As well, if the sum is more negative, it will emit a value closer to 0.
```

The basic idea, as shown by the central neuron in {numref}`fig-ann_neuron`, is that it integrates the signals coming into it by first taking the weighted sum of each of those signals.
If that weighted sum is greater than zero, then the activation or "firing rate" of the neuron is that weighted sum.
If it is less than zero, then the neuron cannot have a negative firing rate, so it emits 0.
This particular activation function is known as a [Rectified Linear Unit (ReLU)](https://en.wikipedia.org/wiki/Rectifier_(neural_networks)).
There are actually many different kinds of activation functions, but the ReLU is quite popular because it is simple and robust {cite}`nwankpaActivationFunctionsComparison2018`.
(In 2007 when Hinton gave his presentation, he used a [Sigmoid](https://en.wikipedia.org/wiki/Sigmoid_function) activation function and not a ReLU activation function, but the translation that inputs map to the amount of activation remains the same, hence the term "activation" function.)

To begin to understand what is so special about this little machine, let us consider the most influential input neurons onto the central neuron.
Input neurons #2 and #4 have the largest weights as signified by the width of the arrows, so whatever those neurons emit will be magnified by those weights.
Because the arrow from the second neuron is negative and the arrow from the fourth neuron is positive, when those neurons both emit values of similar magnitudes, they will cancel each other out.
However, if neuron #2 emits a near-zero value and neuron #4 emits a strong positive value, then the weighted sum from these two neurons will likely make the whole sum much more positive and induce the central neuron to emit a strong positive value.
The neuron is really sensitive to #4, but only when #2 is not active.
In short, this example highlights that *the role of the neuron is a pattern detector* and its outputs reflect the magnitude of the patterns it is sensitive to.

The excitement Dr. Hinton exuded was that this model is a relatively simple, repeatable architecture.
Placed in a chained network where a layer of neurons feeds another layer of neurons followed by more and more layers permits each neuron to participate in an aggregating system that can recognize and represent rather arbitrary and complex patterns {cite}`hintonFastLearningAlgorithm2006a`.
With enough neurons and enough data to capture all the subtle patterns embedded in a dataset, and an algorithm to tune the weights (the magnitudes of the arrows) to what each neuron can become sensitive to, there was no telling what these systems could do!
To support this notion, Yann LeCun presented how the [backprop algorithm](https://en.wikipedia.org/wiki/Backpropagation) {cite}`lecunBackpropagationAppliedHandwritten1989a`, which had been used in traditional, non-deep networks to learn the weights could be used in large network settings in the talk ["Who is afraid of non-convex loss functions?"](https://www.youtube.com/watch?v=8zdo6cnCW2w) {cite}`lecunWhoAfraidNon2007`.
The backprop algorithm in conjunction with Big Data combined with advances in GPUs to perform the computations have us to the point where these networks have billions of neurons and trillions of parameters doing extraordinary AI.



<!-- 


When a network of such neurons can learn the weights of source neurons going to their targets, effectively making the target neurons sensitive to particular 


My failure to fully appreciate and recognize that moment in 2007 was that I didn't fully accept it.
I still don't.

The "it" that I didn't/don't fully accept is the neuron model, that sums its inputs and passes them through an [activation function](https://en.wikipedia.org/wiki/Activation_function).
Granted, summing is *a way* that a neuron can integrate its inputs.
But that these networks need their neurons to have a subsequent, typically nonlinear, activation function to further modulate the sum of those inputs indicates that integration 

and passing them through a modulating function
For the artificial neural network to learn complex patterns and act intelligently requires 

This requires models to have millions, billions, and even trillions of parameters
rigidity and sheer size of these models and the necessary big data to accommodate them.
That said, the successes of deep AI architectures have changed my perspectives considerably.
These AI systems highlight that as our world becomes more digitized, we indeed can create software algorithms to compute on it.
In particular, patterns are everywhere, and with enough data, and millions or billions, of fast, fuzzy decision-making computational units operating on that data (with trillions of parameters), systems can behave intelligently to the point where synthetic judgements are superior to human judgements.
Additionally, there are several ways to realize intelligence (or at least to derive rational bases).
And data, to an intelligent system, can be the universe of all that is knowable.
No more; no less.
We do not need to bootstrap models with hard-coded rules, maybe just some labels and a standard, clean representation of the data.
And with this universe of data we should be careful of its collection errors and sampling biases because any patterns found will likely be used.

https://en.wikipedia.org/wiki/Artificial_general_intelligence
And intelligence can be realized at many scales in many different ways.

In short, General 


At the time, NeurIPS was an interdisciplinary conference of machine learning, neuroscience, statistics, and optimization.
Today, it has become the premiere conference for Machine Learning, especially those approaches using [Deep](https://en.wikipedia.org/wiki/Deep_learning) architectures.
I did not realize at the time


 with its neurons that signal each other through abrupt electrochemical exchanges known as *action potentials*, or more commonly, *spikes*.
The goal



 with a particular focus on models 

The prevailing machine learning models at NeurIPS were known as [*artificial neural networks*](https://en.wikipedia.org/wiki/Artificial_neural_network), which are banks of a few or more non-linear filters called "neurons" that are supposed to mirror the neurons of the brain.

In fact, an internet search of "neural network" needs to be prefaced with the word "biological" if you want it to be constrained to the brain.


In fact, Geoff Hinton gave a presentation 

Even though much of it was over my head, I found the conference extraordinarily inspirational.
Even so, I didn't realize how special of a moment I was experiencing.

Leaders such as Geoff Hinton, Yoshua Bengio, and Yann LeCunn were talking about "deep" architectures.
At the time, "deep" meant "anything more than 1".

I have always been attracted to pattern recognition problems and as a computational neuroscience student, enjoyed learning how the brain works as a pattern recognition machine
As a Computational Neuroscientist, I appreciated how 

This conference married my two areas of interest: Machine Learning/Pattern Recognition with Neuroscience.

Unfortunately, most of the statistics presented was over my head, but quickly caught on that the buzz concerned "Deep" architectures.
I was actually quite surprised. -->

## Works cited

```{bibliography}
:filter: docname in docnames
:style: plain
```
