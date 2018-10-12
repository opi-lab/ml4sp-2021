<a name="imread"> </a>
### Reading in Images

You are given a collection of images of faces (see the downloads section) from image database 1\. These images are all dimension 64x64\. From these you must compute the “best” eigen faces.

The matlab command to read an image is:

	image = double(imread(imagefile));

Note the “double()”. If you do not use it, matlab reads the data as uint8 and some operations cannot be performed.

<a name="buildingmatrix"></a>
### Building a matrix of images

To learn eigen vectors, the collection of faces must be unravelled into a matrix. To unravel an image of any size, you can do the following:

	[nrows, ncolumns] = size(image);  
	image = image(:);

The first line above is used only to retain the size of the original image. We will need it to _fold_ an unravelled image back into a rectangular image. The second line converts the nrows x ncolumns image into a single nrows*ncolums x 1 vector. In our “training” data every image is the same size (64 x 64) and no rescaling of images is necessary. To read in an entire collection of images, you can do the following:

	filenames = textread('FILE_WITH_LIST_OF_IMAGE_FILE_NAMES','%s');  
	nimages = length(filenames);  
	for i = 1:nimages  
		image{i} = double(imread(filenames{i}));  
	end

Since all images are the same size, you can get the size of the image from image{1}.

To compose matrix from a collection of k images, the following matlab script can be employed (you can also do it your own way):

	Y = [];  
	for i = 1:k  
		Y = [Y image{i}(:)];  
	end

<a name="eigenface"></a>
### Computing Eigen faces

Eigen faces can be computed from Y, which is an (nrows*ncolumns) x nimages matrix. There are two ways to to compute eigen faces; one is using eigen analysis and the other is by singular value decomposition.

#### <u>Eigen analysis</u>

First compute the correlation matrix: ``corrmatrix = Y * Y';`` How large is the correlation matrix? You can compute eigen vectors and eigen values of the correlation matrix as:

	[eigvecs, eigvals] = eig(corrmatrix);

The columns of the eigenvecs matrix (in matlab notation the i-th column is given by eigvecs(:,i)) are the eigen vectors. The diagonal entries of the eigvals matrix are the eigen values. You can confirm that ``corrmatrix = eigvecs * eigvals * eigvecs'``. To learn more about eigen analysis and the eig() command, type ``help eig`` into matlab.

The intuition for what Eigen vectors and Eigen values really mean is roughly as follows: The eigen vectors are a number of “orthogonal” bases that can be used to compose every image in the collection. What do we mean by “orthogonoal”? Compute the dot product between any two eigen vectors: ``eigvecs(:,i)*eigvecs(:,j)'`` (note the apostrophe representing the transpose). If i≠j, then the dot product will be 0\. In other words, we cannot explain any part of the i-th eigen vector by the j-th eigen vector. As a consequence, the parts of any image that are explained by the i-th eigen vector cannot be explained by any other eigen vector.

If we composed every image in the collection as a linear combination of our eigen vectors: ``image = a1eigvecs(:,1) + a2eigvecs(:,2) + a3eigvecs(:,3)...,`` then ai indicates the contribution of the i-th eigen vector to the image. The i-th eigenvalue gives us the RMS value of ai. It tells us how much the i-th eigen vector contributes to the images, typically. The lower the i-th eigen value, the less the i-th eigen vector contributes to the composition of the images. If the i-th eigen value is zero, the i-th eigen vector does not contribute to the composition of the images at all!

You will find it instructive to plot the Eigen values: ``plot(diag(eigvals))``;. Matlab's ``eig()`` function returns eigen vectors in order of increasing importance. The first eigen vector is the least important. The last one is the most important. The plot of eigen values will also show this -- the last eigen value is the largest. Note also that if you have k images in Y, no more than k Eigen values are non-zero. That is because you will not need more than k Eigen vectors to explain k images; the importance of the remaining Eigen vectors is zero.

If you have gone through the above exercise, you will quickly realize that it takes a long time to perform eigen analysis: ``eig()`` of a 4096 x 4096 correlation matrix takes a lot of time. If our images were larger (more pixels), it would take much much longer. One of the reasons is that ``eig()`` operates on a large correlation matrix. Another it that it computes all eigen values and eigen vectors, whereas we know that if we have k images in Y, only k of the Eigen vectors (and Eigen values) are relevant -- the others have zero importance. The faster way to achieve the same end is via singluar value decomposition:

#### <u>Singular Value Decomposition (SVD):</u>

SVD can be performed in matlab as ``[U,S,V] = svd(Y,0);``

Note that SVD is performed directly on Y and not on the correlation matrix. The “0” to the right states that we want a “thin” SVD. The thin SVD will give us an (nrows*ncolumns) x k matrix U, and a k x k diagonal matrix of singular values, S. The matrix V are the right singular values and we need not concern ourselves with it for this problem.

You can confirm that the k columns of U are the same as the final k columns of the Eigen vectors matrix returned by ``eig(corrmatrix)``, only in reverse order. i.e. the first column of U is the most important eigen vector, the second column is the second-most important Eigen vector and so on. The diagonal entries of S are also the square roots of the final k diagonal entries of the eigvals matrix returned by ``eig()``, only in reverse order.

The gist of the above is that instead of running eigen analysis using ``eig(corrmatrix)``, you can get the desired Eigen vectors using ``svd(Y,0)`` and it will be much faster.

``svd()`` still computes k eigen vectors and singular values. For our problem we only want one (or, for problem 2, some J) eigen vector. Obviously the effort expended in computing the rest of the eigen vectors is wasted. An even faster version of SVD is given by the ``svds()`` command in matlab which only computes exactly as many Eigen vectors as you want. You can learn more about these by typing ``help svd`` or help svds into matlab.

