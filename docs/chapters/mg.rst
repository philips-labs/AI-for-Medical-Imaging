.. _chap-mg:

Mammography
===========

*94 minutes to read*

First, let's have a look at the clinical aspect.

What is mammography, why it's useful, and how the procedure is done is well-described in this
`Mammography article <https://www.radiologyinfo.org/en/info/mammo>`_.

These two videos give a good overview of the Mammography study:

 - `Introduction to Mammography <https://www.youtube.com/watch?v=dEdR4iOdLh0>`_
 - `How to read the Mammogram <https://www.youtube.com/watch?v=uH03lQqSeSw>`_

Also, please take a look at this `Mammograms fact sheet <https://www.cancer.gov/types/breast/mammograms-fact-sheet>`_
- it answers many disputable questions.

Second, let's get familiar with History and Physics.

`Mammography: Equipment and Basic Physics <https://radiologykey.com/mammography-equipment-and-basic-physics/>`_
- read this article until the "X-ray production" paragraph. X-ray production physics is the same as everywhere.

`Mammography imaging <https://www.radiologycafe.com/frcr-physics-notes/x-ray-imaging/mammography/>`_
- is a more general and less detailed description,
but it covers more aspects than the article above, including the basics of the new X-ray imaging technique such as the tomosynthesis.

`Review of the History and the Physics of Mammography <https://www.aapm.org/meetings/amos2/pdf/41-10112-77849-656.pdf>`_
- a presentation that has lots of very demonstrative images.

DICOM format
------------

*12 minutes to read*

Initially, mammography images were done using films (just like General Purpose X-rays).
But, nowadays mammography images are usually stored in the DICOM format.

Mammography DICOM files can have only one modality -
`MG <https://dicom.innolitics.com/ciods/digital-mammography-x-ray-image/mammography-series/00080060>`_.

Each mammography study consists of at least four full-view images - CC and MLO for each breast.

To get the orientation of the mammography image, you should first refer to the
`View Code Sequence module <https://dicom.innolitics.com/ciods/digital-mammography-x-ray-image/dx-positioning/00540220>`_.
It contains two important tags:

 - `Code Value <https://dicom.innolitics.com/ciods/digital-mammography-x-ray-image/dx-positioning/00540220/00080100>`_
   - to match it with the orientation you should use the
   `CID 4014 View for Mammography <https://dicom.nema.org/medical/Dicom/2018d/output/chtml/part16/sect_CID_4014.html>`_
   table from the DICOM standard.

 - `Code Meaning <https://dicom.innolitics.com/ciods/digital-mammography-x-ray-image/dx-positioning/00540220/00080104>`_
   which is self-descriptive.

Only if the
`View Code Sequence module <https://dicom.innolitics.com/ciods/digital-mammography-x-ray-image/dx-positioning/00540220>`_
is not presented, use the
`View Position tag <https://dicom.innolitics.com/ciods/digital-mammography-x-ray-image/dx-positioning/00185101>`_.
But remember that this tag is not supposed to be used for mammography, it was designed for the CR and DX images,
and actually does not have MLO or CC as defined terms.

Breast laterality can be encoded in two tags:

 - `Image Laterality <https://dicom.innolitics.com/ciods/digital-mammography-x-ray-image/dx-anatomy-imaged/00200062>`_ - in most cases use this one,
 - `Laterality <https://dicom.innolitics.com/ciods/digital-mammography-x-ray-image/general-series/00200060>`_ - if the above is not present.

.. note:: Don't be surprised to find zoomed-in images of the breast tissue in datasets. When a suspicious structure is found,
          doctors want to have a close shot. For screening models, such images are usually discarded.
          But depending on your end goal and a solution pipeline, you may want to use them.

--------------------
How to read the data
--------------------

One study can have multiple series. Each series can have multiple images.

Though the DICOM standard states that a single DICOM file can store multiple images via
`Multi-frame Module <http://dicom.nema.org/medical/dicom/current/output/chtml/part03/sect_C.7.6.6.html>`_,
usually one file corresponds to one slice.

