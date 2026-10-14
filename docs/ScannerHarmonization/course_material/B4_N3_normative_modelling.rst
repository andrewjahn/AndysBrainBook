Data harmonization using Normative Modelling
============================================

Notebook by Augustein de Boer, adapted by Alice Chavanne, adapted by
Johanna Bayer. This workbook is part of this publication:

de Boer, A. A. A., Bayer, J. M. M., Fraza, C., Chavanne, A., Rehak
Buckova, B., Tsilimparis, K., Serin, E., Bernas, A., Cirstian, R.,
Zabihi, M., Rutherford, S., Al Khaledi, A., Wolfers, T., Beckmann, C., &
Marquand, A. F. (2026). Protocol update: The normative modelling
paradigm for computational psychiatry. In bioRxiv
(p. 2026.02.17.706268). bioRxiv.
https://doi.org/10.64898/2026.02.17.706268

Introduction
============

Data from different batches may have different characteristics, and in
order to make sense of the entirety of the data, those characteristics
have to be brought into agreement, which is what we call harmonization.
When a model is fitted to data from different batches with the
PCNtoolkit, a set of parameters is learned for each batch. These
parameters describe an invertible mapping from feature space to
deviation space. The deviation space is assumed to follow a standard
normal distribution for each batch, so in order to harmonize data, we
simply map all features from all batches to deviation space first, and
then map all of them back using a single set of learned parameters. The
choice for the set of parameters that is used for the inverse mapping is
arbitrary, here we choose the batch effect that occurs first
alphabetically.

Overview: This notebook will achieve two different things:
----------------------------------------------------------

A: Harmonize a data set with multiple sites within the data set (within
study harmonisation), We use the fcon1000 data set for that.

B: Harmonize a data set to a pre-fitted model/unseen data set. We load a
pre-fitted model for that.

1. Setup and Imports
--------------------

You first need to install the PCNtoolkit. We recommend creating a
virtual environment first.

.. code:: ipython3

    ! pip install pcntoolkit

Load the core PCNtoolkit classes and plotting/data libraries used
throughout this notebook.

.. code:: ipython3

    # Load the model
    from pcntoolkit import NormativeModel
    from pcntoolkit.util.plotter import plot_centiles_advanced
    import pandas as pd
    import logging
    from pcntoolkit import NormData
    import numpy as np
    import seaborn as sns
    import matplotlib.pyplot as plt
    
    logger = logging.getLogger("pymc")
    logger.setLevel(logging.ERROR)


2. Load and Inspect FCON1000 Data
---------------------------------

Read the dataset, harmonize sex coding to labels, and do a quick sanity
check of site coverage.

.. code:: ipython3

    # Load data
    
    try:
        df = pd.read_csv("../../data/fcon1000.csv")
    except FileNotFoundError:
        # Load the data from the GH repo if the local file is not present
        df = pd.read_csv(
            "https://raw.githubusercontent.com/predictive-clinical-neuroscience/pu25_code/refs/heads/main/data/fcon1000.csv"
        )
    df["sex"] = df.apply(lambda x: {0: "F", 1: "M"}[x["sex"]], axis=1)
    print(df["site"].unique())
    df.describe()


.. parsed-literal::

    <ArrowStringArray>
    [       'AnnArbor_a',        'AnnArbor_b',           'Atlanta',
             'Baltimore',            'Bangor',      'Beijing_Zang',
      'Berlin_Margulies', 'Cambridge_Buckner',         'Cleveland',
                  'ICBM',       'Leiden_2180',       'Leiden_2200',
           'Milwaukee_b',           'Munchen',         'NewYork_a',
        'NewYork_a_ADHD',            'Newark',              'Oulu',
                'Oxford',          'PaloAlto',        'Pittsburgh',
            'Queensland',        'SaintLouis']
    Length: 23, dtype: str




.. raw:: html

    <div>
    <style scoped>
        .dataframe tbody tr th:only-of-type {
            vertical-align: middle;
        }
    
        .dataframe tbody tr th {
            vertical-align: top;
        }
    
        .dataframe thead th {
            text-align: right;
        }
    </style>
    <table border="1" class="dataframe">
      <thead>
        <tr style="text-align: right;">
          <th></th>
          <th>age</th>
          <th>lh_G_and_S_frontomargin</th>
          <th>lh_G_and_S_occipital_inf</th>
          <th>lh_G_and_S_paracentral</th>
          <th>lh_G_and_S_subcentral</th>
          <th>lh_G_and_S_transv_frontopol</th>
          <th>lh_G_and_S_cingul-Ant</th>
          <th>lh_G_and_S_cingul-Mid-Ant</th>
          <th>lh_G_and_S_cingul-Mid-Post</th>
          <th>lh_G_cingul-Post-dorsal</th>
          <th>...</th>
          <th>rh_S_pericallosal</th>
          <th>rh_S_postcentral</th>
          <th>rh_S_precentral-inf-part</th>
          <th>rh_S_precentral-sup-part</th>
          <th>rh_S_suborbital</th>
          <th>rh_S_subparietal</th>
          <th>rh_S_temporal_inf</th>
          <th>rh_S_temporal_sup</th>
          <th>rh_S_temporal_transverse</th>
          <th>rh_MeanThickness</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <th>count</th>
          <td>1078.000000</td>
          <td>1078.000000</td>
          <td>1078.000000</td>
          <td>1078.000000</td>
          <td>1078.000000</td>
          <td>1078.000000</td>
          <td>1078.000000</td>
          <td>1078.000000</td>
          <td>1078.000000</td>
          <td>1078.000000</td>
          <td>...</td>
          <td>1078.000000</td>
          <td>1078.000000</td>
          <td>1078.000000</td>
          <td>1078.000000</td>
          <td>1078.000000</td>
          <td>1078.000000</td>
          <td>1078.000000</td>
          <td>1078.000000</td>
          <td>1078.000000</td>
          <td>1078.000000</td>
        </tr>
        <tr>
          <th>mean</th>
          <td>28.251224</td>
          <td>2.372937</td>
          <td>2.383840</td>
          <td>2.310856</td>
          <td>2.698957</td>
          <td>2.610888</td>
          <td>2.753899</td>
          <td>2.697544</td>
          <td>2.632217</td>
          <td>2.959526</td>
          <td>...</td>
          <td>1.979635</td>
          <td>2.104468</td>
          <td>2.451929</td>
          <td>2.417430</td>
          <td>2.497463</td>
          <td>2.401750</td>
          <td>2.474282</td>
          <td>2.495482</td>
          <td>2.554019</td>
          <td>2.485162</td>
        </tr>
        <tr>
          <th>std</th>
          <td>13.464724</td>
          <td>0.192185</td>
          <td>0.170751</td>
          <td>0.190248</td>
          <td>0.182866</td>
          <td>0.225693</td>
          <td>0.179482</td>
          <td>0.179243</td>
          <td>0.158939</td>
          <td>0.195792</td>
          <td>...</td>
          <td>0.281369</td>
          <td>0.161474</td>
          <td>0.160934</td>
          <td>0.182979</td>
          <td>0.400458</td>
          <td>0.162933</td>
          <td>0.207039</td>
          <td>0.139035</td>
          <td>0.313909</td>
          <td>0.097570</td>
        </tr>
        <tr>
          <th>min</th>
          <td>7.880000</td>
          <td>1.653000</td>
          <td>1.889000</td>
          <td>1.383000</td>
          <td>2.147000</td>
          <td>1.910000</td>
          <td>2.151000</td>
          <td>2.158000</td>
          <td>2.028000</td>
          <td>2.240000</td>
          <td>...</td>
          <td>1.144000</td>
          <td>1.635000</td>
          <td>1.744000</td>
          <td>1.503000</td>
          <td>1.585000</td>
          <td>1.897000</td>
          <td>1.583000</td>
          <td>1.943000</td>
          <td>1.618000</td>
          <td>2.056150</td>
        </tr>
        <tr>
          <th>25%</th>
          <td>21.000000</td>
          <td>2.244250</td>
          <td>2.267250</td>
          <td>2.184250</td>
          <td>2.574000</td>
          <td>2.460000</td>
          <td>2.629000</td>
          <td>2.581000</td>
          <td>2.529000</td>
          <td>2.840250</td>
          <td>...</td>
          <td>1.773250</td>
          <td>1.997000</td>
          <td>2.354000</td>
          <td>2.307000</td>
          <td>2.208250</td>
          <td>2.291250</td>
          <td>2.365000</td>
          <td>2.406500</td>
          <td>2.330000</td>
          <td>2.423033</td>
        </tr>
        <tr>
          <th>50%</th>
          <td>22.000000</td>
          <td>2.361500</td>
          <td>2.382000</td>
          <td>2.313000</td>
          <td>2.691500</td>
          <td>2.608000</td>
          <td>2.749500</td>
          <td>2.703500</td>
          <td>2.641000</td>
          <td>2.956000</td>
          <td>...</td>
          <td>1.943500</td>
          <td>2.100000</td>
          <td>2.450000</td>
          <td>2.433000</td>
          <td>2.437000</td>
          <td>2.400000</td>
          <td>2.498500</td>
          <td>2.499000</td>
          <td>2.538500</td>
          <td>2.487950</td>
        </tr>
        <tr>
          <th>75%</th>
          <td>29.000000</td>
          <td>2.486000</td>
          <td>2.498750</td>
          <td>2.444000</td>
          <td>2.821000</td>
          <td>2.750750</td>
          <td>2.865000</td>
          <td>2.818750</td>
          <td>2.739000</td>
          <td>3.088500</td>
          <td>...</td>
          <td>2.155500</td>
          <td>2.218750</td>
          <td>2.554000</td>
          <td>2.537750</td>
          <td>2.730500</td>
          <td>2.508000</td>
          <td>2.607000</td>
          <td>2.587000</td>
          <td>2.768500</td>
          <td>2.547210</td>
        </tr>
        <tr>
          <th>max</th>
          <td>85.000000</td>
          <td>3.047000</td>
          <td>3.010000</td>
          <td>3.021000</td>
          <td>3.438000</td>
          <td>3.432000</td>
          <td>3.542000</td>
          <td>3.330000</td>
          <td>3.143000</td>
          <td>3.608000</td>
          <td>...</td>
          <td>3.119000</td>
          <td>2.681000</td>
          <td>3.042000</td>
          <td>2.960000</td>
          <td>4.066000</td>
          <td>2.944000</td>
          <td>3.111000</td>
          <td>2.978000</td>
          <td>3.581000</td>
          <td>2.837260</td>
        </tr>
      </tbody>
    </table>
    <p>8 rows × 151 columns</p>
    </div>



3. Load Pre-Fitted Reference Model
----------------------------------

This model is the pre-fitted model and will allow us to harmonize the
focn1000 data set to unseen sites and data. It defines the reference
distribution used later for harmonization across sites.

.. code:: ipython3

    # Let's assume we have a pre-fitted model that we want to hamonize our model to: Change to your file path
    model = NormativeModel.load("/Users/johannabayer/Documents/Github/OHBM2026_Educational_course_harmonization/notebooks/block04/block4_utils/models/main_workflow_model_HBR")

A: Within study harmonisation
=============================

4. Define Model Inputs and Build NormData
-----------------------------------------

We first need to build a normative model from the fcon1000 data set. We
do that using the ``normdata`` object. Specify covariates, batch
effects, and response variables, then convert the dataframe into a
NormData object with basic cleaning.

.. code:: ipython3

    subject_id = "sub_id"
    covariates = ["age"]
    batch_effects = ["site", "sex"]
    non_respvars = [subject_id] + covariates + batch_effects
    
    # Only keep variable that are not covariates, batch effects, or subject id
    response_variables = filter(lambda x: x not in non_respvars, df.columns)
    # Only keep variables with variance
    response_variables = filter(lambda x: df[x].var() > 0, response_variables)
    # Only keep variables that are not categorical
    response_variables = filter(lambda x: df[x].dtype != "object", response_variables)
    # Only keep the first 5
    response_variables = list(response_variables)[:5]
    
    
    reference_norm_data = NormData.from_dataframe(
        name="fcon1000",
        dataframe=df,
        covariates=covariates,
        batch_effects=batch_effects,
        response_vars=response_variables,
        subject_ids=subject_id,
        remove_Nan=True,
        remove_outliers=True,
        z_threshold=10,  # The default here is 3, but we use 10 for demonstration purposes
    )


.. parsed-literal::

    Process: 75540 - 2026-06-11 12:09:45 - Removed 0 NANs
    Process: 75540 - 2026-06-11 12:09:45 - Removed 0 outliers
    Process: 75540 - 2026-06-11 12:09:45 - Dataset "fcon1000" created.
        - 1078 observations
        - 1078 unique subjects
        - 1 covariates
        - 5 response variables
        - 2 batch effects:
        	site (23)
    	sex (2)
        


5. Configure the HBR Template
-----------------------------

We use the HBR normative model. Now we specify the template, and define
priors and sampler settings for a within-study normative model.

.. code:: ipython3

    from pcntoolkit import make_prior, BsplineBasisFunction, NormalLikelihood, HBR, NormativeModel
    
    mu = make_prior(
        # Mu is linear because we want to allow the mean to vary as a function of the covariates.
        linear=True,
        # The slope coefficients are assumed to be normally distributed, with a mean of 0 and a standard deviation of 2.
        slope=make_prior(dist_params=(0, 2)),
        # The intercept is random, because we expect the intercept to vary between sites and sexes.
        intercept=make_prior(
            random=True,
            # Mu is the mean of the intercept, which is normally distributed with a mean of 0 and a standard deviation of 1.
            mu=make_prior(dist_params=(0.0, 1.0)),
            # Sigma is the scale at which the intercepts vary. It is a positive parameter, so we have to map it to the positive domain.
            sigma=make_prior(
                dist_params=(3.0, 1.0),
                mapping="softplus",
                mapping_params=(0.0, 2.0),
            ),
        ),
        # We use a B-spline basis function to allow for non-linearity in the mean.
        basis_function=BsplineBasisFunction(basis_column=0, nknots=5, degree=3),
    )
    sigma = make_prior(
        # Sigma is also linear, because we want to allow the standard deviation to vary as a function of the covariates: heteroskedasticity.
        linear=True,
        # The slope coefficients are assumed to be normally distributed, with a mean of 0 and a standard deviation of 2.
        slope=make_prior(dist_params=(0.0, 2.0)),
        # The intercept is not random, because we assume the intercept of the variance to be the same for all sites and sexes.
        intercept=make_prior(dist_params=(3.0, 2.0)),
        # We use a B-spline basis function to allow for non-linearity in the standard deviation.
        basis_function=BsplineBasisFunction(basis_column=0, nknots=5, degree=3),
        # We use a softplus mapping to ensure that sigma is strictly positive.
        mapping="softplus",
        # We scale the softplus mapping by a factor of 3, to avoid spikes in the resulting density.
        # The parameters (a, b, c) provided to a mapping f are used as: f_abc(x) = f((x - a) / b) * b + c
        # This basically provides an affine transformation of the softplus function.
        # a -> horizontal shift
        # b -> scaling
        # c -> vertical shift
        # You can leave c out, and it will default to 0.
        mapping_params=(0.0, 2.0),
    )
    
    # Set the likelihood with the priors we just created.
    likelihood = NormalLikelihood(mu, sigma)
    
    template_hbr = HBR(
        name="template_hbr",
        # The likelihood we just created.
        likelihood=likelihood,
        ### Sampling parameters
        # The number of draws to sample from the posterior per chain.
        draws=1500,
        # The number of tuning steps to run.
        tune=500,
        # The number of cores to use for sampling.
        cores=4,
        # The number of MCMC chains to run.
        chains=4,
        # The sampler to use for the model.
        nuts_sampler="nutpie",
        # The initialisation method for the samples
        init="jitter+adapt_diag",
        # Whether to show a progress bar during the model fitting.
        progressbar=True,
    )

.. code:: ipython3

    model_within = NormativeModel(
        template_regression_model=template_hbr,  # The HBR model we just configured
        savemodel=True,  # Whether to save the model after fitting -  defaults to True
        evaluate_model=True,  # Whether to evaluate the model after fitting -  defaults to True
        saveresults=True,  # Whether to save the results (Z_scores, centiles, logp, evaluation metrics) -  defaults to True
        saveplots=True,  # Whether to save the plots (centile curves and qq-plots) -  defaults to True
        save_dir="out/models/within_study",  # The directory to save the model
        inscaler="standardize",  # The scaler to use for the covariates, defaults to standardize
        outscaler="standardize",  # The scaler to use for the response variables, defaults to standardize
        name="within_study"# The path where the model will be saved
    )


6. Fit Within-Study Model
-------------------------

Instantiate and fit a model directly on the FCON1000-derived NormData
object.

.. code:: ipython3

    model_within.fit_predict(reference_norm_data, reference_norm_data)


.. parsed-literal::

    Process: 75540 - 2026-06-11 12:09:56 - Fitting models on 5 response variables.
    Process: 75540 - 2026-06-11 12:09:56 - Fitting model for lh_G_and_S_frontomargin.



