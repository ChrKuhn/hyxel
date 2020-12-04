
# Classification with fields

Final project for the Building AI course

## Summary

Aim is the investigation of an artificial field for classification problems.

## Background

Problem 1: Selection of features from a large set of possible features supervised training

The Principal component analysis (PCA) is a well-known analysis method to reduce the dimensionality of a high-dimensional feature space by searching of a smaller number of linear combinations (main components). Aim is to minimize the correlation of higherdi­mensional features by transformation into a new vector space with a new basis [1]. However, the principal component analysis depends on the classification problem because a covariance matrix must be computed for this problem. Furthermore, the PCA is „optimal“ only for normally distributed data sets. In context of classification, the PCA is used to evaluate the contribution of each feature to solve the classification problem.

Another way, which will be preferred in this project, is based on the quantization of the continuous feature space into small hyperspace elements (if we suppose, that we have a higher-dimensional feature space) and the implemented algorithm shall find automatically a small subset of features from a larger set of features in order to reduce the dimension of the feature space and the complexity of the trained classifier. The idea is to examine each hyperspace element (each little subspace within the whole feature space), which must have only objects of one class if the classification problem shall be solved successfully (in this case, class areas have no overlapping and a classifier can be trained with this data). This simple test implements a point-to-point-classification of each preclassified data point to the nearest center of a hyperspace element. Because the whole algorithm implements combinations of feature subsets without repetition [2], the implementation of the algorithm must be very fast.

Problem 2: Usage of an artificial field for classification problems

A well-known solution to implement a classifier algorithm is the nearest-neighbor-algo­rithm, which calculates the (Euclidean) distances between a test object to be classified and all objects of the training sample. If this is done, the algorithm looks for the shortest distance and returns the label of the classified object in the training sample for which the shortest distance was found (argmin decision). An extension to this is the k-nearest-neighbor-algorithm, which searches for the k shortest distances and returns their domina­ting label.

Instead the distance, we want to investigate the usage of a field for classification purposes. The electrostatic field is a field, which is known from physics and which describes the relations between loadings in a real world. In the context of a virtual feature space, which has no equivalent in the objective reality, we suppose each object or data point of a class has an „attraction potential“ in the sense of Hegel’s attraction [3]. Additionally to the nearest-neighbor-classifier, a data point gets a weight (similar to the loading in the context of the electrostatic field), which will be divided by the Euclidean distance to obtain the „attraction potential“ in a given distance from this object. Similar to the electrostatic field and the electrostatic force we assume an „attraction field“ and an „attraction force“ in the feature space, which is the basis for decisions about the class membership of test points to be classified.

A further step can be the reduction of the classification complexity. Until now, we have to calculate the attraction forces to all data points of a classified data set which represents the shapes of classes for a problem. If the data set is large, then we have to compute a large number of attraction forces. Aim of this step is to reduce the computation complexity by introducing prototypes which represent the classes and which can be used to model the class shapes. Class prototypes can be found by solving an optimization problem. The attraction potential causes a potential landscape in the feature space, where we have to found the position of the maximum potential or highest field strength for a class. For simplified classification calculations, a class can be represented by its prototypes, so that for a decision about the class membership of a test data point, only the attraction forces to the prototypes must be computed and not the attraction forces to all training data points. Contraints for solving the optimization problem are the separation planes between classes which must be fixed in order to get a low classification error combined with a reduced amount of representants of each class.

Problem 3: Usage of an artificial field for analysis of stability

The attraction fields form bassins for class shapes in which test data points will be attracted. The stability theory knows so-called Lyapunov functions that make statements  about the stability of dynamical systems [4]. It shall be investigated, in which way the attraction field can be used as a Lyapunov function.

An application for this could be a control for a stepper motor, which must detect and avoid step errors.

## Data and AI techniques

For the selection of a subset of suitable features of a larger feature set for the further training of a classifier I had access to two training samples with each 23 features and 7 different classes (from an electroencephalogram with 7 different sleep states of the proband) . The feature selection itself uses a point-to-point classification-method which examines the frequency vector of each subspace of the feature space. The frequency vectors of all relevant (non-empty) subspaces must be computed very fast in order to find all suitable feature combinations in a reasonable time. To fulfill these requirements, a quantized data set consisting of selected features was used as input for an algorithm which creates a partition tree in short time.

Artificial data were used for the investigation of the attraction field. The artificial data includes convex class shapes, non-convex class areas like moons (crescents) or a sphere embedded in a hollow sphere. Two-dimensional data is a good basis for visualization of the results. However, the algorithms shall be able to handle higher-dimensional problems, too.

