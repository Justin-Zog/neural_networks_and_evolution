Justin Herzog August, 2023

This file is layout and plan for the minimum viable program to experiment with evolution in artificial creatures.

# Software Development Plan

## Phase 0: Requirements and Specifications

### Summary:

&nbsp;&nbsp;&nbsp;&nbsp;This program will simulate organisms with their own unique neural networks, based on genomes,
in order to explore how mutation and reproductive means affect evolution.

### Requirements:

&nbsp;&nbsp;&nbsp;&nbsp;

### Anticipated Challenges:

#### Effective Organization:

&nbsp;&nbsp;&nbsp;&nbsp;With many moving parts, this project will require careful organization and planning, so it can
run as efficiently as possible.

#### Genomes:

&nbsp;&nbsp;&nbsp;&nbsp;Decoded Genomes into Neural Networks is going to be very tough.

#### Visuals and Stats:

&nbsp;&nbsp;&nbsp;&nbsp;I believe that creating a system that shows what is happening during the simulation will be
challenging. I will have to research libraries that allow me to create graphs, charts, and more.

## Phase 1: System Analysis

### Inputs:

### Outputs:

### Key Functions/Algorithms:


## Phase 2: Design

### Graphs:

#### Data Collection:

At the end of each generation, we need to collect any data we want and store it in data structures (a list or array).

#### Graphing Library:

Using Matplotlib in Python to create the graph of survivors and genetic diversity works great.

Using igraph in Python to create the graph like structure represented by a Neural Network works great as well.

### Classes:

Since different node types do not share any common functionality, no abstraction will be used. Separate classes will
be created for Input, Hidden, and Output nodes.

#### Organization Detail 1:

Using delegates will save lots of headaches and time in the future. Every input, hidden and output node are essentially
the same. They just use different functions to determine the input value, activation values, and what the creature does.
We could make different input node classes based on the different inputs, but that would lead to tons of duplicated
code. Instead, we can simply define a single Input Node class that uses a `Delegate` to determine the function it uses.

    // This delegate can store any method that has a return type of 'float'.
    // By defining the method parameter with 'params object[] parameters',
    // the method can accept any number of arguments (including zero) of any type,
    // and those arguments will be stored in an array of objects.   
    // This is to future-proof the program to accommodate for any creative node that might be introduced in the future.

    public delegate float InputNodeFunction(params object[] parameters);

#### Node

The only reason this Superclass Node was made is that it allows NeuralNetworks to be made and helps in allowing
information to be sent across Edges

```
public abstract class Node
{
    public int id { get; private set; }
    
    public Node(int id)
    {
        this.id = id;
    }
}
```

#### InputNode

The InputNode class for version 1 will look something like this:

```
public class InputNode : Node
{
    public Edge[] outputEdges { get; private set; }
    // A delegate that stores the function that determines the value then node will spit out.
    public InputFunction inputFunction { get; private set; }

    public InputNode(int id) : base(id)
    {
        this.outputEdges = new Edge[0];
        this.inputFunction = InputFunctions.GetFunction(this.id); // The file that contains all input node functions
    }
    
    public void addOutputEdge(Edge edge)
    {
        Adds a new output edge to the array.
        This will create a new array with one additional slot for the new output edge 
        and replace the old array with the new one.
    }
    
    public void sendOutput()
    {
        Takes the result of inputFunction and sends it to all the OutputEdges.
    }
}
```

#### OutputNode

The OutputNode class for version 1 will look something like this:

```
public class OutputNode : Node
{
    public float threshold { get; private set; }
    public Edge[] inputEdges { get; private set; }
    // A delegate that stores the function to be carried out by the output node.
    public OutputFunction outputFunction { get; private set; }
    
    public OutputNode(int id) : base(id)
    {
        this.inputEdges = new Edge[0];
        this.outputFunction = OutputFunctions.GetFunction(this.id); // The file that contains all output node functions
    }
    
    public void addInputEdge(Edge edge)
    {
        Adds a new input edge to the array.
        This will create a new array with one additional slot for the new input edge
        and replace the old array with the new one.
    }
    
    public float calculateFrequencyCoding()
    {
        Sends the sum of all values from InputEdges into the activation function (ReLU)
        The frequencyCoding is then calculated. (Activation function's result divided by the threshold value)
        After that the resulting data is sent to the organism to be processed further.
    }
}
```

