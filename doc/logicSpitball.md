I thought it would be important to mention that this project was largely inspired by
David Randall Miller's project and youtube video that can be found here:
>https://www.youtube.com/watch?v=N3tRFayqVtk

Although, I do not agree with how many of things in his code are implemented, I do commend him for igniting a creative
spark within me, and motivating me to create this project.

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
If a larger battery is used to generate a supra-threshold depolarization, the amplitude of that action potential is 
the same as the action potential triggered by a just-threshold stimulus. However, the frequency of action potentials 
is higher. From the video we learn "that the greater the magnitude of the stimulus, the greater is the number
or frequency of action potentials. That's called *frequency coding on the nervous system*".

We learn from the *frequency coding on the nervous system* that the
greater the intensity of a stimulus, the greater the number of action potentials that
are passed to the brain.

After learning about the *threshold* and *frequency coding on the nervous system*, we know we must
implement something similar in the brains of our creatures. This is explained in the **Genome** section below.

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

These different and distinct kind of connections must be implemented in our program. This is
explained in the **Genome** section below.

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
will call this node an `oscillatory node`. It is a special type of input node that will fire off a
signal after a set period of time.

### How can we incorporate this in our program?

&nbsp;&nbsp;&nbsp;&nbsp;Keep in mind that a neuron in our program is called a `node`. A neuron
has `dendrites` that feed a 'charge' to the `soma`. If the charge given to the `soma` is high enough,
a signal is sent through the `synapses` of the neuron and received by the `dendrite` of another `neuron`.
Similarly, our `node` has `input edges` that feed a 'charge' or value to the `node`. The node
then takes the sum of the values given to it and sends them through an `activation function`.
This value is then sent through the `node's` `output edges` and received by another `node`.

# Small Picture

## Genome

### Node Connection Rules

&nbsp;&nbsp;&nbsp;&nbsp;Before choosing a method in which the genome will create a creature's neural network, we need to
understand the rules behind which connections between nodes is allowed. The table below shows this information:

|        | input  | hidden | output |
|:------:|:------:|:------:|:------:|
| input  |        |   *    |   *    |
| hidden |        |   *    |   *    |
| output |        |        |        |

An asteriks shows that a connection from the type on the left, (the **source**), to the type on the top, (the **sink**) 
can be made. An empty box shows that a connection between the **source** to the **sink** can not be made.

This rules of this chart are largely based on nature. Although an uncommon and highly specialized connection called a 
*feedback loop*, (input --> output --> input), does occasionally occur the cases of them being used between input and 
output functions exclusively are extremely specialized and removing these from the simulation will likely drive our 
artificial creatures to have a more natural behavior. I am, however, allowing the hidden nodes to create feedback loops
amongst themselves. This could lead to the emergence of unexpected or advantageous behaviors.

### How Genomes Are Organized

A `Genome` in our program will simply define neural connections. A single 8 digit hexadecimal number would result in a
`NeuralNetwork` that has two nodes and one neural connection between the nodes. It is also important to emphasize that
these neural connections are **one way**.

Here is how the 8 digit hexadecimal number forms a connection between two nodes while still following the rules defined
above:

1. The 8 digit hexadecimal number is converted into 32 binary bits of data
2. These bits are split into sections and these sections determine different characteristics of the connection

This process will be demonstrated below using this 8 digit hexadecimal number, which we call a 'strand': `28e90f86`

**Convert the hexadecimal number to binary**: `0010 1000 1110 1001 0000 1111 1000 0110`

**Split into sections**: `0 0101000 1 1101001 0000111110000110`

Starting from the left, and going right:

- `0` this single bit determines the **source** of the connection. This will be either an input node or a hidden node.
This is because, referencing the table above, output nodes are incapable of being the source of a connection.


- `0101000` this unsigned 7-bit binary number is converted to its base 10 number, which we will denote by`x`. In this case, 
`x=40`. The second number we need is the `number of unique nodes of that type`, (input or hidden), which we'll denote 
by `y`. We then use the **modulo** operator to determine which `node` is defined by the 'strand': `x%y=node`


