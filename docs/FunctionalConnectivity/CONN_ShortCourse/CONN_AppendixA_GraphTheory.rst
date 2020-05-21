.. _CONN_AppendixA_GraphTheory:


========================
Appendix A: Graph Theory
========================

------------------


Overview
********

This chapter is a brief overview of **graph theory**, a method of describing the pairwise relationships between two or more objects. In mathematics, graph theory can model any pair of objects - neurons, people, cities, and so on. For our purposes, we will be focusing on graph theory as applied to neuroimaging data, and in particular resting-state data. In this scenario, individual **voxels** or clusters of voxels are the pairs of objects that we are interested in modeling. Graph theory can provide a different perspective on how these voxels are connected, and in turn inform us of how the brain is organized.


Graph Theory Basics
*******************

Nodes and Edges
^^^^^^^^^^^^^^^

Let's use a set of railroads as an example. A train may be able to go directly from one city to another, such as from Kalamazoo to Port Huron, Michigan. However, the train cannot go *directly* from Kalamazoo to Port Huron; it has to pass through Battle Creek first. Someone taking a train from Battle Creek, on the other hand, can go to either Port Huron, Detroit, or Kalamazoo, without having to stop at any other cities. One can also travel directly from Port Huron to Detroit and vice versa, and go from either of those cities to Battle Creek.

.. figure:: AppendixA_TrainExample.png

Instead of looking at these train stops on a map, let's instead assign each of them a number; that way, we can compare these networks whether they are train stops or neural connections. For example, let's assign the following:

::

  Battle Creek = 0
  Detroit = 1
  Port Huron = 2
  Kalamazoo = 3
  
We can compress the train map into a more compact figure using the numbers as indicators for each city:

.. figure:: AppendixA_GraphTheoryDemo.png

In this figure, we represent each city as a **node** (also called a **vertex**). Each node has connections to different nodes; in graph theory, these connections are called **edges**. On the right side of the figure, we can present the same network in a different way, as an **adjaceny matrix**. Connections between nodes are marked with a 1, whereas nodes that are not connected are marked with a 0. For example, Kalamazoo (3) is directly connected to Battle Creek (0), but not with wither Detroit (1) or Port Huron (2). The fact that Battle Creek has so many direct connections within this network makes it a **hub**, or node with more edges than average. We will return to this concept later.

.. note::

  Adjacency matrices are **symmetrical**, meaning that the upper diagonal (i.e., all of the numbers above the diagonal of zeros that bisects the graph) are redundant with the lower diagonal (i.e., all of those numbers below the diagonal of the graph).
  
To sum up, this **network** is represented as a collection of nodes connected by edges. Some of the connections are direct - for example, between Detroit and Port Huron - while others are indirect, such as the connection between Port Huron and Kalamazoo. More complicated networks will have different levels of connectivity depending on how many steps each node is removed from the other nodes, but all networks in graph theory rest upon these building blocks.


Modularity
^^^^^^^^^^

If we zoom out of Michigan and look at the train system across all of America, you will notice something else: Certain regions of the country have a high density of high-traffic train connections within themselves (such as Boston, New Haven, and New York City on the East Coast), but only a few, lower-traffic connections for traveling to other cities in other regions of the country (such as Seattle, Houston, and San Diego).

.. figure:: AppendixA_TrainNetworkUSA.png

This clustering of nodes and edges into discrete pockets is known as **modularity**. Technically, this is a threshold that is set to determine when the density of intra-modular connections (such as the train network of the East Coast) is greater than inter-modular connections (such as connections between New York City and Los Angeles). Many algorithms exist for creating this threshold, but the general idea is the same. The basic idea behind modularity boils down to: When is the density of intra-modular connections greater than inter-modular connections?

The simplest algorithm is to maximize the value of what is called the **modularity index**, represented by the letter **Q**:

.. figure:: AppendixA_ModularityIndex.png

  The modularity index, as defined by `Newman (2005) <https://www.pnas.org/content/103/23/8577.full>`__.
  
