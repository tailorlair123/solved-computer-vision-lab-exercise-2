Download Link: https://assignmentchef.com/product/solved-computer-vision-lab-exercise-2
<br>
<h1></h1>

Harris Corner Detector and SIFT Descriptor

Students should work on this lab exercise in groups of two people. In this assignment, you will be implementing the scale invariant Harris Corner Detector. You will then use this detector to match images with the SIFT descriptor.

<h2>1        Scale selection using Laplacian</h2>

We want to find corner-points that are scale invariant. We can find these as extrema points of the Laplacian for the optimal scale, <em>σ</em><sup>∗</sup>. To find the scale we need to create a scale-space as in the SIFT paper [1] by blurring the image with increasing <em>σ </em>values. The SIFT paper is attached on Brightspace.

<strong>Computing the Laplacian: </strong>We need to compute the Laplacian over these multiple scales and find the local extrema within a neighborhood. The Laplacian of a Gaussian can be computed using the formula below. And then a

standard image convolution operation can be used for extracting the Laplacian responses.

<strong>Computing the DoG approximation of Laplacian: </strong>Alternatively, the Laplacian can be approximated with the DoG (Difference of Gaussians), directly from the scale-space.

We will search for the maximum Laplacian responses within a neighborhood.

We can then filter the points to remove the ones with low-responses. We then, obtain a list of interest points of the form: [<em>r<sub>i</sub>,c<sub>i</sub>,σ<sub>i</sub></em>]. These points can also be edges and blobs. All these steps are being performed by the provided ‘DoG.m’ file in Brightspace. This file returns a set of image locations together with their selected scale. We want to retain only the corners.

<h2>2        Harris Corner Detector</h2>

In this part, you will be implementing a scale invariant Harris Corner Detector. In the first step you need to complete the code in the Harris function that is provided on Brightspace. All the necessary steps are indicated in the code with the hint for the completion. You need to compute the entries of the structure tensor matrix which you will use to form your ‘cornerness’(R)). (Please review your lecture slides for this task.)

The corner points are the local maxima of R. Therefore, in your function you should check for every point in the list of initial interest points, if in R its value is greater than all its neighbours (in an [<em>n </em>× <em>n</em>] window centered around this point) and if it is greater than the user-defined threshold, then it is a corner point.

Now, the points which are returned are corners and scale invariant. Your function should return the rows of the detected corner points r, and the columns of those points c and the scale at which they were found: <em>σ </em>(the first corner is given by (<em>r</em>(1)<em>,c</em>(1)<em>,σ</em>(1))). Your function should also plot the original image with the corner points plotted on it (Extra: You could plot them as circles where the radius is the corresponding <em>σ<sub>i</sub></em>.)

<h2>3         Image Matching with SIFT</h2>

In this part, you will be using your corner points detected in the previous section. You will need to build a descriptor for each of these corner points in the given image-a. And try to find closest descriptor in the other given image-b using an Euclidean distance. You can define your own threshold for finding matches based on the Euclidian distance, to be accepted as correct based on the effect of the ratio-of-the-second-best-match, as below. Also you can reject matches that have distance ratios:

secondBestDist<sup>bestDist </sup>≥ 0<em>.</em>8.

<table width="527">

 <tbody>

  <tr>

   <td width="27">1</td>

   <td width="500">% Find features and make descriptor of image 1</td>

  </tr>

  <tr>

   <td width="27">2</td>

   <td width="500">[r1,c1,s1] = harris(im1);</td>

  </tr>

  <tr>

   <td width="27">3</td>

   <td width="500">[f1,d1] = sift(single(im1),r1,c1, s1);</td>

  </tr>

  <tr>

   <td width="27">45</td>

   <td width="500">% Find features and make descriptor of image 1</td>

  </tr>

  <tr>

   <td width="27">6</td>

   <td width="500">[r2,c2,s2] = harris(im2);</td>

  </tr>

  <tr>

   <td width="27">7</td>

   <td width="500">[f2,d2] = sift(single(im2),r2,c2, s2);</td>

  </tr>

  <tr>

   <td width="27">89</td>

   <td width="500">% Loop over the descriptors of the first image</td>

  </tr>

  <tr>

   <td width="27">10</td>

   <td width="500">for index1 = 1:size(d1, 2)</td>

  </tr>

  <tr>

   <td width="27">11</td>

   <td width="500">              bestDist                        = Inf</td>

  </tr>

  <tr>

   <td width="27">12</td>

   <td width="500">secondBestDist = Inf</td>

  </tr>

  <tr>

   <td width="27">13</td>

   <td width="500">             bestmatch                      = [0 0]</td>

  </tr>

  <tr>

   <td width="27">1415</td>

   <td width="500">% Loop over the descriptors of the second image</td>

  </tr>

  <tr>

   <td width="27">16</td>

   <td width="500">for inex2=1:size(d2, 2)</td>

  </tr>

  <tr>

   <td width="27">17</td>

   <td width="500">desc1 = d1(:,index1);</td>

  </tr>

  <tr>

   <td width="27">18</td>

   <td width="500">desc2 = d2(:,index2);</td>

  </tr>

  <tr>

   <td width="27">1920</td>

   <td width="500">% Computer the Euclidian distance of desc1 and desc2</td>

  </tr>

  <tr>

   <td width="27">21</td>

   <td width="500">dist = …</td>

  </tr>

  <tr>

   <td width="27">2223</td>

   <td width="500">% Threshold the ditances</td>

  </tr>

  <tr>

   <td width="27">24</td>

   <td width="500">if dist <em>&lt; </em>threshold</td>

  </tr>

  <tr>

   <td width="27">25</td>

   <td width="500">if secondBestDist <em>&gt; </em>dist</td>

  </tr>

  <tr>

   <td width="27">26</td>

   <td width="500">if bestDist <em>&gt; </em>dist</td>

  </tr>

  <tr>

   <td colspan="2" width="527">27                                                                                      seccondBestDist = bestDist;28                                                                                      bestDis       = dist;29                                                                                      bestmatch = [index1 index2];30                                                                                      else % if not smaller than both best and …second best dist31                                                                                      secondBestDist = dist;32                                                                                      end33                                                                                      end34                                                                                      end35                                                                                      end3637                                      % Keep the best match and draw.38                                      if (bestDist / secondBestDist) <em>&lt; </em>0.839                                      … % You can use the ‘line’ function in matlab to … draw the matches.40                                      end41                                      end4243 % Return matches between the images</td>

  </tr>

 </tbody>

</table>

You can find an implementation of SIFT at <a href="http://www.vlfeat.org/index.html">http://www.vlfeat.org/index. </a><a href="http://www.vlfeat.org/index.html">html</a><a href="http://www.vlfeat.org/index.html">.</a> Download and setup the vlfeat package for Matlab.

<h2>References</h2>

[1] Lowe, David G. ”Distinctive image features from scale-invariant keypoints.” International journal of computer vision 60.2 (2004): 91-110.