.. raw:: html

    
    <style>
        :root {
            --column-width-1: 40%; /* Progress column width */
            --column-width-2: 15%; /* Chain column width */
            --column-width-3: 15%; /* Divergences column width */
            --column-width-4: 15%; /* Step Size column width */
            --column-width-5: 15%; /* Gradients/Draw column width */
        }
    
        .nutpie {
            max-width: 800px;
            margin: 10px auto;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            //color: #333;
            //background-color: #fff;
            padding: 10px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            border-radius: 8px;
            font-size: 14px; /* Smaller font size for a more compact look */
        }
        .nutpie table {
            width: 100%;
            border-collapse: collapse; /* Remove any extra space between borders */
        }
        .nutpie th, .nutpie td {
            padding: 8px 10px; /* Reduce padding to make table more compact */
            text-align: left;
            border-bottom: 1px solid #888;
        }
        .nutpie th {
            //background-color: #f0f0f0;
        }
    
        .nutpie th:nth-child(1) { width: var(--column-width-1); }
        .nutpie th:nth-child(2) { width: var(--column-width-2); }
        .nutpie th:nth-child(3) { width: var(--column-width-3); }
        .nutpie th:nth-child(4) { width: var(--column-width-4); }
        .nutpie th:nth-child(5) { width: var(--column-width-5); }
    
        .nutpie progress {
            width: 100%;
            height: 15px; /* Smaller progress bars */
            border-radius: 5px;
        }
        progress::-webkit-progress-bar {
            background-color: #eee;
            border-radius: 5px;
        }
        progress::-webkit-progress-value {
            background-color: #5cb85c;
            border-radius: 5px;
        }
        progress::-moz-progress-bar {
            background-color: #5cb85c;
            border-radius: 5px;
        }
        .nutpie .progress-cell {
            width: 100%;
        }
    
        .nutpie p strong { font-size: 16px; font-weight: bold; }
    
        @media (prefers-color-scheme: dark) {
            .nutpie {
                //color: #ddd;
                //background-color: #1e1e1e;
                box-shadow: 0 4px 6px rgba(0,0,0,0.2);
            }
            .nutpie table, .nutpie th, .nutpie td {
                border-color: #555;
                color: #ccc;
            }
            .nutpie th {
                background-color: #2a2a2a;
            }
            .nutpie progress::-webkit-progress-bar {
                background-color: #444;
            }
            .nutpie progress::-webkit-progress-value {
                background-color: #3178c6;
            }
            .nutpie progress::-moz-progress-bar {
                background-color: #3178c6;
            }
        }
    </style>




.. raw:: html

    
    <div class="nutpie">
        <p><strong>Sampler Progress</strong></p>
        <p>Total Chains: <span id="total-chains">4</span></p>
        <p>Active Chains: <span id="active-chains">0</span></p>
        <p>
            Finished Chains:
            <span id="active-chains">4</span>
        </p>
        <p>Sampling for 26 seconds</p>
        <p>
            Estimated Time to Completion:
            <span id="eta">now</span>
        </p>
    
        <progress
            id="total-progress-bar"
            max="8000"
            value="8000">
        </progress>
        <table>
            <thead>
                <tr>
                    <th>Progress</th>
                    <th>Draws</th>
                    <th>Divergences</th>
                    <th>Step Size</th>
                    <th>Gradients/Draw</th>
                </tr>
            </thead>
            <tbody id="chain-details">
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.22</td>
                        <td>63</td>
                    </tr>
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.18</td>
                        <td>31</td>
                    </tr>
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.20</td>
                        <td>63</td>
                    </tr>
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.19</td>
                        <td>127</td>
                    </tr>
    
                </tr>
            </tbody>
        </table>
    </div>



.. parsed-literal::

    Process: 75540 - 2026-06-11 12:10:31 - Fitting model for lh_G_and_S_occipital_inf.



.. raw:: html

    
    <style>
        :root {
            --column-width-1: 40%; /* Progress column width */
            --column-width-2: 15%; /* Chain column width */
            --column-width-3: 15%; /* Divergences column width */
            --column-width-4: 15%; /* Step Size column width */
            --column-width-5: 15%; /* Gradients/Draw column width */
        }
    
        .nutpie {
            max-width: 800px;
            margin: 10px auto;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            //color: #333;
            //background-color: #fff;
            padding: 10px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            border-radius: 8px;
            font-size: 14px; /* Smaller font size for a more compact look */
        }
        .nutpie table {
            width: 100%;
            border-collapse: collapse; /* Remove any extra space between borders */
        }
        .nutpie th, .nutpie td {
            padding: 8px 10px; /* Reduce padding to make table more compact */
            text-align: left;
            border-bottom: 1px solid #888;
        }
        .nutpie th {
            //background-color: #f0f0f0;
        }
    
        .nutpie th:nth-child(1) { width: var(--column-width-1); }
        .nutpie th:nth-child(2) { width: var(--column-width-2); }
        .nutpie th:nth-child(3) { width: var(--column-width-3); }
        .nutpie th:nth-child(4) { width: var(--column-width-4); }
        .nutpie th:nth-child(5) { width: var(--column-width-5); }
    
        .nutpie progress {
            width: 100%;
            height: 15px; /* Smaller progress bars */
            border-radius: 5px;
        }
        progress::-webkit-progress-bar {
            background-color: #eee;
            border-radius: 5px;
        }
        progress::-webkit-progress-value {
            background-color: #5cb85c;
            border-radius: 5px;
        }
        progress::-moz-progress-bar {
            background-color: #5cb85c;
            border-radius: 5px;
        }
        .nutpie .progress-cell {
            width: 100%;
        }
    
        .nutpie p strong { font-size: 16px; font-weight: bold; }
    
        @media (prefers-color-scheme: dark) {
            .nutpie {
                //color: #ddd;
                //background-color: #1e1e1e;
                box-shadow: 0 4px 6px rgba(0,0,0,0.2);
            }
            .nutpie table, .nutpie th, .nutpie td {
                border-color: #555;
                color: #ccc;
            }
            .nutpie th {
                background-color: #2a2a2a;
            }
            .nutpie progress::-webkit-progress-bar {
                background-color: #444;
            }
            .nutpie progress::-webkit-progress-value {
                background-color: #3178c6;
            }
            .nutpie progress::-moz-progress-bar {
                background-color: #3178c6;
            }
        }
    </style>




.. raw:: html

    
    <div class="nutpie">
        <p><strong>Sampler Progress</strong></p>
        <p>Total Chains: <span id="total-chains">4</span></p>
        <p>Active Chains: <span id="active-chains">0</span></p>
        <p>
            Finished Chains:
            <span id="active-chains">4</span>
        </p>
        <p>Sampling for 17 seconds</p>
        <p>
            Estimated Time to Completion:
            <span id="eta">now</span>
        </p>
    
        <progress
            id="total-progress-bar"
            max="8000"
            value="8000">
        </progress>
        <table>
            <thead>
                <tr>
                    <th>Progress</th>
                    <th>Draws</th>
                    <th>Divergences</th>
                    <th>Step Size</th>
                    <th>Gradients/Draw</th>
                </tr>
            </thead>
            <tbody id="chain-details">
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.21</td>
                        <td>31</td>
                    </tr>
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>1</td>
                        <td>0.20</td>
                        <td>31</td>
                    </tr>
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.17</td>
                        <td>63</td>
                    </tr>
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.19</td>
                        <td>63</td>
                    </tr>
    
                </tr>
            </tbody>
        </table>
    </div>



.. parsed-literal::

    Process: 75540 - 2026-06-11 12:10:54 - Fitting model for lh_G_and_S_paracentral.



.. raw:: html

    
    <style>
        :root {
            --column-width-1: 40%; /* Progress column width */
            --column-width-2: 15%; /* Chain column width */
            --column-width-3: 15%; /* Divergences column width */
            --column-width-4: 15%; /* Step Size column width */
            --column-width-5: 15%; /* Gradients/Draw column width */
        }
    
        .nutpie {
            max-width: 800px;
            margin: 10px auto;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            //color: #333;
            //background-color: #fff;
            padding: 10px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            border-radius: 8px;
            font-size: 14px; /* Smaller font size for a more compact look */
        }
        .nutpie table {
            width: 100%;
            border-collapse: collapse; /* Remove any extra space between borders */
        }
        .nutpie th, .nutpie td {
            padding: 8px 10px; /* Reduce padding to make table more compact */
            text-align: left;
            border-bottom: 1px solid #888;
        }
        .nutpie th {
            //background-color: #f0f0f0;
        }
    
        .nutpie th:nth-child(1) { width: var(--column-width-1); }
        .nutpie th:nth-child(2) { width: var(--column-width-2); }
        .nutpie th:nth-child(3) { width: var(--column-width-3); }
        .nutpie th:nth-child(4) { width: var(--column-width-4); }
        .nutpie th:nth-child(5) { width: var(--column-width-5); }
    
        .nutpie progress {
            width: 100%;
            height: 15px; /* Smaller progress bars */
            border-radius: 5px;
        }
        progress::-webkit-progress-bar {
            background-color: #eee;
            border-radius: 5px;
        }
        progress::-webkit-progress-value {
            background-color: #5cb85c;
            border-radius: 5px;
        }
        progress::-moz-progress-bar {
            background-color: #5cb85c;
            border-radius: 5px;
        }
        .nutpie .progress-cell {
            width: 100%;
        }
    
        .nutpie p strong { font-size: 16px; font-weight: bold; }
    
        @media (prefers-color-scheme: dark) {
            .nutpie {
                //color: #ddd;
                //background-color: #1e1e1e;
                box-shadow: 0 4px 6px rgba(0,0,0,0.2);
            }
            .nutpie table, .nutpie th, .nutpie td {
                border-color: #555;
                color: #ccc;
            }
            .nutpie th {
                background-color: #2a2a2a;
            }
            .nutpie progress::-webkit-progress-bar {
                background-color: #444;
            }
            .nutpie progress::-webkit-progress-value {
                background-color: #3178c6;
            }
            .nutpie progress::-moz-progress-bar {
                background-color: #3178c6;
            }
        }
    </style>




.. raw:: html

    
    <div class="nutpie">
        <p><strong>Sampler Progress</strong></p>
        <p>Total Chains: <span id="total-chains">4</span></p>
        <p>Active Chains: <span id="active-chains">0</span></p>
        <p>
            Finished Chains:
            <span id="active-chains">4</span>
        </p>
        <p>Sampling for 22 seconds</p>
        <p>
            Estimated Time to Completion:
            <span id="eta">now</span>
        </p>
    
        <progress
            id="total-progress-bar"
            max="8000"
            value="8000">
        </progress>
        <table>
            <thead>
                <tr>
                    <th>Progress</th>
                    <th>Draws</th>
                    <th>Divergences</th>
                    <th>Step Size</th>
                    <th>Gradients/Draw</th>
                </tr>
            </thead>
            <tbody id="chain-details">
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.17</td>
                        <td>127</td>
                    </tr>
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.18</td>
                        <td>63</td>
                    </tr>
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.18</td>
                        <td>127</td>
                    </tr>
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.16</td>
                        <td>127</td>
                    </tr>
    
                </tr>
            </tbody>
        </table>
    </div>



.. parsed-literal::

    Process: 75540 - 2026-06-11 12:11:21 - Fitting model for lh_G_and_S_subcentral.



.. raw:: html

    
    <style>
        :root {
            --column-width-1: 40%; /* Progress column width */
            --column-width-2: 15%; /* Chain column width */
            --column-width-3: 15%; /* Divergences column width */
            --column-width-4: 15%; /* Step Size column width */
            --column-width-5: 15%; /* Gradients/Draw column width */
        }
    
        .nutpie {
            max-width: 800px;
            margin: 10px auto;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            //color: #333;
            //background-color: #fff;
            padding: 10px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            border-radius: 8px;
            font-size: 14px; /* Smaller font size for a more compact look */
        }
        .nutpie table {
            width: 100%;
            border-collapse: collapse; /* Remove any extra space between borders */
        }
        .nutpie th, .nutpie td {
            padding: 8px 10px; /* Reduce padding to make table more compact */
            text-align: left;
            border-bottom: 1px solid #888;
        }
        .nutpie th {
            //background-color: #f0f0f0;
        }
    
        .nutpie th:nth-child(1) { width: var(--column-width-1); }
        .nutpie th:nth-child(2) { width: var(--column-width-2); }
        .nutpie th:nth-child(3) { width: var(--column-width-3); }
        .nutpie th:nth-child(4) { width: var(--column-width-4); }
        .nutpie th:nth-child(5) { width: var(--column-width-5); }
    
        .nutpie progress {
            width: 100%;
            height: 15px; /* Smaller progress bars */
            border-radius: 5px;
        }
        progress::-webkit-progress-bar {
            background-color: #eee;
            border-radius: 5px;
        }
        progress::-webkit-progress-value {
            background-color: #5cb85c;
            border-radius: 5px;
        }
        progress::-moz-progress-bar {
            background-color: #5cb85c;
            border-radius: 5px;
        }
        .nutpie .progress-cell {
            width: 100%;
        }
    
        .nutpie p strong { font-size: 16px; font-weight: bold; }
    
        @media (prefers-color-scheme: dark) {
            .nutpie {
                //color: #ddd;
                //background-color: #1e1e1e;
                box-shadow: 0 4px 6px rgba(0,0,0,0.2);
            }
            .nutpie table, .nutpie th, .nutpie td {
                border-color: #555;
                color: #ccc;
            }
            .nutpie th {
                background-color: #2a2a2a;
            }
            .nutpie progress::-webkit-progress-bar {
                background-color: #444;
            }
            .nutpie progress::-webkit-progress-value {
                background-color: #3178c6;
            }
            .nutpie progress::-moz-progress-bar {
                background-color: #3178c6;
            }
        }
    </style>




.. raw:: html

    
    <div class="nutpie">
        <p><strong>Sampler Progress</strong></p>
        <p>Total Chains: <span id="total-chains">4</span></p>
        <p>Active Chains: <span id="active-chains">0</span></p>
        <p>
            Finished Chains:
            <span id="active-chains">4</span>
        </p>
        <p>Sampling for 19 seconds</p>
        <p>
            Estimated Time to Completion:
            <span id="eta">now</span>
        </p>
    
        <progress
            id="total-progress-bar"
            max="8000"
            value="8000">
        </progress>
        <table>
            <thead>
                <tr>
                    <th>Progress</th>
                    <th>Draws</th>
                    <th>Divergences</th>
                    <th>Step Size</th>
                    <th>Gradients/Draw</th>
                </tr>
            </thead>
            <tbody id="chain-details">
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.19</td>
                        <td>31</td>
                    </tr>
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.17</td>
                        <td>31</td>
                    </tr>
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.17</td>
                        <td>15</td>
                    </tr>
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.19</td>
                        <td>47</td>
                    </tr>
    
                </tr>
            </tbody>
        </table>
    </div>



.. parsed-literal::

    Process: 75540 - 2026-06-11 12:11:46 - Fitting model for lh_G_and_S_transv_frontopol.



.. raw:: html

    
    <style>
        :root {
            --column-width-1: 40%; /* Progress column width */
            --column-width-2: 15%; /* Chain column width */
            --column-width-3: 15%; /* Divergences column width */
            --column-width-4: 15%; /* Step Size column width */
            --column-width-5: 15%; /* Gradients/Draw column width */
        }
    
        .nutpie {
            max-width: 800px;
            margin: 10px auto;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            //color: #333;
            //background-color: #fff;
            padding: 10px;
            box-shadow: 0 4px 6px rgba(0,0,0,0.1);
            border-radius: 8px;
            font-size: 14px; /* Smaller font size for a more compact look */
        }
        .nutpie table {
            width: 100%;
            border-collapse: collapse; /* Remove any extra space between borders */
        }
        .nutpie th, .nutpie td {
            padding: 8px 10px; /* Reduce padding to make table more compact */
            text-align: left;
            border-bottom: 1px solid #888;
        }
        .nutpie th {
            //background-color: #f0f0f0;
        }
    
        .nutpie th:nth-child(1) { width: var(--column-width-1); }
        .nutpie th:nth-child(2) { width: var(--column-width-2); }
        .nutpie th:nth-child(3) { width: var(--column-width-3); }
        .nutpie th:nth-child(4) { width: var(--column-width-4); }
        .nutpie th:nth-child(5) { width: var(--column-width-5); }
    
        .nutpie progress {
            width: 100%;
            height: 15px; /* Smaller progress bars */
            border-radius: 5px;
        }
        progress::-webkit-progress-bar {
            background-color: #eee;
            border-radius: 5px;
        }
        progress::-webkit-progress-value {
            background-color: #5cb85c;
            border-radius: 5px;
        }
        progress::-moz-progress-bar {
            background-color: #5cb85c;
            border-radius: 5px;
        }
        .nutpie .progress-cell {
            width: 100%;
        }
    
        .nutpie p strong { font-size: 16px; font-weight: bold; }
    
        @media (prefers-color-scheme: dark) {
            .nutpie {
                //color: #ddd;
                //background-color: #1e1e1e;
                box-shadow: 0 4px 6px rgba(0,0,0,0.2);
            }
            .nutpie table, .nutpie th, .nutpie td {
                border-color: #555;
                color: #ccc;
            }
            .nutpie th {
                background-color: #2a2a2a;
            }
            .nutpie progress::-webkit-progress-bar {
                background-color: #444;
            }
            .nutpie progress::-webkit-progress-value {
                background-color: #3178c6;
            }
            .nutpie progress::-moz-progress-bar {
                background-color: #3178c6;
            }
        }
    </style>