- `1` this single bit determines the **sink** of the connection. This will be either a hidden node or an output node.
This is because, referencing the table above, input nodes are incapable of being the sink


- `1101001` similar to the first 7-bit field, this is also converted to a base 10 number. The same math is applied to
determine which specific hidden or output `node` to connect the first node to.


- `0000111110000110` a signed 16-bit value that is divided by an arbitrary constant. This number determines the weight
of the edge.

There we have it, a freshly wired connection between two "neurons"!

### Genome Nuances

This section addresses questions that may arise when trying to decode the `Genome` into a `Neural Network`.

Here are some questions that must be answered:

- Can a `Neural Network` contain more than one of a specific type of output node?


- Can a `Neural Network` contain more than one of a specific type of input node?


- Can a `Neural Network` contain more than one of a specific type of hidden node?

We will answer these questions here:

#### Can A Neural Network Contain More Than One Of A Specific Type Of Output Node?

&nbsp;&nbsp;&nbsp;&nbsp;Traditionally in nature, the answer to this question is no. For our program, this means that 
if a creature has 25 unique output nodes, it will be able to have a maximum of 25 output nodes.

#### Can A Neural Network Contain More Than One Of A Specific Type Of Input Node?

&nbsp;&nbsp;&nbsp;&nbsp;In nature, we see examples of this all the time. We have two ears that can each hear and allow 
us to determine what direction a sound came from for example. However, implementing this in our simulator would be 
rather difficult and allowing more than one specific type of input node would most likely lead to inefficient organisms.
For this reason, our program will only allow an organism to have one type of each input node.

&nbsp;&nbsp;&nbsp;&nbsp;There is one exception to this, and that is the special case of the oscillatory node. I will 
further discuss this exception in a separate section below.

#### Can A Neural Network Contain More Than One Of A Specific Type Of Hidden Node?

&nbsp;&nbsp;&nbsp;&nbsp;We absolutely see this in nature. In fact, having more hidden nodes can lead to more complex
and advanced behaviors. However, having too many hidden nodes can be inhibitory and having too little can lead to
an organism with hidden nodes that are being overwhelmed with far too many connections. We then have to choose how our
`Neural Network` will handle these cases. On the one hand, we could opt to have more hidden nodes with fewer connections
and risk an inefficient system. On the other, we could opt to have less hidden nodes with a greater amount of 
connections. 

This in turn raises a question, "**Where is the Goldilocks zone?**" Unfortunately, this question goes far beyond my
understanding and I could not even begin to give an answer. The good news? I do not have to have an answer as Natural
Selection within the simulator will answer the question for us. We do, however, have to solve one of the two problems 
below:

1. How can we dynamically determine the amount of hidden nodes an organism can have?


2. How will a `Neural Network` decide a hidden node is 'overloaded' and therefore needs to create a new node?

&nbsp;&nbsp;&nbsp;&nbsp;**David Miller's Attempt At Finding The Goldilocks Zone**:

&nbsp;&nbsp;&nbsp;&nbsp;David Miller makes a weak attempt at solving question one in his program. In his program, the
maximum amount of hidden nodes an organism can have is a variable. David defines that variable at the start of the
simulation. While it is a potential solve, having a person set the maximum amount of nodes does not guarantee we will
get creatures in the 'Goldilocks Zone'. While we could tinker with the number and iterate through many simulations to
find what we believe to be the Goldilocks Zone, this seems inefficient and time-consuming.

&nbsp;&nbsp;&nbsp;&nbsp;**My Attempt At Finding The Goldilocks Zone**:

