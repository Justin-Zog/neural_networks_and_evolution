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

&nbsp;&nbsp;&nbsp;&nbsp;Ensuring that genomes are spliced together correctly when two or more organisms breed is a
complicated process that is key to this experiment working.

#### Visuals and Stats:

&nbsp;&nbsp;&nbsp;&nbsp;I believe that creating a system that shows what is happening during the simulation will be
challenging. I will have to research libraries that allow me to create graphs, charts, and more.

## Phase 1: System Analysis

### Inputs:

### Outputs:

### Key Functions/Algorithms:


## Phase 2: Design

### Classes:

Since different node types do not share any common functionality, no abstraction will be used. Separate classes will
be created for Input, Hidden, and Output nodes.

#### Organization Detail 1:

Using delegates will save lots of headaches and time in the future. Every input, hidden and output node are essentially
the same. They just use different functions to determine the input value, activation values, and what the creature does.
We could just make different input node classes based on the different inputs, but that would lead to tons of duplicated
code. Instead, we can define a single Input Node class that uses a `Delegate` to determine the function it uses.

    // This delegate can store any method that has a return type of 'float'.
    // By defining the method parameter with 'params object[] parameters',
    // the method can accept any number of arguments (including zero) of any type,
    // and those arguments will be stored in an array of objects.   
    // This is to future-proof the program to accommodate for any creative node that might be introduced in the future.

    public delegate float InputNodeFunction(params object[] parameters);

#### Node

The only reason this Superclass Node was made is that it allows values to be sent across Edge objects seamlessly.
```
public abstract class Node
{
    public int Id {get; private set; }
    
    public Node(int id)
    {
        Id = id;
    }
}
```

#### InputNode

The InputNode class for version 1 will look something like this:
```
public class InputNode : Node
{
    public Edge[] OutputEdges { get; private set; }
    // A delegate that stores the function that determines the value then node will spit out.
    public InputFunction InputFunction { get; private set; }

    public InputNode(int id) : base(id)
    {
        OutputEdges = new Edge[0];
        InputFunction = InputFunctions.GetFunction(Id); // The file that contains all input node functions
    }
    
    public void AddOutputEdge(Edge edge)
    {
        Adds a new output edge to the array.
        This will create a new array with one additional slot for the new output edge 
        and replace the old array with the new one.
    }
    
    public void SendOutput()
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
    public float Threshold { get; private set; }
    public Edge[] InputEdges { get; private set; }
    // A delegate that stores the function to be carried out by the output node.
    public OutputFunction OutputFunction { get; private set; }
    
    public OutputNode(int id) : base(id)
    {
        InputEdges = new Edge[0];
        OutputFunction = OutputFunctions.GetFunction(Id); // The file that contains all output node functions
    }
    
    public void AddInputEdge(Edge edge)
    {
        Adds a new input edge to the array.
        This will create a new array with one additional slot for the new input edge
        and replace the old array with the new one.
    }
    
    public float CalculateFrequencyCoding()
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
    public Edge[] InputEdges { get; private set; }
    public Edge[] OutputEdges { get; private set; }
    // A delegate that stores the activation function to be used by the hidden node.
    public ActivationFunction ActivationFunction { get; private set; }
    
    public HiddenNode(int id) : base(id)
    {
        InputEdges = new Edge[0];
        OutputEdges = new Edge[0];
        ActivationFunction = ActivationFunctions.GetFunction(Id); // The file that contains all the activation functions
    }
    
    public void AddInputEdge(Edge edge)
    {
        Adds a new input edge to the array.
        This will create a new array with one additional slot for the new input edge
        and replace the old array with the new one.
    }
    
    public void AddOutputEdge(Edge edge)
    {
        Adds a new output edge to the array.
        This will create a new array with one additional slot for the new output edge 
        and replace the old array with the new one.
    }
    
    public float SendOutput()
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
    public float Weight { get; private set; }
    public Node Source { get; private set; }
    public Node Sink { get; private set; }
    public float Value { get; set; }
    
    public Edge(Node source, Node sink, float weight)
    {
        Source = source;
        Sink = sink;
        Weight = weight;
        Value = 0.0f;
    }
    
    public void SetValue(float value)
    {
        Value = value * Weight;
    }
}
```


## Phase 3: Implementation


## Phase 4: Testing & Debugging


## Phase 5: Deployment


## Phase 6: Maintenance