---
layout: page_math
title: Assignment 1
permalink: /assignment1/
---

## Machine Learning for Signal Processing - 2P2018

## Assignment 1: Linear Algebra Refresher

### Due date: September 14, 11:59 PM

Adapted from [Bhiksha Raj](http://mlsp.cs.cmu.edu/people/bhiksha/index.php).

###  Problem: 

Below are links to pieces of music and recordings of several notes. You are required to transcribe the music. 

For transcription, you will have to determine the note or set of notes being played at each time 

### Blowin' in the wind 
[\[This file\]][data] contains a recording of a harmonica piece rendering the song "Blowin' in the wind". Also given are a collection of notes, and an example of musical scale. Transcribe both the musical scale and the main song in terms of notes.

[data]: {{site.url}}/a1/MLSP_HW1_data.tgz

[\[wav\]][wav] version of music_extract.mp3

[wav]: {{site.url}}/a1/music_extract.wav

### Polyushka Polye
[\[This\]][polye] is a recording of "Polyushka Polye", played on the harmonica. 

[polye]: {{site.url}}/a1/polyushka.wav

Below are a set of notes from a harmonica 

<table  >
<col style="vertical-align: top; text-align: left;" width="35%" />
<col style="vertical-align: top; text-align: left;" width="45%" />
<tr>
<th> Note </th>
<th> Wav file </th>
</tr>

<tr>
<td> E</td>
<td> <a href="{{site.url}}/a1/e.wav"> e.wav </a></td>
</tr>

<tr>
<td> F</td>
<td> <a href="{{site.url}}/a1/f.wav"> f.wav </a></td>
</tr>

<tr>
<td> G</td>
<td> <a href="{{site.url}}/a1/g.wav"> g.wav </a></td>
</tr>

<tr>
<td> A</td>
<td> <a href="{{site.url}}/a1/a.wav"> a.wav </a></td>
</tr>

<tr>
<td> B</td>
<td> <a href="{{site.url}}/a1/b.wav"> b.wav </a></td>
</tr>

<tr>
<td> C</td>
<td> <a href="{{site.url}}/a1/c.wav"> c.wav </a></td>
</tr>

<tr>
<td> D</td>
<td> <a href="{{site.url}}/a1/d.wav"> d.wav </a></td>
</tr>

<tr>
<td> E2</td>
<td> <a href="{{site.url}}/a1/e2.wav"> e2.wav </a></td>
</tr>

<tr>
<td> F2</td>
<td> <a href="{{site.url}}/a1/f2.wav"> f2.wav </a></td>
</tr>

<tr>
<td> G2</td>
<td> <a href="{{site.url}}/a1/g2.wav"> g2.wav </a></td>
</tr>

<tr>
<td> A2</td>
<td> <a href="{{site.url}}/a1/a2.wav"> a2.wav </a></td>
</tr>

</table>

**Download the following matlab files:[\[stft.m\]]({{site.url}}/a1/stft.m)**

<h3>Matlab Instructions</h3>

You can read a wav file into Matlab as follows: 

	[s,fs] = wavread('filename');
	s = resample(s,16000,fs)

The recordings of the notes can be computed to a spectrum as follows: 
  
	spectrum = mean(abs(stft(s',2048,256,0,hann(2048))),2); 

'spectrum' will be a 1025 x 1 vector

  The recordings of the complete music can be read just as you read the notes. To convert it to a spectrogram, do the following:
   
	sft = stft(s',2048,256,0,hann(2048)); 
	sphase = sft./abs(sft); smag = abs(sft); 
  
  'smag' will be a 1025 x K matrix where K is the number of spectral vectors in the matrix. We will also need 'sphase' to reconstruct the signal later 

<h3>Additional Info</h3>

 Compute the spectrum for each of the notes. Compute the spectrogram matrix 'smag' for the music signal. This matrix is composed of K spectral vectors. Each vector represents 16 milliseconds of the signal. 
 
You may find, projections, pseudo inverses, and dot products useful. If you know of any other techniques, you can use those too. Tricks like thresholding (setting all values of some variable that fall below a threshold to 0) might also help. 

The output should be in the form of a matrix: 

	1 1 0 0 0 0 0 1 ...
	0 0 0 1 1 0 1 1 ...
	0 1 1 1 0 1 1 1 ...
	...........................

Each row of the matrix represents one note. Hence there will be as many rows as you have notes in table 1. 

Each column represents one of the columns in the spectrogram for the music. So if there are K vectors in the spectrogram, there will be K vectors in your output. 

Each entry will denote if a note was found in that vector or not. For instance, if matrix entry (4,25) = 0, then the fourth note (d) was not found in the 25th spectral vector of the signal. 

<h3>Synthesizing Audio</h3>

You can use the notes and the transcription matrix thus obtained to synthesize audio. Note that matrix multiplying the notes and the transcription will simply give you the magnitude spectrum. In order to create meaningful audio, you will need to use the phases as well. Once you have the phases included, you can use the stft to synthesize a signal from the matrix. Submit the synthesized audio along with the matrix. 

<h3>Linear Algebra</h3>
  
<!-- Let's warm up with a simple problem.  -->

<h4><b>Rotational Matrices:</b></h4>
  
A rotation in 3-D space is characterized by two angles. We will characterize them as a rotation along the $X-Y$ plane, and a rotation along the $Y-Z$ plane. Derive the equations that transform a vector $[x, y, z]^\top$ to a new vector $[x', y', z']^\top$ by rotating it counterclockwise by angle $\theta$ along the $X-Y$ plane and by an angle $\delta$ along the $Y-Z$ plane. Represent this as a matrix transformation of the column vector $[x, y, z]^\top$ to the column vector $[x', y', z']^\top$. The matrix that transforms the former into the latter is a rotation matrix.

#### Projecting Instrument Notes:
  
For this problem you will transform the harmonica notes of problem 1 to piano notes, by a matrix transform. The piano notes can be downloaded from [here](assignment1/pianonotes.tar.gz). Note that, in this case, you don't know which piano notes correspond to which notes from the harmonica. There are 3 parts to this problem: 

- Find the piano note corresponding to each note from the harmonica. The dot product is your friend.
- Find a transformation that converts the harmonica notes to piano notes. To do so, you must list the spectra for all harmonica notes as a matrix $H$. List the corresponding piano notes as a matrix $P$. There must be a one-to-one correspondence between the notes represented by the columns of $H$ and those represented by the columns of $P$. The desired transformation is a matrix $M$ such that $MH \approx P$. Provide the matrix $M$.
- Synthesize the music piece from Problem 1, using both the actual piano notes and those obtained by transforming the harmonica notes. Submit both synthesized recordings.


<!--### Linear Algebra II

 <p><b>The following matrix transforms 4-dimensional vectors into 3-dimensional ones:  </b></p>
 &nbsp;
\[
A = \begin{bmatrix}
    1 & 2 & 3 & 4 \\
    3 & 4 & 5 & 7 \\
    5 & 7 & 9 & 11
    \end{bmatrix}
\]


<ul><b>
  <li>A 4x1 vector $v$ of length 4 is transformed by $A$ as $u = Av$. What is the longest that $u$ can be? What is the shortest length of $u$? </li>
  <li>  The &ldquo;Restricted Isometry Property&rdquo; (RIP) constant of a matrix characterizes the change in length of vectors transformed by sub-matrices of the matrix. For our matrix $A$, let $A_s$ be a matrix formed of any $s$ columns of $A$. If $A$ is $M \times N$, $A_s$ will be $M\times s$. We can form $A_s$ in $^NC_s$ ways from the $N$ columns of $A$ (we assume that the order of vectors in $A_s$ is immaterial). Let $w$ be an $s \times 1$ vector of length 1. Let $l_{max}$ be the longest vector that one can obtain by transforming $w$ by <i>any</i> $A_s$. Let $l_{min}$ be the shortest vector obtained by transforming $w$ by any $A_s$. The RIP-$s$ constant $\delta_s$ of the matrix $A$ is defined as:<br>
\[
\delta_s = \frac{l_{max} - l_{min}}{l_{max} + l_{min}}
\]
What is $\delta_2$ (<i>i.e.</i> $\delta_s$ for $s = 2$) for the matrix $A$ given above?  Hint: You must consider all $^4C_2$ possible values for $A_s$.
</li></b>
</ul> -->


<!--### Linear&nbsp Algebra III

In this problem we will use basic linear algebraic operations to "invert" a photograph and recover the 3-D coordinates of the scene.

Images taken by conventional cameras are in two dimensions, but they are records of a three dimensional world.  When a camera takes an picture,  the camera is seeing the scene as if it were painted on a 2-dimensional window. The X,Y location of a feature on the image gives you no indication of the distance of that location from the camera itself. The recorded image is also affected by perspective. To understand this consider what happens when a real-world location $P_r = (X_r, Y_r, Z_r)$ is recorded by a camera.

 Figure XXX illustrates how an image is recorded by the camera.  Rays of light from $P_r$ travels into the &ldquo;eye&rdquo; of the camera in a straight line (we are making the simplifying assumption of a pinhole camera here). The point where all the rays converge ($O$ in the picture) is called the &ldquo;principal point&rdquo; of the camera. The rectangluar section $R$ through which the ray passes is the &ldquo;image plane&rdquo; for the camera.  The camera effectively records all pictures as if they were painted on this image plane.  The location $P_r = (X_r, Y_r, Z_r)$ is captured as the location $P_i = (X_i, Y_i, Z_i)$, which is the $X,Y$ location on the image plane where the ray from $P_r$ to $O$ intersects it.

<p><b>N.B:</b> In reality, the actual pixel position recorded is $P_c = (X_c, Y_c)$, where $X_c = s_x X_i + \epsilon_x,~Y_c = s_y, Y_i + \epsilon_y$, where $s_x$ and $s_y$ are <i> scaling factors</i> that scale the image plane down onto to actual pixel locations on the final, captured image. The two scaling factors are not equal, because the image may be differently scaled in the $X$ and $Y$ directions (<i>e.g.</i> the camera may have a different number of pixels per inch in the X and Y directions).  $\epsilon_x$ and $\epsilon_y$ are quantization error.  For the purpose of this homework we will ignore this aspect and assume that the pixel positions <i>are</i> the $X,Y$ coordinates of the image plane, <i>i.e.</i> $P_c = P_i$.

<p><b>The problem, then, is to figure out real-world 3-D coordinates of $P_r$ translate to the $X,Y$ coordinate $P_i$ on the image plane. A secondary problem is to figure out how to <i>recover</i> real-world 3-D co-ordinates from one or more camera images.</b>

<p><b> For this homework problem we will make some additional simplifying assumptions. We will also assume that the principal point of the camera is <i>centralized</i>,  <i>i.e.</i> it is located exactly behind the center of the image plane. Effectively, we are assuming that the rays converge at a point that on the normal drawn from the centre of the image.  We will treat this principal point as the <i>origin</i>, <i>i.e.</i> the $(0,0,0)$ point of the <i>camera coordinate system</i>. This is the point with respect to which all $X,Y,Z$ coordinate values are obtained from the perspective of the camera.  In order to indicate that we are talking of the <i>Camera Coordinates</i> we will use a superscript $c$, such that the actual real-life location, specified in camera coordinates, is represented as $P^c_r = (X^c_r, Y^c_r, Z^c_r)$, and the image-plane coordinates are represented as $P^c_i = (X^c_i, Y^c_i, Z^c_i)$.  We will refer to $Z^c_i$ as $f$, that is, $f = Z^c_i$ is the <i>focal length</i> of the camera.</b>


<p> <b>Having set our notation, let us consider the relation between the camera-system coordinates of $P^c_r$ and its image  $P^c_i$.  Note that $P^c_r$ and $P^c_i$ are on a straight line. Moreover, since we have set the principal point through which the line passes to be the origin, we get the the following relationships:</b>
\[
1.~~~~~~~~~~~~~~~~~~\frac{X^c_r}{X^c_i} ~~=~~ \frac{Y^c_r}{Y^c_i} ~~=~~ \frac{Z^c_r}{f}
\]
<p><b>or alternately</b>
\[\begin{array}{ll}
2.~~~~~~~~~~~~~~~~~~X^c_i = f\frac{X^c_r}{Z^c_r} \\
~~~~~~~~~~~~~~~~~~~~Y^c_i = f\frac{Y^c_r}{Z^c_r}
\end{array}\]

<p> <b>The above equation is entirely in terms of <i> Camera </i> coordinates. A camera is a mobile object and the relationship of camera's view to the arrangement of objects in the real world can change as you move and rotate the camera. In order to account for this, we must also establish <i>world coordinates</i> -- a fixed coordinate system with respect to which the objects in the 3-D world may be described. This comprises a specified <i>origin</i> and <i> axes</i> in the real-world, with respect to which all objects, <i>including the camera's principal point</i> can be assigned coordinates. This is particularly important if we want to take pictures with multiple cameras and &ldquo;register&rdquo; them, so that we can find correspondences and attempt to rebuilt a 3-D world from camera snapshots.</b>

<p><b> Let $Q$ be the origin of the world coordinates as in the figure below, and let $O = (X_0, Y_0, Z_0)$ be the location of the camera's principal point, <i>i.e.</i> the orgin of the camera coordinates. The relationship between the camera's coordinate system, and the world coordinate system can be defined through a rotation $R$ and a translation,  the fact that not only is the camera's origin shifted with respect to the world-coordinate origin, but the cameras axes are also rotated with respect to the axes of the world coordinates. Thus, the relationship between the world-coordinate representation of a point $P^w,~~P^w = (X^w, Y^w, Z^w)$, and the camera-coordinate representation of the real-world location $P^c_r = (X^c_r, Y^c_r, Z^c_r)$ of the same point is:</b>
\[
3.~~~~~~~~~~~~~\begin{bmatrix}X^c_r \\ Y^c_r \\ Z^c_r\end{bmatrix}
= R\begin{bmatrix}X^w \\ Y^w \\ Z^w\end{bmatrix} + \begin{bmatrix}X_0 \\ Y_0 \\ Z_0\end{bmatrix}
\]
<b><i>i.e.</i> it is obtained by rotating the axes through $R$ and translation to the origin $O$</b>.

<p><b>In order to establish the complete relationship between the world-coordinates of $P^w$ and the camera image-plane coordinates $P^c_i$, we only need to know the world-coordinates of the principal point of the camera, $X_0, Y_0, Z_0$, the rotation matrix $R$, and the focal length $f$.</b>

<p><b>Although the rotation matrix $R$ has 9 components, they are represented by only two variables: the two angles of rotation. In addition to these, the world-coordinates of the camera principal point, $X_0, Y_0, Z_0$, and the focal length $f$, together completely specify the relationship between the world coordinates $P^w$ of a point and the camera-coordinates of its image $P^c_i$ on the image plane.</b>

<p><b> We will, however, attempt to derive a smaller set of numbers that can nevertheless be used to derive image-plane coordinates from world coordinates.</b>

<p><b> Writing out the individual elements of the rotation matrix $R$ as $r_{ij}$, and combining Equations 2 and 3, the relationship between the world coordinates of $P^w$ and the camera-image-plane representation $P^c_i = (X^c_i, Y^c_i, f)$ of the point is:</b>
\[
\frac{X^c_i}{Y^c_i} = \frac{r_{11}X^w + r_{12}Y^w + r_{13}Z^w + X_0} {r_{21}X^w + r_{22}Y^w + r_{23}Z^w + Y_0}
\]
<p><b>or, alternately:</b>
\[
4.~~~~~~~~~~~~~{X^c_i}(r_{21}X^w + r_{22}Y^w + r_{23}Z^w + Y_0) =
{Y^c_i}(r_{11}X^w + r_{12}Y^w + r_{13}Z^w + X_0)
\]
<p><b>This implies that knowing $r_{11}, r_{12}, r_{13}, r_{21}, r_{22}, r_{23}, X_0$ and $Y_0$ gives us a relationship that specifies a relationship between $X^c_i$ and $Y^c_i$. Recall that $R$ is an orthogonal matrix, therefore knowing the first two rows of $R$ (namely the terms in Equation 4) specifies the third row to within a factor of $\pm 1$, and thereby specifies the $R$ matrix almost fully.</b>

<p><b>Problem:&nbsp</b>
<ul>
<li> <b> Given the world-cordinates of a set of $N$ points, $P^w_i = (X^w_i, Y^w_i, Z^w_i),~~i=1\cdots N$, $N>7$, and their corresponding camera image plane coordinates $P^c_{i,j} = (X^c_{i,1},Y^c_{i,1}),~~i=1\cdots N$, show how $r_{11}\cdots r_{23}$ and $X_0$ can be recovered. Assume $Y_0 = 1$.</b>
<li> <b> You are given the camera image plane coordinates $P^c_{i,j} = (X^c_{i,1},Y^c_{i,1}),~~i=1\cdots N$ of $N$ points. You are also given the vector difference $P^w_i - P^w_{j}$ for $K$ $(i,j)$ pairs, $K>7$, show how $r_{11}\cdots r_{23}$ and $X_0$ can be recovered. Once again assume $Y_0 = 1$.</b>
</ul>


<h3>Due&nbsp Date</h3>
<p><b>The assignment is due at the beginning of class on September 26th. The assignment is worth 15 points. Each day of delay thereafter will automatically deduct 0.5 points from your score.</b></p>

<p><b>Solutions may be emailed to James Ding or Varun Gupta, and must be cc-ed to Bhiksha. The message must have the subject line "MLSP assignment 1". It should include a report (1 page or longer) of what you did, and the resulting matrix as well as the synthesized audio. </b></p> -->

<!-- </div> -->

