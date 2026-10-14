Applying ComBat on simulated data:
==================================

Combat assumes data is of the form:

.. math::

    y_{i,j} = \alpha_0 + X_i\beta_j + \gamma_{b(i)j} + \delta_{b(i)j}\epsilon_{ij}

Where:

.. math:: 
    y_{i,j} \text{ is the derived features (i.e. the volume of the hippocampus)}
    \alpha_0 \text{ is the mean of the given feature}
    X_i \text{ is a design matrix of covariates}
    \beta_j \text{ are the estimated covariate effects (usually from taking the psuedoinverse of the design with the dataset)}
    \gamma_{b(i)j} \text{ is the additive (location) effect}
    \delta_{b(i)j} \text{ is the multiplicative (scaling) effect}
    \epsilon_{ij} \text{ is the subject specific term, assumed to capture subject specific effects not described by covariates as well as measurement noise}

Here, we will simulate some data as we did in section 1 and apply ComBat
to it in order to see how it works and how well it does.

We can play around with each of the batch (sites) characteristics to see
how confounding between age, sex or other variables may effect
harmonisation ad estimation of the batch effect.


We work with the following variables:

-  data -> :math:`y_{ij}`
-  covariates -> :math:`X_i`, the subjects :math:`\times` covariate
   matrix
-  betas -> :math:`\beta_{j}`, the true covariate effects
-  add_mean -> :math:`\gamma_{jb(i)}`, the mean additive effect on the
   data from a given batch
-  multi_mean -> :math:`\delta_{jb(i)}`, the mean multiplicative effect
   on the data from a given batch
-  noise -> :math:`\epsilon_{ij}`, the subject specific
   measurement/noise term

Following the form of the ComBat equation, we then linerally combine
these variables get the data.

ComBat requires at least two columns of data and a vectors containing
the batch labels in order to run. It is also advised to include any
biologically relevant variables

You might want to create two dataframes here with the same covariate
effects but with and without a batch effect for easier comparisson

.. code:: ipython3
    
    df = simulate_batched_data(
        data=data,
        batch=batch,
        covariate_specs=covariate_specs,
        betas=betas,
        noise_sd=1.0,
        seed=123,
    )

.. image:: images/B3_N0_ComBat_applications_3_0.png

.. image:: images/B3_N0_ComBat_applications_3_1.png


.. code:: ipython3

    # Adding batch effects 
    
    batch_params = {
        "Batch1": {"add_mean": 0, "add_sd": 0.05, "multi_mean": 0.5, "multi_shape": 25.0},
        "Batch2": {"add_mean": 3, "add_sd": 0.08, "multi_mean": 1.9, "multi_shape": 18.0},
        "Batch3": {"add_mean": -9, "add_sd": 0.06, "multi_mean": 0.7, "multi_shape": 30.0},
    }


Now we will apply ComBat to the data:
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Several versions of ComBat are available freely online in Python, R and
MATLAB.

