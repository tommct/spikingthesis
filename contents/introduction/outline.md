# Thesis outline

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