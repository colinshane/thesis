# Modelling learning tasks in artificial and spiking neural networks
Building on research from cognitive and computational neuroscience, deep
learning is evolving rapidly and has even surpassed humans in some recognition
tasks [1]. Contemporary theories from cognitive neuroscience however, tell us
that learning in the biological brain occurs in spiking neural networks (SNN)
instead of the layered artificial neural networks (ANN) traditionally used in machine learning [2].

Because of this biological similarity SNN are of great interest to neuroscientists [7].
They are unfortunately difficult to program, and the experiments that are being executed in spite of this
have a low external validity, because of the heterogeneous and unstable software underlying the simulations.
The expertise needed to overcome these obstacles are inaccessible for most neuroscientists.

A further challenge for SNNs is the relatively poor understanding of learning mechanisms compared to ANN [4, 5].
Several different techniques have been applied to understand learning in SNNs [6, 7], but they are difficult to test because of the diverse implementations.

This thesis sets out to create a unified domain specific language (DSL), Volr, for modelling both ANNs and SNNs. 
The DSL aims to provide a concise description of neural networks and to ensure the correct translation into executable neural network models.
This thesis will implement translations into three heterogenous backends: 
Futhark for non-spiking ANNs, NEST (Neural Simulation Toolkit) for simulated SNNs and finally
BrainScaleS for analogue SNNs.

To validate the DSL, a cognitive model inspired by the theory for the Reorganisation
of Elementary Functions (REF) [8] is constructed and applied to solve a small MNIST classification task.

## Hypotheses

This thesis examines two hypotheses:

1. *The Volr DSL can translate into spiking and non-spiking neural networks such that the network topologies are retained.*
2. *Using training it is possible for spiking and non-spiking models to solve an MNIST recognition task.*

The topology of a network is understood as typed declarations of neural network structure, defined as a collection of nodes, edges and connectivity descriptions.

The first hypothesis tests that the neural networks generated by the DSL is translated without significant deviations, regardless of the backend.
This is an important concept to ensure correct and reproducible experiments.
It is also vital to further the understanding of spiking neural networks, because a correct translation bridges the semantics of artificial and spiking neural networks.

The second hypothesis tests that the DSL adequately models the domain it sets out to describe: cognitive neural network models.
By drawing on the REF theory [9] a network model is written and evaluated to verify that the DSL can capture complex neural network concepts.

## Experimental setup
The network description will be translated into a spiking and non-spiking model.
For the layered non-spiking ANN model, code will be generated to run on Futhark and will be executed through OpenCL [10].
For the spiking model the neural simulation framework PyNN will act as an
interface to both the simulated and analog backends.
PyNN is compatible with both NEST and BrainScales, and
will in turn generate code that can execute on NEST [9] and 
BrainScaleS [6].

The three executable models will be applied to the same MNIST task, but will be trained differently due to hardware limitations.
Both the NEST and Futhark model will be trained using supervised learning, but the BrainScaleS model will be constructed based on the network weights of the NEST simulation, and will not be trained directly.
If any of the three implementations perform significantly different with respect to learning rate and accuracy, it falsifies the first hypothesis, proving that the translations cannot be compared.

The DSL itself will be implemented in Haskell.

## Tasks
The tasks involved in this thesis are as follows:

* Implement the Volr DSL in Haskell
* Implement a translation from the DSL to an executable and trainable Futhark ANN
* Implement a translation from the DSL to an executable and trainable NEST simulation
* Implement a translation from the DSL to an executable program on the BrainScaleS platformn
* Implement encoders and decoders for the spiking neural networks to correctly translate stimuli and outputs between spiking and non-spiking network models
* Build a model that can solve an MNIST experiment in the DSL
* Translate the MNIST model to Futhark, NEST and BrainScaleS
* Train the Futhark and NEST programs, and emulate the BrainScaleS program using the weights from the NEST model
* Compare the performance of the experiments, with a focus on learning rate and accuracy.

## Learning objectives