For investigation of the control of a stepper motor, an evaluation board produces its own data in real-time by measurement of interesting voltages and output values. But the used microprocessor is too small for analysis and visualization of these data (it supports only integer arithmetic and has the task of measurement and control). So it is necessary to use a fast connection (hardware bus) to transmit data to a more comfortable analysis and test system with a graphical user interface.


## How is it used?

Test data for classification and feature selection comes from electroencephalograms, so that results could be used in the context of biomedical techniques. The control of a stepper motor is a problem coming from drive technology. But is conceivable, that algorithms could be used in image processing, too.


## Challenges

Although the algorithm for the feature selection can be realized with integer arithmetic, which is faster than floating point arithmetic, it needs approximately 10 days for a complete solution. The algorithm can be massively parallelized using OpenCL and a graphics card, so that it runs now 60 times faster than the solution with pure integer arithmetic and takes „only“ 4 hours. Fortunately, if the algorithm starts with a subset consisting of one feature only and increases successively the complexity, the first solution can be found after some seconds and is the best, and any solution found further (with more features) is worse compared to the first solution because the classification costs (for a classifier to be trained later) are higher, if we consider the number of features as costs only.

A precondition of any stepper control loop is the determination of the real rotor position of the stepper motor in comparison of the position of the rotating field. Some solutions uses additional sensors to determine the rotor position, Conceivable could be an incremental encoder with a high resolution, too. The preferred solution in this project is the computation of the rotor angle with aid of the inductances, but it places high demands on the microprocessor and its periphery. Now we have (after a many trials) found a microcontroller, which is potentially usable to realize a sensorless determination of the rotor position.

A headless analysis and test system (usable in an embedded environment) contains imple­mentations of algorithms for data processing. It is desirable that small applications of them can be created for experimental purposes like small Python programs. There are environ­ments on the market, which support graphical programming of data flow diagrams. However, a pure data-flow diagram don’t allow the creation and the destruction of data elements or objects. Sometimes it is necessary, that objects can change their state (a classifier is unusable if it is untrained and can be used if it was trained), so that a graphical language is needed, which should allow the data processing (or data flow) and additionally a control flow which is well-known from imperative programming languages. For its realization, it was neccessary to create a database with meta-knowledge about the implemented algorithms, their interfaces, parameters, data types and so on. Unfortunately, its maintenance is a very complex process. 


## What next?

The source code of all implemented algorithms must be reviewed and debugged. Some hardware architectures have restrictions that make this necessary in order to port the code to these architectures.

The sensorless determination of the rotor angle is a precondition for all further investigations of a control loop or stability with the stepper evaluation board. For this, the source code and the circuitry must be debugged and optimized. A possible kind of optimization could be an evolutionary strategy for parameter optimization in order to realize a sensorless position detection with a high resolution, suitable for a control loop and stability analysis.

The actually preferred workflow of programming with the graphical language is as follows: create a graphical diagram, which represents a small application → translate it and load it to the embedded analyzing system → execute it. In this case, creator of the application is a human with knowledge about the problem to be solved, the data and needed transformation algorithms. It is conceivable, that an algorithm with access to the meta-knowledge about algorithms, parameters, data types as well as the environment of the system, quality of data could change itself. To realize this, an evolutionary strategy must be able to create new systems derived from the last generation, which are able to process incoming data, but must also be able to valuate this processing and the generated results (but this is still a thought experiment only and not one of the next main tasks).


## Acknowledgements

[1]	https://de.wikipedia.org/wiki/Hauptkomponentenanalyse

[2]	https://de.wikipedia.org/wiki/Kombination_(Kombinatorik)

[3]	http://www.zeno.org/Philosophie/M/Hegel,+Georg+Wilhelm+Friedrich/Wissenschaft+der+Logik/Erster+Teil.+Die+objektive+Logik/Erstes+Buch%3A+Die+Lehre+vom+Sein/Erster+Abschnitt%3A+Bestimmtheit+(Qualit%C3%A4t)/Drittes+Kapitel%3A+Das+F%C3%BCrsichsein/C.+Repulsion+und+Attraktion/a.+Ausschlie%C3%9Fendes+Eins 

[4]	Mikut, R.: Modellgestützte online Stabilitätsüberwachung komplexer Systeme auf der Basis 	unscharfer Ljapunov-Funktionen, Fortschritts-Berichte VDI, Düsseldorf, 1999

[5]	The EEG data for testing the feature selection algorithm came from Prof. Wenzel, University 	of Applied Sciences, Schmalkalden.

[6]	Other EEG test data came from A. Goetze, TU Ilmenau and G. Schickhuber, Ostbayerische Technische Hochschule Regensburg.
