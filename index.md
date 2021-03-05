---
layout: default
title: Machine Learning for Signal Processing
---

# Machine Learning for Signal Processing

### First semester 2021

Andrés Marrugo, PhD
*Universidad Tecnológica de Bolívar*

##  Aims and Scope

Signal Processing deals with the extraction of information from signals of various kinds. This process has two distinct aspects: characterization, and categorization. Traditionally, signal characterization has been performed with mathematically-driven transforms and operations, whereas categorization and classification are operations associated with the use of statistical tools.

Machine learning uses statistical techniques to design algorithms that give computer systems the ability to learn about the state of the world directly from data. In the context of Computer Science, to learn can be explicitly defined as to improve performance on a specific task progressively, without being explicitly programmed. An increasingly popular trend has been to develop and apply machine learning algorithms to both aspects of signal processing.

In this course, we discuss the use of machine learning techniques to process signals. We cover a variety of topics, from data-driven approaches for characterization of signals such as audio including speech, images and video, and machine learning methods for a variety of speech and image processing problems.

In this course, the student will obtain proficiency in:

- Machine learning concepts: methods of modeling, estimation, classification, and prediction.
- In sound processing: such as denoising and separating sounds in mixtures.
- In image processing and computer vision: such as image restoration,
object detection, recognition, biometrics.
- In carrying out the software implementation of simple applications.

Prior knowledge of this course includes probability, linear algebra, and calculus. Programming experience in MATLAB or Python is desirable, but not required.


## Useful Resources

### Tutorials, review materials

