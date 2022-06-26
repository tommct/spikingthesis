# Introduction

Signals flowing through computational units to sculpt decision paths
underlie all intelligent systems. In brains, the computational units are
nerve cells that signal each other by activating the synapses between
neighboring cells via abrupt electrochemical changes known as *action
potentials*, commonly called *spikes*. The spikes in one set of neurons
activate synapses in another set of neurons, which, in turn, affects the
spiking of their associated neurons, inducing ripples of spiking
activity across the system. While this cascading protocol is seemingly
simple, we have not unearthed the "neural code" of how brains compute.
Indeed, brains likely use spikes in a variety of ways to regulate body
functions, induce behaviors, and realize intelligence.

While we are inspired by the brain and explore some of its spiking
mechanisms and properties, our intent is not to reverse-engineer the
brain. Rather, we investigate, more broadly and abstractly, various
computations with spikes. In particular, we focus on systems built with
generic computational units that are sensitive to *coincident* and
*precisely-timed* events. We present a theory for how artificial systems
can and how brains may use neurons that are sensitive to *coincident*
spikes. We also show how such neurons can self-organize into networks
that learn and exhibit generally intelligent behaviors in real-time.

## Thesis outline

We begin with various definitions and the components that make up any
intelligent system. We then cast the computational elements of an
intelligent system as sensors whose inputs and outputs are information.
Critically, information operates as a pattern for the sensors. In more
complex systems, the outputs from earlier sensors serve as more
meaningful patterns for subsequent sensors. Such sequencing is the
mechanism by which intelligent systems build semantic representations.

We then show how spiking neural networks (SNNs)---networks composed of
neurons that communicate with their neighbors via spikes---are ideally
suited for intelligent learning systems, especially those systems that
operate in real-time, because these neurons are simultaneously pattern
detectors and generators that utilize a common protocol to provide
multiple semantic representations in dynamic settings. We describe our
rationale for not pursuing a rate-based neural code and focus on systems
sensitive to the synchronous spikes of multiple neurons onto a target
neuron as the source of information sensing and transmission.

To develop coincident-sensitive neurons and networks, we start with
homogenous point neurons; homogenous in that the population of neurons
have the same statistical firing patterns and "point" meaning that they
have no morphology such as dendrites. We construct measures of
coincidence from simulations with these neurons. We then construct rules
governing the dynamics of a set of spiking neurons that will be used.
These rules describe a neuron's internal state over time, how it
responds to incoming spikes, and how it propagates spikes to its
targets. Given that we want to promote the recognition of coincident
spikes, we develop those learning rules and show how spiking networks
can self-organize.

Given our findings from point neuron networks, we explore the impact of
modulating a neuron's spiking sensitivity given its spiking history. For
example, if it has not fired an action potential recently, it can become
more sensitive so that it does spike. Contrastingly, if it has been
quite active and has been rapidly emitting spikes, we can dial its
sensitivity down. Relatedly, we explore modulating the emitted spike
efficacy onto a neuron's targets. In this area, we evaluate the effect
and possibilities of networks where even when a spike is generated, it
is not communicated to all targets. As well, we consider setting some
targets to receive more spikes than others depending on the source
neuron's spiking history. In this case, some targets may receive most of
the source neuron's spikes while other targets may only receive spikes
after the source neuron has generated several rapid spikes.

We also explore possible roles of dendrites. We describe how
coincident-based detection and processing can be optimized with neurons
with dendritic trees that accentuate positive signals with superadditive
rules and particular branching structures.

In all of our networks, our neurons follow Dale's Law that loosely
states that a neuron acts completely excitatory or inhibitory to all of
its synaptic targets [@dalePharmacologyNerveEndings1935]. (It cannot
excite some targets and inhibit others. It has to be one or the other.)
This allows us to explore the roles of excitation and inhibition more
distinctly. More to point, we argue that inhibition is not overtly to
decrease the firing rate in targets as much as it is to help coordinate
synchronous spikes between them. Another option for inhibition is to
gate propagating signals entirely.

We then show some working examples of these spiking networks, starting
with simple toy problems, to image problems, NLP datasets, and in
robotics.