#### HiddenNode

The HiddenNode class for version 1 will look something like this:

```
public class HiddenNode : Node 
{
    public Edge[] inputEdges { get; private set; }
    public Edge[] outputEdges { get; private set; }
    // A delegate that stores the activation function to be used by the hidden node.
    public ActivationFunction activationFunction { get; private set; }
    
    public HiddenNode(int id) : base(id)
    {
        this.inputEdges = new Edge[0];
        this.outputEdges = new Edge[0];
        this.activationFunction = ActivationFunctions.GetFunction(this.id); // The file that contains all the activation functions
    }
    
    public void addInputEdge(Edge edge)
    {
        Adds a new input edge to the array.
        This will create a new array with one additional slot for the new input edge
        and replace the old array with the new one.
    }
    
    public void addOutputEdge(Edge edge)
    {
        Adds a new output edge to the array.
        This will create a new array with one additional slot for the new output edge 
        and replace the old array with the new one.
    }
    
    public float sendOutput()
    {
        Sums up the values from the InputEdges and puts the sum through the activation function.
        Then sends that value to each of the OutputEdges
    }
}
```

#### Edge

Edge Objects are primarily responsible for connecting two nodes together and sending important information between 
those nodes.

```
public class Edge
{
    public float weight { get; private set; }
    public Node source { get; private set; }
    public Node sink { get; private set; }
    public float value { get; set; }
    
    public Edge(Node source, Node sink, float weight)
    {
        this.source = source;
        this.sink = sink;
        this.weight = weight;
        this.value = 0.0f;
    }
    
    public void setValue(float value)
    {
        this.value = value * this.weight;
    }
}
```

#### Genome

The Genome Class is responsible for creating valid genomes of a specified length.

```
public class Genome
{
    public int length { get; private set; }
    public string[] genes { get; private set; }
    
    // constructor for creating random creatures
    public Genome(int length)
    {
        this.length = length;
        this.genes = CreateGenome(Length);
    }
    
    // constructor for creating creatures based on an already formed genome (ie: offspring)
    public Genome(int length, string[] genes)
    {
        this.length = length;
        this.genes = genes;
    }
    
    private Gene[] createGenome(int length)
    {
        string[] genes = new string[length];
        
        // Create the Genome here.
    
        return genes;
    }
}
```

#### BreedingManager

This is a utility class that contains the methods and logic for breeding creatures.

```
public static class BreedingManager
{
    // Asexual
    public static Genome breedGenomes(Genome parent)
    {
        Copies the Genome with a chance for mutation.
    }
    
    // Heterosexual
    public static Genome breedGenomes(Genome parent1, Genome parent2)
    {
        string[] genes = new string[parent1.Length];
        
        Take the first half of parent1's genome and add it to genes
        Take the last half of parent2's genome and add it to genes
        Genome offspringGenome = new Genome(genes.length, genes);
        
        return offspringGenome;
    }
    
    // Tri-sexual
    public static Genome breedGenomes(Genome parent1, Genome parent2, Genome parent3)
    {
        Add the logic for breeding here
        return Genome;
    }
    
    // Quad-sexual
    public static Genome breedGenomes(Genome parent1, Genome parent2, Genome parent3, Genome parent4)
    {
        Add the logic for breeding here
        return Genome;
    }
    
    // Hexa-sexual
    public static Genome breedGenomes(Genome parent1, Genome parent2, Genome parent3, Genome parent4, Genome parent5, Genome parent6)
    {
        Add the logic for breeding here
        return Genome;
    }
}
```

#### NeuralNetwork

The Neural Network Class is in charge of decoding a genome into a complex graph of nodes.
This is by far the most complicated thing in this program. Because of this, I will list the steps in which a neural
network must be made before creating the pseudocode class.

**PreProcessing**:

**Using the Genome To Determine An Overload Limit on Hidden Nodes**:

If `Genome.Length > 12` this formula will be applied:

1. Take the first digit of the first strand, the second digit of the second strand, the third digit of the third strand 
and the fourth digit of the fourth strand and combine them together to create a 4-digit hexadecimal number.


2. Convert the 4-digit hexadecimal number to base 10 and divide by 65535. Now we have a floating point number between
0.0 and 1.0


3. Multiply the floating point number by the Genome length and convert it to an int. `(int) (x * Genome.Length)`


