
# Iris Segmentation Code Based on the Generalized Structure Tensor (GST)


This repository is the original implementation of the paper **[Iris Boundaries Segmentation Using the Generalized Structure Tensor. A Study on the Effects on Image Degradation](http://urn.kb.se/resolve?urn=urn:nbn:se:hh:diva-19310)**, published at
[BTAS](https://sites.google.com/a/nd.edu/btas_2012/) (International Conference on Biometrics: Theory, Applications and Systems). 

The theory behind this method is further extended in the paper **[Near-infrared and visible-light periocular recognition with Gabor features using frequency-adaptive automatic eye detection](https://arxiv.org/abs/2007.08566)**, published at
[IET Biometrics Journal](https://digital-library.theiet.org/content/journals/10.1049/iet-bmt.2014.0038).

The software accepts an iris image as input, and outputs segmentation information of the input iris image (see below for more information). It is capable of handling images acquired both in near-infrared (NIR) and visible (VW) spectrum.

![Gst_iris_image_header](https://user-images.githubusercontent.com/6042693/106274154-17ad8000-6234-11eb-9625-04685b1e8ae4.png)

---

**The GST code consist of the following steps (some can be deactivated or customized, please read the documentation included with the code):**

  1) Image **downsampling** for speed purposes. This will not jeopardize accuracy, since the detected iris circles are later fitted to the irregular iris contours, so any loss of resolution in iris circles detection due to downsampling is compensated.

  2) **Contrast normalization** based on imadjust (Matlab function). This increases the image contrast, spreading grey values fully in the 0-255 range.

  3) **Specular reflection removal** based on the method published in reference 3.

  4) Computation of the **image frequency** based on the method published in reference 2. This helps to customize inner parameters of steps 5-8 to the input image.

  5) **Adaptive eyelash removal** using the image frequency, as indicated in reference 2. The method is based on p-rank filters as published in reference 4. Eyelashes are removed since they create strong vertical edges that may mislead the filters used for eye center estimation and iris segmentation in steps 7 and 9.

  6) **Adaptive edge map computation** using the image frequency, as indicated in reference 2. Edge map is the basis for eye center estimation and iris boundaries detection, see references [1, 2] for further details.

  7) **Estimation of the eye center** based on the method published in reference 2 using circular symmetry filters. The estimated center is used to mask candidate regions for the centers of iris circles, helping to improve detection accuracy in step 9.

  8) **Detection of eyelids** based on linear symmetry detection of horizontal edges (unpublished and unoptimized, only return a straight line). The detected eyelids are used to mask candidate regions for the centers of iris circles too, helping to improve detection accuracy in step 9.

  9) **Detection of iris boundaries** based on the method published in reference 1 using the Generalized Structure Tensor (GST). In NIR images, the inner (pupil) circle is detected first, while in VW images, the outer (sclera) filter is detected first. This is because in NIR images, pupil-to-iris transition is sharper than iris-to-sclera transition, thus more reliable to detect in the first place. The opposite happens with VW images.

  10) **Irregular contour fitting** based on active contours as published in reference reference 5.

---

![800px-Gst_iris_image_prepro](https://user-images.githubusercontent.com/6042693/106274160-1b410700-6234-11eb-81e8-ec5ff74c5463.png)

---

**The code outputs the following information of the input iris image:**

  - **Segmentation circles** of the iris region (inner and outer circle) as well as eyelids (in the form of a straight line)
  - Irregular (non-circular) **iris boundaries** fitted by active contours
  - Estimated **eye center** (computed at the beginning and used to assist in the segmentation)
  - **Intermediate images** after contrast normalization, specular reflection removal, and eyelash removal
  - **Complex edge map** of the input image
  - **Binary segmentation mask**

---

![800px-Gst_iris_image_segment](https://user-images.githubusercontent.com/6042693/106274167-1f6d2480-6234-11eb-9e73-b3208011d31d.png)

---

# Requirements
  - Matlab software running under Windows (the current release is compiled in Windows)
  
# Description of Release Files
  - This code is provided "as is", without any warranty, and for research purposes only.
  - The code is provided in the form of executables compiled with Matlab (mcc command) under Windows. It accepts as input grayscale and RGB images in any format supported by Matlab "imread" (uint8 only).
  - Certain parameters of the code are customizable, please read the documentation included with the code for more information.
  - Please remember to cite references [1] and [2] (below) if you make use of this code in any publication.
  - By downloading the code, you agree with the terms and conditions indicated above.

There are two releases. The code is the same, the only difference is the software used to compile it:

 - **Release 1:** Compiler: Matlab r2009b 32 bits (mcc command) under Windows 8.1 (latest release: September 2015).
 - **Release 2:** Compiler: Matlab r2018b 64 bits (mcc command) under Windows 10 (latest release: October 2019). Note that there are two files that you need to download in order to decompress the code properly.

**You may are also interested in our [database of iris segmentation groundtruth](http://wiki.hh.se/caisr/index.php/Iris_Segmentation_Groundtruth).**
# People & Contact
  - [Fernando Alonso-Fernandez](http://wiki.hh.se/caisr/index.php/Fernando_Alonso-Fernandez)  (contact person).
  - [Josef Bigun](http://wiki.hh.se/caisr/index.php/Josef_Bigun)
  
# References
  1) F. Alonso-Fernandez, J. Bigun, “Iris Boundaries Segmentation Using the Generalized Structure Tensor. A Study on the Effects on Image Degradation”, Proc. Intl Conf on Biometrics: Theory, Apps and Systems, BTAS, Washington DC, September 23-26, 2012 [link to the publication](http://hh.diva-portal.org/smash/record.jsf?searchId=2&pid=diva2:545745)
  2) F. Alonso-Fernandez, J. Bigun, “Near-infrared and visible-light periocular recognition with Gabor features using frequency-adaptive automatic eye detection”, IET Biometrics, Volume 4, Issue 2, pp. 74-89, June 2015 [link to the publication](http://digital-library.theiet.org/content/journals/10.1049/iet-bmt.2014.0038)
  3) C. Rathgeb, A. Uhl, P. Wild, "Iris Biometrics. From Segmentation to Template Security", Springer, 2013
  4) Z. He, T. Tan, Z. Sun, X. Qiu, "Toward accurate and fast iris segmentation for iris biometrics", IEEE Transactions on Pattern Analysis and Machine Intelligence, 2010, 31, (9), pp. 1295–1307 [link to the publication](https://ieeexplore.ieee.org/document/4586378)
  5) J. Daugman, "New methods in iris recognition", IEEE Transactions on Systems, Man, and Cybernetics, Part B: Cybernetics, 37(5), 2007 [link to the publication](https://ieeexplore.ieee.org/document/4305270)
  