And finally, we discuss the ramifications of this theory. We claim that
meaning is self-emergent in learning systems. This means that a learning
system's inputs, its internal computing capabilities, and the way it is
taught are critical to how it derives meaning. A spiking neuron need not
be passive and a network of active, spiking neurons is able to generate
and create novel behaviors. We realize it in our own lives, living under
a spiking brain. Therefore, we claim this a Strong AI, closely
resembling the way biological brains work. As such, there are hypotheses
for neuroscience that need further testing and opportunities to develop
AIs that interface with humans and behave in a more human-like way.

## Intelligence and the elements of an intelligent system

We subscribe to [Wikipedia's definition of
*intelligence*](https://en.wikipedia.org/wiki/Intelligence)
[@wikipediacontributorsIntelligenceWikipediaFree2021], which, at the
time of this writing, is:

::: displayquote
\...\[T\]he ability to perceive or infer information, and to retain it
as knowledge to be applied towards adaptive behaviors within an
environment or context.
:::

Given this definition, any intelligent system must contain the
following:

1.  **Sensors.** "The ability to perceive or infer information" requires
    the intelligent system to have sensing capabilities to recognize or
    differentiate information. We label any mechanism that performs this
    function a sensor. This may be a programmatic operation or rule;
    when executed, it "senses" an input and provides an output. A binary
    logic gate, for example, operates as a sensor since it monitors for
    a particular input pattern and emits TRUE or FALSE accordingly. We
    can also say that simple sensors, such as logic gates or rules, can
    aggregate into more complex groupings, such as circuits, to form
    more specialized and nuanced sensing. Such meta-sensors will be able
    to perceive or infer more complex forms of information.

    The role of sensing is to translate information into more meaningful
    and operational representations, possibly by transducing signals
    through other sensors in an encoding and decoding paradigm.

2.  **Memory.** "\...to retain \[information\] as knowledge" requires
    the intelligent system to have some means of storing information,
    making this content available to inform subsequent actions.

3.  **Rational output/action.** "\...to be applied towards adaptive
    behaviors within an environment or context" means that an
    intelligent system provides an output that makes reasonable sense
    given the context. This could be a simple decision or some sort of
    complex behavior depending on the system's capabilities.

    It may also be that the system's outputs are not overtly exposed to
    an external observer. Such behaviors may be strategizing, planning,
    imagining counterfactuals, or predicting possible futures. Since
    these behaviors are *informational*---they add internal meta-data
    and knowledge---the system may have a recursive or hierarchical
    characteristic; there must be sensors and memory to deal with the
    internal meta-information as well as the external information.

## Information, patterns, and meaning

Within our definition of *intelligence*, let us be clearer about
*information*. The Oxford English dictionary defines *information* as
"What is conveyed or represented by a particular arrangement or sequence
of things [@oxforduniversitypressInformation2021]." We will adopt this
definition of information and restate it more simply as "a pattern and
its meanings." Note that this definition of information, on its face, is
not the same as Shannon information, frequently used in digital designs,
which equates information as the rarity of a message/pattern and does
not directly consider *meaning*. Specifically, with respect to a
communication system, Shannon stated (emphasis theirs):

::: displayquote
Frequently the messages have *meaning*; that is they refer to or are
correlated according to some system with certain physical or conceptual
entities. These semantic aspects of communication are irrelevant to the
engineering problem. The significant aspect is that the actual message
is one *selected from a set* of possible messages
[@shannonMathematicalTheoryCommunication1948].
:::

Indeed, communication systems such as those Shannon was focused on,
chartered with the engineering task of optimally *transmitting*
messages, need not consider meaning. His focus was on the digitization
of a pattern and the computations to encode, decode, transform, and
transmit its bits; not to construct an intelligent system.

While embedding or encoding *meaning* may not be required in many
computational tasks, meaning is critical to intelligent systems. Using
the Oxford English Dictionary definition of *information* above,
*meaning* is the "conveyed or represented" part. This means that
intelligent systems go beyond pattern detection to also encode context
and representations. There is a comprehension of where a pattern fits in
relation to other patterns. But where a given thing fits relative to
other things is simply "a particular arrangement or sequence of things"
that we also recognize as a pattern. In short, **meaning is a pattern;
it is a *pattern of patterns.*** This is the most important point of
this whole treatise. When meaning is a pattern, the endeavor to
construct an intelligent system becomes the design and analysis of the
interplay between pattern detectors and generators. Any intelligent
system must have mechanisms to deal with and wield patterns of patterns
that encode deeper, varied, and more "meaningful" representations. Such
mechanisms may be engineered rules, such as programmatic instructions,
for example. In this case, "meaning" is ascribed by the engineers who
design the set of possible representations, decision paths, and outputs
of the system. And even though the system must output "adaptive
behaviors", those behaviors, and the selection criteria to exhibit them,
are rule-based in engineered systems. Indeed, our adopted definition of
intelligence affords that an intelligent system may be entirely
designed. We are interested, however, in learning systems that do not
require imposed designs.

Since any intelligent system must have mechanisms to operate with
patterns of patterns, when we compare designed/engineered systems to
agents that learn, it follows that learning systems must *create*
inferences, context, and representations. In short, any learning system
will also be able to demonstrate its learning through the creation of
patterns of patterns---both internal and external representations.
Therefore, in contrast to engineered systems where meaning is ascribed
by the designers, **meaning is an emergent property of learning
systems.**

Paradoxically, meaning-as-a-pattern is compatible with various
quantitative modern information theories, including Shannon's, when
meaning is a pattern that is encodable, decodable, and amenable to
computation. Therefore, while these information theories may not
directly care about semantics, they can certainly utilize meaningful
patterns
[@adriaansCriticalAnalysisFloridi2010; @adriaansInformation2020].

We have been using the word *pattern* quite liberally. To be more
specific, we will state that with respect to an intelligent system, **a
pattern is a discernible entity**. A pattern has a complement: every
foreground has a background; the detection of signal implies a
definition of what is not the signal; a plaid is described as
repetitive, lined grids, which is different from other repetitive
structures; etc. From the intelligent system's point of view, "the
ability to perceive or infer information" means something is only a
pattern when it is perceptible by the system. That is, it must
differentiate the entity, becoming sensitive to it as some *thing*. The
system must therefore contain (or develop) the sensors and their rules
and relationships to intelligently operate with these "things", which
may be even abstract, vague, internal concepts.

In a complex circuit composed of various sensors, each sensor may be
sensitive to its own unique set of inputs. In a signaling cascade, these
inputs may include the transformed signals of preceding sensors in the
circuit. When the downstream sensor activates following some set(s) of
transformed signals (and does not activate with other sets of signals),
it has effectively ascribed meaning to those sets by recognizing the
groupings as entities. That is, the conditions to activate a sensor are
semantic---they can be contrasted descriptively against the conditions
that do not activate the sensor.

The above arguments seem reasonable in the ideal case of strict input
signals followed by reliable sensor output. It is not so clear how
sensors and the larger system should accommodate noise. Intelligence is
about navigating a universe with incomplete information and noise. We
therefore recognize that noise and incomplete information are at least
factors for external sensors and the system's memory capabilities. We
will tackle noise later in our development, but claim that the deeper
sensors that process internal meta-information should also be resilient
to noise and accommodate incomplete information because it permits the
system to be more generally robust and adaptable.

In summary, *information*, *pattern*, and especially *meaning* are very
subjective terms relative to the intelligent system. Nevertheless, we
can describe any intelligent system and its processing of information as
a cascade of patterns through sensors in this patterns-of-patterns
framework.

## Intelligent system examples

To help illustrate the *patterns-of-patterns* framework in intelligent
systems, it is worth considering some examples.

### Deep Neural Networks

Deep neural networks (DNNs) illustrate our *patterns-of-patterns*
framework of an intelligent system in obvious ways. The neurons of a DNN
are sensors;[^1] the learned weights are its effective memory because a
new input will trigger related, historical activations; and its rational
outputs may include classifications, examples of possible futures (like
sentence completions and deep fakes), or other transformations.

Each neuron in a DNN is sensitive to the activations of a subset of
neurons in the previous layer. The weights between the neurons in the
previous layer and a target neuron in the next layer define how
sensitive the target neuron is to each neuron in the previous layer. The
target neuron is effectively blind to the source neurons in the previous
layer whose weights are near zero. And for those neurons that can
contribute because they have non-zero edges, the target neuron will
respond differentially depending on how those source neurons activate.
The target neuron may only be sensitive to a particular subset of
neurons being positive and another subset being negative, for example.
To that target neuron, it means something different when it is activated
by the pattern vs. when it is not. Via the backpropagation algorithm,
the target neuron gets punished less as it gets better at honing
relevant patterns and by how well the target neuron contributes to the
different neurons in the next layer. In DNNs, the "ultimate" meaning is
ascribed by the optimization task designed by the engineer and
implemented in the form of a loss function, but through learning, the
neural network develops several internal representations of meaning.
Each neuron in the filtering cascade participates in the building of
patterns of patterns.

As a more explicit example, let us consider the DNN, GoogLeNet
[@szegedyGoingDeeperConvolutions2015], used in image segmentation, as
illustrated by @olahFeatureVisualization2017. As that web page shows,
different neurons are sensitive to different pixel patterns. And
furthermore, the deeper the layers, the more we see the groupings of
earlier features: the first layers capture high-frequency edges,
followed by textures, then grid-like repetitions, parts, and finally,
objects. The neurons of one layer aggregate various combinations of
neurons in the previous layer. Each neuron is sensitive to a particular,
discernible set of neurons in the previous layer.

### TODO: Another example. Non-digital. *Aplysia*?

## Spikes, coupling, and neural coding strategies

*Spikes* are simply digitized events, a blip to signal something in
time. The term comes from electrophysiology recordings of nerve cells in
animals. When a source neuron fires an action potential to activate its
target synapses, it induces an abrupt electrochemical change that is
quite "spiky" compared to its non-firing state. An example of spikes in
electrophysiology recordings is given in Figure
[\[fig:kashiwadani_oscillations\]](#fig:kashiwadani_oscillations){reference-type="ref"
reference="fig:kashiwadani_oscillations"}, which shows the simultaneous
recording from two electrodes in a rabbit's brain, each trace capturing
the electrical activity of two different neurons. In this example, many
of the spikes of these two neurons oscillate and are synchronized as the
animal inhales an odor.

::: SCfigure
![image](kashiwadani_oscillations){width="60%"}
:::

In *spiking neural networks* (SNNs) such as brains, the spiking activity
of source neurons impacts the spiking activity of target neurons. While
each target neuron may be awash in a milieu of spiking activity from its
source neurons, it is ultimately charged with emitting its own spikes in
ways to modulate the system's behavior. These blips that "signal
something using time" are, therefore, each neuron's voice; the way it
shows acknowledgement of signals received, integrating its understanding
of the external environment as obtained primarily from spiking inputs,
parsing meaningful spikes from irrelevant, and to nudge the system
toward different output behaviors. Spiking neurons are, therefore,
perpetual decision-making relays that integrate and propagate signals
through their chirps.

Spiking neurons may not be as passive as the description above alludes.
As dynamical machines, they may be arbitrarily complex and capable of
integrating a variety of other external signals; not just spikes. They
may also have a variety of mechanisms to process incoming spikes
differentially, being more sensitive to spikes from some sources as
compared to others, or more sensitive with respect to the relative
timing of spikes. They may also have internal states that modulate their
sensitivity to incoming spikes, perhaps becoming more sensitive to
barrages of incoming spikes, for example. Or even the opposite could be
true, that they adapt and become numb to a barrage of inputs. When they
emit spikes, they might also be selective in their broadcasting,
reliably signaling most spikes to some targets and only signaling to
other targets under certain conditions. And they may even have default
active spiking behaviors, to maintain a default spiking frequency, for
example, or to spike regularly like an oscillator. In this regard, as
compared to the activation functions of artificial, deep learning
networks, which are typically utilized as passive filters, spiking
neurons can be *purposeful*. Locally, each neuron might have particular
optimal states of input and output behaviors and seek the environment
that provides this optimal state. And more globally, the spiking
network/agent may seek environments that balances the "desires"/optimal
states of its population of neurons. In short, spiking neurons use
spikes as their most immediate mechanism to interpret and respond to
their environment, but the way they integrate and incorporate these
signals and emit spikes may be quite variable.

Even though neurons of animal brains use spikes as their most immediate
communication protocol (other cell-signaling mechanisms can also be at
play albeit at slower timescales), as previously noted, the brain
probably uses spikes in a variety of ways. The rationale for spikes
being used in diverse ways is severalfold: Principally, the brain has
several types of neurons, each type with its own genetic makeup of ion
channels and dendritic and axonal morphologies that result in different
dynamical properties that provide a variety of input sensitivities and
spiking characteristics. Some cortical pyramidal neurons, for example,
toggle between a bursting mode where they emit a series of rapid spikes
under some conditions and a non-bursting mode where they fire one to a
few spikes when activated [@zeldenrustNeuralCodingBursts2018]. As well,
particular types of neurons frequently form microcircuits that belong to
distinct brain regions to carry out specific functions
[@HandbookofBrainMicrocircuits]. These microcircuits may generate their
own rhythms and distinct spiking characteristics. In short, brains are
composed of many types of neurons with different types of connections,
properties, and subnetworks that dictate their firing characteristics
and capabilities. As neuroscientists attempt the daunting task to
decipher the neural code as emitted by neurons of the brain, they
effectively take recordings of the brain under different conditions,
accompanying stimuli, and known characteristics of the cells and the
underlying network, then attempt to tease the information being
transmitted
[@bialekReadingNeuralCode1991; @stanleyReadingWritingNeural2013].

Measures of information often incorporate Shannon's notions of encoding
and decoding. Spike encoding tasks are often expressed as a conditional
probability: $P\left(y|x\right)$ where $y$ is the response---the spiking
output of a neuron or some set of neurons, for example, and $x$ is the
stimulus. In neuroscience studies, the stimulus is often external and
may be far removed from the neurons being measured. For example,
researchers may study the visual cortex of an animal in response to an
image presented. In this case, photons activate neurons of the retina
that modulate perhaps thousands to millions of other neurons in a
cascade that eventually modulates the cortical neurons being measured.
Decoding is cast as the inverse conditional probability:
$P\left(x|y\right)$, the probability of the stimulus, $x$, given the
observed response, $y$. Together, Bayes formula,
$P(x|y) = \frac{P(y|x)P(x)}{P(y)}$, can be applied in encoding/decoding
paradigms.

While neuroscientists often cast experimental results into an
encoding/decoding paradigm to help with their interpretations of
recorded phenomena, it is difficult to find evidence that the brain
really operates in this statistical realm
[@bretteCodingRelevantMetaphor2019]. In the Shannon sense, encoding is
the transduction of a pattern and decoding is the process to recreate
that input pattern at the other end. But using statistics to find
optimal compression and codec schemes, perhaps accommodating noise, is
not the objective of organisms with brains or many intelligent systems.
In short, decoding is not usually the objective. Instead, for animals,
interfacing with the world in ways to help them gain and survive is the
objective. This means that the better these types of systems are at
interpreting their inputs, possibly understanding themselves and their
relationship to that world, and manipulating feasible responses, the
more likely they will gain and survive.

### Coupling

As noted above, the microcircuits and connectivity in a spiking neural
network impacts its dynamics, so the network topology also plays a role
in the interpretation of spiking neural codes. There are many ways
neurons of a spiking neural network can be coupled, but neural coupling
broadly falls into two categories: *directly* connected and
*functionally* connected. In the context of the brain, we will say that
a directly coupled network exists when neurons connect via synapses or,
rarely, electrophysical coupling via gap junctions. Functional
connectivity exists between subsets of neurons when they participate in
a patterned signaling cascade. When the activity of an upstream neuron,
not directly connected to a downstream neuron, influences the activity
of that downstream neuron, we can say those neurons are functionally
connected. Ultimately, all neurons must have specific subsets of direct
connections to participate in a functional cascade.

### Rate coding

While spikes may be encoded in several ways, neuroscientists frequently
promote the *rate code*. This is because they observe neurons spiking
faster following many stimuli. In patch cell recording, for example, it
is possible to inject electrical current into a single cell to induce it
to fire rapidly. But evidence of neurons firing faster or slower is also
seen without direct electrode stimulation. An early example came from
@hubelReceptiveFieldsSingle1959 where they recorded from single neurons
in the cat V1 visual system showing that particular neurons spiked
faster or slower depending on the angle and motion of a visible bar
presented to the cat. Certain neurons were sensitive to bars at a
particular angle moving across the visual field and would spike faster
when the bar was at the sensitive angle, and as the angle of the bar
deviated from that angle, the neuron would spike less frequently. This
indicated a "tuning curve" for a neuron's response; that firing rates
were indicative of the sensitivity to specific inputs to which the
neuron was tuned.

Paradoxically, rate coding is problematic for systems operating in time.
While neurons of the brain have been observed to fire faster and slower
under various conditions, within the context of a behaving system, the
rate code is unlikely to be the primary readout for downstream neurons
[@brettePhilosophySpikeRateBased2015; @guoNeuralCodingSpiking2021]. To
illustrate the problem, imagine swatting at a fruit fly. If your swipe
is longer than 100 ms, the fly will likely escape. Within that
time---literally, the blink of an eye---the fly senses danger, makes an
escape plan, jumps in a particular direction to get aloft, coordinates
the flapping of its wings, and propels away [@fryeFlyFlightModel2001].
Using some armchair calculations, even if all 100,000 neurons of the
fly's brain were devoted to this evading behavior and each neuron was
capable of firing at a very fast rate of 200 Hz, each neuron would emit
between 0 and 20 spikes in the 100 ms window for its coding. For
simplicity, if we say that sensing danger, making an escape plan,
jumping in a particular direction, and flapping wings are subsequent
stages taking the same time, then that leaves 25 ms for each stage,
resulting in 0-5 spikes from each neuron to encode the fly's actions.
This also assumes that each stage can emit and induce the correct,
coordinated action in one synaptic relay and does not need other
intermediate layers of processing. Indeed, it seems quite problematic
for organisms that operate in real-time to adopt such rate codes because
for a target neuron to evaluate fast vs. slow spiking from its inputs
requires a window of time that delays its own output. In a signaling
cascade, these delays compound so complex behaviors such as those
exhibited by the evading fruit fly would be impossible to carry out
using purely rate coding.

### Other sequential patterning

Earlier, we characterized *pattern* as the "particular arrangement or
sequence of things". In the "sequence of things" camp, we can also
imagine a single neuron emitting particular spiking patterns: fast,
slow, rhythmic, or even something like Morse code. But as just
discussed, these patterns only induce delays for a downstream neuron
listening to this neuron trying to discriminate its signals.

However, in this "sequence of things" camp, we can also imagine
ensembles of neurons participating in a chain reaction. For example,
given neurons labeled $A$, $B$, and $C$, if $A$ spikes, then $B$ spikes,
and then $C$ spikes, then pattern $A-B-C$ could mean something different
from the opposite activity, $C-B-A$. Or, perhaps $A$ chirps for a while
before completing the pattern: $A-A-A-A-A-B-C$. So, the order of the
sequence may matter, repetitions may matter, and durations could even
matter. Indeed, we recognize from our own experience and understanding
of how brains direct actions that sequential spiking patterns must
encode memories, future imaginations, and induce behaviors. The evading
fruit fly example illustrated above requires very complex, coordinated
sequences of neuron signaling. We argue that such sequential patterns
are the result of *functional* coupling that occurs through a system of
*directly* coupled connections. That is, a target neuron is not
monitoring several source neurons across relatively long time-windows to
decide its output. Rather, the target neuron is tuned for much more
immediate responses to its directly connected sources; and the network
is configured such that the neuron may participate in many arbitrarily
complex temporal patterns at the system level operating through these
functional cascades.

### Coincidence as a "particular arrangement" pattern

We develop a *coincidence*-based code as an alternative to any code that
involves long time-scale integrations such as the rate code. In this
alternative case, a target neuron is sensitive to particular populations
of other neurons that are activated simultaneously. As Figure
[\[fig:kashiwadani_oscillations\]](#fig:kashiwadani_oscillations){reference-type="ref"
reference="fig:kashiwadani_oscillations"} shows, in animal brains, some
neurons that respond to the same stimulus will fire rapidly (in this
example, around 30 Hz), but moreover, it is the synchronous behavior
that may propagate signal to downstream targets. As a simplistic
illustration, a target neuron that has synaptic connections from these
two input neurons may itself only be activated when these two neurons
spike together; shift one of these spike trains out of phase to the
other so that they do not synchronize and the target neuron would remain
silent. In this paradigm, the target neuron could immediately fire an
action potential following the first set of coincident spikes that it
detects. Coincidence---at least for positive, activating spikes---is
therefore an AND operation. The role of a series of subsequent
coincident spikes as observed in Figure
[\[fig:kashiwadani_oscillations\]](#fig:kashiwadani_oscillations){reference-type="ref"
reference="fig:kashiwadani_oscillations"} may be to continue to activate
the downstream neuron and encode the duration of the AND signal. The
repeated synchronized spikes may also add insurance that the signal is
propagated.

Going back to our definition of information---what is conveyed or
represented by a pattern---we have discussed ways of interpreting and
representing patterns with spikes. When two or more neurons share a
common, driving signal, then their output spikes will tend to co-occur
more often than by chance. That must mean something. Something is
triggering these neurons to behave similarly. To derive that meaning and
modify system behavior if need be, we need a target neuron to connect to
those neurons that sporadically fire together, to recognize when they
are synchronized, and respond accordingly. The target neuron, in turn,
can participate in subsequent downstream signaling with other neurons
that are sensitive to other signals that happen to be co-occurring. It
is in these ways that associations can be made to integrate many
elements in a dynamic context.

## Sensors and the digitization of events

But why spikes? How and why did biology come upon spikes and evolve
nervous systems and all they comprise (hemispheres, spinal cords, nerve
cells and their neurite growth, dendritic trees and spines, axonal
projections and myelin sheaths, electrochemical properties, synaptic
formation, the myriad of neuron types distributed in particular regions
within a dynamic network to form microcircuits, etc., as well as
supporting cells like glia and anatomical features like cerebral-spinal
fluid, the blood-brain barrier, and skulls to protect it)? The human
brain consumes 2/3rds of the energy used by the body [@TODO] (which is
still only as much energy as a 40-watt lightbulb [@TODO]). All of this
effort and complexity is directed at spike communication and processing.
But are spikes simply an evolutionary kludge? That is, as with many
components of biological organisms, simple mechanisms are coopted and
jury-rigged into ever more complex systems that are more evolutionarily
advantageous than their ancestors' solution, but nevertheless, may fall
short of an optimal design [@dawkinsBlindWatchmakerWhy2015]. Do spikes
fall into that category?å Does the complexity of the nervous system
indicate the importance and diverse possibilities of spikes, or is this
complexity the outcome of a slow accrual of haphazard computational
mechanisms available given biophysical, chemical, cellular, and
evolutionary constraints?

Any answers to the questions above will not only help us better
understand the brain, but they will also assist with bio-inspired
designs. Nature evolved various means for locomotion---numerous types of
wings, legs, and slithering ways to get around---but humans still
invented the wheel. Analogously, nature evolved the spike and humans
invented bits. We do not yet understand the computational capabilities
of spikes to determine where they might be used over computational
architectures using bits. Broadly speaking, spikes are rather imprecise.
Sure, the blip/moment of the spike may be precise, but like bits, what
they represent is abstract. And in the case of spikes, the timing could
be important. A spike shifting 1 millisecond (or attosecond!) in time
may be critical to the computations that use it, or it may not. While
computing with spikes or bits are inevitably areas of neverending
research, when it comes to spikes, we currently lack a basis to build a
computational framework. In this section, we aim to establish such a
basis.

### Events

As stated earlier, spikes are blips to signal something in time. It
might be more accurate to say "something *using* time." Both expressions
apply, but *in* time connotes something more timely and relevant in the
moment. This may often be the case in biological brain functions and
real-time systems, but *using* time permits time to be an arbitrary axis
to portray positive bits. That is, historical time is a substrate of the
encoding, but one could replace time with another dimension, like 1D
space, and our general theory would hold. While *using* time may be
generically applicable to our theory, we will remain with *in* time for
its more obvious and relevant notions of systems operating in real-time.
Therefore, spikes are encodings of *events*, where an event is an
encapsulation of a pattern along one dimension, time.

### Sensors that recognize events and emit spikes

In a spiking neural network, the neurons are sensors that recognize
particular temporal events and emit a spike to connote their
recognition. But what does it mean to recognize an event? And why should
spikes be the output? Let us first consider the recognition of an event.
We can cast any cause-and-effect paradigm as sequential stages of two
temporal patterns. The effect operates as a *sensor* to the
cause---without a cause, the effect does not happen; and when the effect
is observed, it indicates the presence of a cause. (Granted, there may
be several different causes that induce a particular effect and a single
cause that can produce several different effects. We will ignore the
many-to-many issue for the moment. As well, we will currently ignore
probabilistic effects where the fidelity of the effect following a
response is not perfectly realized---where the effect may occur
spontaneously without the cause, be delayed, have different
characteristics with each repeated trial of the cause, etc. For the
purposes of the current discussion, some cause is adequately defined and
induces some effect, which is also adequately defined; it is
deterministic.) We will adopt propositional logic

With $P_{i} \rightarrow Q_{j}; i,j \in \mathbb{R}$

## Causes and Effects

We can cast any cause-and-effect paradigm as sequential stages of two
temporal patterns. Our ability to ascribe a label to each event, one for
"cause" and one for "effect", acknowledges each event as a distinct
pattern/entity. Additionally, given our semantic understanding of these
labels, we also understand the relational pattern that *cause* precedes
and induces *effect*. Because of this relationship, we actually have 3
patterns: 1) the first event, which is the cause; 2) the second event,
the effect; and 3) the rule that this particular cause pattern induces
the effect pattern. (Anything that can be described, including rules,
exists as an entity.) It is the third pattern that is necessary for
intelligence. It is the "pattern of patterns"; the pattern that ascribes
meaning between the two events.

