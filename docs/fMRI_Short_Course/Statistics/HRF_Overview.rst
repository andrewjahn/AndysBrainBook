.. _HRF_Overview.rst

The Hemodynamic Response Function (HRF)
=============

-------

The BOLD Response as an Indirect Measure of Neural Firing
********

Whenever there is a stimulus - such as a flash of light, or a sudden noise - that stimulus is transduced by your sensory organs into nerve impulses, which in turn stimulate neuronal firing in the brain. Neurons that fire require oxygen, and oxygen is delivered by the blood. That oxygenated blood in turn reduces the interference with the signal from the hydrogen in the water in your body, which can lead to inferences about which brain region is “active”. 

The chain of reasoning goes like this: neural activity leads to a demand for oxygen, which in turns leads to greater oxygenated blood flow to the area with neurons that are firing, which in turn leads to greater signal in that area. Consequently, we assume that the BOLD response is an indirect measure of neural firing.

From the BOLD Response to the HRF
*********

Another assumption we make is what the BOLD response looks like. This is also very important, not only for modeling the link between neural activity and blood flow, and from there to the observed signal - a phenomenon known as neurovascular coupling - but for how we define our statistical models for what brain regions are associated with which condition. 

Empirical studies of the BOLD response demonstrated that, after a stimulus was presented, any part of the brain responsive to that stimulus - say, the visual cortex in response to a visual stimulus - will show a rise in the BOLD response, peaking around six seconds, and then falling back to baseline over the next several seconds. This can be modeled with a mathematical function called a **Gamma Distribution**. The Gamma Distribution constructed with parameters to best fit the BOLD response observed by most empirical studies is referred to as the canonical hemodynamic response function, or **HRF**.

.. figure:: HRF_Demo.gif


