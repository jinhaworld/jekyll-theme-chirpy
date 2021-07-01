---
title: Intermediate Graph Theory 
categories: [Computer Science, Graph Theory]
tags: [algorithms]     # TAG names should always be lowercase
--- 
This is a guide that I made a while ago for graph theory. It doesn't include any algorithms (will have another post on it) but more of formal definitions and properties of different types of graphs. It will likely be updated & improved in the future. Please also note that this post does not necessarily deal with cp. 

# Core Terminology 

A graph $G$ is defined as a pair of sets $(V, E, \phi)$ where $E$ is the set of edges, $V$ is the set of nodes, and $\phi$ is a mapping from $E \rightarrow V \times V$. Note that the mapping $\phi$ is the incidence function, mapping an edge into a pair of vertices (end-vertices of an edge). However, it is common for a graph to be defined just as $(V, E)$. 

We refer to the graph's edges as $E(G)$ and vertices as $V(G)$ while their orders are respectively $\mid E(G) \mid$ and $\mid V(G) \mid$. 

A directed graph (digraph) is a graph $G = (V, E)$ where $V$ is a set of nodes and $$E \subseteq \{ (x, y) \mid (x,y) \in V^{2} \textrm{ and  } x \neq y \} $$. Essentially, each edge in a directed graph has a direction. Using the incidence function, for $x \in E, \phi(x) = (u, v)$, then for the end-vertices $u, v$ in a directed graph, edge $x$ is both the in-coming edge of $v$ and also the out-going edge of $u$.    

Common problems involving directed graphs are those that involve an one-sided relationship such as the transferral of money from one person to another (resulting in directed edges). 

A more precise name for the directed graphs as defined above are directed simple graphs. Directed multigraphs accounts for multiple edges unlike directed graphs. (multiple edges/parallel edges are two edges between the same two nodes).  

A directed multigraph is defined as the ordered triple $G = (V, E, \phi)$, where $V$ is the set of nodes, $E$ is the set of Edges, and $\phi$ is an incidence function $$ \phi: E \rightarrow \{(x,y) \mid (x,y) \in V^{2} \textrm{ and } x \neq y \}$$. As shown by the incidence function, a directed multigraph by definition cannot have multiple edges. As such, it is trivial to note that the mapping $\phi$ is injective. 

A symmetric digraph is a directed graph such that there are symmetric edges (two directed edges).

An undirected graph is a graph where $V \times V$ is a set of unordered pairs. Essentially, none of the edges have direction and are denoted as undirected edges. Also note that an undirected graph is a symmetric digraph. 

A simple graph is a graph that does has neither loops nor multiple edges. By the same logic above for directed multigraphs, the mapping $\phi$ will be injective for all simple graphs. 

A bipartite graph $G$ is a graph where $V(G)$ can be partitioned into two subsets $U, V$ such that every edge's two end-vertices are split into $U$ and $V$. The partition of $G$, $\{U, V\}$ is noted as the bipartition of graph $G$.  

# Density of Graph  
The density of a graph displays the relationship between the max of edges with the order of $E$ as a function of $V$. 

The maximum number of edges for an undirected graph $G(V,E)$ is 

$$ Max_{undirected}(V) = { {\mid V \mid} \choose {2}} = \frac{\mid V \mid \cdot (\mid V \mid - 1)}{2} $$. 

This is simply due to basic combinatorics that for vertices $V$, the maximum possible number of edges occurs when there is an edge between all pairs of vertices. 

The maximum number of edges for a directed graph $G(V,E)$ (more specifically, a directed multigraph) is 

$$ Max_{directed} = 2 \cdot { {\mid x \mid} \choose {2}} = \frac{2 \cdot \mid V \mid \cdot (\mid V \mid - 1)}{2} = 2 \cdot Max_{undirected}(V) $$. 

This is true as well as a directed multigraph allows multiple / parallel edges. Thus, we can simply multiply 2 to the value of the maximum number of edges in an undirected graph by creating two multiple edges for the end-vertices of each edge. 

The density of a graph $G(V, E)$ can be defined as $\frac{\textrm{tot num of edges}}{\textrm{maximum number of edges}}$. Note that by definition, the density is $\leq 1$. 

The density for an undirected graph $G(V, E)$ is 
$$  D_{undirected}(V, E) = \dfrac{\mid E \mid}{Max_{undireccted}(V)}  = \frac{\mid E \mid}{\frac{\mid V \mid \cdot (\mid V \mid -1)}{2}} = \frac {2 \cdot \mid E \mid}{\mid V \mid \cdot (\mid V \mid -1)}$$ 

Logicially, it follows that as $Max_{undirected} = 2 \cdot Max_{directed}$, 

$D_{directed}(V, E) = \frac{D_{undirected}(V, E)}{2}.$ 

Now we can use our new metric for density to distinguish between sparse and dense graphs. 

A graph that has a density is lesser than a half is sparse while a graph that has a density that is grater than half is dense. 


# Cut in Graph Theory 

A cut for a graph $G(V,E)$ is a partition of $V$ into two disjoint subsets, two sets with no elements in common. A cut $X$ is a proper subset of $V$, $X \neq \emptyset$, $X \subset V$. 

A cut-set, created by a cut, is the set of $E$ where one of its end-vertices are in each partition. Such edges are also noted to cross the cut. 

# Conductance of graphs and cuts   

Unlike the section for density of a graph, we will not prove these results (as its more of definitions). 

For an undirected graph $G(E,V)$ and a cut $(X, \bar{X})$, the conductance of X is defined as 

$\Phi_{G}(X) = \frac{\delta(X)}{min(vol_G(X), vol_G(V-X))}$. where volume is 
defined as the sum of degrees of $v$ in $S$. 

The conductance of graph G is defined as $min_{\textrm{x cuts of G}}\Phi_{G}(S)$. 

Conductance of graphs have various applications in Markov chains and for spectral clustering. 