**Example:** The (classical DICOM) files structure for a mammography study with the two series.
::

    └── 1.3.6.1.4.1.9328.50.4.0005  <-- patient's folder
        ├── 1.3.6.1.4.1.9328.50.4.4715  <-- study
        │   ├── 1.3.6.1.4.1.9328.50.4.4716  <-- series
        │   │   ├── 1-001.dcm  <-- e.g., left CC
        │   │   ├── 1-002.dcm  <-- e.g., left MLO
        │   │   ├── 1-003.dcm  <-- e.g., right CC
        │   │   └── 1-004.dcm  <-- e.g., right MLO
        │   └── 1.3.6.1.4.1.9328.50.4.5094  <-- series
        │       ├── 1-001.dcm  <-- e.g., zoomed-in images of the finding
        │       └── 1-002.dcm
        └── 1.3.6.1.4.1.9328.50.4.5514  <-- another study of the same patient

So, to read mammo data, it can be required to walk recursively through the files and read all of them.
Then use information from the DICOM header of each file to get access to a full study or series.

Of course, if you're working with public datasets, a files index may already exist. Alongside with the annotations or whatsoever.
But even in this case, it is useful to look into the aggregated from the DICOM files metadata.

---------------------------
Typical image preprocessing
---------------------------

The mammography DICOM image preprocessing is mostly the same as the one for the other DICOM X-rays.
Please see the :ref:`General Purpose X-ray -> Typical image preprocessing <xray_image_preprocessing>` paragraph.

As image contrast is essential for mammography, the images are usually stored using the 16-bit pixel values.
So, keep this in mind if you are going to save your images after preprocessing,
as the most typical imaging formats support only 8-bit values.
This could lead to signal loss and thus degrade models' performance.
Consider the following image formats for saving your preprocessed data (as they support 16-bit images):

 - PNG
 - TIFF
 - JPEG 2000
 - custom formats like .pickle or NumPy arrays, maybe compressed (.gz)

.. warning:: Picking an image format with 16-bit support doesn't mean that the images will indeed be saved with the 16-bit gray levels.
  Not all Python image-processing libraries support saving and/or opening 16-bit images.
  For ones that do, in most cases, you will need to specify 16-bit mode explicitly.

  Moreover, you want to double-check that saved images are actually saved in 16-bits.
  Please note, that checking the min and max pixel values of the image is not enough.
  Do check the actual number of grey levels!
  For example, take a look at the pixel intensity histogram.

Image formats
-------------

*1 minute to read*

As already mentioned, the mammography images are saved in 16-bits. In 8-bit images some information is already lost.
Also, these images are already somehow preprocessed, and you can never be sure how, and whether it was done correctly or not.
So, the general recommendation would be to avoid image-typed datasets.
But if you want to use them, please refer to the :ref:`General Purpose X-ray -> Image formats <xray_image_formats>`
section for the details on how to preprocess such images.

How to split the data
---------------------

*7 minutes to read*

The general recommendations are :ref:`the same as for the General Purpose X-ray images <xray_split>`.
Note that for mammography studies there can be several series for each patient (a mammography image also has a
`PatientID <https://dicom.innolitics.com/ciods/digital-mammography-x-ray-image/patient/00100020>`_ tag).

So, to split a DICOM dataset you need to read all files' headers,
aggregate the slices into series, group them by patients, and then make a split by patients.

If you are using a public dataset, you can use the annotations provided for that,
but it's always a good idea to have a look at the DICOM metadata.

Exercise
--------

*3 days of work*

**Task**: Train a multi-label classification model on
`The Chinese Mammography Database (CMMD) <https://wiki.cancerimagingarchive.net/pages/viewpage.action?pageId=70230508>`_.

- Try to use different preprocessing techniques, and enable/disable some of the preprocessing steps.

- What steps are crucial and what is not helpful at all? Why?

- Make your model suspicious/balanced/confident about its predictions by varying the thresholds for the predictions.

- [optional] Draw GradCAMs as a visualization of the found pathology.



