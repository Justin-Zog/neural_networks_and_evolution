I thought it would be important to mention that this project was in large inspired by
David Randall Miller's project and youtube video that can be found here:
>https://www.youtube.com/watch?v=N3tRFayqVtk

# Neural Network and Creature Creation Logic 

#### By Justin Herzog

**"In this world, is the destiny of mankind controlled by some transcendental entity or law? Is it like the hand of God
hovering above? At least it is true, that man has no control, even over his own will." -Void of the God Hand**

# Big Picture

**"In order to understand the finer details of a program one needs to understand what the big
picture is.  Imagine drawing a conifer tree with simple shapes. Just a green triangle for the 
needles and a brown square for the trunk."**

**- Justin Herzog**

## Part 1: Learn From Nature

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

## Part 2: Attempting To Replicate Nature

### Replication

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

### Blueprint

&nbsp;&nbsp;&nbsp;&nbsp;Our creatures blueprint will be contained within a `Genome`.
Each `Genome` will consist of 'DNA' or genes. A single `gene` will be represented by an 8 digit
hexadecimal number that represents 32 binary bits of data. This `Genome` is then fed to a 
function that creates a `NeuralNetwork`, or artificial brain, from the blueprint.
Here is an example of a `Genome`:

```
    28e90f86 44bf3514 32430165 b19a8864
```

### Inherit Blueprint

&nbsp;&nbsp;&nbsp;&nbsp;I would imagine that, just like the real world, this will differ 
from artificial creature to artificial creature. I as the creator of these artificial creatures
will give them different requirements as I see fit.

### Mutation

&nbsp;&nbsp;&nbsp;&nbsp;Every so often, a letter or two of a creature's `Genome` will be changed
in a way that is random. I will set the odds for this per simulation for now, 1/1000 chance
seems like a good statistic to go by.

### Selection Method:

&nbsp;&nbsp;&nbsp;&nbsp;This will differ not only from creature to creature, but also from
simulation to simulation. 

&nbsp;&nbsp;&nbsp;&nbsp;Since the overall goal of this project is to create advanced creatures
for an ecosystem video game I will give special selection methods based on their role in the
ecosystem. ie: If they can follow a pheromone trail, avoid a predator, find food, etc.

## Part 3: Neural Networks

### Intro

&nbsp;&nbsp;&nbsp;&nbsp;All information in this section is coming from a lecture by neuroscience professor 
John H. Byrne, Ph.D. from McGovern Medical School. As of August 20th 2023, the lecture can be found at this link:

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

### Basic Neuron Structure

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

### Resting Potential and Action Potentials

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

### How Nerve Cells Are Connected To Each Other

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

### MicroNetwork Motifs

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
This new `node` would send a 'value' through its `output edges` even if it has received no input. I
will call this node an `oscillatory node`.

### How can we incorporate this in our program?

&nbsp;&nbsp;&nbsp;&nbsp;Keep in mind that a neuron in our program is called a `node`. A neuron
has `dendrites` that feed a 'charge' to the `soma`. If the charge given to the `soma` is high enough,
a signal is sent through the `synapses` of the neuron and received by the `dendrite` of another `neuron`.
Similarly, our `node` has `input edges` that feed a 'charge' or value to the `node`. The node
then takes the sum of the values given to it and sends them through an `activation function`.
This value is then sent through the `node's` `output edges` and received by another `node`.

# Small Picture

## Types of Static Nodes

### Notes and Questions

#### Should Output Nodes Send Output to Other Nodes?

&nbsp;&nbsp;&nbsp;&nbsp;Originally, I thought it would be pretty stupid for output nodes to be able to connect to other
nodes. For some reason, it just did not make sense to me. However, I started thinking about how I could create an output
within my own body that creates an input. For example, if I decide to touch my skin, the output would be me moving my
hand to touch my skin, the input would be the skin 'feeling' the sensation and starting a chain reaction. For this
reason, I decided to allow output nodes to send output to other nodes.