.. raw:: html

    
    <div class="nutpie">
        <p><strong>Sampler Progress</strong></p>
        <p>Total Chains: <span id="total-chains">4</span></p>
        <p>Active Chains: <span id="active-chains">0</span></p>
        <p>
            Finished Chains:
            <span id="active-chains">4</span>
        </p>
        <p>Sampling for 18 seconds</p>
        <p>
            Estimated Time to Completion:
            <span id="eta">now</span>
        </p>
    
        <progress
            id="total-progress-bar"
            max="8000"
            value="8000">
        </progress>
        <table>
            <thead>
                <tr>
                    <th>Progress</th>
                    <th>Draws</th>
                    <th>Divergences</th>
                    <th>Step Size</th>
                    <th>Gradients/Draw</th>
                </tr>
            </thead>
            <tbody id="chain-details">
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.20</td>
                        <td>63</td>
                    </tr>
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.18</td>
                        <td>127</td>
                    </tr>
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.18</td>
                        <td>63</td>
                    </tr>
    
                    <tr>
                        <td class="progress-cell">
                            <progress
                                max="2000"
                                value="2000">
                            </progress>
                        </td>
                        <td>2000</td>
                        <td>0</td>
                        <td>0.20</td>
                        <td>127</td>
                    </tr>
    
                </tr>
            </tbody>
        </table>
    </div>



.. parsed-literal::

    Process: 75540 - 2026-06-11 12:12:11 - Saving model to:
    	../out/models/within_study.
    Process: 75540 - 2026-06-11 12:12:12 - Making predictions on 5 response variables.
    Process: 75540 - 2026-06-11 12:12:12 - Computing z-scores for 5 response variables.
    Process: 75540 - 2026-06-11 12:12:12 - Computing z-scores for lh_G_and_S_frontomargin.
    Process: 75540 - 2026-06-11 12:12:13 - Computing z-scores for lh_G_and_S_paracentral.
    Process: 75540 - 2026-06-11 12:12:14 - Computing z-scores for lh_G_and_S_transv_frontopol.
    Process: 75540 - 2026-06-11 12:12:15 - Computing z-scores for lh_G_and_S_subcentral.
    Process: 75540 - 2026-06-11 12:12:16 - Computing z-scores for lh_G_and_S_occipital_inf.
    Process: 75540 - 2026-06-11 12:12:17 - Computing centiles for 5 response variables.
    Process: 75540 - 2026-06-11 12:12:17 - Computing centiles for lh_G_and_S_frontomargin.
    Process: 75540 - 2026-06-11 12:12:20 - Computing centiles for lh_G_and_S_paracentral.
    Process: 75540 - 2026-06-11 12:12:24 - Computing centiles for lh_G_and_S_transv_frontopol.
    Process: 75540 - 2026-06-11 12:12:28 - Computing centiles for lh_G_and_S_subcentral.
    Process: 75540 - 2026-06-11 12:12:32 - Computing centiles for lh_G_and_S_occipital_inf.
    Process: 75540 - 2026-06-11 12:12:36 - Computing log-probabilities for 5 response variables.
    Process: 75540 - 2026-06-11 12:12:36 - Computing log-probabilities for 5 response variables.
    Process: 75540 - 2026-06-11 12:12:36 - Computing log-probabilities for lh_G_and_S_frontomargin.
    Process: 75540 - 2026-06-11 12:12:36 - Computing log-probabilities for lh_G_and_S_paracentral.
    Process: 75540 - 2026-06-11 12:12:38 - Computing log-probabilities for lh_G_and_S_transv_frontopol.
    Process: 75540 - 2026-06-11 12:12:39 - Computing log-probabilities for lh_G_and_S_subcentral.
    Process: 75540 - 2026-06-11 12:12:40 - Computing log-probabilities for lh_G_and_S_occipital_inf.
    Process: 75540 - 2026-06-11 12:12:40 - Computing yhat for 5 response variables.
    Process: 75540 - 2026-06-11 12:12:45 - Dataset "centile" created.
        - 150 observations
        - 150 unique subjects
        - 1 covariates
        - 5 response variables
        - 2 batch effects:
        	site (1)
    	sex (1)
        
    Process: 75540 - 2026-06-11 12:12:45 - Computing centiles for 5 response variables.
    Process: 75540 - 2026-06-11 12:12:45 - Computing centiles for lh_G_and_S_frontomargin.
    Process: 75540 - 2026-06-11 12:12:47 - Computing centiles for lh_G_and_S_paracentral.
    Process: 75540 - 2026-06-11 12:12:49 - Computing centiles for lh_G_and_S_transv_frontopol.
    Process: 75540 - 2026-06-11 12:12:51 - Computing centiles for lh_G_and_S_subcentral.
    Process: 75540 - 2026-06-11 12:12:53 - Computing centiles for lh_G_and_S_occipital_inf.
    Process: 75540 - 2026-06-11 12:12:55 - Harmonizing data on 5 response variables.
    Process: 75540 - 2026-06-11 12:12:55 - Harmonizing data for lh_G_and_S_frontomargin.
    Process: 75540 - 2026-06-11 12:12:56 - Harmonizing data for lh_G_and_S_paracentral.
    Process: 75540 - 2026-06-11 12:12:58 - Harmonizing data for lh_G_and_S_transv_frontopol.
    Process: 75540 - 2026-06-11 12:12:59 - Harmonizing data for lh_G_and_S_subcentral.
    Process: 75540 - 2026-06-11 12:13:01 - Harmonizing data for lh_G_and_S_occipital_inf.
    Process: 75540 - 2026-06-11 12:13:04 - Making predictions on 5 response variables.
    Process: 75540 - 2026-06-11 12:13:04 - Computing z-scores for 5 response variables.
    Process: 75540 - 2026-06-11 12:13:04 - Computing z-scores for lh_G_and_S_frontomargin.
    Process: 75540 - 2026-06-11 12:13:04 - Computing z-scores for lh_G_and_S_paracentral.
    Process: 75540 - 2026-06-11 12:13:05 - Computing z-scores for lh_G_and_S_transv_frontopol.
    Process: 75540 - 2026-06-11 12:13:06 - Computing z-scores for lh_G_and_S_subcentral.
    Process: 75540 - 2026-06-11 12:13:07 - Computing z-scores for lh_G_and_S_occipital_inf.
    Process: 75540 - 2026-06-11 12:13:07 - Computing centiles for 5 response variables.
    Process: 75540 - 2026-06-11 12:13:07 - Computing centiles for lh_G_and_S_frontomargin.
    Process: 75540 - 2026-06-11 12:13:11 - Computing centiles for lh_G_and_S_paracentral.
    Process: 75540 - 2026-06-11 12:13:15 - Computing centiles for lh_G_and_S_transv_frontopol.
    Process: 75540 - 2026-06-11 12:13:19 - Computing centiles for lh_G_and_S_subcentral.
    Process: 75540 - 2026-06-11 12:13:23 - Computing centiles for lh_G_and_S_occipital_inf.
    Process: 75540 - 2026-06-11 12:13:26 - Computing log-probabilities for 5 response variables.
    Process: 75540 - 2026-06-11 12:13:26 - Computing log-probabilities for 5 response variables.
    Process: 75540 - 2026-06-11 12:13:26 - Computing log-probabilities for lh_G_and_S_frontomargin.
    Process: 75540 - 2026-06-11 12:13:27 - Computing log-probabilities for lh_G_and_S_paracentral.
    Process: 75540 - 2026-06-11 12:13:28 - Computing log-probabilities for lh_G_and_S_transv_frontopol.
    Process: 75540 - 2026-06-11 12:13:29 - Computing log-probabilities for lh_G_and_S_subcentral.
    Process: 75540 - 2026-06-11 12:13:30 - Computing log-probabilities for lh_G_and_S_occipital_inf.
    Process: 75540 - 2026-06-11 12:13:31 - Computing yhat for 5 response variables.
    Process: 75540 - 2026-06-11 12:13:36 - Dataset "centile" created.
        - 150 observations
        - 150 unique subjects
        - 1 covariates
        - 5 response variables
        - 2 batch effects:
        	site (1)
    	sex (1)
        
    Process: 75540 - 2026-06-11 12:13:36 - Computing centiles for 5 response variables.
    Process: 75540 - 2026-06-11 12:13:36 - Computing centiles for lh_G_and_S_frontomargin.
    Process: 75540 - 2026-06-11 12:13:38 - Computing centiles for lh_G_and_S_paracentral.
    Process: 75540 - 2026-06-11 12:13:40 - Computing centiles for lh_G_and_S_transv_frontopol.
    Process: 75540 - 2026-06-11 12:13:42 - Computing centiles for lh_G_and_S_subcentral.
    Process: 75540 - 2026-06-11 12:13:44 - Computing centiles for lh_G_and_S_occipital_inf.
    Process: 75540 - 2026-06-11 12:13:46 - Harmonizing data on 5 response variables.
    Process: 75540 - 2026-06-11 12:13:46 - Harmonizing data for lh_G_and_S_frontomargin.
    Process: 75540 - 2026-06-11 12:13:48 - Harmonizing data for lh_G_and_S_paracentral.
    Process: 75540 - 2026-06-11 12:13:50 - Harmonizing data for lh_G_and_S_transv_frontopol.
    Process: 75540 - 2026-06-11 12:13:52 - Harmonizing data for lh_G_and_S_subcentral.
    Process: 75540 - 2026-06-11 12:13:54 - Harmonizing data for lh_G_and_S_occipital_inf.




