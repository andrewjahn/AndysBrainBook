.. _ML_00_Introduction:

==========================================================
Machine Learning: Introduction to Basic Terms and Concepts
==========================================================

---------------

What is Machine Learning?
*************************

Machine Learning is a method of using data to train a classifier; this is called **training data**. The classifier is then provided with new data (also known as **testing data**), and it attempts to distinguish between different classes within the data based on the training data. The classifier's performance is judged by its accuracy - how many of the testing data points it managed to correctly classify.

The training data has one or more **features** that are used to train the classifier. These features can be any characteristic; for example, height and hair length. You have probably met several thousand people in your life, and you've seen many thousands more in movies, pictures, and magazines. Over time, you've learned that in general males tend to be taller than females and have shorter hair, which implies that women tend to be shorter and have longer hair. There are of course exceptions: Some males are quite short and have long hair (such as Sam Kinison), while some females are taller than the average male and have short hair (for example, Tilda Swinton). All of these experiences can be thought of as "training data" which you've observed during your life.

.. figure:: 00_Kinison_Swinton.png

  Example of a short man with long hair (Sam Kinison, left) and a tall woman with short hair (Tilda Swinton, right).

Now imagine that I tell you there's a person standing outside the door, and that their height is six feet four inches, and that their hair length is three inches. (The average height for men in the United States, by the way, is five feet, nine inches, with an average hair length somewhere between two and six inches.) What would be your best guess about whether it is a male or female? In this case, given the average height and hair distributions for males and females, it would very likely be a male.

If I now say that there's another person behind the door, and that they are five feet and seven inches tall with a hair length of eight inches, what would you guess? This is a more difficult classification, since both features tend to be somewhere around the middle range for both males and females. We would expect you - the "classifier" in this example - to be less accurate for these "testing data", and more accurate for the testing data that tend towards one end of the distribution.

Machine Learning in Neuroimaging
********************************

During the 1990s, fMRI studies focused on activation - which region of the brain responded to particular stimuli. fMRI was a new method, and researchers were able to use it to non-invasively map which regions of the brain responded to touch, pictures, noises, and other basic stimuli. These experiments measured the *amplitude* of the BOLD response to each of the different stimuli, and then compared the amplitudes between conditions to see which one elicited greater brain activity. 

This method, being straightforward and easy to use, led to several publications demonstrating the functional role of different areas of the brain in response to sound, touch, pictures, and other stimuli. In this vein, Nancy Kanwisher (1997) showed that part of the ventral temporal cortex responded with higher signal to faces; and, furthermore, selectively to faces, as opposed to pictures of houses or other body parts, such as hands.

Such was the progress of fMRI studies in the 1990s. A new direction came in 2001, when James Haxby and his colleagues ran an experiment that instead focused on *patterns* of activity instead of the amplitude itself. If we take a 2x2 grid of squares and assign a number in each square representing the BOLD response, certain stimuli may elicit a specific pattern that can be detected by a classifier. If that pattern is consistent and unique, we will be able to distinguish that pattern from a pattern elicited by another stimulus.

Haxby calculated the correlation of the beta maps within each category and between the categories for each stimulus type, instead of the machine classifier that we used in the previous video; regardless, the idea of matching patterns of activity is the same. He found that the correlations within categories were higher than between categories, and that these correlations appeared to be higher for faces and houses relative to the other categories.