That analogy does not actually make much sense to me now... Maybe I'll have some sort of switch that will restrict, or 
allow those connections to be made.

### Abstract SuperClass Node
Here is the abstract class that all nodes will inherit from:
```
public abstract class Node
{
    string NodeID { get; private set; }
    public List<Node> InputNodes { get; private set; }
    public List<Node> OutputNodes { get; private set; }
    
    public Node(string nodeID)
    {
        NodeID = nodeID;
        InputNodes = new List<Node>();
        OutputNodes = new List<Node>();
    }
    
    public void AddInputNode(Node inputNode)
    {
        InputNodes.Add(inputNode);
    }
    
    public void AddOutputNode(Node outputNode)
    {
        OutputNodes.Add(outputNode);
    }
}
```

### Input

### Hidden

### Output

### Oscillatory

Similar to the *endogenous* neurons mentioned above, the `oscillatory node` will be a subclass of `Node` and will look
something like this:
```
public class OscillatoryNode : Node 
{
    // How many game steps the node takes to fire off
    private float period;
    private int currentStep;
    
    public OscillatoryNode(int period)
    {
        this.period = period;
        this.currentStep = 0;
    }
    
    public float sendOutput()
    {
        // Simulate sending the output signal
        return 1.0f;
    }
    
    public void Step()
    {
        currentStep++;
        
        if (currentStep >= period)
        {
            SendOutput();
            currentStep = 0;
        }
    }
}
 ```

## Types of Dynamic Nodes

### Input

### Hidden

### Output

### Oscillatory

### Temporary Input

# Random Thoughts

This section is for random thoughts that do not correspond with any section I have created so far.

## Dynamic Nodes

### Intro

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

### What is a Dynamic Node?

&nbsp;&nbsp;&nbsp;&nbsp;A `dynamic node` is a node that, along with sending a numerical value (the charge),
such as a `float`, it would also be able to send other critical information along the line. `Dynamic nodes`
could theoretically allow creatures to exhibit behaviors that are influenced by sensory input, leading to
more sophisticated and adaptive responses to their environment than the `classic node` could.

&nbsp;&nbsp;&nbsp;&nbsp;In my head I was trying to think of a solution to the problem of a creature being able to 'see' 
a dangerous creature. How might it 'know' when something is dangerous or not? Coding real eyes and 
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

### Figuring Out Which `x` A Dynamic Node Chooses

I suppose to find out which `x` a `dynamic output node` chooses, a `dynamic input node` and `dynamic hidden node`
would also have to be created. In my head this looks like the input node collecting key information about what it is
sensing and passing it down the line.

I know that when I see something, that input is coming from my eyes. As of now, these creatures have no way of knowing
what input triggered which output. Not ideal. To compensate for this, every input node will send a `reference tag` along
with the `charge`. This `reference tag` will be passed down the chain so when an `output node` is activated it will
know what `input` lead to it being activated. Obviously more information needs to be passed down, but that will differ
depending on what kind of input is being sent. Here is a non-exhaustive list of potential additional data points that
an input would send pass along:
- Distance Stimulus: Information about the distance of the sensed stimulus.
- Direction Stimulus: Information about the direction the stimulus is coming from.
- Intensity Stimulus: Quantifies the intensity or strength of a stimulus. (Useful for smell or sound)
- Type of Stimulus: Sends the type of stimulus being sent. (eg: scent, visual cue, sound)
- Internal State: Info about the creature's internal state, such as hunger, fear, stress, etc.

The additional stimulus could also affect the weight of the data being sent. For example, if a predator is far away from
the creature it might only act cautiously instead of immediately running away. This could be caused by the additional
weight (mimicking urgency) that the additional data points add to the 'charge' that is sent.

### Sense Carnivore Node

