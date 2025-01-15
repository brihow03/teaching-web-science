# **HOMEWORK 5 – GRAPH PARTITIONING**

### **Brianna Howell,** **Data Science 440-07 \- Web Science, fall 2024** 

# Assignment

You will investigate the split of the Karate Club (Zachary, 1977), describing starting on slide 92 in the Module-07 Social Networks lecture slides. You must use a Pythn or JavaScript library (as discussed in Modue-09 Graph Vis) to generate the graphs required in this assignment.

**Question 1:  Color nodes based on final split**  
Draw the original Karate club graph (before the split) and color the nodes according to the factions they belong to (John A or Mr. Hi). This should look similar to the graph on slide 92 \- all edges should be present, just indicate the nodes in the eventual split by color.

# **Q1: How many nodes (students) eventually go with John and how many with Mr. Hi?**

## **Answer 1:**   Fifteen student eventually go with Mr. Hi and nineteen go with John.

**Discussion 1:** To create the graph in Figure 1, I began by importing the necessary libraries: networkx for graph manipulation and matplotlib.pyplot for visualization. I then generated the Karate Club graph using the nx.karate\_club\_graph() function, which automatically provides the structure of the Karate Club network. To define the appearance of each node, I created the function get\_node\_colors\_and\_sizes(G). In this function, I iterated through each node in the graph and checked the 'club' attribute. If the node belonged to Mr. Hi’s group, I assigned it a red color and set its size to 800\. If the node belonged to John A’s group, it was assigned a white color, also with a size of 800\.

I then used the draw\_graph() function to visualize the network. This function accepts the graph GGG, node positions (calculated using nx.spring\_layout(G, seed=42)), and the node colors and sizes. The layout ensures that the nodes are spaced clearly so you can see. I also added labels to the nodes, which are numbered starting from 1\. The edges were drawn in black, and the borders of the nodes were given a thicker line to emphasize Mr. Hi (node 1\) and John A (node 34). Finally, I displayed the graph using plt.show() and added a title indicating that the graph represents the Karate Club network, colored by the factions of Mr. Hi and John A.

