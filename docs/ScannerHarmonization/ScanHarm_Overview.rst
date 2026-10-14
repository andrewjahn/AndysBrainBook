.. _ScanHarm_Overview:

=======================
Data harmonization for neuroscientific research: Theory, challenges, and applications.
=======================


Overview
--------

This course provides a comprehensive, hands-on journey from understanding the fundamental challenges of site effects in neuroimaging data to implementing state-of-the-art harmonization techniques.

This course is based on the **OHBM 2026 Educational Course on "Data harmonization for neuroscientific research: Theory, challenges, and applications,"** originally presented at the OHBM 2026 annual meeting. 
Developed by `Nicolás Nieto <https://github.com/N-Nieto>`_ [✉️](n.nieto@fz-juelich.de), `Johanna Bayer <https://github.com/likeajumprope>`_, `Gaurav Bhalerao <https://github.com/gvbhalerao591>`_, `Emma Prevot <https://github.com/emmaprevot>`_, and `Jacob Turnbull <https://github.com/Jake-Turnbull>`_, 
this material provides comprehensive hands-on instruction for understanding and implementing harmonization techniques in neuroimaging. For the complete course materials and code, please visit the `original repository <https://github.com/N-Nieto/OHBM2026_Educational_course_harmonization/tree/main>`_.

Learning Objectives
-------------------

By the end of this course, you will be able to:

- **Diagnose** site effects and batch artifacts in multi-site neuroimaging datasets
- **Implement** location-scale harmonization methods (ComBat, ComBat-GAM, CovBat)
- **Evaluate** harmonization quality using appropriate metrics and validation strategies
- **Handle** longitudinal and test-retest data with specialized techniques (Long-Combat, BART)
- **Integrate** harmonization into machine learning pipelines and statistical analyses
- **Apply** advanced alternatives including deep learning and normative modeling approaches

Course Structure
-------------------
The course is divided into four main blocks, each focusing on a specific aspect of harmonization:  

- **Block 1 (B1): Effects of Site Introduction** 

The first step in any harmonization journey is to see the problem.
In this block, we open with real images and tangible examples to build an intuitive understanding of what site effects actually look like in practice.

We will ask—and answer—the questions that motivate everything that follows:

- What exactly is an Effect of Site, and where does it come from?
- If we scan the same brain on two different scanners, can we see the difference with the naked eye?
- How do these scanner-driven differences alter the distributions and statistical properties of the data?
- And, most importantly: do we truly need harmonization, or can we simply ignore the problem?

By the end of this block, you will have a concrete sense of why site effects matter and why harmonization is not merely a preprocessing nicety, but a scientific necessity.


- **Block 2 (B2): Harmonization Evaluation**

Once you know site effects exist, the next task is to measure them. In this block we shift from intuition to quantification, learning how to diagnose the severity of site-related bias in a dataset and how to judge whether a harmonization method has actually worked.

We will tackle questions such as:

- Is it possible to put a number on the effect of site?
- How deeply is my data contaminated by scanner-driven variance?
- Is there a single metric that tells me everything I need to know, or do we need a panel of complementary checks?

You will learn to think like a diagnostician: assessing damage before treatment, and validating improvement after it.


- **Block 3 (B3): Location-Scale Methods: ComBat Applications**

ComBat is the workhorse of neuroimaging harmonization, and this block is its deep-dive manual. We will move from the underlying mathematics to the practical assumptions, from celebrated successes to well-documented limitations
Because ComBat has inspired an entire family of location-scale methods, we will also survey its most important descendants—ComBat-GAM, CovBat, and others—and examine how each addresses a specific shortcoming of the original.
Throughout this block, we will keep a special eye on the machine-learning context, where harmonization must coexist with cross-validation and generalization.

Key questions include:

- What is the mathematical logic behind ComBat?
- What assumptions does it make about the data, and what happens when those assumptions are violated?
- Can ComBat be applied universally, or are there situations where it should be avoided?
- How does class imbalance across sites distort ComBat's behavior in statistical analysis?
- How can ComBat be integrated safely into a machine-learning pipeline without leaking information?

By the end of this block, you will be able to wield ComBat—and its relatives—with both confidence and caution.

- **Block 4 (B4): Alternatives & Future Directions**

ComBat is not the end of the story. In this final block, we look beyond location-scale methods to emerging techniques that treat site effects in fundamentally different ways.
We will explore whether every image from the same scanner truly shares an identical bias, and we will introduce powerful alternatives that challenge the one-size-fits-all assumption.
From image-quality-metric harmonization and direct image synthesis to normative modeling and deep-learning approaches, this block maps the frontier of the field and points toward the tools you may be using tomorrow.

Key questions include:

Does all the images from the same site has the same effect of site?


.. toctree::
   :maxdepth: 1
   :caption: Start to Finish Harmonisation material

   course_material/B1_N1_Intro_To_Eos
   course_material/B1_N2_Simulated_example_of_Eos
   course_material/B2_N1_HarmonisationEvaluation
   course_material/B2_N2_SiteRegression_forHarmonisation
   course_material/B2_N3_Regression_versus_ComBat_harmonisation
   course_material/B3_N0_ComBat_applications
   course_material/B3_N1_ComBat_limitations
   course_material/B3_N2_EoS_in_ML
   course_material/B3_N3_Metrics_by_Site
   course_material/B3_N4_ComBat_in_Imbalance_Classes
   course_material/B4_N1_IQM-Harmonisation_Light
   course_material/B4_N2_HarmoniseImages
   course_material/B4_N3_Normative_Modelling
   

Acknowledgement
-------------------

This course was developed as one of the outcomes of OxCIN’s (Oxford University Centre for Integrative Neuroimaging) collaborative work through the Harmonisation Working Group. It was designed by members of the group, including both internal and external collaborators, and was presented at OHBM 2026 annual meeting. We are grateful to the other members of the Harmonisation Working Group for their regular contributions to discussions and for helping shape the broader collaborative context of this work. Additionally, this work was supported by the Rosetrees Trust and the NIHR Oxford Health Biomedical Research Centre (NIHR203316). The views expressed are those of the author(s) and not necessarily those of the NIHR or the Department of Health and Social Care. The Centre for Integrative Neuroimaging was supported by core funding from the Wellcome Trust (203139/Z/16/Z and 203139/A/16/Z).
This course was also partially supported by the EBRAIN-Health Research Infrastructures Grant (101058516), the Institute of Neurosciences and Medicine – Brain and Behaviour (INM-7) at Forschungszentrum Jülich (FZJ), and Heinrich Heine University Düsseldorf (HHU).