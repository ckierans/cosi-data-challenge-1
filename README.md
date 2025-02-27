# COSI Data Challenge 2022

Welcome to the first COSI Data Challenge! This is the first of many COSI Data Challenges to be released on a yearly basis in preparation for the launch of the COSI Small Explorer mission ([Tomsick et al. 2019](https://ui.adsabs.harvard.edu/abs/2019BAAS...51g..98T/abstract)) in 2027. The main goals of the COSI Data Challenges are to facilitate the development of the COSI data pipeline and analysis tools, and to provide resources to the astrophysics community to become familiar with COSI data. This first COSI Data Challenge was funded through NASA’s Astrophysics Research and Analysis (APRA) for the release of high-level analysis tools for the COSI Balloon instrument ([Kierans et al. 2017](https://ui.adsabs.harvard.edu/abs/2017arXiv170105558K/abstract)), and thus the COSI Balloon model and flight data will be the focus this year. Future COSI Data Challenges will be released for the SMEX mission with increasingly more sophisticated tools and a larger range of astrophysical models and simulated sources each year. By the time we’re ready to fly COSI, we will have simulated all of the main science objectives, developed the tools required to analyze each case, and have educated a broader community to perform the analyses.

## Download and Install

### Already have COSItools installed:
If you already have the COSItools installed on your computer, type `cosi` to navigate to the COSItools directory and activate the cosi python environment. Clone the cosi-data-challenge-1 repository.

The file [requirements.txt](requirements.txt) lists all of the dependencies for running the data challenge, and they can all be installed using pip: `pip install -r requirements.txt`. 

You will also need to add the [cosipy-classic](cosipy-classic) directory to your python path, e.g. `export PYTHONPATH=$PYTHONPATH:/path/to/COSItools/cosi-data-challenge-1/cosipy-classic`. 

Some of the data products are large and we are using the Git Large File Server. You will need to install git-lfs: https://docs.github.com/en/repositories/working-with-files/managing-large-files/installing-git-large-file-storage. After Git LFS has been successfully installed, navigate to your local cosi-data-challenge-1 directory and `git lfs pull` to download all of the data products. If this step is not performed, the file names will appear locally, but they will only be placeholders and the size will be only a few hundred kB. The total size of the data products should be 4.6 GB.

You should now be able to start a python session, or open one of the provided notebooks, and import the COSIpy_dc1 module, i.e. `from COSIpy_dc1.py import *`, without issue. If you have any issues with the initialization, please see **Getting Help** below.

### New to COSItools:
Head to the feature/initialsetup branch of the cosi-setup Git repository and follow the readme guide: https://github.com/cositools/cosi-setup/tree/feature/initialsetup. Note that when making the installation you must include the option "--extras=cosi-data-challenge-1". This installation will include MEGAlib, ROOT, Geant4, Git LFS, and all packages in the [requirements.txt](requirements.txt) file. After successful installation (this will take time), you will want to activate the cosi python environment by typing `cosi`. 

You will also need to add the [cosipy-classic](cosipy-classic) directory to your python path, e.g. `export PYTHONPATH=$PYTHONPATH:/path/to/COSItools/cosi-data-challenge-1/cosipy-classic`. 

## Getting Help

Since this is the first release for COSIpy there will likely be issues, and we definitely hope to get feedback from the community on what worked well, what didn't, and how can we improve for next time. Please submit a New Issue in git if you have issues with the code. If you have general feedback, or need further assistance, please reach out to the COSI Data Challenge team lead: [Chris Karwin: christopher.m.karwin@nasa.gov](mailto:christopher.m.karwin@nasa.gov).


## Getting Started
The COSI pipeline tools, COSItools, are divided into two programs (see figure below): MEGAlib and COSIpy. MEGAlib (https://megalibtoolkit.com and https://github.com/zoglauer/megalib) is the Medium Energy Gamma-ray Astronomy Library, which is a general purpose tool that is state-of-the-art for MeV telescopes. MEGAlib performs the data calibration, event identification, reconstruction, as well as detailed source simulations. There are some high-level analysis tools in MEGAlib, but most of the COSI science analysis will be performed in COSIpy. COSIpy is the user-facing, python-based, high-level analysis tool for COSI data. 

<img width="800" alt="Screen Shot 2022-10-16 at 10 19 29 PM" src="https://user-images.githubusercontent.com/33991471/196075227-8d1fe6c8-eb4b-40aa-905e-549e15ecabe8.png">

This Data Challenge will serve to introduce the community to COSIpy and general Compton telescope analysis. We have prepared Jupyter Notebooks to walk the user through the analyses which are provided under [spectral-fit](spectral-fit) and [imaging](imaging); however, we suggest reading through the below description before attempting the notebooks.

COSIpy was first developed by Thomas Siegert in 2019 to perform 511 keV image analysis from the 2016 COSI balloon flight ([Siegert et al. 2020](https://ui.adsabs.harvard.edu/abs/2020ApJ...897...45S/abstract)). Since then, it has been used for point source imaging and spectral extraction (e.g. [Zoglauer et al. 2021](https://ui.adsabs.harvard.edu/abs/2021arXiv210213158Z/abstract)), aluminum-26 spectral fitting ([Beechert et al. 2022](https://ui.adsabs.harvard.edu/abs/2022ApJ...928..119B/abstract)) and Al-26 imaging, all using data from the COSI Balloon 2016 flight. These analyses and the current Data Challenge use what we refer to as “COSIpy-classic.” The team is currently working on improved response handling and streamline tools built from the bottom up, and the new and improved COSIpy will be the focus of next years’ Data Challenge! With that in mind, there are still known issues and limitations with COSIpy-classic that we will call out throughout this work.

## The Simulated Data

For the first Data Challenge, we wanted to give the users a basic look at COSI data analysis, so we chose 3 straightforward examples based on the COSI science goals: </br>
1. Extracting the spectra from the Crab, Cen A, Cyg X-1, and Vela. </br>
2. Imaging bright point sources, such as the Crab and Cyg-X1. </br>
3. Imaging diffuse emission from 511 keV and the Al-26 1.8 MeV gamma-ray line. </br>

For each of these examples, we have provided a detailed description of the simulated sources and data products in the [data_products](data_products) directory. Each of the sources was simulated at 10x the astrophysical flux since the balloon flight had limited observation time, and because there were multiple detector failures during the balloon flight which reduced the effective area significantly. 

## General Compton Telescope Analysis Procedure

COSI is a Compton telescope, and analysis of MeV data is challenging due to the high-backgrounds and complicated instrument response. Thus, we will provide a basic description of Compton telescope data analysis here as an introduction for new users before diving into the Data Challenge analysis. For those who are new to Compton telescopes, we encourage you to read the review: [Kierans, Takahashi & Kanbach 2022](https://ui.adsabs.harvard.edu/abs/2022arXiv220807819K/abstract). 

Compton telescopes provide a technique for single-photon detection, and each photon is measured as a number of energy deposits in the detector volume, shown in blue in the figure below. These interactions must first be sequenced, and then reconstructed to determine the original direction of the photon, which is constrained to a circle on the sky (a so-called Compton event circle) defined with opening angle equal to the Compton scattering angle $(\phi)$ of the first interaction. The classic schematic for Compton telescopes is shown in the figure below, where this example shows a photon that results in 2 Compton scattering interactions, and finally a photoelectric absorption in interaction 3, fully containing the energy of the photon in the active detector volume. The event sequencing and reconstruction all occurs in the MEGAlib tool, and we will be starting our analysis with events that are already defined by their total energy deposit in the detector $(E_0)$, and the Compton scattering angle.

<img width="760" alt="Screen Shot 2022-10-16 at 11 00 42 PM" src="https://user-images.githubusercontent.com/33991471/196080097-d3fdde9c-e7ae-494b-af02-1ffb846dbc33.png">

The above figure is traditionally shown for Compton telescopes, where an image of the source distribution can be realized by finding the overlap of event circles from multiple source photons. A point-source image can be recovered by performing deconvolution techniques. This List-mode imaging approach ([Zoglauer 2005](https://megalibtoolkit.com/documents/Zoglauer_PhD.pdf)) is implemented in MEGAlib, and can be used for strong point sources, for example in laboratory measurements. However, this assumes a simplified detector response and cannot be used for the most sensitive analyses.

When performing analyses of astrophysical sources with high backgrounds, a more sophisticated description of the data space is required. This data space, along with the fundamentals of modern Compton telescope analysis techniques, was pioneered for the COMPTEL mission ([Schönfelder et al. 1993](https://ui.adsabs.harvard.edu/abs/1993ApJS...86..657S/abstract) & [Diehl et al. 1992](https://ui.adsabs.harvard.edu/abs/1992NASCP3137...95D/abstract)) and is sometimes referred to as the COMPTEL Data Space, or simply the Compton Data Space (CDS). In the CDS, each event is defined by a minimum of 5 parameters. Three parameters describe the geometry of each event: the Compton scatter angle $(\phi)$ of the first interaction, and the polar and azimuthal angles of the scattered photon direction $(\chi, \psi)$. These angles as defined in instrument coordinates are shown in the figure below. The other two parameters in the CDS are the total photon energy $(E_0)$ and the event time $(t)$, but these are not always explicitly written. 

<img width="722" alt="Screen Shot 2022-10-16 at 11 02 38 PM" src="https://user-images.githubusercontent.com/33991471/196080315-86545b58-7104-4ba1-b68d-e3a634e69fa5.png">

The three scattering angles $(\phi, \chi, \psi)$ make up 3 orthogonal axes of the CDS. As photons from a point source at location $(\chi_0, \psi_0)$ scatter in the detector, the CDS will be populated in the shape of a cone with apex at the source location, as shown in the figure on the right. This is the point spread function of a Compton telescope. The opening angle of the CDS cone is 90º since the Compton scatter angle is equal (within measurement error) to the deviation of the scattered photon direction. An extended source will appear as a broadened cone. The more familiar Angular Resolution Measure (ARM) for Compton telescopes is a 1-dimensional projection of the width of the CDS cone walls, representing the angular resolution. 

All analyses with COSIpy start with reconstructed events defined in a photon list (MEGAlib’s .tra files). After reading in the data from the .tra file, the first step of any analysis is to bin the data into the CDS. For this Data Challenge, we are using 6º bin sizes for each of the scattering angles. This is because the angular resolution of the COSI Balloon instrument is ~6º at best, and smaller bins would be more computationally demanding. We are also using 10 energy bins for the continuum analyses, and 2 hour time bins for the spectral analysis and 30 min time bins for the imaging. For the narrow-line sources, such as 511 keV or Al-26, we use only 1 energy bin centered on the gamma-ray line of interest.

As a visual representation of the data (D) in the CDS, see the figure below. The three axes are the 3 scatter angles, and each bin contains the number of events, or counts, with that $(\phi, \chi, \psi)$, shown with the red color fill (this is just a random distribution and not representative of what real data looks like in the CDS). The CDS is filled for each energy and time bin, represented by the subscript $E,t$ in the figure. In COSIpy-classic, $\chi$ and $\psi$ are binned into in 1145 FISBEL bins. FISBEL stands for Fixed Integral Square Bins in Equi-Longitude, and is MEGAlib’s spherical axis binnings that has approximately equal solid angle for each pixel.

<img width="400" alt="Screen Shot 2022-10-16 at 11 03 38 PM" src="https://user-images.githubusercontent.com/33991471/196080461-6e97c2c2-4d36-4cfb-b13c-935f438616c1.png">

Now that we have a better understanding of the Compton telescope data space, and have our data binned in the CDS, we’re ready to understand how to perform spectral analysis and imaging with COSI. There are 3 key pieces needed: 
1. Response Matrix
2. Sky Model
3. Background Model

We will describe these components here and explain how they are used for general fitting procedures with COSI data. When running through the analysis notebooks pay attention to when these are initialized.

### Response Matrix

The response matrix (R) for Compton telescopes represents the probability that a photon with energy E initiating from Galactic coordinates $(l,b)$ interacts in the detector resulting in an event with measured energy E’ and scattering angles $(\phi,\chi,\psi)$. The probability distribution is normalized to match the total effective area of the instrument, which allows us to go from counts to physical parameters. The matrix that encodes this information is 2 dimensions larger $(l,b)$ than the CDS and describes the transformation between image space and the CDS, taking into account the accurate response of the instrument. We build the response matrix through large MEGAlib simulations of an isotropic source.

<img width="965" alt="Screen Shot 2022-10-16 at 11 05 26 PM" src="https://user-images.githubusercontent.com/33991471/196080645-6f1eecb9-a34a-489f-ae7c-79f43224ae32.png">

### Sky Model

For forward-folding analyses methods, we assume a source sky distribution, referred to as the sky (or signal) model (S). The sky model in image space can be simply a point source or it can be a complicated diffuse model. The sky model also contains the spectral and polarization assumptions. This model is convolved with the instrument response matrix, and includes knowledge of the instrument aspect during observations, to determine the representation of the sky model in the CDS. For example, here we are showing this for a simple point source, and we regain the expected cone-shape in the CDS.

<img width="1065" alt="Screen Shot 2022-10-16 at 11 06 10 PM" src="https://user-images.githubusercontent.com/33991471/196080720-dc1db3c5-0bee-4843-b8d5-ddb936745bb1.png">

### Background Model

We require an accurate estimate of the backgrounds during observations. This background model (B) can be achieved in a number of ways, for example by using the measured flight data from source-starved regions. Alternatively, one can perform full bottom-up simulations of the gamma-ray background at balloon-flight altitudes, including activation. For this first Data Challenge, we are using this approach, and the simulation is further described in [data_products](data_products); however, for future Data Challenges, we will be employing multiple background-model approaches.

With the background model generated from simulations, then we first have to bin it in the same CDS that we have used for the data and the source model. We have already performed this step for you, and have provided a .npz file, which is a zipped numpy array of the background simulation in the CDS.

<img width="1053" alt="Screen Shot 2022-10-16 at 11 06 45 PM" src="https://user-images.githubusercontent.com/33991471/196080812-bb591627-3127-454d-af92-1d859313774b.png">

### Fitting General Principle

Finally, we have all of our components, and now we can perform our analysis. When model fitting, we can free multiple spectral, polarization and location parameters. For simplicity, let's assume we will only fit for the amplitude of the source and background models that describe the data: **D = $\alpha$ S + $\beta$ B**, shown schematically in the figure below, which is done for each energy bin and time bin independently. Through this procedure for spectral fitting and spatial model fitting, one can maximize the likelihood in the CDS.

<img width="1074" alt="Screen Shot 2022-10-16 at 11 07 20 PM" src="https://user-images.githubusercontent.com/33991471/196080880-83937b0d-15b0-4a93-b0dc-09368d3423ae.png">

If we don’t know what the source should look like, instead of providing a sky model, we can be agnostic and perform image deconvolution. The data is a combination of the response convolved with the source sky distribution, and the addition of the background:

<img width="482" alt="Screen Shot 2022-10-16 at 11 08 00 PM" src="https://user-images.githubusercontent.com/33991471/196080950-0a042af0-0bd8-4360-9f7f-898edcd81ee1.png">

Since the response is non-invertible, we must use iterative deconvolutions to arrive at the sky distribution. In COSIpy-classic, we will introduce you to a modified Richardson-Lucy algorithm, which is a special case of the expectation maximization algorithm developed for COMPTEL’s images of diffuse gamma-ray emission ([Knödlseder et al. 1999](https://ui.adsabs.harvard.edu/abs/1999A%26A...345..813K/abstract)). The Richardson-Lucy algorithm starts from an initial image, and iteratively compares the data in the CDS to the sky distribution convolved with the transpose of the response matrix R:

<img width="257" alt="Screen Shot 2022-10-16 at 11 08 28 PM" src="https://user-images.githubusercontent.com/33991471/196080993-a258bd27-9df9-4996-924c-ec3bc7222aa8.png">

This will evolve into the maximum likelihood solution. There are other image deconvolution techniques that will be used for COSI imaging, and those will be introduced in subsequent Data Challenges.

In order to get an intuition for how this equation is derived and to be understood, consider that we are trying to find the number of photons in an image pixel, given our model expectation $M = R \cdot S + B$. Here, $M$ are the model counts, calculated from a linear combination of the instrumental background $B$ and the sky morphology $S$, to which the image response function $R$ is applied to convert from image space to data space.<br>
In such a counting experiment, the measured data $D$ given the model expectation $M$ follows a Poisson distribution, so that the likelihood of measuring $D$ photons is given by $P(D|M) = \prod \frac{\exp(-M)M^D}{D!}$. The product would be over all sky pixels in this case.<br>
Taking the logarithm of the likelihood and plugging in the model expectation results in $L \equiv \ln P(D|S,B) = \sum \left(-(R \cdot S + B) + D \ln(R \cdot B) \right)$.<br>
Assuming, for the moment, that the background is known, we can optimise the likelihood with respect to the wanted sky signal $S$, such that<br>
$0 = \nabla_S L = -I \cdot R^T + \frac{D \cdot R^T}{R \cdot S + B} = \left(1 - \frac{D}{R \cdot S + B} \right) \cdot R^T$.<br>
Finally, this equation can be solved iteratively, (for each pixel simultaneously) by assuming a starting sky distribution (map) for $S$, similar to Newton's method for example, which will result in the RL equation above.

## Next Steps

Now that you have a better feeling for the Compton telescope analysis process, at least in a general sense, you’re ready to start on the analysis examples. First, we recommend you check out the detailed description of the simulations in the [data_products](data_products) directory. The easiest analysis is the spectral fitting, and we recommend you start there: [spectral_fit](spectral-fit). The Richardson-Lucy imaging is computationally intensive, and still largely hard-coded in this release, and thus we recommend you work through these notebooks second: [imaging](imaging). If you have a workstation with a large memory allocation, we recommend you use that for these analyses - a personal laptop with only 16 GB of memory will be limiting. And then finally, we have also included some of the COSI flight data, specifically for analysis of the Crab as seen during the 2016 flight. After you’ve run through the spectral fitting and image deconvolution with the simulated data, you should be ready to analyze this real flight data: [balloon data](cosi_2016_balloon_data)! 

As mentioned previously, please don't hesitate to reach out to the COSI Data Challenge team if you have any questions, concerns, issues, or suggestions. Email [Chris Karwin](mailto:christopher.m.karwin@nasa.gov).

