.. _Stats_01_History:

=========================================
Chapter #1: A Brief History of Statistics
=========================================

----------------

Random Sampling and the Trial of the Pyx
****************************************

In the years after the Norman Conquest, Henry III directed the London Mint (now called the Royal Mint) to create standards for the currency of the realm. Size, weight, and composition of the coins would need to be closely examined to combat counterfeits and ensure the quality of the money changing hands. But with millions of coins being struck each year, how could this be done?

The king's ministers drew up the following plan: Each day a sample of coins would be deposited into a small box, also known as a pyx, and evaluated by a panel of independent judges. All of the criteria above - size, weight, composition, roundedness, and so on - would be examined by the judges using a sample of coins from the pyx. The contract drawn up between the Mint and the king specified that a certain tolerance would be allowed in the deviation from the standards; in other words, a given coin did not need to conform *exactly* to the Ideal Coin, but it had to be close.

.. note::

  The Trial of the Pyx is still done today every year in Britain, although it has become considerably more complex. The Trial is presided over by an actual judge, and the Deputy Master of the Royal Mint is put on trial as the responsible party for the quality of the coins. A video overview of the Trial and its history can be found `here <https://www.youtube.com/watch?v=UZQfA2cRHJs>`__.
  
This process, motivated by practicality, illustrates two fundamental concepts of statistics: **Sampling** and **Error**. The judges, without having the name for it, took a **random sample** of the coins; although more sophisticated methods of random sampling would be developed over the centuries, the idea was that any coin's chance of being selected was as good as that of any other coin. The coin would be a representative for the other coins in the pyx, similar to how elected politicians and diplomats represent groups of people, and its quality could be taken as representative of the whole. In modern terms, the sample was representative of the **population** that it was drawn from.

The second concept, error, was reflected in their willingness to allow any given coin to deviate a certain amount from the ideal standards. Not every coin individually would be perfect, but taken together, they would be close enough to the standard for practical purposes. In this way the judges could do their work in a reasonable amount of time, and the integrity of the currency would be maintained - every Englishman could be confident that he held in his pocket a genuine coin and not a forgery.


Tobias Mayer and Averaging Equations
************************************

The first systematic treatment of error can be found in the writings of Johann Tobias Mayer, a well-known astronomer in the mid-18th century. One of the most prominent problems in astronomy at the time was the movement of the moon. Although the moon was known to move predictably from month to month, its smaller movements (such as its rotation and precession, also known as librations) were much more complex. A more detailed understanding of the librations would be important both commercially and militarily, as precise localization of the moon would allow ships to accuractely calculate their longitude.

Until the time of Mayer, multiple observations of natural phenomena (such as the moon) were common. What was uncommon was the *combination* of multiple observations; unless the observations were made under what were essentially the same conditions, combining the observations was unthinkable. Any combination, the reasoning went, would contaminate any individual observation, and would end up being worthless.

By a brilliant insight, Mayer discarded this idea and replaced it with a new one: Multiple observations, regardless of the differences in the conditions at the time, could increasingly converge on the truth of the phenomenon being studied. In his logbooks, Mayer had twenty-seven equations that described the librations of the moon. He divided these into three groups of nine equations each based on similarities in the magnitude of the coefficients (for example, group I would have very positive coefficients, group II relatively low coefficients, and group III negative coefficients). He then solved each of the equations separately.

His summary of the approach stated that the accuracy of the calculated values were proportionaly to the number of observations that went into them:

  "Because these last values were derived from nine times as many observations, one can therefore conclude that they are nine times more correct; therefore the error in each of the constants is in inverse relationship to the number of their observations." (Mayer, 1750, p. 155; quoted in Stilger, 1986).
