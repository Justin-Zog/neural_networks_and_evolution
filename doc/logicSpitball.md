I thought it would be important to mention that this project was in large inspired by
David Randall Miller's project and youtube video that can be found here:
>https://www.youtube.com/watch?v=N3tRFayqVtk

# Neural Network and Creature Creation Logic

#### By Justin Herzog

## Big Picture

**"In order to understand the finer details of a program one needs to understand what the big
picture is.  Imagine drawing a conifer tree with simple shapes. Just a green triangle for the 
needles and a brown square for the trunk."**

**- Justin Herzog**

### Part 1: Learn From Nature

From the big picture perspective we can look at the physical world around us.  What are
the conditions necessary for **evolution and natural selection**?  I will list them below:

- **Replication**: Each creature must have a way to self-replicate. In the real world we see 
this through all living things. A fungus might use spores, microscopic creatures may just
duplicate. Humans, dogs, and other species require 2 species to combine their DNA to replicate.
- **Blueprint**: Every creature that is born has to be constructed from a blueprint. In
the real world DNA is carefully organized to create the genome.
- **Inherit Blueprint**: The blueprint must be passed from the **replicator** to the
**inheritor**. In other words, the blueprint must have a way to be inherited from
generation to generation, parent to child, duplicator to duplicate.
- **Mutation**: In order for evolution to be possible, the occasional mutation must occur.
In nature, a single letter of a small part of the DNA may change, resulting in a different
genome.
- **Selection Method**: Who gets to reproduce and who does not. We see in our life and in
nature, those who avoid predators, find food, shelter themselves from nature, overcome
illness and injury, and have the skills necessary to find a mate are the ones who reproduce.
Thus, only the best of traits are passed down from generation to generation.

### Part 2: Attempting To Replicate Nature

#### Replication

&nbsp;&nbsp;&nbsp;&nbsp;Replication should be rather easy in our program. In order to replicate
things we can simply use a class:

```
public class Creature
{
  Coord location;
  Genome genome;
  NeuralNetwork brain;
  Nodes[] availableNodes;
}
```

Now we can just initialize a new 'Creature' object whenever we need.

#### Blueprint

&nbsp;&nbsp;&nbsp;&nbsp;Our creatures blueprint will be contained within a `Genome`.
Each `Genome` will consist of 'DNA' or genes. A single `gene` will be represented by an 8 digit
hexadecimal number that represents 32 binary bits of data. This `Genome` is then fed to a 
function that creates a `NeuralNetwork`, or artificial brain, from the blueprint.
Here is an example of a `Genome`:

```
    28e90f86 44bf3514 32430165 b19a8864
```

#### Inherit Blueprint

&nbsp;&nbsp;&nbsp;&nbsp;I would imagine that, just like the real world, this will differ 
from artificial creature to artificial creature. I as the creator of these artificial creatures
will give them different requirements as I see fit.

#### Mutation

&nbsp;&nbsp;&nbsp;&nbsp;Every so often, a letter or two of a creature's `Genome` will be changed
in a way that is random. I will set the odds for this per simulation for now, 1/1000 chance
seems like a good statistic to go by.

#### Selection Method:

&nbsp;&nbsp;&nbsp;&nbsp;This will differ not only from creature to creature, but also from
simulation to simulation. 

&nbsp;&nbsp;&nbsp;&nbsp;Since the overall goal of this project is to create advanced creatures
for an ecosystem video game I will give special selection methods based on their role in the
ecosystem. ie: If they can follow a pheromone trail, avoid a predator, find food, etc.

### Part 3: Neural Networks

#### Intro

&nbsp;&nbsp;&nbsp;&nbsp;All information in this section is coming from a lecture by neuroscience
professor John H. Byrne, Ph.D. from McGovern Medical School. As of August 20th 2023, the lecture can be found at this link:

>https://nba.uth.tmc.edu/neuroscience/m/s1/introduction.html

&nbsp;&nbsp;&nbsp;&nbsp;Big picture, what is a neural network? In nature, a neural network is
a group of connected neurons.  Neurons are connected to each other through synapses. These
connections are either *excitatory* or *inhibitory*.

