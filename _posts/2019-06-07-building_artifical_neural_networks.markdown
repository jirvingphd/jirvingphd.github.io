---
layout: post
title:      "BUILDING ARTIFICAL NEURAL NETWORKS:"
date:       2019-06-07 13:42:22 -0400
permalink:  building_artifical_neural_networks
---

# A Beginner's Guide to Playing (Artificial) God

Coming from a neuroscience background, performing _in vivo_ electrophysiology experiments (recording and analyzing the electrical activity of neuorns), I was intrigued when first exposed to artificial neural networks (ANNs). 

My understanding of the  neural circuitry, neurotransmission, and neuroplasticity gave me a very strong intuitive understanding of how they work. But an intuitive understanding vs. practical usability are _not_ the same thing...

Below we will first dive into a high-level primer on how neurons communicate before turning to designing ANNs.
## The Brain: A Primer

### In the beginning...


<img src="https://raw.githubusercontent.com/jirvingphd/dsc-4-final-project-online-ds-ft-021119/master/blog/brain_neural_network_illustration.png" width="500">

> The human brain contains approximately **86** ***billion*** neurons. 
>       - Dr. Suzana Herculano-Houzel, Vanderbuilt University

### How Real Neurons Process Information

For the purposes of this introduction, we are going to use a superficial now-ancient-dogma view of how neurons work. The rosy, organized, easier-to-wrap-your-head-around version I was taught back when I started undergrad. This is actually perfect for our needs, considering that this is what we knew back when neural networks inspired data scientists to create the very first artificial neural networks (ANNs) back in late 1950s through late 1980s.


<img src="https://raw.githubusercontent.com/jirvingphd/dsc-4-final-project-online-ds-ft-021119/master/blog/neuron-gif_orig.gif" width="500">

A neuron is a specialized brain cell that receives, processes, and transmits information, and is sort of shaped like a tree. 

It has leaves on branches that spread out from the top of its trunk, which is a long central rod/pole/tube through which water and energy from the sun are transferred. If you follow the trunk down into the ground, it has roots that extend out deep into the earth. In a neuron, the branches and leaves are called the dendtrites. They all spread out form one central trunk of the neuron, called the soma/body. If you travel down the neuron's trunk, called the axon, you will reach its roots, which are called terminals.

~~receive incoming information, like the leaves of a tree receive energy from the sun. The branches all stem from one center point at the top of its trunk.~~

A neuron receives input information in the leaves(synapses) of its dendrites, that information is funneled down the branches to where they all meet, the top of the trunk. Then a calculation is made and if the input signal is strong enough that information is sent down through the trunk of the neuron (down its axon) and down into its roots/terminals.


<i mg src="https://raw.githubusercontent.com/jirvingphd/dsc-4-final-project-online-ds-ft-021119/master/blog/neuron-gif_orig.gif" width="500">


This process is called "neurotransmission" and is called an  _electro-chemical process_, meaning that it has both an electrical component and a chemical component. The chemical component is the many different neurotransmitters(like dopamine, serotonin, and glutamate) that are sent from one neuron to other neurons that are connected by synapses.

<img src="https://raw.githubusercontent.com/jirvingphd/dsc-4-final-project-online-ds-ft-021119/master/blog/synapse-7d_orig.gif" width="300">

The electrical component is a large, incredibly fast, spike of electricity that shoots through a neuron from its head to its tail. This spike of electricity is called the action potential and it we refer to when we say your neurons are "firing". 

> So how and why is information transmission divided into these separate, but interconnected and inter-dependent pieces?


Now, the action potential has a couple special properties that are essential to neurotransmission. 
1) An action potential is all-or-nothing. Either a neuron fires, or it doesn't. It's a binary process. 
2) An action potential only happens if a cell gets enough excitatory input from other neurons. A neuron is constantly receiving excitatory and inhibitory inputs from thousands of neurons that are essentially playing tug-of-war, with excitatory signals pushing the neuron closer and closer to firing, while the inhibitory signals are doing the opposite, trying to prevent the neuron from firing, pushing it down into a quiet state. 

If the excitatory input is strong enough, it overpowers any inhibition, build up enough electrical charge to reach a special electrical threshold that will open a cascade of special channels allow a tidal wave of electrical charge to rush into the neuron and down its axon to its terminals. The terminals are connected to thousands of other neurons, and this rush of electrical activity launches boats of neurotransmitters into the synapse to pass the information to the next layer of neurons. 




The action potential travels down a neuron's tail (the axon) until it reaches the neuron's feet(its terminals), which are like branching roots that form connections with _other_ neurons. But the _connection_ isn't a physical one, there is a gap between each neuron that needs to be traveled through to reach the next neuron. 


A prototypical neuron carries informaton in one-direction, from the top of its head, where it receives neurontransmiters from other neurons, to its tail where it sends out its own neurotranmissters onto other neurons. .

The head contains made of 
1. Dendrites 
2. Soma 



## Artificial Neural Networks: 