&nbsp;&nbsp;&nbsp;&nbsp;Instead of limiting the amount of hidden nodes an organism can have, I will allow the creature
to have an unlimited amount of hidden nodes and instead put a limit, that is determined by the `Genome`, on how many
connections each hidden node can have. Once this limit is reached, the `Neural Network` is forced to create a new
hidden node. Hopefully, with natural selection, this will lead to a Goldilocks Zone of not too many hidden nodes, while
also preventing hidden nodes from becoming overloaded.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**Using The Genome To Determine An Overload Limit**

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;The first thing that I will mention is that with very small genomes, 
this method puts no restrictions on the genome. This is because a hidden input **requires at least 2 connections** to 
even be effective. That is, input ---> hidden ---> output. Therefore, this formula is only applied to organisms where 
it is more plausible to have a hidden node with more than 5 or so inputs going into/out of it.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;If the genome is large enough, here is the method that will be used to 
create dynamic boundaries on the hidden nodes:

1. We will first construct a random `Genome`. An example `Genome`:
`3f7a9cde a2b153f8 8e4d76f1 d0e9cba5 6f18d3b9 94e27c5a 1b5af896 c7d86e2a`


2. We will then add the first digit of the first strand, the second digit of the second strand, the third digit of the
third strand and the fourth digit of the fourth strand together. From our example we get: `3 + 2 + 4 + 9 = 18`. 
We then divide this value by 60. The number `60` is derived from each hexadecimal digit representing a 4-bit binary 
number whose total equates to `15`. Hence, `15 * 4 = 60`. Doing this will give a floating point number between 0.0 and 
1.0.


3. Once we have this floating point number we will multiply it by the total amount of strands in the genome and convert
it to an `int`. From our example we get `(18 * 8) / 60 = 2.4 ---> 2`.  We multiply by the total number of strands
because that is the **maximum amount** of strands that could possibly flow into and out of a single hidden node. 
Granted, a `Neural Network` constructed completely of hidden nodes that all have output edges heading toward the same 
hidden node is almost impossible.


4. Finally, we add the constant `2` to this number to give us the maximum amount of connections allowed on each hidden
node. In our example, we would get, `2 + 2 = 4`.  The constant `2` comes from the fact that each hidden node requires
at least 2 connections and if we were very unlikely, the `int` value derived from the genome has the possibility of 
being `0`.


5. Obviously we do not want the `Neural Network` to be required to max out the hidden nodes before creating a new one
of the same type. This means we also need to determine a minimum amount of connections required to create a new hidden
node of the same type.  For now, I am okay with this being set at the absolute minimum, 2. The reason behind this
choice stems from how I envision the `Neural Network` will decode the `Genome` in the future. Here is the vision:
While decoding, the amount of each type of hidden node is tracked somewhere as well as the connections that these nodes
have. When a node has a viable amount of connections, (between the minimum and maximum), and a new connection is being
made. The function creating the `Neural Network` would look at the first digit of the `strand` that is making the 
connection and run it through a switch statement. I would then determine what digits would lead this switch statement 
to cause the function to create a new hidden node of the same type to be created or which digits cause the function 
to add the connection to the pre-existing hidden node. This allows me to toy with the odds that a new hidden node 
will be created and fine tune it to my liking.

#### Determining How Many Oscillatory Nodes An Organism Can Have Based On Its Genome

An organism with too many oscillatory nodes firing signals would be an issue. This is because more than likely, the
oscillatory node would cause an unwanted output node to activate, leading to less advantageous behavior. Because of
this, the genome of the organism will limit the amount of oscillatory nodes allowed.

A simple formula is going to be used to determine this. Here it is:

1. The last four digits of the first strand in an organisms genome will be converted to base 10 and added up. 


2. I will then normalize this number to be between 0.0 and 1.0 by dividing the sum from step one by 60. The number `60` 
is derived from each hexadecimal digit representing a 4-bit binary number whose total equates to `15`. Hence, 
`15 * 4 = 60`.


3. Once we have this floating point number we will multiply it by the total amount of strands in the genome and convert
it to an `int`. This `int` is the amount of oscillatory nodes that the organism is allowed to have.

#### Determining The Threshold Of Each Node Based On Its Genome

- Input nodes do not have a threshold, so we do not have to worry about them.


