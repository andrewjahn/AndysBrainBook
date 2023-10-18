.. _Appendix_H_DoubleDissociations:

================================
Appendix H: Double Dissociations
================================

------------------

Overview: Associations vs. Dissociations
****************************************

Most neuroimaging studies report that an experimental condition led to activation in a particular region of the brain. This kind of result is called an **assocation**: Given condition X, we see activation in brain region Y. This is why so many publications start with the phrase, "The neural correlates of..." followed by whichever cognitive process they were studying.

These kinds of studies can be interesting, but if only associations are reported, we are limited in what we can say about the functional architecture of the brain. Furthermore, compiling In the :ref:`previous appendix on reverse inference <Appendix_G_ReverseInference>`, we discussed the inference of results by observing an end state, and then making a conclusion about what led to that result. This is especially common in exploratory studies which are not guided by any hypotheses, and which consequently are more susceptible to faulty reverse inferences - that is, favoring one particular cause due to one's bias, even though there are other potential causes that could also be valid. An outside observer, who was not involved in the design of the experiment, is also more likely to be persuaded by faulty reverse inferences if they have no idea how the experiment was constructed.

In this chapter, we focus on a paper by Richard Henson (2006) which discusses how to use **forward inference** to infer **dissociations**; that is, we take the perspective of someone who is designing a study to use a specific cause to elicit a specific effect, and to rule out all other possible alternatives. This approach does involve assumptions, such as **pure insertion**, which assumes that including one condition does not affect the other conditions. In reality, it is probably more complicated than that, with possible interaction effects between the conditions; for example, the inclusion of a working memory task in addition to a Flanker task might lead to nonlinear increases in the overall cognitive effort during a scannign session, or unwanted spillover effects from one task to another - such as a working memory task becoming more difficult if it immediately follows a Flanker task. (Also see, for example, the Gratton effect.)

Examples of Dissociations
*************************

One of the most famous examples of a dissociation was the patient H.M., who had large sections of his hippocampus and parahippocampus removed in order to ameliorate his epileptic seizures. After the procedure, he was still able to recall long-term memories from before the surgery, but was unable to form new episodic memories afterwards - leading one to believe that the hippocampus plays a necessary role in the formation of new memories, but not for the retention of memories that have already been formed.

Lesion studies of Broca's Area and Wernicke's Area - roughly corresponding to the left inferior frontal gyrus and left posterior superior temporal gyrus - illustrate a **double dissocation**. In this case, damage to Broca's Area results in **non-fluent aphasia** - that is, the inability, or extreme difficulty, to speak or write. Damage to Wernicke's Area, on the other hand, leads to **fluent apahsia**, in which the patient is able to speak and write, but in a disordered, random manner. This has led neuroscientists to frame these areas as contributing to similar but distinct language functions - namely, producing versus understanding language - which enriches our understanding of the functional architecture of the brain.

Qualitative Differences in fMRI Data
************************************

In the absence of lesion studies, however, we can still establish dissociations using functional neuroimaging. According to Richard Henson, this is best done through measuring **qualitative differences**. In this figure taken from his paper, we see that simply finding a difference in the BOLD response between two conditions - say, Condition A > Condition B, in Region R1 - does not in itself suggest that this region is selective for Condition A, and not for Condition B; a stronger test would be to compare them directly using a paired-samples t-test. And to further bolster your claim, you should select a contrasting or a control region (let's call it R2) and run the same tests. If you get the same pattern of results, it suggests that activiation of Condition A is general (at least in two regions), and therefore precludes any conclusions about the selectivity of that region; if, on the other hand, you find the opposite pattern in R2, this is stronger evidence of a **double dissociation** - the finding that two regions show qualitatively different patterns of activity in response to two separate conditions. (Note that this also assumes that the conditions in your design are orthongal, which can be achieved by using a complete factorial design, for example.) Lastly, you should run a Region x Condition interaction; a significant interaction provides further statistical evidence that the activation in each region is activated by a specific condition in your design.

To summarize, in order to claim that a double dissociation exists, your data should meet the following criteria:

1. Within R1, there is a significant positive effect of Condition A, but not of Condition B;
2. Within R2, there is a significant positive effect of Condition B, but not of Condition A;
3. Within R1, Condition A is significantly greater than Condition B;
4. Within R2, Condition B is significantly greater than Condition A;
5. There is a significant Region x Condition interaction term.


.. note::

  Also see panel (c) of Figure I, which depicts a significant Condition x Region interaction, but does not show any significant activity for any condition in Region R2. This is a genuine but trivial interaction, which could be obtained by using a region that doesn't contain any grey matter, such as the ventricles. 
