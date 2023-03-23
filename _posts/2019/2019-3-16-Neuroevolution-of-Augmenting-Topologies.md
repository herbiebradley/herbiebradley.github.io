---
title: "Neuroevolution of Augmenting Topologies"
date: "2019-9-22 10:00"
description: Understanding the popular algorithm for evolving neural networks.
image: /assets/img/2019/NEAT_gene.jpg
header-image: true
main: false
tags: [Computer Science, AI]
---

A few months ago in the Spring I presented the 2002 paper [Evolving Neural Networks through Augmenting Topologies (NEAT)][a888f635] for a class, so I thought I should make a blog post summarising this popular algorithm for evolving the architecture of neural networks.

To set the scene, at the time NEAT came out the approach of evolving the weights of neural networks had shown promise for benchmarks such as pole balancing. These days problems like this are usually solved using non evolutionary deep reinforcement learning techniques. Before NEAT, several papers had tried evolving the topology of networks but were not able to outperform a fixed network. NEAT was the first algorithm to do so, which is why it is frequently used and very well known.

The authors identified 3 key problems that would need to be solved:
1. How should we encode network architectures to allow for crossover between topologies?
2. If we have an evolved topology that needs several generations to optimise, how can we protect that innovation and stop it from disappearing?
3. How can we minimise the complexity of topologies throughout evolution?

## Genetic encoding

Any encoding for network architectures will fall into one of two categories, direct or indirect. A direct encoding will describe everything about an individual network - each gene will describe a node or connection in the network. An indirect encoding tends to specify the processes for creating an individual network. As a result they are more compressed and the algorithm can compute each generation faster, but if not designed carefully they can focus the search on suboptimal network topologies, creating a bias in the search space.

The main issue with encoding neural networks as genes is the competing conventions problem: having more than one way to represent the same network. When two genomes representing the same network structure do not have the same encoding, crossover is likely to produce damaged offspring. The key insight in NEAT's encoding is that two genes with the same historical origin represent the same structure. We can mark genes according to their history by including a global "innovation number" for each edge in the network graph, essentially a version number for each edge.

The NEAT encoding scheme is designed to allow corresponding genes to be easily matched up during crossover.  Each genome is a list of connection genes representing edges in the graph, each of which refers to two node genes (see header image). The connection genes specify the in node, out node, weight of the edge, whether or not the edge is in the graph, and the innovation number. So the genome (or genotype) is the list of connection genes and node genes, and the phenotype is the neural network diagram.

There are two possible mutations to a genome: adding a node or adding a connection. When any connection is added, the global innovation number is incremented and assigned to that gene. Therefore the innovation numbers represent a history of every gene in the system.

## Protecting innovation

In a topology evolving network, adding new structure usually causes the fitness of a network to decrease, since adding a new connection (for example) causes the fitness to reduce temporarily before the connection's weight has a chance to optimise. Several generations may be required to optimise new structure and make use of it. NEAT solves this by dividing the population into species based on topological similarity, so individuals compete with others in their own species and have a chance to optimise before they have to compete with the overall population.

NEAT uses the innovation numbers to determine topological similarity, by calculating a compatibility distance between two genomes. This is based on the number of connection genes with different innovation numbers in each genome, along with the average weight difference between connection genes that share the same innovation number. This distance measure allows us to divide the population of genomes into species using a threshold. Genomes are tested sequentially - if a genome's distance to a randomly chosen member of a species is less than the threshold then it is placed into this species, ensuring species do not overlap.

As a reproduction mechanism, NEAT uses fitness sharing, where all organisms in the same species share their fitness. The original fitness for each genome is adjusted by dividing by the number of individuals in the species. Species then grow or shrink depending on how high their average adjusted fitness is. Species reproduce by first eliminating the lowest performing members, then the entire population is replaced by the offspring of the remaining organisms in each species.

## Minimizing dimensionality

Typically, topology evolving networks start with an initial population of random topologies. However, a random population has a large amount of network structure that has not been evaluated for fitness and may be completely unnecessary. Therefore, with random initialisation the algorithm may waste effort by optimising unnecessarily complex structures. NEAT's solution to this is to start with a uniform population of networks with zero hidden nodes. New structure is introduced incrementally with mutations, and the only structures that survive are found to be useful via fitness evaluations.

Hence NEAT minimises the complexity of not only the final network, but all intermediate networks along the way. This significantly reduces the number of generations needed to find a solution.

## Experiments & Results

In the NEAT paper, the algorithm was tested on single and double pole balancing tasks - a benchmark in which one or two poles are connected to a moving cart by a hinge. The objective is to move the cart left or right to keep the poles upright. NEAT was tested against two rival algorithms, Cellular Encoding, which evolves network structure, and Enforced Subpopulations, which only evolves weights. Two versions of this test were run - first allowing the network to see the cart velocity, and then hiding this information. In both tasks NEAT proved to be much more efficient than the rival algorithms. In the harder task with velocity hidden, NEAT used 25 times fewer evaluations to reach a solution than Cellular Encoding, 5 times fewer evaluations than Enforced Subpopulations, and never needed to restart. Hence NEAT showed a significant performance advantage over previous neuroevolutionary algorithms.

NEAT was also evaluated with an ablation study - a study in which different components of the system are removed in turn to see which components are critically important. These experiments used the single pole balancing task, and tested NEAT with no growth of the topology, a random initial population rather than starting from a population with no hidden units, and a version without species. All showed worse results than the full version of NEAT, proving that all components of the algorithm are important for its performance.

## Discussion

One thing NEAT demonstrates is that it is not the ultimate structure of the evolved network that really matters, but rather the structure of all the intermediate networks along the way to finding the solution. The connectivity of every intermediate solution represents a parameter space that evolution must optimize, and the more connections there are, the more parameters need to be optimized. Therefore, if the amount of structure can be minimized throughout evolution, so can the dimensionality of the spaces being explored, leading to significant performance gains. By starting simple and evolving the network as it becomes incrementally more complex, each increase in complexity resulting from new structure leads to a promising part of a higher dimensional space because most of the existing structure is already optimized.

The NEAT algorithm is interesting on its own, but there are also several effective variants of the algorithm including [HyperNEAT][dce12457], which is designed to produce large scale networks by evolving repeated patterns and structures.

I hope this post was informative - I intend to make more posts very soon covering some deep learning models I have been studying.

  [dce12457]: http://axon.cs.byu.edu/~dan/778/papers/NeuroEvolution/stanley3**.pdf "HyperNEAT"
  [a888f635]: http://nn.cs.utexas.edu/downloads/papers/stanley.ec02.pdf "NEAT"