&nbsp;&nbsp;&nbsp;&nbsp;Additionally, the human brain is a rather complex organ. Unlike the liver,
where most of the cells are doing the same thing. Neurons in our brain all have specific jobs 
or functions. Neurons in the occipital cortex are involved in the processing of visual information,
neurons in the temporal lobe are involved in memory function, neurons in the frontal lobe are
involved in planning, and the list could go on. If we look at it like this, it is almost as if the
brain is hundreds of different organs each carrying out their own specific functions. Therefore,
the creatures in our program should have 'different' neurons that perform different tasks. We
will refer to a neuron in our program as a `node`.

##### Basic Neuron Structure

&nbsp;&nbsp;&nbsp;&nbsp;For the purpose of this program, a neuron cell can essentially be defined
based on three different aspects.
1. **Soma**: Also known as the *cell body*, the soma is where most of the general cell functions
take place. In our program, a `node` must have some sort of soma-like functionality. In computer
programs, the soma is often mimicked using **activation functions**. *More on these later.
2. **Dendrite**: The dendrite is a branch-like structure that extends from the *soma*, (cell body).
The dendrites are where the neurons receive connections. In our program, a `node` must have a
dendrite-like structure.  This should not be too hard to implement, we can visualize this as
a single point with lines, or *edges*, going into our `node`. I will call one of these an 
`input edge`. Each `node` will contain an array of `input edges`.
3. **Synapse**: The synapse is also a branch-like structure that extends from the neuron.
It is at the synapses where information transfer occurs between two neurons. Traditionally,
the synapse will connect to a dendrite and the neurons will be able to transfer information.
In our program, therefore, a `node` will also need *edges* going out of our `node`. I will call
one of these edges an `output edge`. Each `node` will contain an array of `output edges`.

##### Resting Potential and Action Potentials

&nbsp;&nbsp;&nbsp;&nbsp;This interesting bit of information can be summed up in a short paragraph.
There was an experiment done that essentially stuck two very tiny electrodes in a neuron. The first
electrode I will call the *measuring electrode* the second electrode is called the *stimulating*
electrode. The stimulating electrode is connected through a switch to a battery. The basis of
the experiment consists of closing the switch with a small battery and slowly increasing the battery
size while measuring the depolarization of the neuron.

&nbsp;&nbsp;&nbsp;&nbsp;The important part to learn from this experiment, within the scope of
our program, is that when the battery gets large enough, the depolarization caused by the battery
is sufficiently large enough to trigger an *action potential*, also known as a *spike* or an *impulse*.
This action potential can be equated to the neuron firing off. In neuroscience the voltage at which
the depolarization becomes sufficient enough to trigger an action potential is called the *threshold*.
If a larger battery is used to generate a supra-threshold depolarization, a single action potential is
still generated and the amplitude of that action potential is the same as the action potential
triggered by a just-threshold stimulus. However, the frequency of action potentials is higher.
From the video we learn 'that the greater the magnitude of the stimulus, the greater is the number
or frequency of action potentials. That's called *frequency coding on the nervous system*'.

We learn from the *frequency coding on the nervous system* that the
greater the intensity of a stimulus, the greater the number of action potentials that
are passed to the brain.

##### How Nerve Cells Are Connected To Each Other

&nbsp;&nbsp;&nbsp;&nbsp;There are two different ways that neurons talk to each other.
1. Excitatory connection
2. Inhibitory connection

&nbsp;&nbsp;&nbsp;&nbsp;The difference between these two connections can be explained using 
our battery experiment above as an analogy. When an excitatory connection is triggered, an 
action potential is sent as a 'positive' charge, similar to our battery, to the receiving neuron.
When an inhibitory connection is triggered, an action potential is sent as a 'negative' charge,
opposite to our battery, to the receiving neuron. Therefore, when an excitatory connection is
triggered at the same time, or shortly after an inhibatory connection is triggered, it reduces
the likelihood that the receiving neuron will fire off an action potential.

##### MicroNetwork Motifs

&nbsp;&nbsp;&nbsp;&nbsp;*MicroNetwork Motifs* is just a fancy word that means: despite the tens
of thousands of different ways that neurons can connect to each other, there is a certain number
of common motifs that occur very frequently in neural circuits used by the neural system.
**!!! The video mentioned above includes pictures and more information of common motifs !!!**.

