.. _Appendix_H_DoubleDissociations:

================================
Appendix H: Double Dissociations
================================

------------------

Overview
********

In the :ref:`previous appendix on reverse inference <Appendix_G_ReverseInference>`, we discussed the inference of results by observing a result, and then making a conclusion about what led to that result. This is especially common in exploratory studies which are not guided by any hypotheses, and which consequently are more susceptible to faulty reverse inferences - that is, favoring one particular cause due to one's bias, even though there are other potential causes that could also be valid. An outside observer, who was not involved in the design of the experiment, is also more likely to be persuaded by faulty reverse inferences if they have no idea how the experiment was constructed.

In this chapter, we focus on a paper by Richard Henson (2006) which discusses **forward inference**; that is, we take the perspective of someone who is designing a study to use a specific cause to elicit a specific effect, and to rule out all other possible alternatives. This approach does involve assumptions, such as **pure insertion**, which assumes that including one condition does not affect the other conditions. In reality, it is probably more complicated than that, with possible interaction effects between the conditions; for example, the inclusion of a working memory task in addition to a Flanker task might lead to nonlinear increases in the overall cognitive effort during a scannign session, or unwanted spillover effects from one task to another - such as a working memory task becoming more difficult if it immediately follows a Flanker task. (Also see, for example, the Gratton effect.)

The crux of the Henson paper is Box 2 and Figure I, in which Henson define a **qualitative difference**. Simply finding a difference in the BOLD response between two conditions - say, Condition A > Condition B, in Region R1 - does not in itself suggest that this region is selective for Condition A, and not for Condition B; a stronger test would be to compare them directly using a paired-samples t-test. And to further bolster your claim, you should select a contrasting or a control region (let's call it R2) and run the same tests. If you get the same pattern of results, then it suggests that activiation of Condition A is general (at least in two regions), and therefore precludes any conclusions about the selectivity of that region; if, on the other hand, you find the opposite pattern in R2, this is stronger evidence of a **double dissociation** - the finding that two regions show qualitatively different patterns of activity in response to two separate conditions. (Note that this also assumes that the conditions in your design are orthongal, which can be achieved by using a complete factorial design, for example.)

Lastly, you should run a Region x Condition interaction; a significant interaction provides further evidence that the activation in each region is activated by a specific condition in your design.

Also see panel (c) of Figure I, which depicts a significant Condition x Region interaction, but does not show any significant activity for any condition in Region R2. This is a genuine but trivial interaction, which could be obtained by using a region that doesn't contain any grey matter, such as the ventricles. 
