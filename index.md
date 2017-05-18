#################################################################
 			DISCLAIMER
#################################################################

This software and documentation (the "Software") were developed at the Food and Drug Administration (FDA) by employees of the Federal Government in the course of their official duties. Pursuant to Title 17, Section 105 of the United States Code, this work is not subject to copyright protection and is in the public domain. Permission is hereby granted, free of charge, to any person obtaining a copy of the Software, to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, or sell copies of the Software or derivatives, and to permit persons to whom the Software is furnished to do so. FDA assumes no responsibility whatsoever for use by other parties of the Software, its source code, documentation or compiled executables, and makes no guarantees, expressed or implied, about its quality, reliability, or any other characteristic. Further, use of this code in no way implies endorsement by the FDA or confers any advantage in regulatory decisions. Although this software can be redistributed and/or modified freely, we ask that any derivative works bear some notice that they have been modified.


File:    	Quick-start-guide.txt (fastDETECT2)
Authors:  	Diksha Sharma (Diksha.Sharma@fda.hhs.gov)
		Aldo Badano (Aldo.Badano@fda.hhs.gov)
Date:    	Apr 30, 2015


#################################################################

*****************************************************************
			INPUT AND OUTPUT FILES
*****************************************************************

fastDETECT2 requires one input file and generates three output files.  The input file contains the location, energy and x-ray history number of the energy deposition events.  The order should be x, y, z, energy, x-ray history number.  x, y, z should be in cm and energy in eV.  For each x-ray interaction, all the energy deposition event information is listed in this format, with each event starting a new line.  fastDETECT2 recognizes the end of an x-ray history by searching for # symbol.  For instance, a file with two x-ray histories may look as follows:  

4.598436841E-02  4.456798661E-02 -6.058012811E-03   2.5000E+04   1
4.598427193E-02  4.456799402E-02 -6.057901975E-03   6.4278E+01   1
4.598425612E-02  4.456799896E-02 -6.057882450E-03   6.0145E+01   1
4.598414321E-02  4.456799981E-02 -6.057783738E-03   4.1207E+00   1
…
# Past hist = 	1 

3.453936841E-02  1.756798661E-02 -2.056012811E-03   1.5000E+03   2
2.324271936E-02  2.956799402E-02 -2.043901975E-03   4.4278E+01   2
3.342425612E-02  1.756799896E-02 -1.042882450E-03   6.0145E+00   2
3.123414321E-02  1.356799981E-02 -1.003783738E-03   7.1207E+00   2
…
# Past hist = 	2 

  
The three output files include: point response function image, pulse height spectrum, and number of detected optical photons per primary.  Point response function image outputs the image based on the lower and upper bounds of x and y hard coded.  Currently it is set to be the entire detector area, however if the user wishes to modify this, they can go to fastDETECT2.c and search for ‘initialize rest of the parameters’, make changes to lbound_x/y and ubound_x/y and re-compile.  One of the file outputs list the number of detected optical photons per primary history in list mode.  This data is binned based on the number of bins provided by the user in the input arguments and the hard coded values of min and maximum optical photons to be binned, to obtain the pulse height spectrum.



*****************************************************************
			CODE COMPILATION AND EXECUTION
*****************************************************************

The code can be compiled by running ./compile.sh.  gcc and GNU scientific library packages have to be installed prior to compiling.  The current version of fastDETECT2 has been tested with gcc version 4.8.2 and GNU scientific library version 1.16.  The compilation will generate fastDETECT2.x executable.  A pre-compiled executable (on Linux platform) is included in the folder.

Execution: ./fastDETECT2.x command line arguments separated by a blank space.

Example:  ./fastDETECT2.x 150 5.1 0.1 1e-4 0.2 0.25 1000 400 2415364 CsI_deposition_events.dat PRF.out PHS.out DPP.out

	

*****************************************************************
			DEMO CASE
*****************************************************************

The following case can be run as a demo to verify that the code is working properly.  
There is a sample input file named ‘CsI_deposition_events.dat’ included in the folder.  This file was generated using PENELOPE 2006 (F. Salvat, J. Fernandez-Varea, and J. Sempau. PENELOPE-2006: A code system for Monte Carlo simulation of electron and photon transport. NEA-OECD, available at 
http://www.oecd-nea.org/science/pubs/2006/nea6222-penelope.pdf, 2006), using 25 keV monoenergetic x-ray beam (point source) incident at the center of the detector (dimensions 909 x 909 x 150 µm3), and 1e4 x-ray histories were simulated.  Please note that maximum 1e4 histories (input argument: total number of primaries to be simulated) can be simulated using this file.

Input arguments:
1. 	Thickness = 150 µm.
2. 	Radius of columns = 5.1 µm.
3. 	Top surface absorption fraction = 0.1 (modeling almost ideal reflector).
4. 	Bulk absorption coefficient = 1e-4 µm-1.
5. 	Roughness coefficient = 0.2.
6. 	Sensor reflectivity = 0.25.
7. 	Total number of primaries to be simulated = 1000.
8. 	Number of bins used for generating the PHS = 400.
9. 	Initial seed to be used for RNG = 2415364.
10. 	Energy deposition file name = CsI_deposition_events.dat.
11. 	Point response function image file name = PRF.out.
12. 	PHS file name = PHS.out.
13. 	Number of detected optical photons per primary file name = DPP.out.


One should expect to see the below optical transport statistics on the console (these are also reported in sample-output.txt):


*****************************************************************
		OPTICAL TRANSPORT STATISTICS
*****************************************************************
	Total number of photons:
		 Generated: 		 2325690
		 Detected: 		 1485981
		 Absorbed at top: 	 208974
		 Absorbed in bulk: 	 42729
		 Lost at the boundary: 	 586550
	 Average path length (microns):  569.258362
	 Total time (seconds): 		 85.247794
	 Speed (primaries/seconds): 	 11.730509
*****************************************************************



***************************************************************
			CONTACT US
***************************************************************

For more details, please refer to README.txt.

Any questions or comments should be directed to Diksha Sharma (Diksha.Sharma@fda.hhs.gov) or Aldo Badano (Aldo.Badano@fda.hhs.gov).