The original version can be found here
[https://github.com/Jfortin1/ComBatHarmonization] and any publication of
work using ComBat should always cite the original implementations for
neuroscientific applications, but also the first implemntation which was
introduced by Johnson et al., in the field of genomics.

The version we will be using today is adapted directly from this
original code.

Some other available versions are as part of the neuroHarmonize Python
package [https://github.com/rpomponio/neuroHarmonize]

As part of the DiagnoseHarmonise package
[https://pypi.org/project/DiagnoseHarmonisation/]

And as part of the uniharmony package
[https://pypi.org/project/uniharmony/]

Or as standalone implementations such as the version by Chen et al.,
which included harmonisation of covariates using principle component
analysis (PCA) [https://github.com/andy1764/CovBat_Harmonization]

For this practical, we will be using a modified version from the
DiagnoseHarmonisation package as it returns by defaults the estimated
betas and the correction terms for each batch so that we can compare
them with the true simulated values.

This version has been installed locally in the block03_utils:

.. code:: ipython3
    # ComBat needs more than one column in order to run so we will create some additional columns that are just random noise for the purpose of demonstrating ComBat. 
    # In a real dataset you would have multiple features that you want to harmonise, but for this example we will just create some random noise columns.
    
    
    sim = simulate_batched_data2(
        data=data,
        batch=batch,
        n_cols=10,
        covariate_specs=covariate_specs,
        betas=betas,
        batch_params=batch_params,
        seed=123,
    )
    
    df = sim["df"]
    y = sim["y"]          # shape (n, 3)
    # Print shape and make sure that data and harmonised data match:
    data = np.asarray(y, dtype=float)
    
    data = np.asarray(data, dtype=float)
    
    print(data.shape)   # quick check
    print(data.ndim)    # 1 for vector, 2 for matrix
    
    covariate_names = ["age", "sex", "height", "weight"]
    X = sim["df"][covariate_names].to_numpy()
    print(X)
    print(X.shape)


.. parsed-literal::

    (900, 10)
    2
    [[ 15.1087865    1.         174.85075733  67.23574406]
     [ 21.32213349   1.         165.32265572  70.19857413]
     [ 37.87925261   1.         149.30053152  62.69556106]
     ...
     [ 71.68757171   0.         164.65047171  72.37865308]
     [ 50.66636748   0.         167.53891688  70.25089182]
     [ 41.48997195   0.         164.95502322  44.3826329 ]]
    (900, 4)


.. code:: ipython3

    # Run the ComBat harmonisation on the data, this version of ComBat takes numpy arrays as the the input so we need to convert the dataframes to numpy arrays first. We also need to specify the batch column and the covariates that we want to include in the model.
    
    ReturnPriors = True # whether to return the priors or not, if True then the output will be a dictionary with the harmonised data and the priors, if False then the output will just be the harmonised data.
    combat_data = combat(
        data=data, # array of data to be harmonised, shape (n_samples, n_features)
        batch=batch, # array of batch labels, shape (n_samples,)
        mod=X, # array of covariates to include in the model, shape (n_samples, n_covariates)
        parametric=True, # whether to use parametric or non-parametric adjustments (we do not offer non-paranetric and generally do not advise it)
        return_priors=ReturnPriors,
        # RegressCovariates=True,
    )
    
    def print_inventory(dct):
        print("Items held:")
        for item, amount in dct.items():  # dct.iteritems() in Python 2
            print("{} ({})".format(item, amount))
    
    # Print the size of the original data and the combat data to show that they are the same shape
    
    if ReturnPriors == False:
        print("Original data shape:", data.shape)
        print("ComBat data shape:", combat_data.shape)
        bayesdata = combat_data
    elif ReturnPriors == True:
        print("Original data shape:", data.shape)
        bayesdata = combat_data["bayesdata"]
    else:
        print("Change return priors to either True or False")
    



.. parsed-literal::

    Reference batch not given, defaulting to no reference
    Empirical Bayes set to true
    [combat] Found 3 batches
    [combat] Adjusting for 4 covariate(s) of covariate level(s)
    [combat] Standardizing Data across features
    [combat] Fitting L/S model and finding priors
    Size of gamma hat: (3, 10)
    Size of delta hat: (3, 10)
    [combat] Finding parametric adjustments
    Size of gamma_star: (3, 10)
    Original data shape: (900, 10)


Compare the age nomograms and some other data characteristics before and after harmonisation:
---------------------------------------------------------------------------------------------

We can compare the data before and after running ComBat.

We will do this three ways:

-  Looking at batch histograms
-  Looking at principle component clusters
-  Examining the mean difference between batches
-  Examining the variance difference between batches

.. code:: ipython3

    # Batch histograms:
    
    plot_combat_before_after(
        y_before=data,
        y_after=bayesdata,
        batch=batch,
        feature_names=None,
        feature_idx=1,   # change this to the feature you care about
        bins=50,
    )

.. image:: images/B3_N0_ComBat_applications_9_0.png

.. image:: images/B3_N0_ComBat_applications_9_1.png


Other visualisations:
---------------------

We can look at other metrics to assess harmonisation accuracy, for
example clustering when projecting the data using principal component
analysis:

Here, distinct batch clusters by batch can be a sign of residual batch
effects.

.. code:: ipython3
    result = plot_pca_before_after(
        y_before=data,
        y_after=bayesdata,
        batch=batch,
        feature_names=None,
        standardize=True,
        show_arrows=True,
    )
    
    plt.show()



.. image:: images/B3_N0_ComBat_applications_11_0.png


Notice that there may still be distinct batch clusters before and after
applying ComBat. However, this doesn’t necessarily mean there is still
residual batch effect.

Try making the covariate distributions more similar between batches and
see if this clustering remains. Another approach is to use regression to
remove these effects. This can be done internally within the ComBat
implementation we provide as seen here:

::

   combat_data = combat(
       data=data, 
       batch=batch, 
       mod=X, 
       parametric=True, 
       return_priors=ReturnPriors,
       RegressCovariates=True,

   )

As an extension, have a look at the other data contained in the ComBat
output when return priors is set to true. This dataframe will then
contain all of the parameters that were used in the estimation of the
final ComBat corrections. You can also compare them directly with the
‘true’ values from simulation to see how close they are.

The solution for this can be found in block 3’s solution tab. Have a
think about what you see. Why might this differ a bit from what you
expect?