- [MATLAB tutorial](matlab.intro.html)
- More MATLAB tutorials: [basic operations][bo], [programming][pro], [working with images][wim]
- [Linear algebra review](http://www.cse.ucsd.edu/classes/wi05/cse252a/linear_algebra_review.pdf)
- [Random variables review](http://www.cse.ucsd.edu/classes/wi05/cse252a/random_var_review.pdf)
 
[bo]: matlab_ops_tutorial.m
[pro]:matlab_prog_tutorial.m
[wim]: matlab_image_tutorial.m

### MATLAB reference

- [MATLAB guide from Mathworks](http://www.mathworks.com/access/helpdesk/help/techdoc/matlab.html)
- [MATLAB image processing toolbox](http://www.mathworks.com/access/helpdesk/help/toolbox/images/)
- [Writing fast code](http://www.mathworks.com/matlabcentral/fileexchange/5685)

### Python references

- [Introduction to Data Science using Python](https://www.udemy.com/course/introduction-to-data-science-using-python/)
- [MatPlotLib with Python](https://www.udemy.com/course/matplotlib-with-python/)
- [Machine Learning With Python](https://www.tutorialspoint.com/machine_learning_with_python/machine_learning_with_python_tutorial.pdf)

## Outline

This website will be updated as we go along.

### Lecture 1: Introduction

We will be discussing the main aspects and motivation for using ML techniques in Signal Processing. 
<!-- Also a brief overview of the Linear Algebra involved in the course. -->

- [Lecture 1 slides](https://www.dropbox.com/s/vzx3delc8q274na/Class1.Introduction.pdf?dl=0)   

   
<!-- - [Linear Algebra slides](https://www.dropbox.com/s/7c3ntm6ohw6ld9w/cs131_linalg_review.pptx?dl=0) -->


### Lecture 2: Linear Algebra Refresher
We will be reviewing the fundamentals of Linear Algebra.

- [Lecture 2 slides](https://www.dropbox.com/s/18ll2ar8qeiig6k/Class2.LinearAlgebra.pdf?dl=0)
- [Projections from Linear Algebra by G Strang](https://www.dropbox.com/s/zzy8hwatr4yvaxt/Projections-Strang.pdf?dl=0)


### Assignment 1
A summary of Linear Algebra exercises. **Due date:** 2021-03-14.

- [Assignment 1]({{site.url}}assignment1)
- [Short-time DFT](https://www.dropbox.com/s/jpl2yofgjud3er3/short-time-dft.pdf?dl=0)
- [Upload link](https://www.dropbox.com/request/ufwyyXKKu8QGFZv2BTjW)

### Lecture 3: Linear Algebra Refresher II
We will be reviewing the fundamentals of Linear Algebra.

- [Lecture 3 slides](https://www.dropbox.com/s/tr3wzkud2s5fiij/Class3.LinearAlgebra.pdf?dl=0)
- [Gilbert Strang's notes on Eigen analysis](http://math.mit.edu/linearalgebra/ila0601.pdf)
- [An awesome video lecture on SVD](http://freevideolectures.com/Course/2052/Linear-Algebra/30)

#### Projections example: music score

- [Lecture notes](https://www.dropbox.com/s/yjv9n95ux6ikngy/class3.projections-music-score.pdf?dl=0)


### Lecture 4: Signal Representations
We will be discussing the representation of signals, especially the DFT.

- [Lecture 4 slides](https://www.dropbox.com/s/k7d86nezcwewex7/Class4.signalrepresentations.pdf?dl=0)
- [Anti-aliasing slides](https://www.dropbox.com/s/dlugghj72ph21c3/Class4.anti-aliasing.pdf?dl=0)
- [Anti-aliasing example](https://www.dropbox.com/s/86t3eigv1lgjn22/demo_subsampling.m?dl=0)


### Lecture 5: Eigenfeatures
We will take a look at finding data-dependent bases.

- [Lecture 5 slides](https://www.dropbox.com/s/3pizuie2wu0dk1p/Class5.eigenfeatures.pdf?dl=0)


### Lecture 6: Face Detection
We will take a look at finding data-dependent bases.

- [Lecture 6 slides](https://www.dropbox.com/s/qz6i2ckk1l01udf/Class6.facedetection.pdf?dl=0)

### Assignment 2
A summary of Linear Algebra exercises. **Due date:** 2021-04-04

- [Assignment 2]({{site.url}}assignment2)
- [Assignment 2 - hints]({{site.url}}assignment2_hints)
- [Upload link](https://www.dropbox.com/request/Sl2Ul3pyEtB1KSGEHkwT)

<!-- ### Lecture 6: Sparse Representations in Image Processing - Invited -->
<!-- We will take a look at finding data-dependent bases. -->

<!-- - [Lecture 6 slides](https://www.dropbox.com/s/5yn5c4y7mu9zudn/Sparsity_Master_UTB_2018_2.pdf?dl=0) -->




<!-- ### Lecture 8: Compressed Sensing - Invited -->
<!-- We will take a look at finding data-dependent bases. -->

<!-- - [Lecture 8 slides](https://www.dropbox.com/s/jfmd7rudbmw78w6/main_compressed_sensing_Bacca.pdf?dl=0) -->

<!-- ### Lecture 9: Independent Component Analysis -->
<!-- We will take a look at finding data-dependent bases. -->

<!-- - [Lecture 9 slides](https://www.dropbox.com/s/hazdpd2jsqxy7i1/Class7.ica.pdf?dl=0) -->

<!-- ### Lecture 10: Clustering -->
<!-- We will take a look at finding data-dependent bases. -->

<!-- - [Lecture 10 slides](https://www.dropbox.com/s/v5y1pn6eo9577dc/class8.clustering.pdf?dl=0) -->

<!-- ### Lecture 11: Expectation Maximization -->
<!-- We will take a look at finding data-dependent bases. -->

<!-- - [Lecture 11 slides](https://www.dropbox.com/s/knd6ujaa55r3gh9/class9.expectationmaximization.pdf?dl=0) -->
<!-- - [Matlab code](https://www.dropbox.com/s/ts9ik3bukszpatm/expectation_maximization.m?dl=0) -->

<!-- ### Lecture 12: Regression and Prediction -->
<!-- We will take a look at finding data-dependent bases. -->

<!-- - [Lecture 12 slides](https://www.dropbox.com/s/eovgx1xmdduuxq0/Class11.regression.pdf?dl=0) -->

<!-- ### Lecture 13: Sparse and Overcomplete Representations -->
<!-- We will take a look at finding data-dependent bases. -->

<!-- - [Lecture 13 slides](https://www.dropbox.com/s/6g8lh3n1ahe3o7k/class12.sparseovercomplete.pdf?dl=0) -->

<!-- ### Final exam -->

<!-- - [Exam]({{site.url}}pdfs/exam-ml4sp.pdf) -->
<!-- - [Upload link](https://www.dropbox.com/request/ChBlKg6CuT5O2Tv5yBEX) -->

