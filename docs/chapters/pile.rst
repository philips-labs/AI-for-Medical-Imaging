.. _chap-pile:

(WIP) A Pile of thoughts
========================

Data sources:

- X-ray

- CT (+ contrast)

- MRI (+ contrast)

- fMRI

- US

- PET

- Microscopy (?)

For each source:

- How images are obtained (physics behind it, raw data format etc.)

- How the images look like

- How the images are used (medical value)

- How the images are stored (DICOM/NIfTI/???)

- Typical image preprocessing schemes

- How to split the data

- Link to the data to play with

What do we do?
--------------

https://machinelearningmastery.com/types-of-learning-in-machine-learning/

We employ (verify that list with Irina):

- Learning Problems

  - Supervised Learning

  - Unsupervised Learning

  - Reinforcement Learning

- Hybrid Learning Problems

  - Semi-Supervised Learning

  - Self-Supervised Learning

  - Multi-Instance Learning

- Statistical Inference

  - Inductive Learning

  - Deductive Inference

  - Transductive Learning

- Learning Techniques

  - Multi-Task Learning

  - Active Learning

  - Online Learning

  - Transfer Learning

  - Ensemble Learning

Our projects
------------

PILRUS->General->Files->Slides

Group by tasks with examples

Typical tasks & approaches
--------------------------

- classification

- localization

- Segmentation

  - Unet

- Noisy/weak labels

  - in classification

  - in segmentation

- image-to-image tasks

  - metrics

  - artifact removal

  - denoising

  - movement correction

  - super-resolution

- synthetic data generation, GANs

- (\*) image registration, fusion


Medical imaging
---------------


- Modalities, clinical indications

- Basics anatomy, terminology (sagittal, axial, coronal, etc)

- Book: M. A. Flower "Webb’s Physics of Medical Imaging", A TAYLOR & FRANCIS BOOK, ISBN 978-1-4665-6895-2

X-ray
-----

- Clinical overview: typical indications, body parts, diagnosis

- Preprocessing: normalization, clipping, DICOM-based vs png/jpeg-based datasets

- Practice: Kaggle / other challenges

  - TODO

CT
--

- Clinical overview: typical indications, body parts, diagnosis

  - Contrast-enhanced imaging

- Signal acquisition: basics of CT, Hounsfield units

  - Tomography, Projection, sinogram, Radon transform, back projection

  - TODO Coursera course link

- CT image preprocessing: clipping, window levels, normalization, resampling

- Artefacts

- Practice: Kaggle / other challenges

  - TODO

MRI
---

- Clinical overview: typical indications, body parts, diagnosis

  - Contrast-enhanced imaging

- Signal acquisition: basics of MR physics

- Different pulse sequences and typical representation

- MRI image preprocessing: B0-correction, normalization, resampling, etc

- artefacts

- Brain MRI

  - MNI template

- Cardiac MR

- Practice: Kaggle / other challenges

  - TODO

Courses
-------
Medical imaging:

CT:

https://www.coursera.org/learn/cinemaxe#syllabus

MRI:

https://www.coursera.org/learn/mri-fundamentals

https://www.coursera.org/learn/functional-mri-2

Practice:

Skoltech course in biomedical image analytics by Prof. Dmitry V. Dylov:

https://github.com/cviaai/BIA-course

2D:

https://www.coursera.org/projects/covid-19-detection-x-ray

2D & 3D:

https://www.coursera.org/learn/ai-for-medical-diagnosis?specialization=ai-for-medicine#syllabus

Complete courses:

Machine Learning for Healthcare Analytics Projects

https://www.coursera.org/learn/evaluations-ai-applications-healthcare#syllabus

https://www.udacity.com/course/ai-for-healthcare-nanodegree--nd320
(`syllabus <https://d20vrrgs8k4bvw.cloudfront.net/documents/en-US/AI+for+Healthcare+Nanodegree+Program+Syllabus.pdf>`_)

https://startdatajourney.com/ru/course/medical-image-analysis-in-python#

Review papers on AI for radiology:

https://www.semanticscholar.org/paper/Artificial-intelligence-in-radiology-Hosny-Parmar/b9ede5f604668d0b62a306392cd03f47086e245e

https://www.semanticscholar.org/paper/Demystification-of-AI-driven-medical-image-past%2C-Savadjiev-Chong/3cb2bd7e165934b1e6c13e2e9efc6ac7fb6ac40a

https://www.semanticscholar.org/paper/Design-Characteristics-of-Studies-Reporting-the-of-Kim-Jang/4693db07bd0fc78f5f862f29711f575652be670c

https://www.semanticscholar.org/paper/Artificial-intelligence-in-radiology%3A-friend-or-foe-Pakdemirli/fe16c888c096204e0b7c5411ab5cf17b9fdc84f9

https://www.semanticscholar.org/paper/A-survey-on-deep-learning-in-medical-image-analysis-Litjens-Kooi/2abde28f75a9135c8ed7c50ea16b7b9e49da0c09
