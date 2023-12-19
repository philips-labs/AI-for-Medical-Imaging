.. _chap-mri:

(WIP) MRI
=========

- How images are obtained (physics behind it, raw data format, etc.)

to be described

- How the images look like

typical sequences T1, T2, FLAIR, STIR, PD, diff (can be different! ACD - average diffusion, or vector) - brain stroke
book - Book - https://www.wiley.com/en-us/MRI+in+Practice%2C+5th+Edition-p-9781119392002
https://www.westbrookmriinpractice.com/

+ contrast-enhanced

- How the images are used (medical value) -

soft tissues, diff - brain stroke/hemorrhage/SD

- How the images are stored (DICOM/NIfTI/images/custom formats - refer to the docs ???)

to be described

orientation tags can have different float values (unlike CT),
e.g. image: https://mrimaster.com/images/POSSITION%20BUTTON/PLANNING/ORBITS/MRI%20ORBITS%20PLANNING%20OF%20SAGITTAL%20RT.jpg

Sasha's impression on NIfTI:
0. Unlike in DICOM, the NIfTI header is limited in size (bytes)
1. Not always transforms is described as affine transformations, can be in quaternions or list of transformations
2. Can include time series (fMRI)
3. Several regimes in one NIfTI file <- read about it
4. Don't use interpolations if resolution across one dimension is poor (MRI case),
but there are studies with high resolution which allow using of interpolations well
5. The interpolations of the pixel data are not performed while creating the NIfTI file from DICOM ones (verify it)
6. There is some "canonical space" to which the coordinate system can be transformed (figure out what's this)

- Typical image preprocessing schemes

read the standard, ask colleagues
seems like there's no Modality LUT
(because there's no physical meaning of the pixel values)
There's VOI LUT
There are approaches to automatically pick the best window for standardization


- How to split the data in a meaningful way

standard CT like + take into account sequences

- What typical tasks are usually solved for this modality

to be described

- Exercises and links to the data to play with

to be described
rotate an image in a way, that patient always look straight upwards

1. 2D-segmentation
2. denoising/DA
3. registration/fusion