You can use any of these techniques to compute the Eigen faces. The first Eigen face will be used to detect faces in the group photograph.

The eigen face will be in the form of a nrows*ncolumns x 1 vector. To convert it to an image, you must fold it into a rectangle of the right size. Matlab will do it for you with:

	eigenfaceimage = reshape(eigenfacevector, nrows, ncolumns);

nrows and ncolumns are the values obtained when you read the image. eigenfacevector is the eigen vector obtained from the eigen analysis (or SVD).

<a name="scanimage"></a>
### Scanning an image for a pattern

Let I be a P x Q image. Let E be an N x M Eigen face. The following matlab loop will compute the normalized dot product of every N x M patch of I as follows:

	E = E/norm(E(:)); 
	for i = 1:(P-N)  
		for j = 1:(Q-M)  
			patch = I(i:i+N-1,j:j+M-1);  
			m(i,j) = E(:)'*patch(:) / norm(patch(:));  
		end  
	end

m(i,j) is the normalized dot product, which represents the match between the eigen face E and the N x M patch of the image with its upper left corner at the (i,j)-th pixel. Note that we are normalizing both the eigen face and the patch in the above code. This will make sure that the results are invariant to both the size and lighting of the images.

<a name="sometricks"></a>
### Some processing tricks for better results

#### <u>Converting Color to Greyscale:</u>

The test image you have been given is a color image. This must be converted to grey scale. To do this, first read the image in as a color image. You can do this imply using imread(), just as you would read a greyscale image. imread() will give you three images, stored together in a three-dimensional array (the elements of which must be accessed using three indicies). So, for example, image(i,j,1) will give you the (i,j)-th pixel of the red image, image(i,j,2) will give you the corresponding green pixel and image(i,j,3) the blue pixel. To convert it to a greyscale image, the red, green and blue images must be added. This can be done by:

	greyscaleimage = squeeze(mean(colorimage,3));

This sums the third dimension of the three-dimensional matrix that represents the color image to give you a greyscale image. We're actually using "mean" instead of "sum" to make sure everything is in the 8-bit range. Note the squeeze(). The result of sum will still have three indices (although the final index only takes the value 1), squeeze() eliminates the third index.

#### <u>Histogram Equalization</u>

Histogram equalization can be performed in matlab using ``histeq(image)``. However this must be done before we convert it to doubles. This means that when we read in the training data, the routine for reading in image files must itself change to:

	image = double(histeq(imread(imagefilename)));

Note that the above only applies to greyscale images. Color images must first be converted to grey scale before histogram equalization, or the results will be very poor.

Histogram equalization must also be performed on every patch that we evaluate to find a face. To do this, we mus modify the manner in which we extract pathces to:

	patch = double(histeq(uint8(I(i:i+N-1,j:j+M-1))));

Note that we're converting the double numbers for the image to uint8 first -- the matlab routine for histogram equalization requires uint8 images. We convert it back to doubles after histogram equalization.

<a name="scalingimages"></a>
### Scaling Images

Any P x Q image can be resized to an N x M image using the ``imresize()`` command as follows:

	scaledimage = imresize(image,[N,M]);

Note that the above command does not consider the size of the original image explicitly. If N and M are not proportional to P and Q respectively, the image will get distorted. To ensure that the image retains its proportions, make sure that N/M = P/Q. To rescale the 64x64 Eigen face to 128x128, the command is:

	neweigenface = imresize(eigenface,[128,128])

<a name="additionalhints"></a>
### Viewing Images

You can view an image simply by ``imagesc(image)``.

### More on Problem 1

You will get significantly better results if you replace ``histeq(x)`` by ``x = x - mean(x(:));``. It may also be useful to follow that with x = x / norm(x(:)), either when computing eigen faces, or when normalizing patches, or both. The first operation is mean normalization. The second is variance normalization.

If you replace ``histeq()`` by the above steps, then you should be able to reformulate the computation of the dot product computation as follows. Here A is an P x Q image, E is a N x M eigenface.

First, at each pixel location in A we compute the mean of the N x M patch cornered on that pixel. We do so using the integral image trick from Viola-Jones. The cumsum() command below computes the integral image:

	pixelsinpatch = N*M; %The size of the eigenface is N x M.  
	%first compute the integral image  
	integralA = cumsum(cumsum(A,1),2);  
	
	%Now, at each pixel compute the mean  
	patchmeansofA = [];  
	for i = 1:size(A,1)-N+1  
		for j = 1:size(A,2)-M+1  
			a1 = integralA(i,j);  
			a2 = integralA(i+N-1,j);  
			a3 = integralA(i,j+M-1);  
			a4 = integralA(i+N-1,j+M-1);  
			patchmeanofA(i,j) = a4 + a1 - a2 - a3;  
		end  
	end

Computation of the dot product of the eigenface with each patch of the image is actually a convolution. This can be done really quickly using the convolution operator in matlab. Note the ``flipud(fliplr())`` operations here. This is required because convolution actually flips the eigenface prior to computing the dotproduct. By pre-flipping the eigenface, we're accounting for this.

	tmpim = conv2(A, fliplr(flipud(E)));  
	convolvedimage = tmpim(N:end, M:end);

The result above has not accounted for the fact that we wanted to subtract out the mean of each patch. We will do that now:

	sumE = sum(E(:));  
	patchscore = convolvedimage - sumE*pathcmeanofA(1:size(convolvedimage,1),1:size(convolvedimage,2));

You can figure out how you would account for variance normalization similarly.