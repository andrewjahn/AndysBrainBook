.. _AppendixD_EffectSizes:

==================================
Appendix D: Reporting Effect Sizes
==================================

------------------

Overview
********

Once you have finished analyzing your dataset with AFNI, you may wonder how you should report the results. Within the field of cognitive neuroscience, it is common to report the t-statistic for each effect or each contrast, and to show the results as a t-statistic map overlaid on a template brain. Furthermore, within the tables of a paper, many researchers will report the peak t-statistic of a given cluster.

While this may seem like a reasonable approach - after all, most readers want to know whether a given result is due to chance, and the author has to decide on how to compress the data into some kind of easily understood result - it can also obscure the full picture, and give a distorted view of the practical significance of the result. For example, suppose that a pharmaceutical company advertises a drug that lowers cholesterol. If that was all the information we had, we might assume that the drug has a significant effect, and is worth paying for. If, however, on closer inspection, it is revealed that the drug leads to a drop of just 0.01% in the person's cholesterol, we may reconsider whether the drug is worth taking - and indeed, whether the drug in fact "works" at all. The discriminating buyer would also weigh this against the cost of the drug and its side effects; a drug that lowers cholesterol by 5% and is relatively cheap without any side effects would probably be worth taking, while a drug that lowers cholesterol by 10%, but costs hundreds of dollars per pill and comes with a host of negative side effects, would probably not be worth taking.

Similarly, we should ask ourselves the same question about the effects we observe in the neuroimaging literature: Does this effect exist? Usually some kind of statistic, such as a t-statistic and its associated p-value, allows us to answer that question with some kind of certainty. However, we also should remember that the commonly used threshold of p=0.05 is arbitrary, and that small variations around this threshold can lead to one effect being judged significant, and therefore real, while the other is dismissed as non-significant, and therefore that the effect does not exist.

Limitations of Reporting Statistics
-----------------------------------

This kind of binary significant/non-significant approach can lead to potentially interesting effects being hidden, as we saw in the previous appendix on :ref:`Highlighting vs. Hiding <AppendixC_HighlightingResults>`. In addition, reporting just the statistic tells you nothing about the magnitude of an effect: Using the cholesterol example above, we could have several clinical trials that all find an extremely consistent decrease of 0.01%, which would lead to an extremely significant p-value - let's say, p<0.00001. If this statistic was all the reader saw, however, they would have no idea about the miniscule effect size, which is what they are actually interested in.

It follows that p-values and t-statistics on their own are difficult to compare with each other; frequently, any comparisons are meaningless, since the values themselves are dimensionless. If you were to find that one effect gives you a p-value of 0.0001, for example, this doesn't tell you anything about the size of another researcher's effect which yielded a p-value of 0.049. If you are both using the nominal p=0.05 threshold, you will have both rejected the null hypothesis, and decided that an effect exists; yet, you may have found a consistent yet extremely small effect, while the other researcher may have found a gigantic effect, albeit with more variability.

Instead, we can report a much richer scope of the data by also including the effect size. With fMRI data, the values we gather from the scanner are arbitrary, and can vary from subject to subject, as well as from scanner to scanner.

.. note::

  As mentioned in the Chen et al. (2017) paper, the average BOLD signal is typically within a similar range across scanners of the same field strength, which for most research institutions is 3-Tesla. However, higher field strengths, such as 7-Teslas, and different scan parameters, such as TE, can lead to much higher average BOLD signal, and therefore bias the effect size. It is good practice to report your field strength and all of your relevant scan parameters, which may permit a conversion of the BOLD effect size to comparable values across scanners.