![\label{fig:pre-split}](https://github.com/brihow03/teaching-web-science/blob/main/fall-2024/homework/hw5/Iteration%20Images/Iteration%201.png?raw=true)

**Code Q1:**
```python
#!/usr/local/bin/python3
# testargs.py

import networkx as nx
import matplotlib.pyplot as plt
import random

G = nx.karate_club_graph()
pos = nx.spring_layout(G, seed=42)
def get_node_colors_and_sizes(G):
    node_colors=[]
    node_sizes=[]
    for node, data in G.nodes(data= True):
        if data['club'] == 'Mr. Hi':
            node_colors.append('red')
            node_sizes.append(800)
        else:
            node_colors.append('white')
            node_sizes.append(800)
    return node_colors, node_sizes

def draw_graph(G, pos, node_colors, node_sizes, title):
    plt.figure(figsize=(12, 8))
    labels = {node: str(node+ 1) for node in G.nodes()}
    linewidths = [5 if node+ 1 in [1, 34] else 1.5 for node in G.nodes()]
    nx.draw(
        G,pos, with_labels= True,
        node_color = node_colors,
        node_size = node_sizes,
        labels = labels,
        edgecolors = 'black',
        linewidths = linewidths,
    )
    plt.title = title
    plt.axis('off')
    plt.show()
node_colors, node_sizes = get_node_colors_and_sizes(G)
draw_graph(G, pos, node_colors, node_sizes, "Q1: Karate Club Colored by Final Factions")
```

# **Question 2: Use the Girvan-Newman algorithm to illustrate the split**

We know the final result of the Karate club split, which you’ve colored in Q1. Use the Girvan-Newman algorithm to check if the split could have been predicted by the social interactions expressed by edges. How well does the mathematical model represent reality? Generously document your answer with all supporting equations, code, graphs, arguments, etc.

Keeping the node colors the same as they were in Q1, run multiple iterations of the Girvan-Newman graph partitioning algorithm (see Modile-07 Social Networks, slides 90-99) on the Karate Club graph splits into two connected components. Include an image of the graph after each iteration in your report.

NOTE: Implement the Girvan-Newman algorithm (See Module-07m slide 91\) rather than relying on a built-in function which hides the intermediate steps. Narrate in your report, the working of the Girvan-Newman algorithm.

Q2: How many iterations did it take to split the graph?

## **Answer 2:** It took 10 iterations to split the graph.

![\label{fig:pre-split}](https://github.com/brihow03/teaching-web-science/blob/main/fall-2024/homework/hw5/Iteration%20Images/iterations%201_6.png?raw=true)
![\label{fig:pre-split}](https://github.com/brihow03/teaching-web-science/blob/main/fall-2024/homework/hw5/Iteration%20Images/Iterations%207_10.png?raw=true)

**Discussion 2:** The Number of iterations to split the graph: 10

Connected components after Girvan-Newman split:

Component 1: \[1, 2, 4, 5, 6, 7, 8, 11, 12, 13, 14, 17, 18, 20, 22\] \= **15**

Component 2: \[3, 9, 10, 15, 16, 19, 21, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34\] \= **19**

I used the code from our class material, as well as some codes I found online, to create the visualizations and apply the Girvan-Newman algorithm to the Karate Club graph. First, I defined a function, get\_node\_colors\_and\_sizes(G), which assigned colors and sizes to the nodes in the graph based on their group. Nodes in Mr. Hi's group were given a red color and a size of 800, while the rest were assigned a white color with the same size. Next, I used the draw\_graph() function to display the graph with a spring layout, then I labeled the nodes, and highlighted Mr. Hi and John A with thicker node borders. To apply the Girvan-Newman algorithm, I created the function girvan\_newman\_algorithm(G, pos), which iteratively removed edges with the highest betweenness centrality. After each removal, I was able to see and save the updated graph to show how it split into separate components. The algorithm printed the number of iterations it took to split the graph and listed the nodes in each resulting component.

**Code Q2:**
```python
#!/usr/local/bin/python3
# testargs.py

import networkx as nx
G = nx.karate_club_graph()
mr_hi_faction = sum(1 for _, attr in G.nodes(data=True) if attr['club'] == 'Mr. Hi')
john_a_faction = G.number_of_nodes() - mr_hi_faction
print(f"Number of nodes in Mr. Hi's faction: {mr_hi_faction}")
print(f"Number of nodes in Mr. Hi's faction: {john_a_faction}")

Number of nodes in Mr. Hi's faction: 17
Number of nodes in Mr. Hi's faction: 17

import networkx as nx
import matplotlib.pyplot as plt
import random

def get_node_colors_and_sizes(G):
   node_colors = []
   node_sizes = []
   for node, data in G.nodes(data=True):
       if data['club'] == 'Mr. Hi':
           node_colors.append('red')
           node_sizes.append(800)
       else:
           node_colors.append('white')
           node_sizes.append(800)
   return node_colors, node_sizes

def draw_graph(G, pos, node_colors, node_sizes, title):
plt.figure(figsize=(12, 8))
   labels = {node: str(node + 1) for node in G.nodes()}
   linewidths = [5 if node + 1 in [1, 34] else 1.5 for node in G.nodes()]
   nx.draw(
       G, pos, with_labels=True,
       node_color=node_colors,
       node_size=node_sizes,
       labels=labels,
       edgecolors='black',
       linewidths=linewidths,

   )
   plt.title(title) 
   plt.axis('off')
   plt.show()

G = nx.karate_club_graph()
pos = nx.spring_layout(G, seed=42)

def girvan_newman_algorithm(G, pos):

   G_copy = G.copy()
   node_colors, node_sizes = get_node_colors_and_sizes(G_copy)
   iterations = 0
   while nx.number_connected_components(G_copy) < 2:
 edge_betweenness = nx.edge_betweenness_centrality(G_copy)
       max_betweenness = max(edge_betweenness.values())
       edges_to_remove = [edge for edge, value, in edge_betweenness.items() if value == max_betweenness]
       G_copy.remove_edges_from(edges_to_remove)
       iterations += 1
       draw_graph(G_copy, pos, node_colors, node_sizes, f"Q2: Girvan-Newman Iteration {iterations}")

   return G_copy, iterations
G_split, num_iterations = girvan_newman_algorithm(G, pos)
print(f"Number of iterations to split the graph: {num_iterations}")
gn_components = list(nx.connected_components(G_split))
print("Connected components after Girvan-Newman split:")
for i, component in enumerate(gn_components, 1):
   component_nodes = sorted(node+1 for node in component)
   print(f"Component {i}: {component_nodes}")
```

# **Question 3: Compare the actual to the mathematical split**

Compare the connected components of the Girvan-Newman split graph (Q2) with the connected components of the actual split Karate Club graph (Q1).

Q: Did all of the same colored nodes end up in the same group? If not, what is different?

## **Answer 3:** 

In the first Girvan-Newman split graph from question 1 all components were connected. We can see that there were 17 red circular student components which stood for Mr. Hi’s students, while the 17 white circular student components symbolized John A’s students. The graph has actual and mathematical balance. It is split (17 students for Mr. Hi and 17 students for John) totaling 34 students.

In question 2, all components are still connected in Iteration 1 thru 9 but we can see that the connection gets weaker with each iteration. This is most noticeable when looking at the connection between student 14 and student 3 in iteration 9\. After 10 interactions there is no longer one group. There is a clear separation now forming two groups. All of the same color nodes did not end up in the same group. Mr. Hi’s red student nodes lost 2 leaving that group with 15 nodes. John’s white student nodes gained 2 of Mr Hi’s red student nodes. John has 19 student nodes in his group. **There is an actual/visual and mathematical difference between the 2 groups but unlike in our slides I see the difference in the connection of the components but not so much the location of the components.**

**Discussion 3:** When the code was applied to the Karate Club graph, the algorithm started with one big group and ended with two smaller groups. I used the Girvan-Newman algorithm to split the Karate Club network into different groups by removing edges that are the most important for connecting parts of the graph. The algorithm starts with the whole network as one big connected piece. In each step, it calculates the edge betweenness centrality to find the edges that are most important for holding the graph together. Then, the edge with the highest centrality is removed, and the process continues until the graph is split into two or more separate pieces. The number of steps it takes to split the graph is counted, and the graph is drawn after each step to show how it is breaking apart.

However, I noticed that the results aren’t always the same. Sometimes, nodes that are supposed to be in the same group (marked by the same color) don’t end up in the same part of the graph after the split. Also, the final result can change depending on the order in which edges are removed. This is similar to what happens in the built-in Karate Club graph split algorithm, where the results also vary based on the edge removal sequence.

This part of the code was used to split the graph and to count and print the number of iterations it took to do so:

```python
#!/usr/local/bin/python3
# testargs.py

return G_copy, iterations
G_split, num_iterations = girvan_newman_algorithm(G, pos)
print(f"Number of iterations to split the graph: {num_iterations}")
gn_components = list(nx.connected_components(G_split))
print("Connected components after Girvan-Newman split:")
for i, component in enumerate(gn_components, 1):
   component_nodes = sorted(node+1 for node in component)
   print(f"Component {i}: {component_nodes}")
```
	

## **References:**

[Detecting Communities in Social Networks Using Girvan-Newman Algorithm in Python](https://techknowledgehub.org/network-visualization-using-matplotlib-and-networkx-libraries/)

[Executing Python Scripts With a Shebang](https://realpython.com/python-shebang/)

[Graph with Networkx and matplotlib](https://www.youtube.com/watch?v=yhZ9nVtCO5g)

[NetworkX Network Analysis in Python Centraility](https://networkx.org/documentation/stable/reference/algorithms/generated/networkx.algorithms.centrality.edge_betweenness_centrality.html)

[NetworkX Netweork Analysis in Python Components](https://networkx.org/documentation/stable/reference/algorithms/generated/networkx.algorithms.components.connected_components.html)

[NetworkX Network Analysis in Python Drawing](https://networkx.org/documentation/stable/reference/drawing.html)

[NetworkX Network Analysis in Python to Create a Graph](https://networkx.org/documentation/stable/tutorial.html)

[NetworkX Network Analysis in Girvan\_Newman](https://networkx.org/documentation/stable/reference/algorithms/generated/networkx.algorithms.community.centrality.girvan_newman.html)

[NetworkX Network Analysis in Python Kamada-Kawai](https://networkx.org/documentation/stable/reference/generated/networkx.drawing.layout.kamada_kawai_layout.html)

[NetworkX Network Analysis in Python Labels and Colors](https://networkx.org/documentation/stable/auto_examples/drawing/plot_labels_and_colors.html)

[NetworkX Network Analysis in Python Random Layout](https://networkx.org/documentation/stable/reference/generated/networkx.drawing.layout.random_layout.html)

[NetworkX Network Analysis in Python Spring](https://networkx.org/documentation/stable/reference/generated/networkx.drawing.layout.spring_layout.html)

[Network Visualization Using Matplotlib and NetworkX](https://techknowledgehub.org/network-visualization-using-matplotlib-and-networkx-libraries/)

[Zachary’s Karate Club](https://en.wikipedia.org/wiki/Zachary%27s_karate_club)