The sensing and encoding of these patterns occurs in the intelligent
agent.

In this paradigm, to a remote observer, Again, an intelligent system
must acquire patterns of patterns.

Granted, it could be that one event follows another completely
independently and because they occurred independently, theyand because
thsoe two events are independent, they are

In a cause-and-effect paradigm, the cause may be opaque to a remote
observer. In this case, to an outside observer, only the effect is
capable of being monitored while the cause is indiscernible. However,
from the perspective of the effect, the cause is discernible---the
effect happened because it responded to the cause. The rules governing
the effect's response can be viewed as its sensing capabilities. "I
tripped and fell down" is a cause and effect. My tripping caused the
effect of me falling down. If you, an external observer, see me falling
down, you may have not seen me stub my toe on the sidewalk crack (the
cause). The falling down pattern is the behavior my body goes into when
it obeys the rules of gravity.

when the cause is recognized as present, the effect will happen and when
it is not recognized as present, then the effect will This
effect-as-causal-sensor argument

### Digitization of events

They are discernible in the sense that we can discuss them as separate
entities, but, admittedly, both may be ill-defined without the
vocabulary and grammars to really operationalize anything given these
patterns.

Furthermore, we can say that the effect serves as a sensor to the cause;
for the effect to be a thing indicates a probable cause. There may be
several causes that can induce the same effect. And there may even be a
set of unknown causes that induce an effect. Perhaps the causes are
completely unknown.

