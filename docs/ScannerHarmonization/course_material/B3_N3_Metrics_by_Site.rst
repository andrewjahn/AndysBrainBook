Discover biases in metrics by site
------------------------------------

While dealing with multisite data, we need to be careful when reporting our metrics.

Performance metrics give us an overall idea of the model's performance across all samples (subjects). However, there might be hidden biases in our data that these metrics might overlook.

In these exercises, besides computing the overall metric, we will compute each of the metrics, but using the subjects present in each site.

Importantly, we will *always* train our ML model on the whole dataset, but only desegregate the metric calculation by site.

.. code:: python
    # Imports
    import matplotlib.pyplot as plt
    import numpy as np
    import seaborn as sns
    from sklearn.linear_model import LogisticRegression
    from sklearn.metrics import balanced_accuracy_score, roc_auc_score
    from sklearn.model_selection import train_test_split

    from uniharmony import verbosity
    from uniharmony.datasets import make_multisite_classification

    sns.set_theme(style="whitegrid")
    verbosity("error")

    random_state = 42

    clf = LogisticRegression(random_state=random_state)

    #### This is the main function for this notebook.
    from uniharmony.metrics import report_metrics_by_site

    # One metric or a list of metrics to calculate.
    metrics_to_use = [balanced_accuracy_score, roc_auc_score]

    # While we compute two metrics, we will only plot one.
    metric_to_plot = str(metrics_to_use[0].__name__)


Let's create Scenario 1, where we have 1 **bad** scenario (with no real signal), and 3 sites **good**, with real signal.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python
    # Data generation
    n_bad_sites = 1
    X_bad, y_bad, sites_bad = make_multisite_classification(
        n_sites=n_bad_sites,
        signal_strength=0.001,
        site_effect_strength=0,  # No EoS
        random_state=random_state,
    )

    # Simulate "good" sites
    n_good_sites = 3
    signal_strength = 1
    X_good, y_good, sites_good = make_multisite_classification(
        n_sites=n_good_sites,
        signal_strength=signal_strength,
        site_effect_strength=0,  # No EoS
    )
    # Increase site labels for good sites to avoid overlap with bad sites
    sites_good = sites_good + n_bad_sites

    # Concatenate both simulated sites
    X = np.concatenate([X_bad, X_good], axis=0)
    y = np.concatenate([y_bad, y_good], axis=0)
    sites = np.concatenate([sites_bad, sites_good], axis=0)

    # Split
    X_train, X_test, y_train, y_test, sites_train, sites_test = train_test_split(
        X, y, sites, random_state=random_state
    )

    clf.fit(X_train, y_train)
    y_pred_s1 = clf.predict(X_test)
    metric_s1 = report_metrics_by_site(
        y_test,
        y_pred_s1,
        sites_test,
        metrics_to_use,
    )


Let's take a look at what is returned from the `report_metrics_by_site`.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
The result is a dictionary containing the metrics as the first key, and then the overall performance followed by the performance obtained in each site.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


.. code:: python
    print(metric_s1)
    {'balanced_accuracy_score': {'overall': 0.6622004866336872, 0: 0.5362376847290641, 1: 0.7884429400386848, 2: 0.8444444444444444, 3: 0.7467296511627908}, 'roc_auc_score': {'overall': 0.6622004866336872, 0: 0.536237684729064, 1: 0.7884429400386848, 2: 0.8444444444444444, 3: 0.7467296511627908}}


Now let's create a Scenario 2: a dataset with 3 bad sites, and 1 good site.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code:: python
    # Scenario 2
    n_bad_sites = 3
    X_bad, y_bad, sites_bad = make_multisite_classification(
        n_sites=n_bad_sites,
        signal_strength=0.001,
        site_effect_strength=0,  # No EoS
        random_state=random_state,
    )

    # Used to simulate "good" sites
    signal_strength = 1
    X_good, y_good, sites_good = make_multisite_classification(
        n_sites=1,
        signal_strength=signal_strength,
        site_effect_strength=0,  # No EoS
        random_state=random_state,
    )
    # Increase site labels for good sites to avoid overlap with bad sites
    sites_good = sites_good + n_bad_sites

    X = np.concatenate([X_bad, X_good], axis=0)
    y = np.concatenate([y_bad, y_good], axis=0)
    sites = np.concatenate([sites_bad, sites_good], axis=0)
    X_train, X_test, y_train, y_test, sites_train, sites_test = train_test_split(
        X, y, sites, random_state=random_state
    )

    clf.fit(X_train, y_train)
    y_pred_s2 = clf.predict(X_test)
    metric_s2 = report_metrics_by_site(y_test, y_pred_s2, sites_test, metrics_to_use)


### Let's analyze the global performance obtained in each Scenario for the metric that we decided to plot.
.. code:: python
    # Extract global performance for both scenarios
    metric_global_s1 = metric_s1[metric_to_plot].pop("overall")
    metric_global_s2 = metric_s2[metric_to_plot].pop("overall")
    print("=="*40)
    print(f" Overall performance for Scenario 1: {metric_global_s1:0.4f} \n Overall performance for Scenario 2: {metric_global_s2:0.4f}")


Overall performance for Scenario 1: 0.6622 
Overall performance for Scenario 2: 0.6354

Looking at the overall metric, the obtained performance is very similar. 
------------------------------------------------------------------------
Now, let's explore the performance obtained in each of the sites.
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
We will plot the performance obtained in each site for both scenarios, and we will also include the overall performance as a dashed line.

.. figure:: images/B3_N3_Metrics_by_Site_im1.png


As expected, the performance of the **bad** sites is near to chance.
--------------------------------------------------------------------


Questions:

--------------------------------------------------------------------

- How is it possible that they have an similar overall performance? Where is the catch?

The performance obtained in each site is not comparable because they have different number of samples!

In the first scenario, the bad site is 3 times bigger than the good sites and vice versa for the second scenario.

If we had only reported the overall performance, we would not be able to unravel the model's behavior.

- In this example, no Eos was used simulated in any of our sites. What do you think it could happen if different EoS are presented in different sites?

As we saw before, it will depend not only on the EoS but also in the site class imbalance. If the EoS acts as noise, the presence of EoS will create noisier sites, which could act as "bad" sites. If we have a big site class imbalance, and for example combined with having one big site (for example with healthy control) and many small sites with patients, the model will only learn the particular EoS of that site. In the extreme case, classifying the site  will be the same as classify the target, from the model's perspective. 