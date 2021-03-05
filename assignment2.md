---
layout: page_math
title: Assignment 2
permalink: /assignment2/
---

## Machine Learning for Signal Processing - 2P2018

## Assignment 2: A Simple Face Detector

### Due date: April 14, 11:59 PM

Adapted from [Bhiksha Raj](http://mlsp.cs.cmu.edu/people/bhiksha/inde.php).

###  Problem: 

You are given a corpus of facial images <a href="{{site.url}}a2/lfw1000.zip">[here]</a> from the <a href="http://www.itee.uq.edu.au/~conrad/lfwcrop/">LFWCrop</a> database. Each image in this corpus is 64 x 64 and grayscale. You must learn a typical (i.e. *Eigen*) face from them

You are also given four group photographs with multiple faces <a href="{{site.url}}a2/group_photos.tar.gz">[here]</a>. You must use the Eigen face you have learnt to detect the faces in these photos

The faces in the group photographs may have different sizes. You must account for these variations 

Matlab is strongly recommended but you are free to use other programs if you want.

Some hints on how to read image files into matlab can be found <a href="{{site.url}}assignment2_hints">here</a> 

You must compute the first Eigen face from this data. To do so, you will have to read all images into a matrix.<a href="{{site.url}}assignment2_hints#buildingmatrix"> Here</a> are instructions for building a matrix of images in matlab. You must then compute the first Eigen vector for this matrix. Information on computing Eigen faces from an image matrix can be found <a href="{{site.url}}assignment2_hints#eigenface">here</a> 

To detect faces in the image, you must scan the group photo and identify all regions in it that &ldquo;match&rdquo; the patterns in Eigen face most. To &ldquo;Scan&rdquo; the image to find matches against an $N\times M$ Eigen face, you must match every $N\times M$ region of the photo against the Eigen face.

The *match* between any $N\times M$ region of an image and an Eigen face is given by the normalized dot product between the Eigen face and the region of the image being evaluated. The normalized dot product between an $N\times M$ Eigen face and a corresponding $N\times M$ segment of the image is given by $E\cdot P / \vert P \vert$, where $E$ is the vector (unrolled) representation of the Eigen face, and $P$ is the unrolled vector form of the $N\times M$ patch.

A simple matlab loop that scans an image for an Eigen vector is given <a href="{{site.url}}assignment2_hints#scanimage">here</a> 

The locations of faces are likely to be where the match score peaks.

Some tricks may be useful to get better results.


<ul>
  <li> Some of your test images (the group photograph) are in color; your Eigen faces are greyscale. You will have to convert the color photograph to greyscale by taking the mean of the red, green and blue values. The matlab method for doing this is given <a href="{{site.url}}assignment2_hints#sometricks"> here.</a> </li>
  <li>You will obtain better Eigen faces if all of the faces in the training data are histogram equalized. The faces in the training data all have somewhat different lighting and contrast. These variations can affect your estimate of the Eigen face. Histogram equalization can be performed in matlab as explained <a href="{{site.url}}assignment2_hints#sometricks">here</a>. </li>
  <li>You will also be able to detect faces better if you histogram-equalize each patch of the group photo before you evaluate its match to the Eigen face. </li>
</ul>


<h4><u>Scaling and Rotation </u></h4>
The Eigen face is fixed in size and can only be used to detect faces of approximately the same size as the Eigen face itself. On the other hand faces in the group photos are of different sizes -- they get smaller as the subject gets farther away from the camera.

The solution to this is to make many copies of the eigen face and match them all.

In order to make your detection system robust, resize the Eigen faces from 64 pixels to 32x32, 48x48, 96x96, and 128x128 pixels in size. You can use the scaling techniques we discussed in the linear algebra lecture. Matlab also provides some easy tools for scaling images. You can find information on scaling images in matlab <a href="{{site.url}}assignment2_hints#scalingimages">here</a>. Once you've scaled your eigen face, you will have a total of five “typical” faces, one at each level of scaling. You must scan the group pictures with all of the five eigen faces. Each of them will give you a “match” score for each position on the image. If you simply locate the peaks in each of them, you may find all the faces. Sometimes multiple peaks will occur at the same position, or within a few pixels of one another. In these cases, you can merge all of these, they probably all represent the same face.

Additional heuristics may also be required (appropriate setting of thresholds, comparison of peak values from different scaling factors, addiitonal scaling etc.). These are for you to investigate.

<a href="{{site.url}}assignment2_hints#additionalhints"><b>[More hints]</b></a>

### Problem 4: A boosting based face detector

You are given a training corpus of facial images. You must learn the first <i>K</i> Eigen faces from the corpus. Set <i>K = 10</i> initially but vary it appropriately such that you get the best results. Mean and variance normalize the images before computing Eigenfaces.

<p>You are given a <i>second</i> training set of facial images. Express each image as a linear combination of the Eigen faces. <i>i.e.</i>, express each face $F$ as<br>
\[
F \approx w_{F,1}E_1 + w_{F,2}W_2 + w_{F,3}E_3 + \cdots + w_{F,K}E_K
\]
</font>
where $E_i$ is the $i$<sup>th</sup> Eigen face and $w_{F,i}$ is the weight of the $i$<sup>th</sup> Eigen face, when composing face $F$.  $w_{F,i}$ can, of course, be computed as the dot product of $F$ and $E_i$  </p>


<p>Represent each face by the set of weights for the Eigen faces, i.e. $F \rightarrow \{w_{F,1}, w_{F,2}, \cdots, w_{F,K}\}$. </p>

<p>You are also given a collection of <i>non-face</i> images in the dataset. Represent each of these images too as linear combinations of the Eigen faces, <i>i.e.</i> express each non-face image $NF$ as<br>
<font color="blue">
\[
NF \approx w_{NF,1}E_1 + w_{NF,2}E_2 + w_{NF,3}E_3 + \cdots + w_{NF,K}E_K
\]
</font> </p>

<p> As before, the weights $w_{NF,i}$ can be computed as dot products.  Represent each of the non-face images by the set of weights <i>i.e.</i> $NF \rightarrow \{w_{NF,1}, w_{NF,2}, \cdots, w_{NF,K}\}$.</p>

<p>The set of weights for the Eigen faces are the <i>features</i> representing all the face and non-face images. </p>

<p> From the set of face and non-face images represented by the Eigenface weights, learn an Adaboost classifier to classify faces vs. non-faces.</p>

<p>You are given a fourth set which is a collection of face and non-face images. Use the adaboost classifier to classify these images. </p>

<p>The classifier you have learned will be for the same size of images that were used in the training data (64 x 64). Scale the classifier by scaling the Eigenfaces to other sizes (32 x 32, 48 x 48, 96 x 96, 128x 128). </p>

<p>Train and test data for this problem is <a href="{{site.url}}a2/BoostingData.tar.gz">here</a>. It is a collection of face and non-face data.Use the data in the "train" subdirectory to train your classifier and classify the data in the "test" subdirectory. </p>


### Problem 5

<p>It will generally not be possible to represent a face exactly using a limited number of typical faces; as a result there will be an error between the face $F$ and the approximation in terms of the $K$ Eigenfaces. You can also compute the <i>normalized</i> total error in representation as:<br>
\[
err_F = \frac{1}{N}\parallel F - \sum_i w_{F,i}E_i \parallel^2
\]
where, $\parallel \bullet \parallel^2$  represents the sum of the squares of the error of each pixel, and $N$ represents the number of pixels in the image. <p>

Represent each face by the set of weights for the Eigen faces and the error, <i>i.e.</i> $F \rightarrow \{w_{F,1}, w_{F,2}, \cdots, w_{F,K}, err_F\}$

As in the case of faces, the approximation of the non-face images in terms of Eigenfaces will not be exact and will result in error. You can compute the normalized total error as you did for the face images to obtain $err_{NF}$.

Represent each of the non-face images by the set of weights <i>i.e.</i>
$NF \rightarrow \{w_{NF,1}, w_{NF,2}, \cdots, w_{NF,K}, err_{NF}\}$

Learn and build a classifier in the same way you did for problem 3 but including normalized error as a feature. Use this classifier for Problem 4.

<h3>Problem 6</h3>

Scan the group photographs to detect faces using your adaboost classifier

You can adjust the tradeoff between missing faces and false alarms by comparing the margin $H(x)$ of the Adaboost classifier to a threshold other than 0.

<!-- ### Problem 7
We will add a final problem on the use of independent component analysis for the face recognizer. This will be put up by next week. <br>
<p>&nbsp; -->

<h3>Submission Details</h3>

<!-- <p>The homework is due at the beginning of class on October 31,2013. </p> -->
<p><b>What to submit:</b></p>
<ul>
  
  <li>A brief writeup of what you did</li>
  <li>The segments that your detector found to be faces. You may either copy those segments into individual files into a folder, or mark them on the group photograph. Make sure I can understand which part(s) of the image was detected as a face.</li>
  <li>The code for all of this</li>
  
</ul>

<!-- <p>Put the above in zipfile called **ml4sp_hw2.zip** and email it to the two instructors with MLSP hw2 in the subject line</p>

<p>Solutions may be emailed to James Ding or Varun Gupta, and must be cc-ed to Bhiksha. The message must have the subject line "MLSP assignment 1". It should include a report (1 page or longer) of what you did, and the resulting matrix as well as the synthesized audio. </p> -->