The total number of edges in the network is represented by **m**, and the fraction **1/4m** is a normalization parameter that seems to work well for most studies. **s** is a column vector which, for two groups, contains either a 1 (if the node belongs to group A) or a -1 (if the node belongs to group B). The last term, **B**, is what is called a **modularity matrix**; this matrix contains the **degree** (i.e., the number of edges) between two nodes if they were placed at random. (For more details about the mathematics behind each of these terms, see the `Newman (2005) paper <https://www.pnas.org/content/103/23/8577.full>`__.) Conceptually, the equation represents the number of edges falling within a group, as compared to the expected number of edges that are placed at random within a similar-sized network.

The Louvain Algorithm
&&&&&&&&&&&&&&&&&&&&&

One of the most popular algorithms for maximizing this index is the **Louvain Algorithm** (`Blondel et al., (2008) <https://iopscience.iop.org/article/10.1088/1742-5468/2008/10/P10008/pdf?casa_token=Bqn_uVUg-N4AAAAA:rmElcqEgc9PmhQY_MDroocX24m-Vmgqd6N_wQon46oD3jvTxOJPmIF-8K9PVbTnzXIOzUW3CHA>`__). The algorithm first assigns a node to a module at random and calculates the resulting modularity index. If the index increases, then the node joins the new module; if the modularity decreases, then the node remains in its original module.

This procedure, also called **community detection**, organizes the nodes into modules, or communities, on each pass. A number of passes can be specified by the user to make as fine-grained partitions as is wanted.

.. figure:: AppendixA_Louvain.png

  An illustration of the Louvain algorithm (figure taken from Blondel et al, 2008). Nodes are assigned to a module based on the density of edges connecting nearby nodes - if the modularity index increases, then the node is assigned to that module. This procedure can proceed through several passes until a desired number of modules is reached.
  
Let's use the brain as an example to illustrate this algorithm. If we calculated all of the correlation coefficients between every voxel in the brain and decided to categorize them into four modules, one possibility is that we would end up dividing the brain into the four lobes (frontal, temporal, occipital, and parietal): regions that are anatomically and functionally distinct from each other. If we decided to do another pass, it is likely that we would end up with a network representation of the two hemispheres of the brain.
  
A related parameter is called **resolution**, which determines how fine-grained the resulting networks are. This is similar conceptually to the idea of multiple passes using the Louvain algorithm, but this method places a limit on how large the resulting modules can be. Using a certain resolution parameter with the brain example above may reproduce the canonical four lobes, while a higher resolution parameter can further divide these lobes into smaller sub-regions.

.. figure:: AppendixA_Resolution.png

  Example of tuning the resolution parameter, as shown in `Betzel & Basset (2017) <https://www.sciencedirect.com/science/article/pii/S1053811916306152>`__. The resolution parameter reflects the topological scale of interest: increasing it leads to finer scaled modules, but at some point it may start to model noise rather than biologically plausible modules. This parameter can't be set using the CONN toolbox, but it can be set in other toolboxes (such as the Brain Connectivity Toolbox).

  
**Thresholding** can also be used to remove edge values below a certain value. For example, a graph analysis of resting-state data may threshold the resulting connectivity maps to only show correlation values above 0.2, and remove everything else.

.. figure:: AppendixA_Thresholding.png

  Example of thresholding, taken from Taya et al. (2016).
  
Graph Theory in the CONN Toolbox
********************************

As you saw in a previous chapter on :ref:`viewing the results <CONN_10_Viewing_Results>`, one of the options to display the group-analysis is called "Graph Theory". Using the correlation maps as input, either ROIs are used as nodes, and the correlation values between the nodes represent the edges. As with any network dataset, the correlation values can be thresholded to only display those values that are the strongest and most robust.

.. figure:: AppendixA_CONN_Graph.png

  Within the CONN Results window, nodes are depicted as red circles, with the strength of the currently selected graph theory metric represented by the size of the circle. Edges between the nodes are depicted as black lines.
  
Here is a brief summary of what some of the measures mean. A fuller treatment of all of the graph theory metrics can be found on the `CONN website <https://web.conn-toolbox.org/fmri-methods/connectivity-measures/graphs-roi-level>`__.

1. **Degree**: Simply the number nodes that the current node is connected to, i.e. its number of edges.
2. **Cost**: Proportion of edges for the current node.
3. **Average path length**: Average shortest-path distance between the node and all other nodes.
4. **Clustering Coefficient**: Proportion of connected nodes across all neighboring nodes.

Which one you use is up to you.
