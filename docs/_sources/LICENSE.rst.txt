Introduction
============
These developments have been conducted in the frame of a project
dedicated to the optimization of the process of PV silicon ingot sawing. To analyse the impact of the sawing conditions on 
the wafer quality, in terms of residual roughness and cristalline integrity, we develloped an automatic 
procedure to detect the features present at the top surface of a raw silicon wafer from diamond wire cutting. 
The aim is to extract and characterize in terms of size and location the topography "features" induced by diamonds 
indentation or fragile fracture present on the wafer surface and superimposed with saw marks. To do so we developped 
a package containing a toolbox to perform image processing on confocal images of the top surface.


Requirements
============

	- Python >= 3.6

	- numpy
	- matplotlib                          
	- pandas
	- PyWavelets
	- scikit-image
	- scipy

To install image_features_extract:
    **pip install vre_eoles**


Methods
=======

Confocal file reading
---------------------

 - Sensofar .txt files
 - Sensofar .plu files

 
Confocal images processing
--------------------------

1. Background removal using
	- Top hat filtering;
	- Gaussian filtering;
	- LS 2D polynomial smoothing;
2. Saw marks removal
	- wavelet filtering;
	- Retrojection method;
3. Features extraction and classification
4. Batch processing of a bunch of files
5. LaTeX report building
	
Usage example
=============

read and process a confocal image
---------------------------------
::

	# parameters initialization
	Top_hat_length = 50 # should be larger than the largest feature to extract
	threshold = -0.3    # should be larger than the residual image background
	
	# read .plu image (N rows by M columns)
	N, M, confocal_img, _ = ife.read_plu_topography(input_file_name)
	
	# substitute unmeasured values by interpolated ones
	confocal_img1 = ife.fill_gap(confocal_img)
	
	# background removal
	im_corr = ife.top_hat_flattening(confocal_img1, Top_hat_length, top_hat_sens=-1 )
	
	# binarization
	im_bin = np.where(im_corr < threshold, 1, 0) 
	
	# morphological analysis stored in df dataframe
	df = ife.analyse_morphologique(im_bin,im_corr) 

.. figure:: /image/example.png

  *Example of the features extraction from a confocal image showing: (i)The raw confocal image with superimposed in blue the area not aquired by the confocal microscope; (ii) The image after filtrering by a top hat; (iii) The image after binarization; (iv)  The result of the features extraction. The circles represent the detected features at their position. The area of the circles are equal to those of the features
  and there color are related to the features depth; (v) The features size histogram and box plot are plotted*

.. csv-table:: Features extraction characteristics
   :header: "idx", "x", "long_x", "y", "long_y", "size", "height"
   :widths: 20, 20, 20, 20, 20, 20, 20

   1, 440, 4, 31, 1, 4, -0.338682
   2, 88, 1, 33, 1, 1, -0.300135
   3, 11, 9, 46, 2, 16, -0.467897
   4, 52, 5, 46, 1, 5, -0.361629
   5, 555, 2, 60, 1, 2, -0.306462
   ..., ..., ..., ..., ..., ..., ...
   70, 672, 10, 401, 5, 35, -0.409538
   71, 88, 9, 450, 5, 28, -0.622761
   72, 3, 8, 475, 7, 49, -0.644196
   73, 109, 3, 477, 4, 9, -0.319589
   74, 382, 7, 478, 4, 15, -0.338711   
   
Jupyter notebook
================
In addition, three companion notebook can be cloned undre the link:
 https://github.com/Bertin-fap/image_features_extraction_example

* the first notebook *ife_simulation.ipynb* applies the automatic features detection on synthesis image;
* the second notebook *ife_dwsi_single.ipynb* applies the automatic features detection on "real" confocal image;
* the third notebook *ife_dwsi_batch.ipynb* applies the automatic features detection on a bunch of "real" confocal images and provides
  Tex reports

	
License
=======

MIT License

Copyright (c) [2020] [François Bertin]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

About the authors
=================
	- François Bertin retired, formally senior scientist at CEA-LETI
	- Amal Chabli retired, formally senior scientist at CEA-LITEN
	
acknowledgements
================
* We are most thankful to R. Rivas and F. Coustier from CEA_LITEN who conducted the sawing experiments and provided us withconfocal images.
* We are also most thankful to J. Bounan with whom we have devellopped a Matlab code to detect inclusions in IR transmission images of silicon wafer. 

Contact
=======

Question? Please contact francois.bertin7@wanadoo.fr or amal.chabli@orange.fr