- Output and Hidden nodes threshold's will be determined by the `strand` when the node is first created.

**Overview**

My idea right now is to determine the threshold for each node when it is first created based on the `strand` that
creates them.

If the node was created from the first half of the hexadecimal strand, (a hidden node), then the first half of the
strand will be used to determine the threshold. I will take the first 2 digits of the hexadecimal number and determine
the base 10 value. It will be a number between 0 and 255. I will then divide this number by 255 to get a number between
0.0 and 1.0. This floating point number is the threshold. If the sum of inputs results in a number equal to or greater
than the threshold, then the neuron will fire off.

#### Frequency Coding Within An Organism

During the simulation, an organism may have both the 'move east' and 'move west' output nodes fire off at the same time.
How are our creatures going to result this conflict of interest? Through the `Frequency Coding Algorithm`.

Simply put, each output node in our program will use the exact same activation function. The result of the activation
function will be divided by the threshold value of the output node. We will call this number the `frequency coding` 
If the node hit its threshold value, this number will be greater than 1. The higher the output of the activation 
function compared to the threshold, the higher this number will be.

In each simulation step, any output node that was activated will send important data to a list. I believe the data will 
look similar to this in the final product:

`outputNodeID, frequencyCoding, type of output (motion, self defense))`

The list of output nodes that were activated will be sent to a function that will determine what output to pick based
off which output node had the greatest frequencyCoding for each type of output. The creature will then take this action.

Implementing this frequency coding algorithm will help organisms with prioritizing which tasks to perform.

## Studying How Reproductive Means Affects Evolution

### Intro

At some point growing up, probably in school, I was taught that creatures that reproduce *asexually* are worse at
adapted to changes in their environment (evolving) than creatures that reproduce *heterosexually*. I want to test this
hypothesis with this project. After researching, I have found that this theory stems from the**Red Queen Hypothesis**:

> (https://www.pbs.org/wgbh/evolution/library/01/5/l_015_03.html). 

### The Study

I will run a simulation where creatures reproduce asexually and evolve to their environment. 
Then I will change the selection method slightly to see how long it takes the creatures to adapt to this change. I 
will then do the exact same thing with creatures that reproduce heterosexually and track the results. 

Additionally, I will add a new dimension to the *Red Queen Hypothesis* by introducing the concept of 'trisexual', 
'quadsexual', and 'hexasexual' reproduction. In 'trisexual' reproduction, the genes of 3 parents will be split into 
thirds then combined to produce a single offspring. The equivalent is true for 'quad' and 'hexa' sexual reproduction. 
Four and Six parents respectively will be required to make one offspring. While these forms of reproduction do not
exist in the world, (at least as far as I know), I am very interested to see if these fictitious forms of reproduction 
would cause creatures to adapt faster/slower than heterosexual reproduction.

For this study, the creatures in the simulation need a genome length that is divisible by 12. This is because 1, 2, 3,
4, and 6 are all factors of 12.

## Types of Static Nodes

### Notes and Questions

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
know what `input`(s) lead to it being activated. Obviously more information needs to be passed down, but that will differ
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

## Cool Node Ideas

### Self Defense Nodes

#### 'Shocker' Node

&nbsp;&nbsp;&nbsp;&nbsp;This node is used in self-defense. I think it would be extremely funny if some of the creatures
that are lower down in the food chain had an ability that essentially 'filled the air with an electric shock'. This
shock would affect any creature in the area the shock fills. The creature that activated the shock remains unaffected.
The shock would cause a random input node to send a full strength charge through the system. This would lead to funny
scenarios where a predator has a weird and random jerk reaction, almost like a shock in the real world.

## Future Updates

### Ecosystem

Add a part to the video that defines what is required for an ecosystem. It could be similar to when you explain what is
required for evolution to occur.

### Odd Breeding

Since Breeding is based entirely off of Genome Length, two creatures that are completely different species but have the
same Genome length could breed. This could lead to some cool and wacky creatures haha.