The effect, which is some sort of change, indicates the presence of a
probable cause, while no effect indicates the absence of such a
stimulus. In short, we can label any stimulus-response, probabilistic
toggle, or whatever expression we want to use to describe a
cause-and-effect event as a pattern.

## Overview of our approach

We have presented several concepts above that demand further
development. While we have incorporated understandings of the brain as
obtained through neuroscience, our objective is not to reverse-engineer
the brain. Instead, we digitize events into spikes and ask how we can
build the computational units to recognize and emit spikes, configured
in a network that behaves intelligently. In this dynamical system, we
desire some units to emit their spikes together when they need to, but
then to de-synchronize when necessary, followed by some of those neurons
to synchronize with other subsets in subsequent signaling.

[^1]: Note that many intelligent systems refer to their networked
    computational units as "neurons". While the role of neurons in these
    systems is to detect and direct signal, these systems employ neurons
    that perform drastically different computations. Importantly for our
    discussion, a neuron in a DNN is quite different from a spiking
    neuron like those used by the brain, despite the insistence from
    many in the Deep Learning community that that's "how the brain
    works" [@Believers2015; @NeuralNetworksDeep]. This misconception
    persists due to the assumption that brain neurons signal downstream
    neurons through their rate, which, as we discuss later, is
    problematic.