&nbsp;&nbsp;&nbsp;&nbsp;It is also important to note in the video that the professor mentioned
some sort of *NanoNetwork* that involves biochemical processes. He talks about neurons that have
a so called *endogenous*, or bursting, behavior. Which means that in the absence of any input,
these *endogenous* neurons will still fire bursts of action potentials, become silent again, then
fire bursts again. He said that these type of rhythmic behaviors could be in charge of respiration.
To relate what I understood to our program, I think I will need to include a fourth type of `node`.
This new `node` would send a 'value' through its `output edges` even if it has received no input.

##### How can we incorporate this in our program?

&nbsp;&nbsp;&nbsp;&nbsp;Keep in mind that a neuron in our program is called a `node`. A neuron
has `dendrites` that feed a 'charge' to the `soma`. If the charge given to the `soma` is high enough,
a signal is sent through the `synapses` of the neuron and received by the `dendrite` of another `neuron`.
Similarly, our `node` has `input edges` that feed a 'charge' or value to the `node`. The node
then takes the sum of the values given to it and sends them through an `activation function`.
This value is then sent through the `node's` `output edges` and received by another `node`.






## Random Thoughts

This section is for random thoughts that do not correspond with any section I have created so far.

### Dynamic Nodes

##### Intro

&nbsp;&nbsp;&nbsp;&nbsp;While thinking about David Miller's evolution and simulation video I came
up with an idea. It spawned from me 'not liking' or rather thinking there had to be a better way
to implement some of his nodes. David's nodes, while they did work quite well for his project, are
not very dynamic. For example, in his video when explaining certain nodes, he mentions how there is one
that causes a creature to move west, one that causes the creature to move in a random direction, one
that causes the creature to move south, east, north, etc. While it is good that this allowed the creature
to move, when a creature hit a wall or some other obstacle it would have to 'luck out' by having a very
specific setup in its neural network that allowed it to somehow move around it. This led to the creatures
not being very adaptable, especially if they could only move in one direction. In order to compensate for
this inefficient method I thought of `Dynamic Nodes`.

##### What is a Dynamic Node?

&nbsp;&nbsp;&nbsp;&nbsp;A `dynamic node` is a node that, along with sending a numerical value (the charge),
such as a `float`, it would also be able to send other critical information along the line. `Dynamic nodes`
could theoretically allow creatures to exhibit behaviors that are influenced by sensory input, leading to
more sophisticated and adaptive responses to their environment than the `classic node` could.

&nbsp;&nbsp;&nbsp;&nbsp;In my head I was trying to think of a solution to the problem of a creature being able to 'see' a 
dangerous creature. How might it 'know' when something is dangerous or not? Coding real eyes and 
trying to simulate that is out of the question. I then thought about how many wild animals have a tendency
to run away from anything that 'looks' intimidating. 

This led me to create an `input node` that would essentially be able to sense how close a 
creatures `genome` is to ones own and fire a signal based on that. Ideally, this *senseGenome* node 
would also tell the creature to either move *away* or *towards* the creature with a different `genome`. 
In my mind, this would create a kind of 'cat and mouse' effect. Causing the predator to move towards 
the prey, and the prey to move away from the predator. However, to effectively do this, I would need 
the idea that I had of nodes in my head to evolve from 'move east', 'move random direction', etc, to 
evolve into something more like 'move away from `x`' or 'move towards `x`'. `x` could be a certain
pheromone, color, light-source, biome, etc. Hence, `dynamic nodes` were created.

##### How Is Data Passed From Node To Node?

### Sensing Predator and Prey

##### Sense Carnivore Node

&nbsp;&nbsp;&nbsp;&nbsp;In my **What is a Dynamic Node?** section I mentioned a node that sensed genetic
diversity. While this is a better way to 'predict' whether something is a predator or not, it is far from
optimal. This led to me thinking about how there is most likely going to be something specific in any
given creatures `genome` that can determine whether it is a 'carnivore' or not. A creature would then only
need to sense whether the creature is a carnivore, omnivore, or herbivore, to know if it is in danger.
 