* Survey models for learning through backpropagation in spiking and non-spiking neural networks
* Implement backpropagation learning in spiking and non-spiking neural networks
* Implement ANNs in the data-parallel functional language Futhark
* Implement SNNs in NEST and BrainScaleS
* Analyse and compare the SNN model performance through learning rate and accuracy scores

## Additional objectives

Due to the extension of the thesis several additions have been made:

* An extra hypothesis that tests the semantic translation
  between the DSL and the backends
* An implementation for the backpropagation algorithm for temporal SNNs
* Additional tasks for the backend implementations (see tasks)

## Thesis milestones
The thesis has been extended to the
23rd of January. The remaining process is divided into four miletones with associated
deadlines:

| Milestone | Date of completion |
| ---------------------------------------------- | ------------------ |
| Complete the translations to the three backends| 10th of November |
| Construct and train the MNIST model | 3rd of December |
| Execute, analyse and compare experiments | 10th of December |
| Describe and evaluate the Futhark, NEST and BrainScales implementations | 17th of December | 
| Finish writing | 31st of December |

## Sources

1. J. Schmidhuber: "Deep Learning in Neural Networks: An Overview",
Neural Networks, pp. "85-117, Volume 61, 2015.
2. Peter Dayan and L. F. Abbot: "Theoretical neuroscience - Computational and Mathematical Modeling of Neural Systems", MIT Press 2001.
3.  Thomas Pfeil, Andreas Grübl, Sebastian Jeltsch, Eric Müller, Paul Müller, Mihai A. Petrovici, Michael Schmuker, Daniel Brüderle, Johannes Schemmel and Karlheinz Meier: "Six networks on a universan neuromorphic computing substrate", Frontiers in Neuroscience, 2013.
4. A. Tavanei and Anthony S. Maida: "A Minimal Spiking Neural Network to Rapidly Train and Classify Handwritten Digits in Binary and 10-Digit Tasks", International Journal of Advanced Research in Artificial Intelligence, Vol. 4, No. 7, 2015.
5. Florian Walter, Florian Röhrbein and Alois Knoll: "Neuromorphic implementations of neurobiological learning algorithms for spiking neural networks", Neural Networks,
Volume 72, pp. 152-167, 2015.
6. Sander M. Bohte, Joost N. Kok and Han La Poutr: "Error-backpropagation in temporally encoded networks of spiking neurons", Neurocomputing, Elsevier 2002.
7. Daniel Brüderle, Mihai A. Petrovici, Bernhard Vogginger, Matthias Ehrlich, Thomas Pfeil, Sebastian Millner, Andreas Grübl, Karsten Wendt, Eric Müller, Marc-Olivier Schwartz, Dan Husmann de Oliveira, Sebastian Jeltsch, Johannes Fieres, Moritz Schilling, Paul Müller, Oliver Breitwieser, Venelin Petkov, Lyle Muller, Andrew P. Davison, Pradeep Krishnamurthy, Jens Kremkow, Mikael Lundqvist, Eilif Muller, Johannes Partzsch, Stefan Scholze, Lukas Zühl, Christian Mayr, Alain Destexhe, Markus Diesmann, Tobias C. Potjans, Anders Lansner, René Schüffny, Johannes Schemmel and Karlheinz Meier: "A Comprehensive Workflow for General-Purpose Neural Modeling with Highly Configurable Neuromorphic Hardware Systems", Biol Cybern. 2011 May;104(4-5):263-96.
8. Jesper Mogensen and Morten Overgaard: Reorganization of the Connectivity between Elementary Functions - A Model Relating Conscious States to Neural Connections, Fronties in Psychology, 8:652, 2017.
9. Kunkel Susanne, Schmidt Maximilian, Eppler Jochen M., Plesser Hans E., Masumoto Gen, Igarashi Jun, Ishii Shin, Fukai Tomoki, Morrison Abigail, Diesmann Markus, Helias Moritz: "Spiking network simulation code for petascale computers", Frontiers in Neuroinformatics, Vol. 8, p. 78, 2014.
10. Henriken, Troels: "Design and Implementation of the Futhark Programming Language", Ph.D. thesis, Faculty of Science, Copenhagen University 2017.

