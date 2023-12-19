.. _chap-medical-imaging:

Medical Imaging
===============

*63 minutes to read*

In the majority of medical institutions, all images are stored in the so-called **PACS**.
PACS (Picture Archiving and Communication System) is a technology used to store and transmit digital images and medical
reports securely and reliably.
The system overview: `Picture Archiving and Communication System <https://blog.peekmed.com/pacs-systems/>`_.

To speak with the doctors in the same language it's *highly recommended* to get familiar
with the **basics of anatomy** and corresponding **terminology**:

- `General anatomy and radiographic positioning terminology
  <https://radiologykey.com/general-anatomy-and-radiographic-positioning-terminology/>`_

- `Anatomical position <https://radiopaedia.org/articles/anatomical-position?lang=us>`_

DICOM
-----

*13 minutes to read*

Most of the time, you can find medical images stored in the **DICOM** files.
This format is useful because it allows medical institutions to store (and transmit using DICOM standard)
sensitive patient information and images in all diversity of the modalities and cases.

- `About the DICOM standard <https://www.dicomstandard.org/about-home>`_
- Several links describing the DICOM file structure:

  `A Very Basic DICOM Introduction <https://dcm4che.atlassian.net/wiki/spaces/d2/pages/1835038/A+Very+Basic+DICOM+Introduction>`_

  `Understanding DICOM <https://towardsdatascience.com/understanding-dicom-bce665e62b72>`_

  More detailed file structure.
  `Overview: Basic DICOM File Structure <https://www.leadtools.com/help/sdk/v21/dicom/api/overview-basic-dicom-file-structure.html>`_

- As finding required information in the original DICOM standard documentation is not very easy, one can use this handy
  `DICOM Standard Browser <https://dicom.innolitics.com/ciods>`_

- A commonly used Python package to work with DICOM files is `Pydicom <https://pypi.org/project/pydicom/>`_

.. _mi_nifti:

NIfTI
-----

*9 minutes to read*

Another popular format to store medical data is **NIfTI** files.
It is mainly used to store already anonymized and preprocessed volumetric data extracted from the studies.

- `A quick introduction to the NIfTI-1.1 Data Format <https://nifti.nimh.nih.gov/nifti-1/documentation/hbm_nifti_2004.pdf>`_

- All the information is well-structured on the NIfTI format site (follow the links inside):
  `About NIfTI format <https://nifti.nimh.nih.gov/>`_

- A commonly used Python package to work with NIfTI files is `NiBabel <https://nipy.org/nibabel/gettingstarted.html>`_

How the datasets are created
----------------------------

*35 minutes to read*

When a medical institution prepares the dataset, it follows a certain procedure, described in the paper
`Preparing Medical Imaging Data for Machine Learning <https://www.ncbi.nlm.nih.gov/pmc/articles/PMC7104701/>`_.

.. _mi_exercises:

Exercises
---------

*2 days of work*

Do Exploratory Data Analysis (EDA) for the two datasets:

1. `DICOM dataset <https://www.kaggle.com/c/siim-acr-pneumothorax-segmentation/data>`_

  - Use `dcmread <https://pydicom.github.io/pydicom/dev/reference/generated/pydicom.filereader.dcmread.html>`_
    function to read the files

  - Use `get() <https://pydicom.github.io/pydicom/dev/reference/generated/pydicom.dataset.Dataset.html#pydicom.dataset.Dataset.get>`_
    method or tag name as a field to access the values of the tags

  - Use `pixel_array <https://pydicom.github.io/pydicom/dev/reference/generated/pydicom.dataset.Dataset.html#pydicom.dataset.Dataset.pixel_array>`_
    field to access the image as a NumPy array

  - Check out `Dataset basics <https://pydicom.github.io/pydicom/stable/tutorials/dataset_basics.html>`_ and
    `Viewing images <https://pydicom.github.io/pydicom/stable/old/viewing_images.html>`_ tutorials

2. `NIfTI dataset <https://www.kaggle.com/andrewmvd/liver-tumor-segmentation>`_

  - Check out `Getting started <https://nipy.org/nibabel/gettingstarted.html>`_ and
    `Images and memory <https://nipy.org/nibabel/images_and_memory.html>`_ tutorials

Optional read
-------------

If you are already familiar with the general principles of obtaining images from different medical imaging apparatus,
you can refresh your knowledge using the two slide decks:

- `Medical Imaging Modalities: An Introduction <https://home.zhaw.ch/~scst/Biomedtec-Dateien/MedIma_Intro.pdf>`_
- `Visual Medicine: Visual Medicine: Data Acquisition and Preprocessing
  <https://studylib.net/doc/18646295/visual-medicine--data-acquisition-and-preprocessin>`_

A book that will help you to deepen your knowledge about medical imaging:
M. A. Flower "Webb’s Physics of Medical Imaging", A TAYLOR & FRANCIS BOOK, ISBN 978-1-4665-6895-2
