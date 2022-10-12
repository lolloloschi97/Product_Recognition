# PRODUCT RECOGNITION ON STORE SHELVES

### Step A- Multiple Product Detection
The aim of this step was to find seven different cereal boxes in supermarket shelters where there was no more than one correspondence for picture. <br/>
We used the Sift operator in order to find keypoints and descriptors for both images and then we matched the two closest elements for each query descriptor; to distinguish good matches from bad matches we used a minimum number of matches equal to 50. After having found the good matches the homography for the discarder point was found and then we projected the corners of the model image in the scene image in order to draw the bounding box indicating the correspondence in the scene image.
<br/>
<br/>
<p align="center">
  <img src="https://github.com/lolloloschi97/Product_Recognition/blob/main/image2.jpeg" width=30% height=30% class="center">
</p>
<br/>

### Step B - Multiple Instance Detection

The second step still aim to detect the previously presented cereal boxes in supermarket shelter images, but now we are considering a multiple correspondence setting for each cereal box. <br/>
- First, for each scene image all the model images are subsequently processed in order to find cereal boxes occurrences in each target image. Hence, given a certain model – reference pair, the first step is finding local invariant features by means of Sift, for the sake of computing the respective set of model features Fi = (Pi, φi, Si, Di, Vi) and target features Fi_ = (Pi_, φi_, Si_, Di_). Flann based Matcher is used for matching correspondent feature together, so the obtained result is refined filtering out good matches trough a properly selected threshold value.At this point, for each target feature, barycenter position is inferred trough a suitable rotation and scaling transformation of the respective model feature’s joining vector. A quantized Accumulator Array is created for generate a voting scheme wrt the barycenter position, hence highly voted bins are selected through nms1 and thresholding successively computed. A list of feature’s indexes for each relevant bin is defined (kp_per_Ptc) for the sake of the computation. Having recorded, in the just defined element, the list of features wrt each relevant bin, is possible to estimate robustly the homography (RANSAC) for each candidate- Finally we can compute corner point, width and heigh of the so found target’s homologous features. For the sake of computation – for each relevant bin of each model image and for each target image – position, width and heigh and number of matches are stored respectively in wid_hei_tar, dst_tar, scores_tar.
- Second, for each scene image we aim to filter out overlapping windows. Firstly, for the sake of computation, the just obtained elements wid_hei_tar, dst_tar, scores_tar are converted in the respective dictionaries dict_wh, dict_dst, dict_scores trough the function convert_wh(). Therefore, for each target image are computed all the pairwise Euclidian distances between the barycenter of the respective found occurrences (results are stored in d_tar): if the computed distance is less then 100px then the lower matched window has been suppressed and dict_wh and dict_scores updated accordingly.
<br/>
<br/>
<p align="center">
  <img src="https://github.com/lolloloschi97/Product_Recognition/blob/main/image3.jpeg" width=40% height=40% class="center">
</p>
<br/>
