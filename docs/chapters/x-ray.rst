.. _chap-x-ray:

General Purpose X-ray
=====================

*46 minutes to read*

- A `Physical principles of X-ray
  <https://www.ncbi.nlm.nih.gov/books/NBK546155/>`_
  chapter of
  `Medical Imaging Systems: An Introductory Guide
  <https://doi.org/10.1007/978-3-319-96520-8>`_
  book.
  Feel free to skip paragraphs that are not relevant to you.

- `Clinical indications for X-ray <https://www.insideradiology.com.au/plain-radiograph-x-ray-hp/>`_

DICOM format
------------

*35 (+10) minutes to read*

----------
Modalities
----------
With X-rays different kinds of images can be created, like tomosynthesis, or CT - both will be touched upon later in this course.
But the most widespread images are so-called plain X-ray images that are produced by conventional X-ray machines.
If plain X-ray images are stored in the DICOM format, they can have one of the three modality tags:

- CR - Computed Radiography

  `What is CR technology? <https://www.scanx-ndt.com/cr-technology.html>`_

  `CR file structure <https://dicom.innolitics.com/ciods/cr-image>`_

- DX - Digital Radiography (DX stands for Digital X-ray)

  `DX file structure <https://dicom.innolitics.com/ciods/digital-x-ray-image>`_

- RG - RadioGraphic imaging (conventional film/screen) - if you are to work with these files, they are already digitized
  but still should be marked as RG.

  The RG DICOM file structure may differ from the CR and DX ones.
  But most tags, and thus preprocessing, should be generally the same.
  We don't mention this modality in the next section because of that.

.. _xray_image_preprocessing:

---------------------------
Typical image preprocessing
---------------------------

Firstly, we should understand what the DICOM LUT (LookUp Table) is:

- `Working with DICOM LUT <https://www.leadtools.com/help/sdk/v21/dh/to/working-with-dicom-lut.html>`_

- `Modality and VOI LUTs <https://gdcm.sourceforge.net/wiki/index.php/Modality_LUT>`_
  (Original link stopped working, but the material is still available via
  `Internet Archive <https://web.archive.org/web/20180527043519/https://gdcm.sourceforge.net/wiki/index.php/Modality_LUT>`_)

The general formula for the DICOM X-ray preprocessing is the following:

.. note:: nice_looking_image = inversion[optional](  VOI_LUT( Modality_LUT(pixel_data) )  )

Each function in the formula is described separately below.

**Modality LUT** is used to obtain physically meaningful values (e.g.,
`HU <https://en.wikipedia.org/wiki/Hounsfield_scale#Definition>`_,
`OD <https://en.wikipedia.org/wiki/Absorbance>`_).

- `Modality LUT tag for CR file <https://dicom.innolitics.com/ciods/cr-image/modality-lut>`_
- `Modality LUT tag for DX file <https://dicom.innolitics.com/ciods/digital-x-ray-image/general-image/00409096>`_
- `Pydicom function to apply Modality LUT
  <https://pydicom.github.io/pydicom/dev/reference/generated/pydicom.pixel_data_handlers.apply_modality_lut.html>`_

**VOI (Value Of Interest) LUT** is used to obtain pictures where a certain type of tissue looks nice (well-contrasted).

- `VOI LUT tag for CR file <https://dicom.innolitics.com/ciods/cr-image/voi-lut>`_

- `VOI LUT tag for DX file <https://dicom.innolitics.com/ciods/digital-x-ray-image/voi-lut>`_

- `Pydicom function to apply VOI LUT
  <https://pydicom.github.io/pydicom/dev/reference/generated/pydicom.pixel_data_handlers.apply_voi_lut.html>`_

.. warning:: In one DICOM file, it can be more than one VOI LUT!

**Photometric Interpretation** tag points to how to understand the value of the pixels.

In the case of X-rays, there are two possible values of the Photometric Interpretation tag:
MONOCHROME1 (dense tissues are dark) and MONOCHROME2 (dense tissues are bright). The last one seems to be used more commonly.

- `Photometric Interpretation tag for CR file <https://dicom.innolitics.com/ciods/cr-image/image-pixel/00280004>`_

- `Photometric Interpretation tag for DX file <https://dicom.innolitics.com/ciods/digital-x-ray-image/image-pixel/00280004>`_

Usually, you will need to pass the images into the model with one pre-selected Photometric Interpretation.
Invert the image explicitly when needed. For example:

.. code-block::

  if PhotometricInterpretation == 'MONOCHROME1':
    img = img.max() - img

`Pydicom tutorial <https://pydicom.github.io/pydicom/dev/old/working_with_pixel_data.html>`_
on how to work with DICOM pixel data.

.. warning:: The tutorial above shows only what you can do with pixel data. It **does not** describe the image preparation sequence.

[optional] The complete description of how to preprocess the DICOM pixel data to the properly looking image can be found
in the `Pixel Transformation Sequence <http://dicom.nema.org/medical/Dicom/2018d/output/chtml/part04/sect_N.2.html>`_
section of the `official DICOM standard documentation <https://www.dicomstandard.org/current>`_.

.. _xray_image_formats:

Image formats
-------------

*14 minutes to read*

Fairly often medical datasets are stored using widespread image file formats like png, jpg/jpeg, tiff, etc.
Though images are much easier to read with Python, usually it's harder to combine such datasets with the other ones.
First of all, those image formats can introduce their own artifacts (like jpeg compression).
Second, the authors of a dataset can use any preprocessing they like (maybe skipping or modifying some steps described in the previous paragraph, or introducing new ones).
As a result, image statistics can vary significantly for images from different sources.
It’s great when authors of the dataset describe the preprocessing they applied in detail, but unfortunately, it’s not always the case.

Typical image **preprocessing** serves multiple purposes:

- To make image statistics better (to ease training)

- Increase visual contrast (disputable, because NNs can learn contrast-enhancing filters by themselves)

- Equalize input image statistics from different datasets (the most important one)

This is usually done with either
`histogram equalization <https://en.wikipedia.org/wiki/Histogram_equalization>`_
or `CLAHE <https://en.wikipedia.org/wiki/Adaptive_histogram_equalization#CLAHE>`_
(`example notebook <https://www.kaggle.com/raddar/popular-x-ray-image-normalization-techniques>`_).

.. _xray_split:

How to split the data
---------------------

*1 minute to read*

As usual, we use standard train/validation/test split.
A good practice is to have another out-of-domain test set (e.g., from a different source/device/dataset)
to measure the final performance of the model.

The details of how to split your data depend on the task.
But usually, images are split based on the patient IDs.
This is commonly required because if, e.g., you are trying to classify pathologies,
the appearance of the X-ray images of the same person with the same pathology in the different data splits would introduce a data leak.
Which basically means that the network will memorize that *this* patient always has *this* pathology.

In the case of DICOM datasets, you can use the
`PatientID <https://dicom.innolitics.com/ciods/cr-image/patient/00100020>`_ tag, if it's not empty.
Otherwise (or in the case of an image-format dataset), the only information you have is what the dataset authors provided.

Exercise
--------

*3(+2) days of work*

**Task**: Train the classification model on
the `PNG dataset <https://www.kaggle.com/paultimothymooney/chest-xray-pneumonia>`_, make it work on
the `DICOM dataset <https://www.kaggle.com/c/rsna-pneumonia-detection-challenge/data>`_.

- Try to use different preprocessing techniques, enable/disable some of the preprocessing steps.

- What steps are crucial and what is not helpful at all? Why?

- [optional] Try to solve the inverse task: train on a DCM dataset and make it work on a PNG dataset.