.. raw:: html

    <div><svg style="position: absolute; width: 0; height: 0; overflow: hidden">
    <defs>
    <symbol id="icon-database" viewBox="0 0 32 32">
    <path d="M16 0c-8.837 0-16 2.239-16 5v4c0 2.761 7.163 5 16 5s16-2.239 16-5v-4c0-2.761-7.163-5-16-5z"></path>
    <path d="M16 17c-8.837 0-16-2.239-16-5v6c0 2.761 7.163 5 16 5s16-2.239 16-5v-6c0 2.761-7.163 5-16 5z"></path>
    <path d="M16 26c-8.837 0-16-2.239-16-5v6c0 2.761 7.163 5 16 5s16-2.239 16-5v-6c0 2.761-7.163 5-16 5z"></path>
    </symbol>
    <symbol id="icon-file-text2" viewBox="0 0 32 32">
    <path d="M28.681 7.159c-0.694-0.947-1.662-2.053-2.724-3.116s-2.169-2.030-3.116-2.724c-1.612-1.182-2.393-1.319-2.841-1.319h-15.5c-1.378 0-2.5 1.121-2.5 2.5v27c0 1.378 1.122 2.5 2.5 2.5h23c1.378 0 2.5-1.122 2.5-2.5v-19.5c0-0.448-0.137-1.23-1.319-2.841zM24.543 5.457c0.959 0.959 1.712 1.825 2.268 2.543h-4.811v-4.811c0.718 0.556 1.584 1.309 2.543 2.268zM28 29.5c0 0.271-0.229 0.5-0.5 0.5h-23c-0.271 0-0.5-0.229-0.5-0.5v-27c0-0.271 0.229-0.5 0.5-0.5 0 0 15.499-0 15.5 0v7c0 0.552 0.448 1 1 1h7v19.5z"></path>
    <path d="M23 26h-14c-0.552 0-1-0.448-1-1s0.448-1 1-1h14c0.552 0 1 0.448 1 1s-0.448 1-1 1z"></path>
    <path d="M23 22h-14c-0.552 0-1-0.448-1-1s0.448-1 1-1h14c0.552 0 1 0.448 1 1s-0.448 1-1 1z"></path>
    <path d="M23 18h-14c-0.552 0-1-0.448-1-1s0.448-1 1-1h14c0.552 0 1 0.448 1 1s-0.448 1-1 1z"></path>
    </symbol>
    </defs>
    </svg>
    <style>/* CSS stylesheet for displaying xarray objects in notebooks */
    
    :root {
      --xr-font-color0: var(
        --jp-content-font-color0,
        var(--pst-color-text-base rgba(0, 0, 0, 1))
      );
      --xr-font-color2: var(
        --jp-content-font-color2,
        var(--pst-color-text-base, rgba(0, 0, 0, 0.54))
      );
      --xr-font-color3: var(
        --jp-content-font-color3,
        var(--pst-color-text-base, rgba(0, 0, 0, 0.38))
      );
      --xr-border-color: var(
        --jp-border-color2,
        hsl(from var(--pst-color-on-background, white) h s calc(l - 10))
      );
      --xr-disabled-color: var(
        --jp-layout-color3,
        hsl(from var(--pst-color-on-background, white) h s calc(l - 40))
      );
      --xr-background-color: var(
        --jp-layout-color0,
        var(--pst-color-on-background, white)
      );
      --xr-background-color-row-even: var(
        --jp-layout-color1,
        hsl(from var(--pst-color-on-background, white) h s calc(l - 5))
      );
      --xr-background-color-row-odd: var(
        --jp-layout-color2,
        hsl(from var(--pst-color-on-background, white) h s calc(l - 15))
      );
    }
    
    html[theme="dark"],
    html[data-theme="dark"],
    body[data-theme="dark"],
    body.vscode-dark {
      --xr-font-color0: var(
        --jp-content-font-color0,
        var(--pst-color-text-base, rgba(255, 255, 255, 1))
      );
      --xr-font-color2: var(
        --jp-content-font-color2,
        var(--pst-color-text-base, rgba(255, 255, 255, 0.54))
      );
      --xr-font-color3: var(
        --jp-content-font-color3,
        var(--pst-color-text-base, rgba(255, 255, 255, 0.38))
      );
      --xr-border-color: var(
        --jp-border-color2,
        hsl(from var(--pst-color-on-background, #111111) h s calc(l + 10))
      );
      --xr-disabled-color: var(
        --jp-layout-color3,
        hsl(from var(--pst-color-on-background, #111111) h s calc(l + 40))
      );
      --xr-background-color: var(
        --jp-layout-color0,
        var(--pst-color-on-background, #111111)
      );
      --xr-background-color-row-even: var(
        --jp-layout-color1,
        hsl(from var(--pst-color-on-background, #111111) h s calc(l + 5))
      );
      --xr-background-color-row-odd: var(
        --jp-layout-color2,
        hsl(from var(--pst-color-on-background, #111111) h s calc(l + 15))
      );
    }
    
    .xr-wrap {
      display: block !important;
      min-width: 300px;
      max-width: 700px;
      line-height: 1.6;
      padding-bottom: 4px;
    }
    
    .xr-text-repr-fallback {
      /* fallback to plain text repr when CSS is not injected (untrusted notebook) */
      display: none;
    }
    
    .xr-header {
      padding-top: 6px;
      padding-bottom: 6px;
    }
    
    .xr-header {
      border-bottom: solid 1px var(--xr-border-color);
      margin-bottom: 4px;
    }
    
    .xr-header > div,
    .xr-header > ul {
      display: inline;
      margin-top: 0;
      margin-bottom: 0;
    }
    
    .xr-obj-type,
    .xr-obj-name {
      margin-left: 2px;
      margin-right: 10px;
    }
    
    .xr-obj-type,
    .xr-group-box-contents > label {
      color: var(--xr-font-color2);
      display: block;
    }
    
    .xr-sections {
      padding-left: 0 !important;
      display: grid;
      grid-template-columns: 150px auto auto 1fr 0 20px 0 20px;
      margin-block-start: 0;
      margin-block-end: 0;
    }
    
    .xr-section-item {
      display: contents;
    }
    
    .xr-section-item > input,
    .xr-group-box-contents > input,
    .xr-array-wrap > input {
      display: block;
      opacity: 0;
      height: 0;
      margin: 0;
    }
    
    .xr-section-item > input + label,
    .xr-var-item > input + label {
      color: var(--xr-disabled-color);
    }
    
    .xr-section-item > input:enabled + label,
    .xr-var-item > input:enabled + label,
    .xr-array-wrap > input:enabled + label,
    .xr-group-box-contents > input:enabled + label {
      cursor: pointer;
      color: var(--xr-font-color2);
    }
    
    .xr-section-item > input:focus-visible + label,
    .xr-var-item > input:focus-visible + label,
    .xr-array-wrap > input:focus-visible + label,
    .xr-group-box-contents > input:focus-visible + label {
      outline: auto;
    }
    
    .xr-section-item > input:enabled + label:hover,
    .xr-var-item > input:enabled + label:hover,
    .xr-array-wrap > input:enabled + label:hover,
    .xr-group-box-contents > input:enabled + label:hover {
      color: var(--xr-font-color0);
    }
    
    .xr-section-summary {
      grid-column: 1;
      color: var(--xr-font-color2);
      font-weight: 500;
      white-space: nowrap;
    }
    
    .xr-section-summary > em {
      font-weight: normal;
    }
    
    .xr-span-grid {
      grid-column-end: -1;
    }
    
    .xr-section-summary > span {
      display: inline-block;
      padding-left: 0.3em;
    }
    
    .xr-group-box-contents > input:checked + label > span {
      display: inline-block;
      padding-left: 0.6em;
    }
    
    .xr-section-summary-in:disabled + label {
      color: var(--xr-font-color2);
    }
    
    .xr-section-summary-in + label:before {
      display: inline-block;
      content: "►";
      font-size: 11px;
      width: 15px;
      text-align: center;
    }
    
    .xr-section-summary-in:disabled + label:before {
      color: var(--xr-disabled-color);
    }
    
    .xr-section-summary-in:checked + label:before {
      content: "▼";
    }
    
    .xr-section-summary-in:checked + label > span {
      display: none;
    }
    
    .xr-section-summary,
    .xr-section-inline-details,
    .xr-group-box-contents > label {
      padding-top: 4px;
    }
    
    .xr-section-inline-details {
      grid-column: 2 / -1;
    }
    
    .xr-section-details {
      grid-column: 1 / -1;
      margin-top: 4px;
      margin-bottom: 5px;
    }
    
    .xr-section-summary-in ~ .xr-section-details {
      display: none;
    }
    
    .xr-section-summary-in:checked ~ .xr-section-details {
      display: contents;
    }
    
    .xr-children {
      display: inline-grid;
      grid-template-columns: 100%;
      grid-column: 1 / -1;
      padding-top: 4px;
    }
    
    .xr-group-box {
      display: inline-grid;
      grid-template-columns: 0px 30px auto;
    }
    
    .xr-group-box-vline {
      grid-column-start: 1;
      border-right: 0.2em solid;
      border-color: var(--xr-border-color);
      width: 0px;
    }
    
    .xr-group-box-hline {
      grid-column-start: 2;
      grid-row-start: 1;
      height: 1em;
      width: 26px;
      border-bottom: 0.2em solid;
      border-color: var(--xr-border-color);
    }
    
    .xr-group-box-contents {
      grid-column-start: 3;
      padding-bottom: 4px;
    }
    
    .xr-group-box-contents > label::before {
      content: "📂";
      padding-right: 0.3em;
    }
    
    .xr-group-box-contents > input:checked + label::before {
      content: "📁";
    }
    
    .xr-group-box-contents > input:checked + label {
      padding-bottom: 0px;
    }
    
    .xr-group-box-contents > input:checked ~ .xr-sections {
      display: none;
    }
    
    .xr-group-box-contents > input + label > span {
      display: none;
    }
    
    .xr-group-box-ellipsis {
      font-size: 1.4em;
      font-weight: 900;
      color: var(--xr-font-color2);
      letter-spacing: 0.15em;
      cursor: default;
    }
    
    .xr-array-wrap {
      grid-column: 1 / -1;
      display: grid;
      grid-template-columns: 20px auto;
    }
    
    .xr-array-wrap > label {
      grid-column: 1;
      vertical-align: top;
    }
    
    .xr-preview {
      color: var(--xr-font-color3);
    }
    
    .xr-array-preview,
    .xr-array-data {
      padding: 0 5px !important;
      grid-column: 2;
    }
    
    .xr-array-data,
    .xr-array-in:checked ~ .xr-array-preview {
      display: none;
    }
    
    .xr-array-in:checked ~ .xr-array-data,
    .xr-array-preview {
      display: inline-block;
    }
    
    .xr-dim-list {
      display: inline-block !important;
      list-style: none;
      padding: 0 !important;
      margin: 0;
    }
    
    .xr-dim-list li {
      display: inline-block;
      padding: 0;
      margin: 0;
    }
    
    .xr-dim-list:before {
      content: "(";
    }
    
    .xr-dim-list:after {
      content: ")";
    }
    
    .xr-dim-list li:not(:last-child):after {
      content: ",";
      padding-right: 5px;
    }
    
    .xr-has-index {
      font-weight: bold;
    }
    
    .xr-var-list,
    .xr-var-item {
      display: contents;
    }
    
    .xr-var-item > div,
    .xr-var-item label,
    .xr-var-item > .xr-var-name span {
      background-color: var(--xr-background-color-row-even);
      border-color: var(--xr-background-color-row-odd);
      margin-bottom: 0;
      padding-top: 2px;
    }
    
    .xr-var-item > .xr-var-name:hover span {
      padding-right: 5px;
    }
    
    .xr-var-list > li:nth-child(odd) > div,
    .xr-var-list > li:nth-child(odd) > label,
    .xr-var-list > li:nth-child(odd) > .xr-var-name span {
      background-color: var(--xr-background-color-row-odd);
      border-color: var(--xr-background-color-row-even);
    }
    
    .xr-var-name {
      grid-column: 1;
    }
    
    .xr-var-dims {
      grid-column: 2;
    }
    
    .xr-var-dtype {
      grid-column: 3;
      text-align: right;
      color: var(--xr-font-color2);
    }
    
    .xr-var-preview {
      grid-column: 4;
    }
    
    .xr-index-preview {
      grid-column: 2 / 5;
      color: var(--xr-font-color2);
    }
    
    .xr-var-name,
    .xr-var-dims,
    .xr-var-dtype,
    .xr-preview,
    .xr-attrs dt {
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      padding-right: 10px;
    }
    
    .xr-var-name:hover,
    .xr-var-dims:hover,
    .xr-var-dtype:hover,
    .xr-attrs dt:hover {
      overflow: visible;
      width: auto;
      z-index: 1;
    }
    
    .xr-var-attrs,
    .xr-var-data,
    .xr-index-data {
      display: none;
      border-top: 2px dotted var(--xr-background-color);
      padding-bottom: 20px !important;
      padding-top: 10px !important;
    }
    
    .xr-var-attrs-in + label,
    .xr-var-data-in + label,
    .xr-index-data-in + label {
      padding: 0 1px;
    }
    
    .xr-var-attrs-in:checked ~ .xr-var-attrs,
    .xr-var-data-in:checked ~ .xr-var-data,
    .xr-index-data-in:checked ~ .xr-index-data {
      display: block;
    }
    
    .xr-var-data > table {
      float: right;
    }
    
    .xr-var-data > pre,
    .xr-index-data > pre,
    .xr-var-data > table > tbody > tr {
      background-color: transparent !important;
    }
    
    .xr-var-name span,
    .xr-var-data,
    .xr-index-name div,
    .xr-index-data,
    .xr-attrs {
      padding-left: 25px !important;
    }
    
    .xr-attrs,
    .xr-var-attrs,
    .xr-var-data,
    .xr-index-data {
      grid-column: 1 / -1;
    }
    
    dl.xr-attrs {
      padding: 0;
      margin: 0;
      display: grid;
      grid-template-columns: 125px auto;
    }
    
    .xr-attrs dt,
    .xr-attrs dd {
      padding: 0;
      margin: 0;
      float: left;
      padding-right: 10px;
      width: auto;
    }
    
    .xr-attrs dt {
      font-weight: normal;
      grid-column: 1;
    }
    
    .xr-attrs dt:hover span {
      display: inline-block;
      background: var(--xr-background-color);
      padding-right: 10px;
    }
    
    .xr-attrs dd {
      grid-column: 2;
      white-space: pre-wrap;
      word-break: break-all;
    }
    
    .xr-icon-database,
    .xr-icon-file-text2,
    .xr-no-icon {
      display: inline-block;
      vertical-align: middle;
      width: 1em;
      height: 1.5em !important;
      stroke-width: 0;
      stroke: currentColor;
      fill: currentColor;
    }
    
    .xr-var-attrs-in:checked + label > .xr-icon-file-text2,
    .xr-var-data-in:checked + label > .xr-icon-database,
    .xr-index-data-in:checked + label > .xr-icon-database {
      color: var(--xr-font-color0);
      filter: drop-shadow(1px 1px 5px var(--xr-font-color2));
      stroke-width: 0.8px;
    }
    </style><pre class='xr-text-repr-fallback'>&lt;xarray.NormData&gt; Size: 605kB
    Dimensions:            (observations: 1078, response_vars: 5, covariates: 1,
                            batch_effect_dims: 2, statistic: 11, centile: 5)
    Coordinates:
      * observations       (observations) int64 9kB 0 1 2 3 ... 1074 1075 1076 1077
      * response_vars      (response_vars) &lt;U27 540B &#x27;lh_G_and_S_frontomargin&#x27; .....
      * covariates         (covariates) &lt;U3 12B &#x27;age&#x27;
      * batch_effect_dims  (batch_effect_dims) &lt;U4 32B &#x27;site&#x27; &#x27;sex&#x27;
      * statistic          (statistic) &lt;U8 352B &#x27;EXPV&#x27; &#x27;MACE&#x27; ... &#x27;SMSE&#x27; &#x27;ShapiroW&#x27;
      * centile            (centile) float64 40B 0.05 0.25 0.5 0.75 0.95
    Data variables:
        subject_ids        (observations) object 9kB &#x27;AnnArbor_a_sub04111&#x27; ... &#x27;S...
        Y                  (observations, response_vars) float64 43kB 2.297 ... 2...
        X                  (observations, covariates) float64 9kB 25.63 ... 23.0
        batch_effects      (observations, batch_effect_dims) &lt;U17 147kB &#x27;AnnArbor...
        Z                  (observations, response_vars) float64 43kB 0.09048 ......
        baseline_logp      (observations, response_vars) float64 43kB -0.9971 ......
        logp               (observations, response_vars) float64 43kB -0.7676 ......
        Yhat               (observations, response_vars) float64 43kB 2.283 ... 2.64
        statistics         (response_vars, statistic) float64 440B 0.2812 ... 0.993
        centiles           (centile, observations, response_vars) float64 216kB 2...
    Attributes:
        real_ids:                       True
        is_scaled:                      False
        name:                           fcon1000
        unique_batch_effects:           {np.str_(&#x27;site&#x27;): [&#x27;AnnArbor_a&#x27;, &#x27;AnnArbo...
        batch_effect_counts:            defaultdict(&lt;function NormData.register_b...
        covariate_ranges:               {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 7.88, &#x27;max&#x27;: 85.0}}
        batch_effect_covariate_ranges:  {np.str_(&#x27;site&#x27;): {&#x27;AnnArbor_a&#x27;: {np.str_...</pre><div class='xr-wrap' style='display:none'><div class='xr-header'><div class='xr-obj-type'>xarray.NormData</div></div><ul class='xr-sections'><li class='xr-section-item'><input id='section-969a48a0-b5fc-4d96-83d5-72ea23f31f3a' class='xr-section-summary-in' type='checkbox' disabled /><label for='section-969a48a0-b5fc-4d96-83d5-72ea23f31f3a' class='xr-section-summary'>Dimensions:</label><div class='xr-section-inline-details'><ul class='xr-dim-list'><li><span class='xr-has-index'>observations</span>: 1078</li><li><span class='xr-has-index'>response_vars</span>: 5</li><li><span class='xr-has-index'>covariates</span>: 1</li><li><span class='xr-has-index'>batch_effect_dims</span>: 2</li><li><span class='xr-has-index'>statistic</span>: 11</li><li><span class='xr-has-index'>centile</span>: 5</li></ul></div></li><li class='xr-section-item'><input id='section-76c844ee-aed5-4b57-9d6c-4a9f02b73506' class='xr-section-summary-in' type='checkbox' checked /><label for='section-76c844ee-aed5-4b57-9d6c-4a9f02b73506' class='xr-section-summary' title='Expand/collapse section'>Coordinates: <span>(6)</span></label><div class='xr-section-inline-details'></div><div class='xr-section-details'><ul class='xr-var-list'><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>observations</span></div><div class='xr-var-dims'>(observations)</div><div class='xr-var-dtype'>int64</div><div class='xr-var-preview xr-preview'>0 1 2 3 4 ... 1074 1075 1076 1077</div><input id='attrs-51d740a8-78a5-4de3-ad75-eccc3f2860a2' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-51d740a8-78a5-4de3-ad75-eccc3f2860a2' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-4867bda3-4ce1-4785-b1ed-d072715268a5' class='xr-var-data-in' type='checkbox'><label for='data-4867bda3-4ce1-4785-b1ed-d072715268a5' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([   0,    1,    2, ..., 1075, 1076, 1077], shape=(1078,))</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>response_vars</span></div><div class='xr-var-dims'>(response_vars)</div><div class='xr-var-dtype'>&lt;U27</div><div class='xr-var-preview xr-preview'>&#x27;lh_G_and_S_frontomargin&#x27; ... &#x27;l...</div><input id='attrs-ce1e6af8-106e-4493-ac6c-7b8ce498577f' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-ce1e6af8-106e-4493-ac6c-7b8ce498577f' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-d1700196-efdc-4c6e-87ab-9c3c6973c771' class='xr-var-data-in' type='checkbox'><label for='data-d1700196-efdc-4c6e-87ab-9c3c6973c771' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([&#x27;lh_G_and_S_frontomargin&#x27;, &#x27;lh_G_and_S_occipital_inf&#x27;,
           &#x27;lh_G_and_S_paracentral&#x27;, &#x27;lh_G_and_S_subcentral&#x27;,
           &#x27;lh_G_and_S_transv_frontopol&#x27;], dtype=&#x27;&lt;U27&#x27;)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>covariates</span></div><div class='xr-var-dims'>(covariates)</div><div class='xr-var-dtype'>&lt;U3</div><div class='xr-var-preview xr-preview'>&#x27;age&#x27;</div><input id='attrs-2d8d5f81-e627-4f69-81e2-ee27503e6b50' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-2d8d5f81-e627-4f69-81e2-ee27503e6b50' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-d1dcc41d-7a2f-420d-863a-cb9a5bb700c2' class='xr-var-data-in' type='checkbox'><label for='data-d1dcc41d-7a2f-420d-863a-cb9a5bb700c2' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([&#x27;age&#x27;], dtype=&#x27;&lt;U3&#x27;)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>batch_effect_dims</span></div><div class='xr-var-dims'>(batch_effect_dims)</div><div class='xr-var-dtype'>&lt;U4</div><div class='xr-var-preview xr-preview'>&#x27;site&#x27; &#x27;sex&#x27;</div><input id='attrs-32a52109-1dfe-4129-9214-f4f4c9ad6d80' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-32a52109-1dfe-4129-9214-f4f4c9ad6d80' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-52e5bcaa-fedc-4cc9-9bbf-e0bf78e884f7' class='xr-var-data-in' type='checkbox'><label for='data-52e5bcaa-fedc-4cc9-9bbf-e0bf78e884f7' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([&#x27;site&#x27;, &#x27;sex&#x27;], dtype=&#x27;&lt;U4&#x27;)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>statistic</span></div><div class='xr-var-dims'>(statistic)</div><div class='xr-var-dtype'>&lt;U8</div><div class='xr-var-preview xr-preview'>&#x27;EXPV&#x27; &#x27;MACE&#x27; ... &#x27;SMSE&#x27; &#x27;ShapiroW&#x27;</div><input id='attrs-b424b50e-ed71-4d6b-8c70-b5385c92efe0' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-b424b50e-ed71-4d6b-8c70-b5385c92efe0' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-b1df4608-ccac-4575-b2ab-a9f12741efca' class='xr-var-data-in' type='checkbox'><label for='data-b1df4608-ccac-4575-b2ab-a9f12741efca' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([&#x27;EXPV&#x27;, &#x27;MACE&#x27;, &#x27;MAPE&#x27;, &#x27;MSLL&#x27;, &#x27;NLL&#x27;, &#x27;R2&#x27;, &#x27;RMSE&#x27;, &#x27;Rho&#x27;, &#x27;Rho_p&#x27;,
           &#x27;SMSE&#x27;, &#x27;ShapiroW&#x27;], dtype=&#x27;&lt;U8&#x27;)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>centile</span></div><div class='xr-var-dims'>(centile)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>0.05 0.25 0.5 0.75 0.95</div><input id='attrs-c8c9a6e7-37b8-48ca-b094-da45ae285d21' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-c8c9a6e7-37b8-48ca-b094-da45ae285d21' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-fa60b616-9f97-4159-b595-f3f7eccb122c' class='xr-var-data-in' type='checkbox'><label for='data-fa60b616-9f97-4159-b595-f3f7eccb122c' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([0.05, 0.25, 0.5 , 0.75, 0.95])</pre></div></li></ul></div></li><li class='xr-section-item'><input id='section-894c6167-c027-46cf-b165-832508e1b435' class='xr-section-summary-in' type='checkbox' checked /><label for='section-894c6167-c027-46cf-b165-832508e1b435' class='xr-section-summary' title='Expand/collapse section'>Data variables: <span>(10)</span></label><div class='xr-section-inline-details'></div><div class='xr-section-details'><ul class='xr-var-list'><li class='xr-var-item'><div class='xr-var-name'><span>subject_ids</span></div><div class='xr-var-dims'>(observations)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>&#x27;AnnArbor_a_sub04111&#x27; ... &#x27;Saint...</div><input id='attrs-990acb4d-5ab0-470a-9925-95a0180c96b3' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-990acb4d-5ab0-470a-9925-95a0180c96b3' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-08f7ccad-ce9b-4112-bd40-ea5aeb6c01f5' class='xr-var-data-in' type='checkbox'><label for='data-08f7ccad-ce9b-4112-bd40-ea5aeb6c01f5' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([&#x27;AnnArbor_a_sub04111&#x27;, &#x27;AnnArbor_a_sub04619&#x27;,
           &#x27;AnnArbor_a_sub13636&#x27;, ..., &#x27;SaintLouis_sub95967&#x27;,
           &#x27;SaintLouis_sub97935&#x27;, &#x27;SaintLouis_sub99965&#x27;],
          shape=(1078,), dtype=object)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>Y</span></div><div class='xr-var-dims'>(observations, response_vars)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>2.297 1.99 1.946 ... 2.701 2.713</div><input id='attrs-81258e4d-f794-45e8-8c02-4e69b948bc84' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-81258e4d-f794-45e8-8c02-4e69b948bc84' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-2b244cd8-bc01-4a7c-9d9e-7f363f13180c' class='xr-var-data-in' type='checkbox'><label for='data-2b244cd8-bc01-4a7c-9d9e-7f363f13180c' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[2.297, 1.99 , 1.946, 2.544, 2.649],
           [2.   , 2.258, 2.115, 2.389, 2.364],
           [2.35 , 2.624, 2.339, 2.578, 2.394],
           ...,
           [2.545, 2.512, 2.536, 2.795, 2.683],
           [2.369, 2.463, 2.488, 2.955, 2.491],
           [2.425, 2.281, 2.491, 2.701, 2.713]], shape=(1078, 5))</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>X</span></div><div class='xr-var-dims'>(observations, covariates)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>25.63 18.34 29.2 ... 27.0 29.0 23.0</div><input id='attrs-f120cebb-9827-4096-8f70-f1d5144432a4' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-f120cebb-9827-4096-8f70-f1d5144432a4' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-dbf2df93-3e34-47af-98f1-114da20b4280' class='xr-var-data-in' type='checkbox'><label for='data-dbf2df93-3e34-47af-98f1-114da20b4280' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[25.63],
           [18.34],
           [29.2 ],
           ...,
           [27.  ],
           [29.  ],
           [23.  ]], shape=(1078, 1))</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>batch_effects</span></div><div class='xr-var-dims'>(observations, batch_effect_dims)</div><div class='xr-var-dtype'>&lt;U17</div><div class='xr-var-preview xr-preview'>&#x27;AnnArbor_a&#x27; &#x27;M&#x27; ... &#x27;F&#x27;</div><input id='attrs-a8ffdec2-c537-4ab3-89cc-a215ce43f88c' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-a8ffdec2-c537-4ab3-89cc-a215ce43f88c' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-75c86f56-a48b-4363-a4a2-e2de7baf387e' class='xr-var-data-in' type='checkbox'><label for='data-75c86f56-a48b-4363-a4a2-e2de7baf387e' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[&#x27;AnnArbor_a&#x27;, &#x27;M&#x27;],
           [&#x27;AnnArbor_a&#x27;, &#x27;M&#x27;],
           [&#x27;AnnArbor_a&#x27;, &#x27;M&#x27;],
           ...,
           [&#x27;SaintLouis&#x27;, &#x27;M&#x27;],
           [&#x27;SaintLouis&#x27;, &#x27;F&#x27;],
           [&#x27;SaintLouis&#x27;, &#x27;F&#x27;]], shape=(1078, 2), dtype=&#x27;&lt;U17&#x27;)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>Z</span></div><div class='xr-var-dims'>(observations, response_vars)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>0.09048 -1.743 ... -0.4829 0.374</div><input id='attrs-bb8c0868-4f9c-408a-a035-897225c6c115' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-bb8c0868-4f9c-408a-a035-897225c6c115' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-b67c3808-c8d9-4a08-bc36-dcfc163f3243' class='xr-var-data-in' type='checkbox'><label for='data-b67c3808-c8d9-4a08-bc36-dcfc163f3243' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[ 0.09048246, -1.74343327, -1.73280504, -0.27931943,  0.46170464],
           [-2.15307041, -0.2155683 , -0.88112563, -1.66514532, -1.49058682],
           [ 0.53237432,  2.32689496,  1.05582115,  0.11430795, -0.79015658],
           ...,
           [ 1.06375743,  0.18651962,  0.35946146,  0.20245872,  0.61710398],
           [-0.0414054 , -0.04584035,  0.07680902,  1.43016538, -0.51893228],
           [ 0.08501928, -1.26394207, -0.12296366, -0.48292281,  0.37401496]],
          shape=(1078, 5))</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>baseline_logp</span></div><div class='xr-var-dims'>(observations, response_vars)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>-0.9971 -3.581 ... -0.919 -1.021</div><input id='attrs-04aa962a-d053-4fb5-9673-97810f5bd9bf' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-04aa962a-d053-4fb5-9673-97810f5bd9bf' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-6f8c4f81-79a6-4184-85a2-d039c4c79161' class='xr-var-data-in' type='checkbox'><label for='data-6f8c4f81-79a6-4184-85a2-d039c4c79161' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[-0.99707249, -3.5814037 , -2.7596024 , -1.27830063, -0.93320988],
           [-2.80347532, -1.1907573 , -1.44934121, -2.35678277, -1.51781187],
           [-0.92606713, -1.90896874, -0.9298906 , -1.13790237, -1.381113  ],
           ...,
           [-1.32009079, -1.20087654, -1.61983023, -1.05698856, -0.97003065],
           [-0.91914855, -1.02650136, -1.35283172, -1.90008247, -1.06015506],
           [-0.95566622, -1.10047564, -1.36765247, -0.91900098, -1.02138371]],
          shape=(1078, 5))</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>logp</span></div><div class='xr-var-dims'>(observations, response_vars)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>-0.7676 -2.387 ... -0.9341 -0.8585</div><input id='attrs-13caf6f1-3d94-42eb-bbe3-440616aaefad' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-13caf6f1-3d94-42eb-bbe3-440616aaefad' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-e94995c4-70f8-4c64-9017-03f1b9d3b798' class='xr-var-data-in' type='checkbox'><label for='data-e94995c4-70f8-4c64-9017-03f1b9d3b798' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[-0.76762804, -2.38670351, -2.18516379, -0.85699579, -0.86029717],
           [-3.1524295 , -0.92386454, -1.09107655, -2.23685454, -1.97711085],
           [-0.88429356, -3.57569009, -1.25864035, -0.82258548, -1.0318994 ],
           ...,
           [-1.31230983, -0.87307701, -0.74281395, -0.82899868, -0.91967917],
           [-0.73488719, -0.85471108, -0.69255238, -1.83205664, -0.84589115],
           [-0.7783036 , -1.65738917, -0.6810508 , -0.93407336, -0.85853176]],
          shape=(1078, 5))</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>Yhat</span></div><div class='xr-var-dims'>(observations, response_vars)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>2.283 2.265 2.2 ... 2.779 2.64</div><input id='attrs-84113603-ed92-4176-affc-77ff942aaf7b' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-84113603-ed92-4176-affc-77ff942aaf7b' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-afd7a3d9-145e-4ab2-86ea-aa08937634bd' class='xr-var-data-in' type='checkbox'><label for='data-afd7a3d9-145e-4ab2-86ea-aa08937634bd' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[2.28250086, 2.26484906, 2.19959119, 2.58893279, 2.56294337],
           [2.37080339, 2.2932932 , 2.24690986, 2.66684514, 2.67635199],
           [2.26674565, 2.2588321 , 2.18189798, 2.55952839, 2.53564253],
           ...,
           [2.37617507, 2.48256586, 2.48313693, 2.76239474, 2.57007622],
           [2.37544398, 2.47011667, 2.47654847, 2.72522749, 2.58420856],
           [2.41111174, 2.48059635, 2.50897854, 2.77941065, 2.64017218]],
          shape=(1078, 5))</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>statistics</span></div><div class='xr-var-dims'>(response_vars, statistic)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>0.2812 0.009833 ... 0.7406 0.993</div><input id='attrs-68d039f7-a0cf-4379-8dd2-cf1e06587b2c' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-68d039f7-a0cf-4379-8dd2-cf1e06587b2c' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-a383fad7-dc1b-4fd9-a4c9-73e7f0f1dbdf' class='xr-var-data-in' type='checkbox'><label for='data-a383fad7-dc1b-4fd9-a4c9-73e7f0f1dbdf' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[ 2.81175268e-001,  9.83302412e-003,  5.50718886e-002,
            -1.53076478e-001,  1.26586206e+000,  2.81174000e-001,
             1.62865824e-001,  4.65612965e-001,  4.18146055e-059,
             7.18826000e-001,  9.96679987e-001],
           [ 1.18383346e-001,  8.31168831e-003,  5.30658022e-002,
            -5.29632595e-002,  1.36597527e+000,  1.18379252e-001,
             1.60251915e-001,  3.23853691e-001,  9.63857634e-028,
             8.81620748e-001,  9.95696405e-001],
           [ 3.88109550e-001,  5.34322820e-003,  5.13168153e-002,
            -2.38110661e-001,  1.18082787e+000,  3.88108738e-001,
             1.48749883e-001,  5.92263347e-001,  4.79731615e-103,
             6.11891262e-001,  9.95245167e-001],
           [ 2.41710322e-001,  7.38404453e-003,  4.62470331e-002,
            -1.24691932e-001,  1.29424660e+000,  2.41710261e-001,
             1.59165369e-001,  4.87840034e-001,  1.55546500e-065,
             7.58289739e-001,  9.95462402e-001],
           [ 2.59435113e-001,  1.23933210e-002,  5.79990867e-002,
            -1.42256398e-001,  1.27668214e+000,  2.59435017e-001,
             1.94132679e-001,  4.65753458e-001,  3.82083876e-059,
             7.40564983e-001,  9.92956432e-001]])</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>centiles</span></div><div class='xr-var-dims'>(centile, observations, response_vars)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>2.019 2.005 1.959 ... 3.047 2.961</div><input id='attrs-45e4054f-5e99-4039-8c5d-1e9866dff246' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-45e4054f-5e99-4039-8c5d-1e9866dff246' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-0f198088-312a-4ff6-b977-b71994f6bc99' class='xr-var-data-in' type='checkbox'><label for='data-0f198088-312a-4ff6-b977-b71994f6bc99' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[[2.01860967, 2.00519274, 1.95857113, 2.32341354, 2.25606192],
            [2.08709986, 2.02335838, 2.00029083, 2.39205237, 2.33119835],
            [2.00897649, 2.00013944, 1.9366181 , 2.29479823, 2.24014366],
            ...,
            [2.1147458 , 2.22316096, 2.24097299, 2.49735522, 2.26851113],
            [2.1173582 , 2.21134852, 2.23160635, 2.4604885 , 2.28827119],
            [2.14198056, 2.22062744, 2.268048  , 2.51210533, 2.31962388]],
    
           [[2.17428947, 2.15837422, 2.10075835, 2.48005379, 2.43710336],
            [2.25446774, 2.18260355, 2.1457811 , 2.55416344, 2.53481806],
            [2.16104466, 2.15275242, 2.08131835, 2.45097296, 2.41447007],
            ...,
            [2.26897321, 2.37619412, 2.38383503, 2.65371246, 2.44641624],
            [2.26961315, 2.36400603, 2.37610735, 2.61666845, 2.46285629],
            [2.30075164, 2.37399333, 2.41018241, 2.66979926, 2.50872793]],
    
           [[2.28250086, 2.26484906, 2.19959119, 2.58893279, 2.56294337],
            [2.37080339, 2.2932932 , 2.24690986, 2.66684514, 2.67635199],
            [2.26674565, 2.2588321 , 2.18189798, 2.55952839, 2.53564253],
            ...,
            [2.37617507, 2.48256586, 2.48313693, 2.76239474, 2.57007622],
            [2.37544398, 2.47011667, 2.47654847, 2.72522749, 2.58420856],
            [2.41111174, 2.48059635, 2.50897854, 2.77941065, 2.64017218]],
    
           [[2.39071226, 2.3713239 , 2.29842404, 2.69781179, 2.68878338],
            [2.48713905, 2.40398284, 2.34803861, 2.77952685, 2.81788591],
            [2.37244663, 2.36491178, 2.28247761, 2.66808381, 2.65681499],
            ...,
            [2.48337692, 2.58893761, 2.58243884, 2.87107702, 2.69373619],
            [2.4812748 , 2.5762273 , 2.57698959, 2.83378653, 2.70556083],
            [2.52147185, 2.58719937, 2.60777468, 2.88902204, 2.77161643]],
    
           [[2.54639206, 2.52450538, 2.44061125, 2.85445204, 2.86982482],
            [2.65450693, 2.56322802, 2.49352888, 2.94163792, 3.02150562],
            [2.5245148 , 2.51752476, 2.42717786, 2.82425855, 2.83114141],
            ...,
            [2.63760434, 2.74197077, 2.72530088, 3.02743427, 2.8716413 ],
            [2.63352976, 2.72888481, 2.72149058, 2.98996648, 2.88014594],
            [2.68024292, 2.74056525, 2.74990908, 3.04671597, 2.96072049]]],
          shape=(5, 1078, 5))</pre></div></li></ul></div></li><li class='xr-section-item'><input id='section-c5bc370a-2ef7-499c-ad83-c9bacff42608' class='xr-section-summary-in' type='checkbox' checked /><label for='section-c5bc370a-2ef7-499c-ad83-c9bacff42608' class='xr-section-summary' title='Expand/collapse section'>Attributes: <span>(7)</span></label><div class='xr-section-inline-details'></div><div class='xr-section-details'><dl class='xr-attrs'><dt><span>real_ids :</span></dt><dd>True</dd><dt><span>is_scaled :</span></dt><dd>False</dd><dt><span>name :</span></dt><dd>fcon1000</dd><dt><span>unique_batch_effects :</span></dt><dd>{np.str_(&#x27;site&#x27;): [&#x27;AnnArbor_a&#x27;, &#x27;AnnArbor_b&#x27;, &#x27;Atlanta&#x27;, &#x27;Baltimore&#x27;, &#x27;Bangor&#x27;, &#x27;Beijing_Zang&#x27;, &#x27;Berlin_Margulies&#x27;, &#x27;Cambridge_Buckner&#x27;, &#x27;Cleveland&#x27;, &#x27;ICBM&#x27;, &#x27;Leiden_2180&#x27;, &#x27;Leiden_2200&#x27;, &#x27;Milwaukee_b&#x27;, &#x27;Munchen&#x27;, &#x27;NewYork_a&#x27;, &#x27;NewYork_a_ADHD&#x27;, &#x27;Newark&#x27;, &#x27;Oulu&#x27;, &#x27;Oxford&#x27;, &#x27;PaloAlto&#x27;, &#x27;Pittsburgh&#x27;, &#x27;Queensland&#x27;, &#x27;SaintLouis&#x27;], np.str_(&#x27;sex&#x27;): [&#x27;M&#x27;, &#x27;F&#x27;]}</dd><dt><span>batch_effect_counts :</span></dt><dd>defaultdict(&lt;function NormData.register_batch_effects.&lt;locals&gt;.&lt;lambda&gt; at 0x163d90a40&gt;, {np.str_(&#x27;site&#x27;): {&#x27;AnnArbor_a&#x27;: 24, &#x27;AnnArbor_b&#x27;: 32, &#x27;Atlanta&#x27;: 28, &#x27;Baltimore&#x27;: 23, &#x27;Bangor&#x27;: 20, &#x27;Beijing_Zang&#x27;: 198, &#x27;Berlin_Margulies&#x27;: 26, &#x27;Cambridge_Buckner&#x27;: 198, &#x27;Cleveland&#x27;: 31, &#x27;ICBM&#x27;: 85, &#x27;Leiden_2180&#x27;: 12, &#x27;Leiden_2200&#x27;: 19, &#x27;Milwaukee_b&#x27;: 46, &#x27;Munchen&#x27;: 15, &#x27;NewYork_a&#x27;: 83, &#x27;NewYork_a_ADHD&#x27;: 25, &#x27;Newark&#x27;: 19, &#x27;Oulu&#x27;: 102, &#x27;Oxford&#x27;: 22, &#x27;PaloAlto&#x27;: 17, &#x27;Pittsburgh&#x27;: 3, &#x27;Queensland&#x27;: 19, &#x27;SaintLouis&#x27;: 31}, np.str_(&#x27;sex&#x27;): {&#x27;M&#x27;: 489, &#x27;F&#x27;: 589}})</dd><dt><span>covariate_ranges :</span></dt><dd>{np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 7.88, &#x27;max&#x27;: 85.0}}</dd><dt><span>batch_effect_covariate_ranges :</span></dt><dd>{np.str_(&#x27;site&#x27;): {&#x27;AnnArbor_a&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 13.41, &#x27;max&#x27;: 40.98}}, &#x27;AnnArbor_b&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 19.0, &#x27;max&#x27;: 79.0}}, &#x27;Atlanta&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 22.0, &#x27;max&#x27;: 57.0}}, &#x27;Baltimore&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 20.0, &#x27;max&#x27;: 40.0}}, &#x27;Bangor&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 19.0, &#x27;max&#x27;: 38.0}}, &#x27;Beijing_Zang&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 18.0, &#x27;max&#x27;: 26.0}}, &#x27;Berlin_Margulies&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 23.0, &#x27;max&#x27;: 44.0}}, &#x27;Cambridge_Buckner&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 18.0, &#x27;max&#x27;: 30.0}}, &#x27;Cleveland&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 24.0, &#x27;max&#x27;: 60.0}}, &#x27;ICBM&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 19.0, &#x27;max&#x27;: 85.0}}, &#x27;Leiden_2180&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 20.0, &#x27;max&#x27;: 27.0}}, &#x27;Leiden_2200&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 18.0, &#x27;max&#x27;: 28.0}}, &#x27;Milwaukee_b&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 44.0, &#x27;max&#x27;: 65.0}}, &#x27;Munchen&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 63.0, &#x27;max&#x27;: 74.0}}, &#x27;NewYork_a&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 7.88, &#x27;max&#x27;: 49.16}}, &#x27;NewYork_a_ADHD&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 20.69, &#x27;max&#x27;: 50.9}}, &#x27;Newark&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 21.0, &#x27;max&#x27;: 39.0}}, &#x27;Oulu&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 20.0, &#x27;max&#x27;: 23.0}}, &#x27;Oxford&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 20.0, &#x27;max&#x27;: 35.0}}, &#x27;PaloAlto&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 22.0, &#x27;max&#x27;: 46.0}}, &#x27;Pittsburgh&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 25.0, &#x27;max&#x27;: 47.0}}, &#x27;Queensland&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 20.0, &#x27;max&#x27;: 34.0}}, &#x27;SaintLouis&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 21.0, &#x27;max&#x27;: 29.0}}}, np.str_(&#x27;sex&#x27;): {&#x27;M&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 9.21, &#x27;max&#x27;: 78.0}}, &#x27;F&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 7.88, &#x27;max&#x27;: 85.0}}}}</dd></dl></div></li></ul></div></div>



7. Visualize Centiles Under a Chosen Reference Batch
----------------------------------------------------

Plot centiles after harmonizing to a chosen reference combination (for
example, NewYork_a and F), while still showing selected comparison
sites.

.. code:: ipython3

    import os
    
    # Pass explicit reference batch effects via the first entry in each batch-effect list.
    # Here reference = site='NewYork_a', sex='F'.
    reference_site = "NewYork_a"
    reference_sex = "F"
    sites_to_plot = ["Oulu", "Leiden_2180"]
    
    available_sites = list(model2.batch_effects_maps["site"].keys())
    available_sex = list(model2.batch_effects_maps["sex"].keys())
    
    unknown_sites = [s for s in [reference_site] + sites_to_plot if s not in available_sites]
    if unknown_sites:
        raise ValueError(f"Unknown site(s) for this fitted model: {unknown_sites}\nAvailable examples: {available_sites[:10]}")
    if reference_sex not in available_sex:
        raise ValueError(f"Unknown sex '{reference_sex}'. Available: {available_sex}")
    
    plot_batch_effects = {
        "site": [reference_site] + sites_to_plot,
        "sex": [reference_sex, "M" if reference_sex == "F" else "F"],
    }
    
    save_dir = "out/plots"
    os.makedirs(save_dir, exist_ok=True)
    
    plot_centiles_advanced(
        model=model2,
        scatter_data=reference_norm_data,
        covariate="age",
        response_vars=[response_variables[0]],
        batch_effects=plot_batch_effects,
        harmonize_data=True,
        save_dir=save_dir,
    )


.. parsed-literal::

    Process: 75540 - 2026-06-11 12:16:02 - Dataset "centile" created.
        - 150 observations
        - 150 unique subjects
        - 1 covariates
        - 1 response variables
        - 2 batch effects:
        	site (1)
    	sex (1)
        
    Process: 75540 - 2026-06-11 12:16:02 - Computing centiles for 1 response variables.
    Process: 75540 - 2026-06-11 12:16:02 - Computing centiles for lh_G_and_S_frontomargin.
    Process: 75540 - 2026-06-11 12:16:04 - Harmonizing data on 1 response variables.
    Process: 75540 - 2026-06-11 12:16:04 - Harmonizing data for lh_G_and_S_frontomargin.


B: Harmonisation to unseen sites and data
=========================================

8. Harmonize to External Reference Model
----------------------------------------

We use the pre-trained model that we have loaded at the beginning. This
way. we apply harmonization using the pre-fitted main workflow model to
map this dataset into the external reference space. Again, our data from
the fcon1000 data set needs to be in the ``normdata`` format.

.. code:: ipython3

    # Harmonize data
    model.harmonize(reference_norm_data)


.. parsed-literal::

    Process: 75540 - 2026-06-11 12:16:48 - Harmonizing data on 5 response variables.
    Process: 75540 - 2026-06-11 12:16:48 - Harmonizing data for lh_G_and_S_frontomargin.
    Process: 75540 - 2026-06-11 12:16:51 - Harmonizing data for lh_G_and_S_paracentral.
    Process: 75540 - 2026-06-11 12:16:53 - Harmonizing data for lh_G_and_S_transv_frontopol.
    Process: 75540 - 2026-06-11 12:16:54 - Harmonizing data for lh_G_and_S_subcentral.
    Process: 75540 - 2026-06-11 12:16:56 - Harmonizing data for lh_G_and_S_occipital_inf.




.. raw:: html

    <div><svg style="position: absolute; width: 0; height: 0; overflow: hidden">
    <defs>
    <symbol id="icon-database" viewBox="0 0 32 32">
    <path d="M16 0c-8.837 0-16 2.239-16 5v4c0 2.761 7.163 5 16 5s16-2.239 16-5v-4c0-2.761-7.163-5-16-5z"></path>
    <path d="M16 17c-8.837 0-16-2.239-16-5v6c0 2.761 7.163 5 16 5s16-2.239 16-5v-6c0 2.761-7.163 5-16 5z"></path>
    <path d="M16 26c-8.837 0-16-2.239-16-5v6c0 2.761 7.163 5 16 5s16-2.239 16-5v-6c0 2.761-7.163 5-16 5z"></path>
    </symbol>
    <symbol id="icon-file-text2" viewBox="0 0 32 32">
    <path d="M28.681 7.159c-0.694-0.947-1.662-2.053-2.724-3.116s-2.169-2.030-3.116-2.724c-1.612-1.182-2.393-1.319-2.841-1.319h-15.5c-1.378 0-2.5 1.121-2.5 2.5v27c0 1.378 1.122 2.5 2.5 2.5h23c1.378 0 2.5-1.122 2.5-2.5v-19.5c0-0.448-0.137-1.23-1.319-2.841zM24.543 5.457c0.959 0.959 1.712 1.825 2.268 2.543h-4.811v-4.811c0.718 0.556 1.584 1.309 2.543 2.268zM28 29.5c0 0.271-0.229 0.5-0.5 0.5h-23c-0.271 0-0.5-0.229-0.5-0.5v-27c0-0.271 0.229-0.5 0.5-0.5 0 0 15.499-0 15.5 0v7c0 0.552 0.448 1 1 1h7v19.5z"></path>
    <path d="M23 26h-14c-0.552 0-1-0.448-1-1s0.448-1 1-1h14c0.552 0 1 0.448 1 1s-0.448 1-1 1z"></path>
    <path d="M23 22h-14c-0.552 0-1-0.448-1-1s0.448-1 1-1h14c0.552 0 1 0.448 1 1s-0.448 1-1 1z"></path>
    <path d="M23 18h-14c-0.552 0-1-0.448-1-1s0.448-1 1-1h14c0.552 0 1 0.448 1 1s-0.448 1-1 1z"></path>
    </symbol>
    </defs>
    </svg>
    <style>/* CSS stylesheet for displaying xarray objects in notebooks */
    
    :root {
      --xr-font-color0: var(
        --jp-content-font-color0,
        var(--pst-color-text-base rgba(0, 0, 0, 1))
      );
      --xr-font-color2: var(
        --jp-content-font-color2,
        var(--pst-color-text-base, rgba(0, 0, 0, 0.54))
      );
      --xr-font-color3: var(
        --jp-content-font-color3,
        var(--pst-color-text-base, rgba(0, 0, 0, 0.38))
      );
      --xr-border-color: var(
        --jp-border-color2,
        hsl(from var(--pst-color-on-background, white) h s calc(l - 10))
      );
      --xr-disabled-color: var(
        --jp-layout-color3,
        hsl(from var(--pst-color-on-background, white) h s calc(l - 40))
      );
      --xr-background-color: var(
        --jp-layout-color0,
        var(--pst-color-on-background, white)
      );
      --xr-background-color-row-even: var(
        --jp-layout-color1,
        hsl(from var(--pst-color-on-background, white) h s calc(l - 5))
      );
      --xr-background-color-row-odd: var(
        --jp-layout-color2,
        hsl(from var(--pst-color-on-background, white) h s calc(l - 15))
      );
    }
    
    html[theme="dark"],
    html[data-theme="dark"],
    body[data-theme="dark"],
    body.vscode-dark {
      --xr-font-color0: var(
        --jp-content-font-color0,
        var(--pst-color-text-base, rgba(255, 255, 255, 1))
      );
      --xr-font-color2: var(
        --jp-content-font-color2,
        var(--pst-color-text-base, rgba(255, 255, 255, 0.54))
      );
      --xr-font-color3: var(
        --jp-content-font-color3,
        var(--pst-color-text-base, rgba(255, 255, 255, 0.38))
      );
      --xr-border-color: var(
        --jp-border-color2,
        hsl(from var(--pst-color-on-background, #111111) h s calc(l + 10))
      );
      --xr-disabled-color: var(
        --jp-layout-color3,
        hsl(from var(--pst-color-on-background, #111111) h s calc(l + 40))
      );
      --xr-background-color: var(
        --jp-layout-color0,
        var(--pst-color-on-background, #111111)
      );
      --xr-background-color-row-even: var(
        --jp-layout-color1,
        hsl(from var(--pst-color-on-background, #111111) h s calc(l + 5))
      );
      --xr-background-color-row-odd: var(
        --jp-layout-color2,
        hsl(from var(--pst-color-on-background, #111111) h s calc(l + 15))
      );
    }
    
    .xr-wrap {
      display: block !important;
      min-width: 300px;
      max-width: 700px;
      line-height: 1.6;
      padding-bottom: 4px;
    }
    
    .xr-text-repr-fallback {
      /* fallback to plain text repr when CSS is not injected (untrusted notebook) */
      display: none;
    }
    
    .xr-header {
      padding-top: 6px;
      padding-bottom: 6px;
    }
    
    .xr-header {
      border-bottom: solid 1px var(--xr-border-color);
      margin-bottom: 4px;
    }
    
    .xr-header > div,
    .xr-header > ul {
      display: inline;
      margin-top: 0;
      margin-bottom: 0;
    }
    
    .xr-obj-type,
    .xr-obj-name {
      margin-left: 2px;
      margin-right: 10px;
    }
    
    .xr-obj-type,
    .xr-group-box-contents > label {
      color: var(--xr-font-color2);
      display: block;
    }
    
    .xr-sections {
      padding-left: 0 !important;
      display: grid;
      grid-template-columns: 150px auto auto 1fr 0 20px 0 20px;
      margin-block-start: 0;
      margin-block-end: 0;
    }
    
    .xr-section-item {
      display: contents;
    }
    
    .xr-section-item > input,
    .xr-group-box-contents > input,
    .xr-array-wrap > input {
      display: block;
      opacity: 0;
      height: 0;
      margin: 0;
    }
    
    .xr-section-item > input + label,
    .xr-var-item > input + label {
      color: var(--xr-disabled-color);
    }
    
    .xr-section-item > input:enabled + label,
    .xr-var-item > input:enabled + label,
    .xr-array-wrap > input:enabled + label,
    .xr-group-box-contents > input:enabled + label {
      cursor: pointer;
      color: var(--xr-font-color2);
    }
    
    .xr-section-item > input:focus-visible + label,
    .xr-var-item > input:focus-visible + label,
    .xr-array-wrap > input:focus-visible + label,
    .xr-group-box-contents > input:focus-visible + label {
      outline: auto;
    }
    
    .xr-section-item > input:enabled + label:hover,
    .xr-var-item > input:enabled + label:hover,
    .xr-array-wrap > input:enabled + label:hover,
    .xr-group-box-contents > input:enabled + label:hover {
      color: var(--xr-font-color0);
    }
    
    .xr-section-summary {
      grid-column: 1;
      color: var(--xr-font-color2);
      font-weight: 500;
      white-space: nowrap;
    }
    
    .xr-section-summary > em {
      font-weight: normal;
    }
    
    .xr-span-grid {
      grid-column-end: -1;
    }
    
    .xr-section-summary > span {
      display: inline-block;
      padding-left: 0.3em;
    }
    
    .xr-group-box-contents > input:checked + label > span {
      display: inline-block;
      padding-left: 0.6em;
    }
    
    .xr-section-summary-in:disabled + label {
      color: var(--xr-font-color2);
    }
    
    .xr-section-summary-in + label:before {
      display: inline-block;
      content: "►";
      font-size: 11px;
      width: 15px;
      text-align: center;
    }
    
    .xr-section-summary-in:disabled + label:before {
      color: var(--xr-disabled-color);
    }
    
    .xr-section-summary-in:checked + label:before {
      content: "▼";
    }
    
    .xr-section-summary-in:checked + label > span {
      display: none;
    }
    
    .xr-section-summary,
    .xr-section-inline-details,
    .xr-group-box-contents > label {
      padding-top: 4px;
    }
    
    .xr-section-inline-details {
      grid-column: 2 / -1;
    }
    
    .xr-section-details {
      grid-column: 1 / -1;
      margin-top: 4px;
      margin-bottom: 5px;
    }
    
    .xr-section-summary-in ~ .xr-section-details {
      display: none;
    }
    
    .xr-section-summary-in:checked ~ .xr-section-details {
      display: contents;
    }
    
    .xr-children {
      display: inline-grid;
      grid-template-columns: 100%;
      grid-column: 1 / -1;
      padding-top: 4px;
    }
    
    .xr-group-box {
      display: inline-grid;
      grid-template-columns: 0px 30px auto;
    }
    
    .xr-group-box-vline {
      grid-column-start: 1;
      border-right: 0.2em solid;
      border-color: var(--xr-border-color);
      width: 0px;
    }
    
    .xr-group-box-hline {
      grid-column-start: 2;
      grid-row-start: 1;
      height: 1em;
      width: 26px;
      border-bottom: 0.2em solid;
      border-color: var(--xr-border-color);
    }
    
    .xr-group-box-contents {
      grid-column-start: 3;
      padding-bottom: 4px;
    }
    
    .xr-group-box-contents > label::before {
      content: "📂";
      padding-right: 0.3em;
    }
    
    .xr-group-box-contents > input:checked + label::before {
      content: "📁";
    }
    
    .xr-group-box-contents > input:checked + label {
      padding-bottom: 0px;
    }
    
    .xr-group-box-contents > input:checked ~ .xr-sections {
      display: none;
    }
    
    .xr-group-box-contents > input + label > span {
      display: none;
    }
    
    .xr-group-box-ellipsis {
      font-size: 1.4em;
      font-weight: 900;
      color: var(--xr-font-color2);
      letter-spacing: 0.15em;
      cursor: default;
    }
    
    .xr-array-wrap {
      grid-column: 1 / -1;
      display: grid;
      grid-template-columns: 20px auto;
    }
    
    .xr-array-wrap > label {
      grid-column: 1;
      vertical-align: top;
    }
    
    .xr-preview {
      color: var(--xr-font-color3);
    }
    
    .xr-array-preview,
    .xr-array-data {
      padding: 0 5px !important;
      grid-column: 2;
    }
    
    .xr-array-data,
    .xr-array-in:checked ~ .xr-array-preview {
      display: none;
    }
    
    .xr-array-in:checked ~ .xr-array-data,
    .xr-array-preview {
      display: inline-block;
    }
    
    .xr-dim-list {
      display: inline-block !important;
      list-style: none;
      padding: 0 !important;
      margin: 0;
    }
    
    .xr-dim-list li {
      display: inline-block;
      padding: 0;
      margin: 0;
    }
    
    .xr-dim-list:before {
      content: "(";
    }
    
    .xr-dim-list:after {
      content: ")";
    }
    
    .xr-dim-list li:not(:last-child):after {
      content: ",";
      padding-right: 5px;
    }
    
    .xr-has-index {
      font-weight: bold;
    }
    
    .xr-var-list,
    .xr-var-item {
      display: contents;
    }
    
    .xr-var-item > div,
    .xr-var-item label,
    .xr-var-item > .xr-var-name span {
      background-color: var(--xr-background-color-row-even);
      border-color: var(--xr-background-color-row-odd);
      margin-bottom: 0;
      padding-top: 2px;
    }
    
    .xr-var-item > .xr-var-name:hover span {
      padding-right: 5px;
    }
    
    .xr-var-list > li:nth-child(odd) > div,
    .xr-var-list > li:nth-child(odd) > label,
    .xr-var-list > li:nth-child(odd) > .xr-var-name span {
      background-color: var(--xr-background-color-row-odd);
      border-color: var(--xr-background-color-row-even);
    }
    
    .xr-var-name {
      grid-column: 1;
    }
    
    .xr-var-dims {
      grid-column: 2;
    }
    
    .xr-var-dtype {
      grid-column: 3;
      text-align: right;
      color: var(--xr-font-color2);
    }
    
    .xr-var-preview {
      grid-column: 4;
    }
    
    .xr-index-preview {
      grid-column: 2 / 5;
      color: var(--xr-font-color2);
    }
    
    .xr-var-name,
    .xr-var-dims,
    .xr-var-dtype,
    .xr-preview,
    .xr-attrs dt {
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      padding-right: 10px;
    }
    
    .xr-var-name:hover,
    .xr-var-dims:hover,
    .xr-var-dtype:hover,
    .xr-attrs dt:hover {
      overflow: visible;
      width: auto;
      z-index: 1;
    }
    
    .xr-var-attrs,
    .xr-var-data,
    .xr-index-data {
      display: none;
      border-top: 2px dotted var(--xr-background-color);
      padding-bottom: 20px !important;
      padding-top: 10px !important;
    }
    
    .xr-var-attrs-in + label,
    .xr-var-data-in + label,
    .xr-index-data-in + label {
      padding: 0 1px;
    }
    
    .xr-var-attrs-in:checked ~ .xr-var-attrs,
    .xr-var-data-in:checked ~ .xr-var-data,
    .xr-index-data-in:checked ~ .xr-index-data {
      display: block;
    }
    
    .xr-var-data > table {
      float: right;
    }
    
    .xr-var-data > pre,
    .xr-index-data > pre,
    .xr-var-data > table > tbody > tr {
      background-color: transparent !important;
    }
    
    .xr-var-name span,
    .xr-var-data,
    .xr-index-name div,
    .xr-index-data,
    .xr-attrs {
      padding-left: 25px !important;
    }
    
    .xr-attrs,
    .xr-var-attrs,
    .xr-var-data,
    .xr-index-data {
      grid-column: 1 / -1;
    }
    
    dl.xr-attrs {
      padding: 0;
      margin: 0;
      display: grid;
      grid-template-columns: 125px auto;
    }
    
    .xr-attrs dt,
    .xr-attrs dd {
      padding: 0;
      margin: 0;
      float: left;
      padding-right: 10px;
      width: auto;
    }
    
    .xr-attrs dt {
      font-weight: normal;
      grid-column: 1;
    }
    
    .xr-attrs dt:hover span {
      display: inline-block;
      background: var(--xr-background-color);
      padding-right: 10px;
    }
    
    .xr-attrs dd {
      grid-column: 2;
      white-space: pre-wrap;
      word-break: break-all;
    }
    
    .xr-icon-database,
    .xr-icon-file-text2,
    .xr-no-icon {
      display: inline-block;
      vertical-align: middle;
      width: 1em;
      height: 1.5em !important;
      stroke-width: 0;
      stroke: currentColor;
      fill: currentColor;
    }
    
    .xr-var-attrs-in:checked + label > .xr-icon-file-text2,
    .xr-var-data-in:checked + label > .xr-icon-database,
    .xr-index-data-in:checked + label > .xr-icon-database {
      color: var(--xr-font-color0);
      filter: drop-shadow(1px 1px 5px var(--xr-font-color2));
      stroke-width: 0.8px;
    }
    </style><pre class='xr-text-repr-fallback'>&lt;xarray.NormData&gt; Size: 648kB
    Dimensions:            (observations: 1078, response_vars: 5, covariates: 1,
                            batch_effect_dims: 2, statistic: 11, centile: 5)
    Coordinates:
      * observations       (observations) int64 9kB 0 1 2 3 ... 1074 1075 1076 1077
      * response_vars      (response_vars) &lt;U27 540B &#x27;lh_G_and_S_frontomargin&#x27; .....
      * covariates         (covariates) &lt;U3 12B &#x27;age&#x27;
      * batch_effect_dims  (batch_effect_dims) &lt;U4 32B &#x27;site&#x27; &#x27;sex&#x27;
      * statistic          (statistic) &lt;U8 352B &#x27;EXPV&#x27; &#x27;MACE&#x27; ... &#x27;SMSE&#x27; &#x27;ShapiroW&#x27;
      * centile            (centile) float64 40B 0.05 0.25 0.5 0.75 0.95
    Data variables:
        subject_ids        (observations) object 9kB &#x27;AnnArbor_a_sub04111&#x27; ... &#x27;S...
        Y                  (observations, response_vars) float64 43kB 2.297 ... 2...
        X                  (observations, covariates) float64 9kB 25.63 ... 23.0
        batch_effects      (observations, batch_effect_dims) &lt;U17 147kB &#x27;AnnArbor...
        Z                  (observations, response_vars) float64 43kB 0.09048 ......
        baseline_logp      (observations, response_vars) float64 43kB -0.9971 ......
        logp               (observations, response_vars) float64 43kB -0.7676 ......
        Yhat               (observations, response_vars) float64 43kB 2.283 ... 2.64
        statistics         (response_vars, statistic) float64 440B 0.2812 ... 0.993
        centiles           (centile, observations, response_vars) float64 216kB 2...
        Y_harmonized       (observations, response_vars) float64 43kB 2.297 ... 2...
    Attributes:
        real_ids:                       True
        is_scaled:                      False
        name:                           fcon1000
        unique_batch_effects:           {np.str_(&#x27;site&#x27;): [&#x27;AnnArbor_a&#x27;, &#x27;AnnArbo...
        batch_effect_counts:            defaultdict(&lt;function NormData.register_b...
        covariate_ranges:               {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 7.88, &#x27;max&#x27;: 85.0}}
        batch_effect_covariate_ranges:  {np.str_(&#x27;site&#x27;): {&#x27;AnnArbor_a&#x27;: {np.str_...</pre><div class='xr-wrap' style='display:none'><div class='xr-header'><div class='xr-obj-type'>xarray.NormData</div></div><ul class='xr-sections'><li class='xr-section-item'><input id='section-dc0366fa-3454-4d76-a659-6950142de193' class='xr-section-summary-in' type='checkbox' disabled /><label for='section-dc0366fa-3454-4d76-a659-6950142de193' class='xr-section-summary'>Dimensions:</label><div class='xr-section-inline-details'><ul class='xr-dim-list'><li><span class='xr-has-index'>observations</span>: 1078</li><li><span class='xr-has-index'>response_vars</span>: 5</li><li><span class='xr-has-index'>covariates</span>: 1</li><li><span class='xr-has-index'>batch_effect_dims</span>: 2</li><li><span class='xr-has-index'>statistic</span>: 11</li><li><span class='xr-has-index'>centile</span>: 5</li></ul></div></li><li class='xr-section-item'><input id='section-8fcee6d5-4c4a-4b89-ab0b-25207e7623de' class='xr-section-summary-in' type='checkbox' checked /><label for='section-8fcee6d5-4c4a-4b89-ab0b-25207e7623de' class='xr-section-summary' title='Expand/collapse section'>Coordinates: <span>(6)</span></label><div class='xr-section-inline-details'></div><div class='xr-section-details'><ul class='xr-var-list'><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>observations</span></div><div class='xr-var-dims'>(observations)</div><div class='xr-var-dtype'>int64</div><div class='xr-var-preview xr-preview'>0 1 2 3 4 ... 1074 1075 1076 1077</div><input id='attrs-46f6d0b0-0ecb-48da-a459-134e924d21b0' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-46f6d0b0-0ecb-48da-a459-134e924d21b0' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-5365b96c-dedd-468f-a8c6-f314c0f6a3bd' class='xr-var-data-in' type='checkbox'><label for='data-5365b96c-dedd-468f-a8c6-f314c0f6a3bd' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([   0,    1,    2, ..., 1075, 1076, 1077], shape=(1078,))</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>response_vars</span></div><div class='xr-var-dims'>(response_vars)</div><div class='xr-var-dtype'>&lt;U27</div><div class='xr-var-preview xr-preview'>&#x27;lh_G_and_S_frontomargin&#x27; ... &#x27;l...</div><input id='attrs-7d41f06e-7dcc-41fc-8d04-747a8f4663f8' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-7d41f06e-7dcc-41fc-8d04-747a8f4663f8' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-0c585b64-bee5-474d-b3d4-aba6487dd5a7' class='xr-var-data-in' type='checkbox'><label for='data-0c585b64-bee5-474d-b3d4-aba6487dd5a7' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([&#x27;lh_G_and_S_frontomargin&#x27;, &#x27;lh_G_and_S_occipital_inf&#x27;,
           &#x27;lh_G_and_S_paracentral&#x27;, &#x27;lh_G_and_S_subcentral&#x27;,
           &#x27;lh_G_and_S_transv_frontopol&#x27;], dtype=&#x27;&lt;U27&#x27;)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>covariates</span></div><div class='xr-var-dims'>(covariates)</div><div class='xr-var-dtype'>&lt;U3</div><div class='xr-var-preview xr-preview'>&#x27;age&#x27;</div><input id='attrs-80ddd35c-2b01-4532-96fc-d376dbace2b5' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-80ddd35c-2b01-4532-96fc-d376dbace2b5' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-3a01a457-6180-4640-85bf-242604042a9c' class='xr-var-data-in' type='checkbox'><label for='data-3a01a457-6180-4640-85bf-242604042a9c' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([&#x27;age&#x27;], dtype=&#x27;&lt;U3&#x27;)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>batch_effect_dims</span></div><div class='xr-var-dims'>(batch_effect_dims)</div><div class='xr-var-dtype'>&lt;U4</div><div class='xr-var-preview xr-preview'>&#x27;site&#x27; &#x27;sex&#x27;</div><input id='attrs-01ce1ff7-36cb-4b7c-bea9-a010f82d9fee' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-01ce1ff7-36cb-4b7c-bea9-a010f82d9fee' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-44b6aaee-2c28-400d-a6d0-daf4eecef3ae' class='xr-var-data-in' type='checkbox'><label for='data-44b6aaee-2c28-400d-a6d0-daf4eecef3ae' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([&#x27;site&#x27;, &#x27;sex&#x27;], dtype=&#x27;&lt;U4&#x27;)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>statistic</span></div><div class='xr-var-dims'>(statistic)</div><div class='xr-var-dtype'>&lt;U8</div><div class='xr-var-preview xr-preview'>&#x27;EXPV&#x27; &#x27;MACE&#x27; ... &#x27;SMSE&#x27; &#x27;ShapiroW&#x27;</div><input id='attrs-809a41bb-0cc1-4aeb-bd92-7675a5b37f89' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-809a41bb-0cc1-4aeb-bd92-7675a5b37f89' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-0ccddd95-b7a8-473c-87fc-d304c1f466a0' class='xr-var-data-in' type='checkbox'><label for='data-0ccddd95-b7a8-473c-87fc-d304c1f466a0' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([&#x27;EXPV&#x27;, &#x27;MACE&#x27;, &#x27;MAPE&#x27;, &#x27;MSLL&#x27;, &#x27;NLL&#x27;, &#x27;R2&#x27;, &#x27;RMSE&#x27;, &#x27;Rho&#x27;, &#x27;Rho_p&#x27;,
           &#x27;SMSE&#x27;, &#x27;ShapiroW&#x27;], dtype=&#x27;&lt;U8&#x27;)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span class='xr-has-index'>centile</span></div><div class='xr-var-dims'>(centile)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>0.05 0.25 0.5 0.75 0.95</div><input id='attrs-da9f6f73-8d3f-4a21-a012-589c5ad4a007' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-da9f6f73-8d3f-4a21-a012-589c5ad4a007' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-c387b95a-27be-470e-801e-003dad51734d' class='xr-var-data-in' type='checkbox'><label for='data-c387b95a-27be-470e-801e-003dad51734d' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([0.05, 0.25, 0.5 , 0.75, 0.95])</pre></div></li></ul></div></li><li class='xr-section-item'><input id='section-de0b0548-1d5a-43e6-a835-f967b4471eeb' class='xr-section-summary-in' type='checkbox' checked /><label for='section-de0b0548-1d5a-43e6-a835-f967b4471eeb' class='xr-section-summary' title='Expand/collapse section'>Data variables: <span>(11)</span></label><div class='xr-section-inline-details'></div><div class='xr-section-details'><ul class='xr-var-list'><li class='xr-var-item'><div class='xr-var-name'><span>subject_ids</span></div><div class='xr-var-dims'>(observations)</div><div class='xr-var-dtype'>object</div><div class='xr-var-preview xr-preview'>&#x27;AnnArbor_a_sub04111&#x27; ... &#x27;Saint...</div><input id='attrs-4d6ff258-ad1e-44fa-a075-001a45a46370' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-4d6ff258-ad1e-44fa-a075-001a45a46370' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-62b43a06-ad97-4115-8bed-8eacb06d50ec' class='xr-var-data-in' type='checkbox'><label for='data-62b43a06-ad97-4115-8bed-8eacb06d50ec' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([&#x27;AnnArbor_a_sub04111&#x27;, &#x27;AnnArbor_a_sub04619&#x27;,
           &#x27;AnnArbor_a_sub13636&#x27;, ..., &#x27;SaintLouis_sub95967&#x27;,
           &#x27;SaintLouis_sub97935&#x27;, &#x27;SaintLouis_sub99965&#x27;],
          shape=(1078,), dtype=object)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>Y</span></div><div class='xr-var-dims'>(observations, response_vars)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>2.297 1.99 1.946 ... 2.701 2.713</div><input id='attrs-7bee9dfe-816a-414c-b1d2-8fe4940f5a92' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-7bee9dfe-816a-414c-b1d2-8fe4940f5a92' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-943fbd5f-2bd2-4041-9ded-0acbd9bc5044' class='xr-var-data-in' type='checkbox'><label for='data-943fbd5f-2bd2-4041-9ded-0acbd9bc5044' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[2.297, 1.99 , 1.946, 2.544, 2.649],
           [2.   , 2.258, 2.115, 2.389, 2.364],
           [2.35 , 2.624, 2.339, 2.578, 2.394],
           ...,
           [2.545, 2.512, 2.536, 2.795, 2.683],
           [2.369, 2.463, 2.488, 2.955, 2.491],
           [2.425, 2.281, 2.491, 2.701, 2.713]], shape=(1078, 5))</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>X</span></div><div class='xr-var-dims'>(observations, covariates)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>25.63 18.34 29.2 ... 27.0 29.0 23.0</div><input id='attrs-7489f99a-811a-4c81-ac8b-f2fd16840cc0' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-7489f99a-811a-4c81-ac8b-f2fd16840cc0' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-ada1f615-c15c-4779-aa50-173bbfb0c75d' class='xr-var-data-in' type='checkbox'><label for='data-ada1f615-c15c-4779-aa50-173bbfb0c75d' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[25.63],
           [18.34],
           [29.2 ],
           ...,
           [27.  ],
           [29.  ],
           [23.  ]], shape=(1078, 1))</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>batch_effects</span></div><div class='xr-var-dims'>(observations, batch_effect_dims)</div><div class='xr-var-dtype'>&lt;U17</div><div class='xr-var-preview xr-preview'>&#x27;AnnArbor_a&#x27; &#x27;M&#x27; ... &#x27;F&#x27;</div><input id='attrs-72078562-c83f-447b-8ec0-28bf4c1691ac' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-72078562-c83f-447b-8ec0-28bf4c1691ac' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-3c466b36-3f15-4878-8bd7-16fa01bfa10b' class='xr-var-data-in' type='checkbox'><label for='data-3c466b36-3f15-4878-8bd7-16fa01bfa10b' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[&#x27;AnnArbor_a&#x27;, &#x27;M&#x27;],
           [&#x27;AnnArbor_a&#x27;, &#x27;M&#x27;],
           [&#x27;AnnArbor_a&#x27;, &#x27;M&#x27;],
           ...,
           [&#x27;SaintLouis&#x27;, &#x27;M&#x27;],
           [&#x27;SaintLouis&#x27;, &#x27;F&#x27;],
           [&#x27;SaintLouis&#x27;, &#x27;F&#x27;]], shape=(1078, 2), dtype=&#x27;&lt;U17&#x27;)</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>Z</span></div><div class='xr-var-dims'>(observations, response_vars)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>0.09048 -1.743 ... -0.4829 0.374</div><input id='attrs-45a61cc7-b8c3-42e2-b9ad-87fe931e5633' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-45a61cc7-b8c3-42e2-b9ad-87fe931e5633' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-39e23ba2-2cfc-465d-bc02-9afb894ca048' class='xr-var-data-in' type='checkbox'><label for='data-39e23ba2-2cfc-465d-bc02-9afb894ca048' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[ 0.09048246, -1.74343327, -1.73280504, -0.27931943,  0.46170464],
           [-2.15307041, -0.2155683 , -0.88112563, -1.66514532, -1.49058682],
           [ 0.53237432,  2.32689496,  1.05582115,  0.11430795, -0.79015658],
           ...,
           [ 1.06375743,  0.18651962,  0.35946146,  0.20245872,  0.61710398],
           [-0.0414054 , -0.04584035,  0.07680902,  1.43016538, -0.51893228],
           [ 0.08501928, -1.26394207, -0.12296366, -0.48292281,  0.37401496]],
          shape=(1078, 5))</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>baseline_logp</span></div><div class='xr-var-dims'>(observations, response_vars)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>-0.9971 -3.581 ... -0.919 -1.021</div><input id='attrs-1ca1d8f9-41d8-4dc4-b0b9-03efccdc2040' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-1ca1d8f9-41d8-4dc4-b0b9-03efccdc2040' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-57e49614-d799-414c-a5c3-1cd75c8d94e1' class='xr-var-data-in' type='checkbox'><label for='data-57e49614-d799-414c-a5c3-1cd75c8d94e1' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[-0.99707249, -3.5814037 , -2.7596024 , -1.27830063, -0.93320988],
           [-2.80347532, -1.1907573 , -1.44934121, -2.35678277, -1.51781187],
           [-0.92606713, -1.90896874, -0.9298906 , -1.13790237, -1.381113  ],
           ...,
           [-1.32009079, -1.20087654, -1.61983023, -1.05698856, -0.97003065],
           [-0.91914855, -1.02650136, -1.35283172, -1.90008247, -1.06015506],
           [-0.95566622, -1.10047564, -1.36765247, -0.91900098, -1.02138371]],
          shape=(1078, 5))</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>logp</span></div><div class='xr-var-dims'>(observations, response_vars)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>-0.7676 -2.387 ... -0.9341 -0.8585</div><input id='attrs-8b4b231f-e8bc-428e-84a5-97d325ecb16d' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-8b4b231f-e8bc-428e-84a5-97d325ecb16d' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-afefc527-c958-43e1-98b2-d0fda9af3088' class='xr-var-data-in' type='checkbox'><label for='data-afefc527-c958-43e1-98b2-d0fda9af3088' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[-0.76762804, -2.38670351, -2.18516379, -0.85699579, -0.86029717],
           [-3.1524295 , -0.92386454, -1.09107655, -2.23685454, -1.97711085],
           [-0.88429356, -3.57569009, -1.25864035, -0.82258548, -1.0318994 ],
           ...,
           [-1.31230983, -0.87307701, -0.74281395, -0.82899868, -0.91967917],
           [-0.73488719, -0.85471108, -0.69255238, -1.83205664, -0.84589115],
           [-0.7783036 , -1.65738917, -0.6810508 , -0.93407336, -0.85853176]],
          shape=(1078, 5))</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>Yhat</span></div><div class='xr-var-dims'>(observations, response_vars)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>2.283 2.265 2.2 ... 2.779 2.64</div><input id='attrs-913896a4-32f6-4580-9965-7f8759a6ba44' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-913896a4-32f6-4580-9965-7f8759a6ba44' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-42659782-1408-4ffa-83a9-324c1fd481d6' class='xr-var-data-in' type='checkbox'><label for='data-42659782-1408-4ffa-83a9-324c1fd481d6' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[2.28250086, 2.26484906, 2.19959119, 2.58893279, 2.56294337],
           [2.37080339, 2.2932932 , 2.24690986, 2.66684514, 2.67635199],
           [2.26674565, 2.2588321 , 2.18189798, 2.55952839, 2.53564253],
           ...,
           [2.37617507, 2.48256586, 2.48313693, 2.76239474, 2.57007622],
           [2.37544398, 2.47011667, 2.47654847, 2.72522749, 2.58420856],
           [2.41111174, 2.48059635, 2.50897854, 2.77941065, 2.64017218]],
          shape=(1078, 5))</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>statistics</span></div><div class='xr-var-dims'>(response_vars, statistic)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>0.2812 0.009833 ... 0.7406 0.993</div><input id='attrs-11538fc8-e683-40a6-bfed-f42a51139f38' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-11538fc8-e683-40a6-bfed-f42a51139f38' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-714b8b32-7197-4e0c-9fae-3e2abf293ad5' class='xr-var-data-in' type='checkbox'><label for='data-714b8b32-7197-4e0c-9fae-3e2abf293ad5' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[ 2.81175268e-001,  9.83302412e-003,  5.50718886e-002,
            -1.53076478e-001,  1.26586206e+000,  2.81174000e-001,
             1.62865824e-001,  4.65612965e-001,  4.18146055e-059,
             7.18826000e-001,  9.96679987e-001],
           [ 1.18383346e-001,  8.31168831e-003,  5.30658022e-002,
            -5.29632595e-002,  1.36597527e+000,  1.18379252e-001,
             1.60251915e-001,  3.23853691e-001,  9.63857634e-028,
             8.81620748e-001,  9.95696405e-001],
           [ 3.88109550e-001,  5.34322820e-003,  5.13168153e-002,
            -2.38110661e-001,  1.18082787e+000,  3.88108738e-001,
             1.48749883e-001,  5.92263347e-001,  4.79731615e-103,
             6.11891262e-001,  9.95245167e-001],
           [ 2.41710322e-001,  7.38404453e-003,  4.62470331e-002,
            -1.24691932e-001,  1.29424660e+000,  2.41710261e-001,
             1.59165369e-001,  4.87840034e-001,  1.55546500e-065,
             7.58289739e-001,  9.95462402e-001],
           [ 2.59435113e-001,  1.23933210e-002,  5.79990867e-002,
            -1.42256398e-001,  1.27668214e+000,  2.59435017e-001,
             1.94132679e-001,  4.65753458e-001,  3.82083876e-059,
             7.40564983e-001,  9.92956432e-001]])</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>centiles</span></div><div class='xr-var-dims'>(centile, observations, response_vars)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>2.019 2.005 1.959 ... 3.047 2.961</div><input id='attrs-cd66afa1-24e5-433b-8a7b-c1a44ccba9c5' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-cd66afa1-24e5-433b-8a7b-c1a44ccba9c5' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-906a67b1-67e7-4a28-bc05-cc0a7e4be37b' class='xr-var-data-in' type='checkbox'><label for='data-906a67b1-67e7-4a28-bc05-cc0a7e4be37b' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[[2.01860967, 2.00519274, 1.95857113, 2.32341354, 2.25606192],
            [2.08709986, 2.02335838, 2.00029083, 2.39205237, 2.33119835],
            [2.00897649, 2.00013944, 1.9366181 , 2.29479823, 2.24014366],
            ...,
            [2.1147458 , 2.22316096, 2.24097299, 2.49735522, 2.26851113],
            [2.1173582 , 2.21134852, 2.23160635, 2.4604885 , 2.28827119],
            [2.14198056, 2.22062744, 2.268048  , 2.51210533, 2.31962388]],
    
           [[2.17428947, 2.15837422, 2.10075835, 2.48005379, 2.43710336],
            [2.25446774, 2.18260355, 2.1457811 , 2.55416344, 2.53481806],
            [2.16104466, 2.15275242, 2.08131835, 2.45097296, 2.41447007],
            ...,
            [2.26897321, 2.37619412, 2.38383503, 2.65371246, 2.44641624],
            [2.26961315, 2.36400603, 2.37610735, 2.61666845, 2.46285629],
            [2.30075164, 2.37399333, 2.41018241, 2.66979926, 2.50872793]],
    
           [[2.28250086, 2.26484906, 2.19959119, 2.58893279, 2.56294337],
            [2.37080339, 2.2932932 , 2.24690986, 2.66684514, 2.67635199],
            [2.26674565, 2.2588321 , 2.18189798, 2.55952839, 2.53564253],
            ...,
            [2.37617507, 2.48256586, 2.48313693, 2.76239474, 2.57007622],
            [2.37544398, 2.47011667, 2.47654847, 2.72522749, 2.58420856],
            [2.41111174, 2.48059635, 2.50897854, 2.77941065, 2.64017218]],
    
           [[2.39071226, 2.3713239 , 2.29842404, 2.69781179, 2.68878338],
            [2.48713905, 2.40398284, 2.34803861, 2.77952685, 2.81788591],
            [2.37244663, 2.36491178, 2.28247761, 2.66808381, 2.65681499],
            ...,
            [2.48337692, 2.58893761, 2.58243884, 2.87107702, 2.69373619],
            [2.4812748 , 2.5762273 , 2.57698959, 2.83378653, 2.70556083],
            [2.52147185, 2.58719937, 2.60777468, 2.88902204, 2.77161643]],
    
           [[2.54639206, 2.52450538, 2.44061125, 2.85445204, 2.86982482],
            [2.65450693, 2.56322802, 2.49352888, 2.94163792, 3.02150562],
            [2.5245148 , 2.51752476, 2.42717786, 2.82425855, 2.83114141],
            ...,
            [2.63760434, 2.74197077, 2.72530088, 3.02743427, 2.8716413 ],
            [2.63352976, 2.72888481, 2.72149058, 2.98996648, 2.88014594],
            [2.68024292, 2.74056525, 2.74990908, 3.04671597, 2.96072049]]],
          shape=(5, 1078, 5))</pre></div></li><li class='xr-var-item'><div class='xr-var-name'><span>Y_harmonized</span></div><div class='xr-var-dims'>(observations, response_vars)</div><div class='xr-var-dtype'>float64</div><div class='xr-var-preview xr-preview'>2.297 1.99 1.946 ... 2.505 2.622</div><input id='attrs-c0eb18da-2e80-4054-bf59-7277fca45b09' class='xr-var-attrs-in' type='checkbox' disabled><label for='attrs-c0eb18da-2e80-4054-bf59-7277fca45b09' title='Show/Hide attributes'><svg class='icon xr-icon-file-text2'><use xlink:href='#icon-file-text2'></use></svg></label><input id='data-f93e1320-f71a-4567-9818-8e2345158fb8' class='xr-var-data-in' type='checkbox'><label for='data-f93e1320-f71a-4567-9818-8e2345158fb8' title='Show/Hide data repr'><svg class='icon xr-icon-database'><use xlink:href='#icon-database'></use></svg></label><div class='xr-var-attrs'><dl class='xr-attrs'></dl></div><div class='xr-var-data'><pre>array([[2.29709629, 1.98976542, 1.94577128, 2.54384863, 2.64905888],
           [1.99939114, 2.25788204, 2.11474128, 2.38867268, 2.36362419],
           [2.35025559, 2.62518612, 2.33956087, 2.57792873, 2.39354976],
           ...,
           [2.42936211, 2.24909345, 2.21265908, 2.57761854, 2.61763587],
           [2.24980952, 2.20647905, 2.16363795, 2.75989659, 2.39978038],
           [2.30584554, 2.02438001, 2.16661521, 2.50539373, 2.62209086]],
          shape=(1078, 5))</pre></div></li></ul></div></li><li class='xr-section-item'><input id='section-7e1c6280-b0a1-4a29-b91b-b2c0762257a7' class='xr-section-summary-in' type='checkbox' checked /><label for='section-7e1c6280-b0a1-4a29-b91b-b2c0762257a7' class='xr-section-summary' title='Expand/collapse section'>Attributes: <span>(7)</span></label><div class='xr-section-inline-details'></div><div class='xr-section-details'><dl class='xr-attrs'><dt><span>real_ids :</span></dt><dd>True</dd><dt><span>is_scaled :</span></dt><dd>False</dd><dt><span>name :</span></dt><dd>fcon1000</dd><dt><span>unique_batch_effects :</span></dt><dd>{np.str_(&#x27;site&#x27;): [&#x27;AnnArbor_a&#x27;, &#x27;AnnArbor_b&#x27;, &#x27;Atlanta&#x27;, &#x27;Baltimore&#x27;, &#x27;Bangor&#x27;, &#x27;Beijing_Zang&#x27;, &#x27;Berlin_Margulies&#x27;, &#x27;Cambridge_Buckner&#x27;, &#x27;Cleveland&#x27;, &#x27;ICBM&#x27;, &#x27;Leiden_2180&#x27;, &#x27;Leiden_2200&#x27;, &#x27;Milwaukee_b&#x27;, &#x27;Munchen&#x27;, &#x27;NewYork_a&#x27;, &#x27;NewYork_a_ADHD&#x27;, &#x27;Newark&#x27;, &#x27;Oulu&#x27;, &#x27;Oxford&#x27;, &#x27;PaloAlto&#x27;, &#x27;Pittsburgh&#x27;, &#x27;Queensland&#x27;, &#x27;SaintLouis&#x27;], np.str_(&#x27;sex&#x27;): [&#x27;M&#x27;, &#x27;F&#x27;]}</dd><dt><span>batch_effect_counts :</span></dt><dd>defaultdict(&lt;function NormData.register_batch_effects.&lt;locals&gt;.&lt;lambda&gt; at 0x163d90a40&gt;, {np.str_(&#x27;site&#x27;): {&#x27;AnnArbor_a&#x27;: 24, &#x27;AnnArbor_b&#x27;: 32, &#x27;Atlanta&#x27;: 28, &#x27;Baltimore&#x27;: 23, &#x27;Bangor&#x27;: 20, &#x27;Beijing_Zang&#x27;: 198, &#x27;Berlin_Margulies&#x27;: 26, &#x27;Cambridge_Buckner&#x27;: 198, &#x27;Cleveland&#x27;: 31, &#x27;ICBM&#x27;: 85, &#x27;Leiden_2180&#x27;: 12, &#x27;Leiden_2200&#x27;: 19, &#x27;Milwaukee_b&#x27;: 46, &#x27;Munchen&#x27;: 15, &#x27;NewYork_a&#x27;: 83, &#x27;NewYork_a_ADHD&#x27;: 25, &#x27;Newark&#x27;: 19, &#x27;Oulu&#x27;: 102, &#x27;Oxford&#x27;: 22, &#x27;PaloAlto&#x27;: 17, &#x27;Pittsburgh&#x27;: 3, &#x27;Queensland&#x27;: 19, &#x27;SaintLouis&#x27;: 31}, np.str_(&#x27;sex&#x27;): {&#x27;M&#x27;: 489, &#x27;F&#x27;: 589}})</dd><dt><span>covariate_ranges :</span></dt><dd>{np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 7.88, &#x27;max&#x27;: 85.0}}</dd><dt><span>batch_effect_covariate_ranges :</span></dt><dd>{np.str_(&#x27;site&#x27;): {&#x27;AnnArbor_a&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 13.41, &#x27;max&#x27;: 40.98}}, &#x27;AnnArbor_b&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 19.0, &#x27;max&#x27;: 79.0}}, &#x27;Atlanta&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 22.0, &#x27;max&#x27;: 57.0}}, &#x27;Baltimore&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 20.0, &#x27;max&#x27;: 40.0}}, &#x27;Bangor&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 19.0, &#x27;max&#x27;: 38.0}}, &#x27;Beijing_Zang&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 18.0, &#x27;max&#x27;: 26.0}}, &#x27;Berlin_Margulies&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 23.0, &#x27;max&#x27;: 44.0}}, &#x27;Cambridge_Buckner&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 18.0, &#x27;max&#x27;: 30.0}}, &#x27;Cleveland&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 24.0, &#x27;max&#x27;: 60.0}}, &#x27;ICBM&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 19.0, &#x27;max&#x27;: 85.0}}, &#x27;Leiden_2180&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 20.0, &#x27;max&#x27;: 27.0}}, &#x27;Leiden_2200&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 18.0, &#x27;max&#x27;: 28.0}}, &#x27;Milwaukee_b&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 44.0, &#x27;max&#x27;: 65.0}}, &#x27;Munchen&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 63.0, &#x27;max&#x27;: 74.0}}, &#x27;NewYork_a&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 7.88, &#x27;max&#x27;: 49.16}}, &#x27;NewYork_a_ADHD&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 20.69, &#x27;max&#x27;: 50.9}}, &#x27;Newark&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 21.0, &#x27;max&#x27;: 39.0}}, &#x27;Oulu&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 20.0, &#x27;max&#x27;: 23.0}}, &#x27;Oxford&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 20.0, &#x27;max&#x27;: 35.0}}, &#x27;PaloAlto&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 22.0, &#x27;max&#x27;: 46.0}}, &#x27;Pittsburgh&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 25.0, &#x27;max&#x27;: 47.0}}, &#x27;Queensland&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 20.0, &#x27;max&#x27;: 34.0}}, &#x27;SaintLouis&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 21.0, &#x27;max&#x27;: 29.0}}}, np.str_(&#x27;sex&#x27;): {&#x27;M&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 9.21, &#x27;max&#x27;: 78.0}}, &#x27;F&#x27;: {np.str_(&#x27;age&#x27;): {&#x27;min&#x27;: 7.88, &#x27;max&#x27;: 85.0}}}}</dd></dl></div></li></ul></div></div>



.. code:: ipython3

    harmonized_y = reference_norm_data[("Y_harmonized")]
    harmonized_y = harmonized_y.to_dataframe().reset_index().pivot(index="observations",columns="response_vars", values="Y_harmonized")
    harmonized_y.columns = [f"harmonized_{c}" for c in harmonized_y.columns]
    df_with_obs = df.merge(reference_norm_data[("subject_ids")].to_dataframe().reset_index(), left_on="sub_id", right_on="subject_ids")
    df = pd.merge(left=df_with_obs, right=harmonized_y, on="observations")

.. code:: ipython3

    fig, ax = plt.subplots(len(response_variables),2, figsize=(15,30), sharex=True)
    
    # First plot the data as it is 
    for i, f in enumerate(response_variables):
        ax[i,0].set_title(f"Non harmonized {f}")
        ax[i,1].set_title(f"Harmonized {f}")
        sns.scatterplot(df, x="age", y=f, hue="site", style="sex", legend=False, ax=ax[i,0])
        sns.scatterplot(df, x="age", y=f"harmonized_{f}", hue="site", style="sex", legend=False, ax=ax[i,1])
    
    plt.show()



.. image:: B4_N3_normative_modelling_files/B4_N3_normative_modelling_22_0.png


9. Interpretation
-----------------

After harmonization, site-driven spread is reduced and observations are
brought onto a more comparable scale. This makes downstream analyses
less sensitive to nuisance group effects.
