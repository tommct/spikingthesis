# Introduction

## Popular notions of how the brain works 

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
And because these events look "spiky" compared to a neuron's non-firing state in electrophysiology recordings, they are also called *spikes*.
(See the blips in {numref}`fig-hubel_wiesel_dayan_abbott_tuningcurve`**a**.)
A neuron fires an action potential, and nearly instantaneously, that spike radiates down its axon and activates the synapses of its target neurons.
If the neuron doing the spiking is excitatory, then it promotes the target neurons to fire their own action potentials; and if it is inhibitory, it will impede them from spiking.
In a network context, any one neuron is stimulated by many, even thousands, of other neurons and may have as many targets, so each neuron of the brain is awash in a barrage of inhibitory and excitatory signals.

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
**a)** Example responses from a neuron of a monkey in the primary visual cortex to bars of light moving across its visual field at different angles from {cite:p}`hubelReceptiveFieldsFunctional1968`. The duration of each trace is 2 seconds. **b)**&nbsp;Corresponding firing rates of Hubel and Wiesel's spike traces by deviations from the most sensitive orientation as translated by {cite:p}`henryOrientationSpecificityCells1974`. Reprinted from Figure 1.5 from {cite:p}`abbottTheoreticalNeuroscienceComputational2001`.
```

Given the broad understanding of the excitatory and inhibitory mechanisms that drive neurons to fire rapidly or slowly, it is easily accepted that information can be conveyed by a neuron's firing rate.
Indeed, firing rate *does* convey information and it is this interpretation that drives deep learning systems.
(Spoiler alert: we will challenge that firing rate should be the principal mechanism for spiking neurons to convey information.)

### The Deep Learning Neuron

Sitting cross-legged in Hinton's impromptu NeurIPS talk, I listened as he took this notion of a neuron conveying information via its firing rate and presented a neuron model somewhat like the one depicted in {numref}`fig-ann_neuron`, which remains the central portrayal of a neuron in deep learning systems.

```{figure} ./figs/ann_neuron.png
---
width: 100%
name: fig-ann_neuron
---
Cartoon of a neuron and its input and output neurons in an [artificial neural network](https://en.wikipedia.org/wiki/Artificial_neural_network).
For aid in understanding, we illustrate with neurons that have a [Rectified Linear Unit (ReLu)](https://en.wikipedia.org/wiki/Rectifier_(neural_networks)) activation function, the "elbow" function that is 0 for negative values, but is then the value of the summation when that summation is positive.
Therefore, each neuron emits its "firing rate", the sum of the inputs driving it, when that sum is positive.
The width of arrows indicate the magnitude or weight of influence each neuron has on its target.
Red indicates negative weights while black indicates positive weights.
The value emitted by the input neuron is multiplied by this weight onto its target.
```

The basic idea, as shown by the central neuron in {numref}`fig-ann_neuron`, is that the neuron integrates the signals coming into it and then translates them into a "firing rate", emitting a number to indicate how much it is activated.
More concretely, the integration is a sum of the weighted inputs that then go to an *activation function*.
The activation function shown in {numref}`fig-ann_neuron` is known as a [Rectified Linear Unit (ReLU)](https://en.wikipedia.org/wiki/Rectifier_(neural_networks)):
when the weighted sum is greater than zero, then the activation or "firing rate" of the neuron is that weighted sum.
When the weighted sum is less than zero, then the neuron cannot have a negative firing rate, so it emits 0.
There are actually many different kinds of activation functions, but the ReLU is quite popular because it is intuitively simple, computationally robust, and can yield highly accurate systems {cite}`nwankpaActivationFunctionsComparison2018`.
(In 2007 when Hinton gave his presentation, he used a [Sigmoid](https://en.wikipedia.org/wiki/Sigmoid_function) activation function and not a ReLU activation function, but the translation that inputs map to the amount of activation remains the same.
Additionally, many activation functions used in AI systems can emit a negative value, discussed later.)

To begin to understand what is so special about this little machine, let us consider the most influential input neurons onto the central neuron.
Input neurons #2 and #4 have the largest weights as signified by the width of the arrows, so whatever those neurons emit will be magnified by those weights.
Because the arrow from the second neuron is negative and the arrow from the fourth neuron is positive, when those neurons both emit values of similar magnitudes, they will cancel each other out.
However, if neuron #2 emits a near-zero value and neuron #4 emits a strong value, then the weighted sum from these two neurons will likely make the whole sum much more positive and induce the central neuron to emit a strong positive value.
We can say that the central neuron is really sensitive to #4, but only when #2 is not active.
In short, this example highlights that *the role of the neuron is a pattern detector* and its outputs reflect the magnitude of the patterns it is sensitive to.

The excitement Dr. Hinton exuded was that this model is a relatively simple, repeatable architecture.
Placed in a chained network where a layer of neurons feeds another layer of neurons followed by more and more layers permits each neuron to participate in an aggregating system that can recognize and represent rather arbitrary and complex patterns {cite}`hintonFastLearningAlgorithm2006a`.
With enough neurons and enough data to capture all the subtle patterns embedded in a dataset, and an algorithm to tune the weights (the magnitudes of the arrows) to what each neuron becomes sensitive to, there was no telling what these systems could do!
To support this notion, Yann LeCun presented how the [backprop algorithm](https://en.wikipedia.org/wiki/Backpropagation) {cite}`lecunBackpropagationAppliedHandwritten1989a`, which had been used in traditional, non-deep networks to learn the weights could be used in large network settings in the talk ["Who is afraid of non-convex loss functions?"](https://www.youtube.com/watch?v=8zdo6cnCW2w) {cite}`lecunWhoAfraidNon2007`.
Since then, the backprop algorithm, in conjunction with Big Data, combined with advances in GPUs to perform the computations have us to the point where these networks have billions of neurons and trillions of parameters (the weighted arrows) doing extraordinary AI.

Sitting on the floor listening to Dr. Hinton, I agreed with his general premise that these systems, more formally known as [*Restricted Boltzmann Machines*](https://en.wikipedia.org/wiki/Restricted_Boltzmann_machine), could be stacked to learn complex patterns.
Over 15 years later, as Google finishes my sentences, the power of these systems is obvious.
However, as a neuroscientist, the simplistic abstraction that the function of a neuron is to translate the sum of its inputs into an activation value for subsequent signaling is overreaching.
Indeed, Deep Learning researchers, including Hinton, readily admit that their conceptualizations of neurons are simplistic, idealized models; that these systems use "neurons that communicate real values rather than discrete spikes of activity" {cite}`hintonNeuralNetworksMachine2012`.
(See [Hinton's lecture slides](https://www.cs.toronto.edu/~hinton/coursera/lecture1/lec1.pdf), especially pages 21-29.)
Regardless, the mapping of "discrete spikes of activity" to a real value largely assumes that biological neurons convey and process information via their firing rate.
As such, artificial neurons used in deep learning systems collapse time and encapsulate spiking activity of the biological neurons they are modeled after as the inferred firing frequency, which is reported as the artificial neuron's real value.

## Animals evolved spikes, not continuous functions.

But animals did not evolve signaling networks that use continuously-valued functions. 
They evolved spikes.
This implies, therefore, that spikes must be more beneficial to animals than real-value outputs.

### How biological cells compute

But what do we even mean when we talk about biological neurons emitting spikes or continuous values? 
It is not like cells know anything about numbers to calculate and convey the value 3.5873.
In this regard, we have to understand that cells and especially biological neural networks are a form of [wetware](https://en.wikipedia.org/wiki/Wetware_computer).
Despite their undetectible size by eye, cells are extraordinarily complex living environments characterized by the dynamical interactions of molecules.
As such, we can say that metabolic pathways operate as input-output functions.
Breaking it down further, enzymatic reactions can operate as a series of [molecular logic gates](https://en.wikipedia.org/wiki/Molecular_logic_gate) {cite}`brayWetwareComputerEvery2009`.
So when thinking about what a cell computes, we can characterize every dynamical change that is experienced by the cell as a computation and quickly fall into the rabbit hole of *pancomputationalism*---"The universe is a computer. Everything computes." {cite}`andersonPancomputationalismComputationalDescription2017`.
Obviously, pancomputationalism is too open-ended to be helpful.
We are only interested in the underlying computations that cells in a communication network perform, and at a granularity to talk about them semantically.

As shown in {numref}`fig-Integral-Peripheral-and-Surface-Proteins`, cells have a variety of classes of proteins in their cell walls that interface with the external environment.
An example of the functionality that some of these proteins provide is that they enable cells to attach to neighboring cells.
Some of these proteins also serve as gate-keepers for the ions and molecules that enter or exit the cell.
As a prominent example, all cells have a transmembrane protein known as the [sodium-potassium pump](https://en.wikipedia.org/wiki/Sodium%E2%80%93potassium_pump) that pumps out 3 sodium ions from the cell and pulls in 2 potassium ions using the energy of one ATP molecule ({numref}`fig-Sodium-potassium_pump_and_diffusion`).
The steady-state of most mammalian cells is to have about 14 times less sodium in the cell than outside and 28 times more potassium inside than outside.
The differential this pump creates is involved in a number of critical cellular functions {cite}`pirahanchiPhysiologySodiumPotassium2022a`, but for our discussion, we focus on the electrochemical gradient established by this machine and as summarized in {cite:p}`pivovarovNaPumpNeurotransmitter2018`.
Both sodium and potassium are positively charged ions.
By pumping 3 positively charged ions out for every 2 positively charged ions that it brings in, this mechanism is the main driver that makes cells more negatively charged inside compared to their external environment.
This difference in electrical charge across the cell membrane is known as the [*membrane potential*](https://en.wikipedia.org/wiki/Membrane_potential).

```{figure} ./figs/Integral-Peripheral-and-Surface-Proteins.webp
---
width: 100%
name: fig-Integral-Peripheral-and-Surface-Proteins
---
Cartoon of some of the various classes proteins in and around the cell membrane. [Image Source](https://commons.wikimedia.org/wiki/File:OSC_Microbio_03_04_EukPlasMem.jpg), [CNX OpenStax, CC BY 4.0](https://creativecommons.org/licenses/by/4.0), via Wikimedia Commons
```

```{figure} ./figs/Sodium-potassium_pump_and_diffusion.png
---
width: 100%
name: fig-Sodium-potassium_pump_and_diffusion
---
Cartoon of the Sodium-Potassium Pump. Center: The pump that actively extrudes 3 sodium (Na$^{+}$) ions for two potassium (K$^{+}$) ions with each ATP of energy. Other channels to allow the free exchange of sodium and potassium across the membrane may open under various conditions. Image Source: [Blausen.com staff (2014). "Medical gallery of Blausen Medical 2014". WikiJournal of Medicine 1 (2). DOI:10.15347/wjm/2014.010. ISSN 2002-4436. Derivative by Mikael Häggström](https://commons.wikimedia.org/wiki/File:Sodium-potassium_pump_and_diffusion.png), [CC BY 3.0](https://creativecommons.org/licenses/by/3.0), via Wikimedia Commons
```

### The Action Potential

[Spikes](https://en.wikipedia.org/wiki/Action_potential) are induced largely through the quick opening and closing of transmembrane proteins sensitive to sodium or potassium (the green channels in {numref}`fig-Sodium-potassium_pump_and_diffusion`) {cite}`hodgkinQuantitativeDescriptionMembrane1952, hausserHodgkinHuxleyTheoryAction2000`.
([Hodgkin](https://en.wikipedia.org/wiki/Alan_Hodgkin) and [Huxley](https://en.wikipedia.org/wiki/Andrew_Huxley) earned the [1963 Nobel Prize in Physiology or Medicine](https://www.nobelprize.org/prizes/medicine/1963/summary/) for describing the action potential, which follows.)
When sodium channels open, they let in a huge influx of sodium ions because they follow the electrical gradient to the negative cell interior as well as following the concentration gradient that has less sodium inside the cell than outside.
These sodium channels are voltage-gated meaning that these transmembrane proteins change shape when the cell becomes *less* negative.
Therefore, this electrical depolarization induces positive feedback: the less negative the cell is, the more sodium channels open up, which depolarizes the cell even more, which opens more sodium channels to let more positively charged sodium channels in....
This rush of sodium into the cell characterizes the abrupt change in membrane potential, i.e., the spike. 
Potassium also plays a role.
The Hodgkin-Huxley model of the action potential as derived from their work in the giant squid axon describes the voltage-gated conformations that the channels undertake to serve as a gate to allow sodium or potassium ions to pass or not.
In the case of the sodium channel, there are 4 different conformations and with the potassium channel, there are 2.
Their model is illustrated in {numref}`fig-hh_gates` with the probability of the gates being open and the resulting spike from the membrane potential illustrated in {numref}`fig-hh_gateprobs`.

`````{grid}
````{grid-item}
:columns: 12 12 12 6
```{figure} ./figs/hh_gates.png
---
width: 100%
name: fig-hh_gates
---
Illustration of the voltage-gated sodium and potassium transmembrane ion channels that give rise to the action potential as described by Hodgkin-Huxley equations from the giant squid axon {cite}`hodgkinQuantitativeDescriptionMembrane1952`. Image from Matthews, G. G. (1991) Cellular physiology of nerve and muscle, second edition.
```
````
````{grid-item}
:columns: 12 12 12 6
```{figure} ./figs/hh_gateprobs.png
---
width: 100%
name: fig-hh_gateprobs
---
The membrane potential (black) during a spike as described by the Hodgkin-Huxley equations from the giant squid axon {cite}`hodgkinQuantitativeDescriptionMembrane1952`. Tick marks are every 3 ms. The colored lines indicate the probability that the gate in a given ion channel protein is open. The green line, for example shows that at spike onset, virtually all $m$-gates of the voltage-gated sodium channels are open and less than 3 ms later, they are closed.
```
````
`````

### Spikes seem computationally inefficient.

Spikes are a lot of work for the cell.
In particular, we can see that action potentials need to be extremely brief (and blips instead of a wide arc) to maintain the sodium-potassium balance.
After all, the longer action potentials are in duration---the wider they are, the more sodium enters the cell and the more potassium leaves the cell making it so that the cell has to hydrolyse more ATP to restore homeostasis and keep this effective battery charged.
In fact, it is estimated that the brain consumes 20% of humans' energy and 75% of the ATP used by grey matter of the brain is from signaling {cite}`laughlinCommunicationNeuronalNetworks2003, wangNeuralEnergySupplyConsumption2017`.
Collectively, spikes seem expensive.
So why is there an evolutionary drive for them?
Why not use some sort of continuous signaling mechanism that deep learning has shown to be so effective?

### Continuous signaling cell mechanisms and coding

There are several mechanisms available to the cell to emit a continuous signal.
Back to our discussion of the computations a cell can perform, it is virtually any dynamical change.
Of note are gap junctions.
These are low-resistance couplings between cells that permit ions to flow between cells.

{cite}`piccininiNeuralComputationComputational2013`


maintain a different electrochemical environment internally compared to their external environment.
Some membrane proteins also interface with the membrane proteins of neighboring cells. 

to communicate and interface with other cells and their environment.
All cells 

Spikes are generated from the particular exchange of charged ions across the cell's membrane through various protein channels.



These same mechanisms 
With all of the charged ion channels available to living cells---and spikes are generated from charged ion 
Sure, neurons in the brain receive excitatory and inhibitory synaptic events that modulate their spiking outputs.

An example of spikes in electrophysiology recordings is given in {numref}`fig-kashiwadani_oscillations`, which shows the simultaneous recording from two electrodes in a rabbit's brain, each trace capturing the electrical activity of two different neurons.
In this example, many of the spikes of these two neurons oscillate and are synchronized as the animal inhales an odor.

```{figure} ./figs/kashiwadani_oscillations.jpeg
---
width: 100%
name: fig-kashiwadani_oscillations
---
Simultaneous extracellular electrophysiology recordings of two neurons from the rabbit olfactory bulb in response to odor stimulation. Arrows demarcate coincident spikes between these neurons. 
Image courtesy of {cite:p}`kashiwadaniSynchronizedOscillatoryDischarges1999`.
```

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

## Thesis outline

We begin with various definitions and the components that make up any intelligent system.
We then cast the computational elements of an intelligent system as sensors whose inputs and outputs are information.
Critically, information operates as a pattern for the sensors. 
In more complex systems, the outputs from earlier sensors serve as more meaningful patterns for subsequent sensors. 
Such sequencing is the mechanism by which intelligent systems build semantic representations.

We then show how spiking neural networks (SNNs)---networks composed of neurons that communicate with their neighbors via spikes---are ideally suited for intelligent learning systems, especially those systems that operate in real-time, because these neurons are simultaneously pattern detectors and generators that utilize a common protocol to provide multiple semantic representations in dynamic settings.
We describe our rationale for not pursuing a rate-based neural code and focus on systems sensitive to the synchronous spikes of multiple neurons onto a target neuron as the source of information sensing and transmission.

To develop coincident-sensitive neurons and networks, we start with homogenous point neurons; homogenous in that the population of neurons have the same statistical firing patterns and "point" meaning that they have no morphology such as dendrites.
We construct measures of coincidence from simulations with these neurons. 
And then we construct rules governing the dynamics of a set of spiking neurons that will be used.
These rules describe a neuron's internal state over time, how it responds to incoming spikes, and how it propagates spikes to its targets. 
Given that we want to promote the recognition of coincident spikes, we develop those learning rules and show how spiking networks can self-organize.

Given our findings from point neuron networks, we explore the impact of modulating a neuron's spiking sensitivity given its spiking history. 
For example, if it has not fired an action potential recently, it can become more sensitive so that it does spike. 
Contrastingly, if it has been quite active and has been rapidly emitting spikes, we can dial its sensitivity down. 
Relatedly, we explore modulating the emitted spike efficacy onto a neuron's targets. 
In this area, we evaluate the effect and possibilities of networks where even when a spike is generated, it is not communicated to all targets. 
As well, we consider setting some targets to receive more spikes than others depending on the source neuron's spiking history. 
In this case, some targets may receive most of the source neuron's spikes while other targets may only receive spikes after the source neuron has generated several rapid spikes.

We also explore possible roles of dendrites. 
We describe how coincident-based detection and processing can be optimized with neurons with dendritic trees that accentuate positive signals with superadditive rules and particular branching structures.

In all of our networks, our neurons follow [Dale's Law](https://en.wikipedia.org/wiki/Dale%27s_principle) that loosely states that a neuron acts completely excitatory or inhibitory to all of its synaptic targets {cite}`dalePharmacologyNerveEndings1935`. 
(It cannot excite some targets and inhibit others. It has to be one or the other.)
This allows us to explore the roles of excitation and inhibition more distinctly. 
More to point, we argue that inhibition is not overtly to decrease the firing rate in targets as much as it is to help coordinate synchronous spikes between them. 
Another option for inhibition is to gate propagating signals entirely.

We then show some working examples of these spiking networks, starting with simple toy problems, to image problems, NLP datasets, and in
robotics.

And finally, we discuss the ramifications of this theory. 
We claim that meaning is self-emergent in learning systems. 
This means that a learning system's inputs, its internal computing capabilities, and the way it is taught are critical to how it derives meaning. 
A spiking neuron need not be passive and a network of active, spiking neurons is able to generate and create novel behaviors. 
We realize it in our own lives, living under a spiking brain. 
Therefore, we claim this a [Strong AI](https://en.wikipedia.org/wiki/Artificial_general_intelligence), closely resembling the way biological brains work. 
As such, there are hypotheses for neuroscience that need further testing and opportunities to develop AIs that interface with humans and behave in a more human-like way.

## Works cited

```{bibliography}
:filter: docname in docnames
:style: plain
```