4. Add the constant `2` to this number, this will give us the maximum connections allowed on each hidden node.
The constant `2` comes from the fact that each hidden node requires at least 2 connections and if we were very unlucky,
the `int` value derived from the Genome has the possibility of being `0`.

**Processing**:

**Important Notes**:

While Processing the Genome, the function that is creating the Neural Network will keep track of some key data that is
required for creating the Neural Network. It will keep track of all nodes it has created using separate lists for input,
output, and hidden nodes respectively. 

**Determining When To Create a New Hidden Node**:

From the PreProcessing step, we found out the maximum connections that each hidden node is allowed to have. For now,
the minimum connections that each hidden node is allowed to have is always set to 2, the absolute minimum. We also know
that the newest hidden node added to the Neural Network is stored at the front of the `Hidden Nodes List`. Now we can
follow these streamlined steps to determine when to create a new hidden node of a specific type.

```
    HiddenNode hiddenNode;
    // Checks to see if a hidden node of the specific type has already been added to the NeuralNetwork
    if (anyNode in hiddenNodeList contains hiddenNode.id)
    {
        // Check the amount of connections the pre-existing node has using 
        // int connections = anyNode.InputEdges.Length + anyNode.OutputEdges.Length
        // Checks to see if a new connection can be made
        if (connections >= maxConnections)
        {
            // Create a new hidden node of the specific type, add the new connection to it and add it to hiddenNodeList
        }
        else if (connections < 2)
        {
            Add the connection
        }
        else if (connections >= 2 && connections < maximum && notHookedUpInAViableWay)
        {
            Add the connection
        }
        else
        {
            Take the first digit of the hexadecimal strand and convert it to base 10.  Run that number through a switch
            statement that determines if a new hidden node should be made or not.
        }
    }
```

You may have noticed the `notHookedUpInAViableWay` parameter on one of the else if statements. This is a check to see
if a hidden node is connected to both an input and output node in some way. If the hidden node is not, it is essentially
useless as any connections leading to it won't lead anywhere useful. Here is a pseudocode recursive function I made that 
should check to see if a hidden node is connected to an input node. The function should also be very easy to alter to 
check if the hidden node is connected to an output node.

```
public bool isConnectedToInputNode(Node node)
{
    return IsConnectedToInputNode(new List<Node>(), node);
}

private bool isConnectedToInputNode(List<Node> previousNodes, Node currentNode)
{
    // Check if the path was a feedback loop
    if (previousNodes.Contains(currentNode))
    {
        return false;
    }

    // Check if the node is an input node
    if (currentNode is InputNode)
    {
        return true;
    }

    // Add the current node to the list of visited nodes
    previousNodes.Add(currentNode);

    // Recursively check the input edges
    foreach (var edge in currentNode.InputEdges)
    {
        if (isConnectedToInputNode(previousNodes, edge.Source))
        {
            return true;
        }
    }

    return false;
}

```

**Determining The Threshold Of Each Node Based On Its Genome**:

**Note**: Only Output Nodes Have A Threshold. All Other Nodes Simply Pass Their Values Down The Chain.

1. When an output node is first created, take the last four digits of the hexadecimal number and determine the base 10
value. It will be a number between 0 and 65535.


2. Divide this number by 65535 to get a floating point number between 0.0 and 1.0. This is the threshold.


3. If the result of the activation function is greater than or equal to the threshold, then the output node will fire
off.

**The NeuralNetwork Class**

```
public class NeuralNetwork
{
    
    
}
```

#### NeuralNetworkFactory

This is a utility class that contains the methods and logic for creating a Neural Network from a Genome.

```
public static class NeuralNetworkFactory
{
    public static NeuralNetwork CreateNeuralNetworkFromGenome(Genome genome)
    {
        create the neural network here.
        
        return neuralNetwork;
    }
}
```

#### Creature

This class is responsible for storing important information about a creature. In version 1 of the project it will
look something like this.

```
public class Creature
{
    public Coord location { get; private set; }
    public Genome genome { get; private set; }
    public NeuralNetwork brain { get; private set; }
    
    public Creature()
    {
        this.location = GenerateValidLocation();
        this.genome = new Genome();
        this.brain = new NeuralNetwork();
    }
}
```


## Phase 3: Implementation


## Phase 4: Testing & Debugging


## Phase 5: Deployment


## Phase 6: Maintenance