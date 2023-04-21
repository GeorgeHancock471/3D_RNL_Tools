
Updated (U) , New (N)

.............................................................
	
	0 Misc
.............................................................
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

[N 0.1] micaToolbox/Image Analysis/Batch_Scale_Bar_Calculation_MF
------------------------------------------------------------

TITLE: 	

'Batch Scale Bar Calculation MF'


FUNCTION(S):

'Modified version of the base batch scale bar calculator, but works for 1 layer of subfolders'


	
.............................................................
	
	1 RNL Colour Maps
.............................................................
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


[U 1.1] micaToolbox/RNL Colour Maps
------------------------------------------------------------

o. Relabelled the scripts.

o. Added two new scripts: 


	3.5)_Compare_RNL_Colour_Maps

	 "Carries out the same function ad Plot and Compare, but
	 it does not prodce a plot."



	4.0)_Compare_RNL_Distance

	 "New RNL Distance Measure (See New for Details)."


[N 1.2] micaToolbox/RNL Colour Maps/4.0)_Compare_RNL_Distance 
------------------------------------------------------------

TITLE: 	

'4.0 Compare RNL Distance'


FUNCTION:

'Unlike the base compare function which looks at the difference in colour frequency, the compare 
 RNL distance tool measures the 2D or 3D radial distance between the RNL colours of all ROIs to 
 each other. Colours which are the same have a dstance of 0. The greater the distance the worse
 the match.'


FUNCTION(S):

' Threshold: Sets the threshold frequency for a colour to be used (inverse %).
	     E.g. 0.95 would use the colours that make up 95% of the image.'


' Measure: Either Mean or Max. 
		Mean. gives the average colour distance.
		Max. gives the largest colour distance.'


' Frequency: Yes/No weights the distance values based on the frequency with which a colour appears. 
	     The less frequent a colour is the lower its weighting.'


' Asymmetry: Distance metrics aren't symmetrical. E.g. the colours of a target could fit entirely
             within their background but the background may have colours the target lacks.


.............................................................
	
	2 Image Batch Analyses
.............................................................
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


[N 2.1] micaToolbox/Image Batch Analyses/Batch_XYZ_CIELAB
------------------------------------------------------------

TITLE: 	

'Batch XYZ CIELAB'


FUNCTION(S):

'Batch Measures the CIE Lab Mean,Min,Max,Dev values for a set of MSPEC images
 This script was made with the intention of being used to produce colour maps
 and hex values for plots with R package .colorspace. 

 NOTE-requires a XYZ human camera model to function'


FEATURES:

' Subfolders: Batch analyses can be carried out for a singular folder or folders within folders.'


' ROIs: Specific ROIs can be selected e.g. [roi1,roi2,roi3] to measure or all ROIs can be measured (AllRois).'


' Prescript: Allows for changes to be made prior to the conversion to MSPEC, e.g. generation of
             temporary ROIs.'




[N 2.2] micaToolbox/Image Batch Analyses/RNL_Colour_Mappers
------------------------------------------------------------

TITLE: 	

'RNL Colour Mappers'


FUNCTION(S):

'Allows the user to batch create RNL colour maps for a specified cone model and weber fractions model
 then they can compare the maps to each other either for just the individual folder using the 
 |RNL Compare Frequencies| or the |RNL Measure Distances scripts| alternatively all maps can be compared
 to one another using a chimera comparison |RNL Chimera| - chimera comparison takes a long time' 

' 0_Batch_Create_Maps = batch creates RNL maps '

' 1_RNL_Compare_Frequencies = compares RNL frequencies in folder '

' 1_RNL_Measure_Distances = compares RNL distances in folder '

' 2_RNL_Chimera = compares either frequencies/distances of all ROIs across to each other. '



FEATURES:

' Subfolders: Batch analyses can be carried out for a singular folder or folders within folders.'


' ROIs: Specific ROIs can be selected e.g. [roi1,roi2,roi3].'


' Prescript: Allows for changes to be made prior to the conversion to MSPEC, e.g. generation of
             temporary ROIs.'



[N 2.3] micaToolbox/Image Batch Analyses/RNL_Measures
------------------------------------------------------------

TITLE: 	

'RNL Measures'


FUNCTION(S):

'Allows the user to batch measure a number of different metrics for both a luminance channel and RNL
 colour channels. The output is saved into the selected folder as /t delimited text' 

' 0_RNL_Acuity_Mean.Dev = Measures the Mean & Dev of Luminance and RNL colour channels of specified ROIs'

' 1_RNL_Acuity_Mean.Dev.GabRat = Measures the Mean, Dev GabRat of Luminance and RNL colour channels of specified ROIs'