&nbsp;&nbsp;&nbsp;&nbsp;In my **What is a Dynamic Node?** section I mentioned a node that sensed genetic
diversity. While this is a better way to 'predict' whether something is a predator or not, it is far from
optimal. This led to me thinking about how there is most likely going to be something specific in any
given creatures `genome` that can determine whether it is a 'carnivore' or not. A creature would then only
need to sense whether the creature is a carnivore, omnivore, or herbivore, to know if it is in danger.
 
## Memory Persistence

### Intro

&nbsp;&nbsp;&nbsp;&nbsp;As it stands right now, a creature, we'll call 'glom', could have a few sensory inputs
that detect when a predator is in front of it and allows it to smell food. The glom could smell food and move
towards the food source when its 'predator sensor' activates and causes the output sensor 'move away from `x`' 
to activate. Now our glom has successfully moved away from a potential predator, yay! However, now the glom's 
input sensor says that there is no predator in front of him, is not activating and the glom again senses food.
The poor glom would immediately turn back around just to discovery the predator again. How do we counteract this?

### Short Term Memory Banks

&nbsp;&nbsp;&nbsp;&nbsp;Implementing some sort of memory will provide the creatures with a sense of persistence 
and continuity in their behaviors. This ensures that they remember recent events and avoid making immediate
contradictory choices, such as the glom above.

Making it so each creature has a *memory buffer* could be beneficial. The buffer would store recent events
or sensory inputs that are of interest (eg: detecting a predator, moving away from a threat). Each event in the buffer
would also have some sort of memory duration. Less significant events might stay in the buffer for less time than more
significant ones.

### Event Retrieval and Decision-making Influence

&nbsp;&nbsp;&nbsp;&nbsp;A potential solve for this is having events from the buffer act as 'temporary input nodes'.
These 'temporary input nodes' as blasphemous as it may seem could feed directly into a 'permanent input node'.
Information from the buffer would determine where the output from the temporary input node would go. The output would
also be affected by *memory decay*. *Memory decay* would gradually reduce the influence of older events making it less
likely that an older event would trigger and action potential in an input node. The events in the buffer could even be
deleted as soon as the influence of the event reaches zero. This would ensure that recent events have a stronger impact 
on decisions compared to distant past events.

Adding `temporary input nodes` to the buffer is a great idea, however I can already see a few potential problems with 
this short term memory solve. It stems from these logic examples:

**Infinite Loop**
1. 'sense predator' node activated
2. 'temporary sense predator' node added to the buffer
3. 'temporary sense predator' node activates 'sense predator' node
4. 'temporary sense predator' node added to the buffer because of the original 'temporary sense predator node'
5. Infinite loop occurs where the creature is constantly 'sensing a predator' and added a temporary node to the buffer

**Unexpected Results from Dynamic Nodes**

&nbsp;&nbsp;&nbsp;&nbsp;Another problem is that this temporary node would 'mess' with the dynamic nodes in unwanted 
ways. Using the sense predator node as an example. More than likely, an evolved species would trigger the 'move away 
from x' node when the sense predator node is activated. Now, the 'sense predator input node' would 'sense' a predator 
because of a temporary input node which would in turn, trigger the 'move away from x' node. However, now that the 
predator is not being sensed by the creature, 'move away from x' could cause the creature to move away from a pheromone 
or food, instead of the predator, as the predator would not be a valid `x`.

## Coordination and Cooperation

### Intro

&nbsp;&nbsp;&nbsp;&nbsp;This idea is likely for version 3 of this program. As it stands right now, all creatures are
their own thing. They do not have any way to communicate with each other. I would not expect any of them to ever team 
up or coordinate anything complex. That's when I thought of a very complex type of `dynamic output node`. Essentially 
this output node would work almost like a short range radio, allowing creatures to communicate with each other. Having 
an output node that sends signals to other nodes could help in achieving collective behaviors within the creatures. 
For example, one creature's output could influence the behavior of nearby creatures.