' 2_RNL_Acuity_Mean_DoG = Measures the Mean, Dev and Difference of Gaussian Energy at different spatial scales
                          Scales can be specified relative to a set mm scale or to the scale of a ROI'

' Test_Energy_Scale = Allows the user to test spatial scales for running pattern energy analyses'




FEATURES:

' Subfolders: Batch analyses can be carried out for a singular folder or folders within folders.'


' ROIs: Specific ROIs can be selected e.g. [roi1,roi2,roi3].'


' Prescript: Allows for changes to be made prior to the conversion to MSPEC, e.g. generation of
             temporary ROIs.'



' DoG Crop: Images can be cropped at two stages. The first is prior to running the DoG, this reduces the area 
            of the calculation and speeds up analysis. The second is for the DoG this is useful for preventing the
            ROIs from blurring into one another at larger spatial scales.'


' DoG Offset: Allows you to do scales above e.g. -2 means the last two octaves are multiples above the set scale.'




EXAMPLE:

' I'm a researcher interested at comparing the luminance and colour pattern energy of target triangles placed
  on tree bark. I'm interested in looking at how pattern match improves camouflage as well as how background
  noise increases survival. I've already created .mspec images, cone catch models and labelled ROIs 
  for all my targets. But what scales should I use? To run my measures I'll use the following steps. As 
  my targets are all the same size, I can use the ROI scaler.

  1| Step : Test Spatial Scales --

	  1|i.  Open one of your MSPECs and measure the area of the ROI
 
 	  1|ii. run(Test Energy Scale)
		
		- Set the scale to the mm wave length of the ROI (area^0.5 * scalebar)

		- Set the number of octaves to 8

		- Set the offset to -2 (will measure two scales above the target)

		- Scales should be [1/64, 1/36, 1/8, 1/4, 1/2, 1, 2, 4]
		
 	 1|iii. Check DoG of luminance channel (is the scale too small/large, is the resolution too low?)


  2| Step : Run with chosen scale --

 	 2|i.  run( 2_RNL_Acuity_DoG_(SetScale) ) using the settings that best isolate the pattern elements 
						  of the target and surrounding background

	 2|ii. Repeat the above for each ROI, setting the crop and DoG crop to that ROI (prevents bleed over)


  3| Step : Compare Energy --

 	 3|i. Only compare the energy of the target to the background at scales smaller then the target.



.............................................................
	
	3 Depth Measures (Requires MeshLab)
.............................................................
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

All PLY fines should be standardised/normalised using meshlab.

Open the .ply file and export with additional paremeters set to [none] and binary encoding.




[N 3.1] /DepthMeasures/0_Create_Height_Maps
------------------------------------------------------------
TITLE: 	

'0 Create Height Maps'


FUNCTION(S):

'Allows the user to convert .ply files into .tif Depth Maps (height maps) containg x,y & z coordinates with a resolution of
 1px per mm' 

' 0_Batch_Create_DepthMaps = Loop Create Depth Maps for selected folders'

' ROIs_Clutch = Create ROIs for the clutch images'

' ROIs_Null = Create ROIs for the null images'


FEATURES:

' Folders: Select .ply location and save location for quick saving 
           (NB: depth maps should be saved as [Depth Map.tif] in seperate folders'


' NaN Value Handling: NaNs are automatically marked out with an ROI and can either be
                      left as NaN or replaced with the mean background value.'


' ROI Script: You can insert a custom script to make batch ROI labelling easier.





[N 3.2] /DepthMeasures/0_Create_Height_Maps
------------------------------------------------------------
TITLE: 	

'1 Depth Energy'


FUNCTION(S):

'Measures Depth Maps in SubFolders' 

' Measure_3D_DoG_ScaleRoi = Batch measures the DoG Energy of specified ROIs at spatial 
  scales relative to a named ROI'

' Measure_3D_DoG_ScaleSet = Batch measures the DoG Energy of specified ROIs at spatial 
  scales relative to a set wave length in mm'

' Measure_3D_ROIs_Mean.Min.Max.Dev.Area = Batch SIMPLE measures of mean,min,max,dev and area of specified ROIs'

' Test_DoG-Scale = Allows the user to preview DoG energy scales'




FEATURES:

' DoG Crop: Images can be cropped at two stages. The first is prior to running the DoG, this reduces the area 
            of the calculation and speeds up analysis. The second is for the DoG this is useful for preventing the
            ROIs from blurring into one another at larger spatial scales.'


' DoG Offset: Allows you to do scales above e.g. -2 means the last two octaves are multiples above the set scale.'
