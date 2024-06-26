.. _codependence-correlation_based_metrics:

.. note::
   The following implementations and documentation, closely follows the lecture notes from Cornell University, by Marcos Lopez de Prado:
   `Codependence (Presentation Slides) <https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3512994>`_.

=========================
Correlation-Based Metrics
=========================

.. raw:: html

    <div style="position: relative;
                padding-bottom: 56.25%;
                margin-bottom: 5%;
                height: 0;
                overflow: hidden;
                max-width: 100%;
                height: auto;">

        <iframe src="https://www.youtube.com/embed/YyMouLPj2QA?start=375"
                frameborder="0"
                allowfullscreen
                style="position: absolute;
                       top: 0;
                       left: 0;
                       width: 100%;
                       height: 100%;">
        </iframe>
        <br/>
    </div>

|

.. Note::
    **Underlying Literature**

    The following sources elaborate extensively on the topic:

    - `Codependence (Presentation Slides) <https://ssrn.com/abstract=3512994>`__ *by* Marcos Lopez de Prado.
    - `Measuring and testing dependence by correlation of distances <https://projecteuclid.org/download/pdfview_1/euclid.aos/1201012979>`__ *by* Székely, G.J., Rizzo, M.L. and Bakirov, N.K.
    - `Introducing the discussion paper by Székely and Rizzo <https://arxiv.org/pdf/1010.3575.pdf>`__ *by* Michael A. Newton.
    - `Building Diversified Portfolios that Outperform Out-of-Sample <https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2708678>`_ *by* Marcos Lopez de Prado.
    - `Kullback-Leibler distance as a measure of the information filtered from multivariate data. <https://arxiv.org/pdf/0706.0168.pdf>`__ *by* Tumminello, M., Lillo, F. and Mantegna, R.N.

--------------------------------------------

Distance Correlation
####################

**Distance correlation** can capture not only linear association but also non-linear variable dependencies which Pearson correlation can not.
It was introduced in 2005 by Gábor J. Szekely and is described in the work
`"Measuring and testing independence by correlation of distances". <https://projecteuclid.org/download/pdfview_1/euclid.aos/1201012979>`_
It is calculated as:

.. math::
    \rho_{dist}[X, Y] = \frac{dCov[X, Y]}{\sqrt{dCov[X, X]dCov[Y,Y}}

Where :math:`dCov[X, Y]` can be interpreted as the average Hadamard product of the doubly-centered Euclidean distance matrices of
:math:`X, Y`. (`Cornell lecture slides, p.7 <https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3512994>`_)

Values of distance correlation fall in the range:

.. math::
    0 \leq \rho_{dist}[X, Y] \leq 1

Distance correlation is equal to zero if and only if the two variables are independent (in contrast to Pearson correlation
that can be zero even if the variables are dependant).

.. math::
    \rho_{dist}[X, Y] = 0 \Leftrightarrow X \perp Y

As shown in the figure below, distance correlation captures the nonlinear relationship.

.. image:: images/distance_correlation.png
   :scale: 70 %
   :align: center

The numbers in the first line are Pearson correlation values and the values in the second line are Distance correlation values.
This figure is from `"Introducing the discussion paper by Székely and Rizzo" <https://www.researchgate.net/publication/238879872_Introducing_the_discussion_paper_by_Szekely_and_Rizzo>`_
by Michale A. Newton. It provides a great overview for readers.

Implementation
**************

.. py:currentmodule:: arbitragelab.codependence.correlation

.. autofunction:: distance_correlation

Standard Angular Distance
#########################

**Angular distance** is a slight modification of the Pearson correlation coefficient which satisfies all distance metric conditions.
This measure is known as the angular distance because when we use *covariance* as an *inner product*, we can interpret correlation as :math:`cos\theta`.

A proof that angular distance is a true metric can be found in the work by Lopez de Prado
`Building Diversified Portfolios that Outperform Out-of-Sample: <https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2708678>`_

"Angular distance is a linear multiple of the Euclidean distance between the vectors :math:`\{X, Y\}` after z-standardization,
hence it inherits the true-metric properties of the Euclidean distance."

According to Lopez de Prado:

"The [standard angular distance] metric deems more distant two random variables with negative correlation than two random
variables with positive correlation".

"This property makes sense in many applications. For example, we may wish to build a **long-only portfolio**, where holdings
in negative-correlated securities can only offset risk, and therefore should be treated as different for diversification purposes".

Formula used to calculate standard angular distance:

.. math::
    d_\rho[X, Y] = \sqrt{\frac{1}{2}(1-\rho[X,Y])}

where :math:`\rho[X,Y]` is Pearson correlation between the vectors :math:`\{X, Y\}` .

Values of standard angular distance fall in the range:

.. math::
    d_\rho[X, Y] \in [0, 1]

.. figure:: images/angular_distance.png
   :scale: 70 %
   :align: center
   :figclass: align-center
   :alt: Angular Distance

   The angular distance satisfies all the conditions of a true metric, (Lopez de Prado, 2020.)

Implementation
**************

.. autofunction:: angular_distance

Absolute Angular Distance
#########################

This modification of angular distance uses an absolute value of Pearson correlation in the formula.

This property assigns small distance to elements that have a high negative correlation. According to Lopez de Prado, this
is useful because "in **long-short portfolios**, we often prefer to consider highly negatively-correlated securities as similar,
because the position sign can override the sign of the correlation".

Formula used to calculate absolute angular distance:

.. math::
    d_{|\rho|}[X, Y] = \sqrt{1-|\rho[X,Y]|}

where :math:`\rho[X,Y]` is Pearson correlation between the vectors :math:`\{X, Y\}` .

Values of absolute angular distance fall in the range:

.. math::
    d_{|\rho|}[X, Y] \in [0, 1]

.. figure:: images/modified_angular_distance.png
   :scale: 70 %
   :align: center
   :figclass: align-center
   :alt: Modified Angular Distance

   In some financial applications, it makes more sense to apply a modified definition of angular distance, such that the
   sign of the correlation is ignored, (Lopez de Prado, 2020)

Implementation
**************

.. autofunction:: absolute_angular_distance

Squared Angular Distance
########################

Squared angular distance uses the squared value of Pearson correlation in the formula and has similar properties to absolute
angular distance. The only difference is that a higher distance is assigned to the elements that have a small absolute correlation.

Formula used to calculate squared angular distance:

.. math::
    d_{\rho^2}[X, Y] = \sqrt{1-{\rho[X,Y]}^2}

where :math:`\rho[X,Y]` is Pearson correlation between the vectors :math:`\{X, Y\}` .

Values of squared angular distance fall in the range:

.. math::
    d_{\rho^2}[X, Y] \in [0, 1]

.. figure:: images/modified_angular_distance.png
   :scale: 70 %
   :align: center
   :figclass: align-center
   :alt: Modified Angular Distance

Implementation
**************

.. autofunction:: squared_angular_distance

Kullback-Leibler Distance
#########################

The Kullback-Leibler distance is a measure of distance between two probability densities, say p and q, which is defined as

.. math::
    K(p,q) = E_p \left[log\left(\frac{p}{q}\right)\right]

Where :math:`E_p[.]` indicates the expectation value with respect to the probability density :math:`p`. Here we consider
the Kullback-Leibler distance between multivariate Gaussian random variables (aka. Correlation matrices).

Given two positive definite correlation matrices :math:`C_1` and :math:`C_2` associated with random variable :math:`X`, we can compute
their probability density functions to :math:`P(C_1,X)` and :math:`P(C_1,X)` resulting in the following formula

.. math::
    \begin{gather*}
    K(P(C_1,X),P(C_2,X)) = E_{P(C_1,X)} \left[log\left(\frac{P(C_1,X)}{P(C_2,X)}\right)\right] = \\
    \frac{1}{2}\left[log\left(\frac{|C_1|}{|C_2|}\right)+tr(C^{-1}_2 C_1) -n) \right]
    \end{gather*}

where :math:`n` is the dimension of the space spanned by :math:`X`, and :math:`|C|` indicates the determinant of :math:`C`

The Kullback-Leibler distance can be used to measure the stability of filtering/de-noising procedures with respect to statistical
uncertainty.

.. tip::
    It is worth noting that the Kullback-Leibler distance takes naturally into account the statistical nature of correlation matrices
    which is uncommon with other measures of distance between matrices such as the Frobenius distance which is based on
    the iso-morphism between the matrices space and the vectors space. For more information on using the Kullback-Leibler
    distance to measure the statistical uncertainty of correlation matrices check out
    `Michele Tumminello's research paper <https://www.researchgate.net/publication/5915427>`__.


Implementation
**************

.. autofunction:: kullback_leibler_distance

Norm Distance
#############

A Norm is a function that takes a random variable and returns a value(Norm Distance) that satisfies certain properties pertaining to
scalability and additivity.

The :math:`L` norms are the most common type of norms. They use the same logic behind the SRSS (Square Root of the Sum of Squares)

.. math::
    ||x||_r := \sqrt[r]{x^r_1 + ... + x^r_n}

where :math:`r` is a positive integer. The Euclidean norm is by far the most commonly used norm on multi-dimensional variables with :math:`r = 2`
which makes the Euclidean norm an :math:`L^2` type norm.

Implementation
**************

.. autofunction:: norm_distance

Examples
########

The following examples show how the described above correlation-based metrics can be used on real data:


.. code-block::

    import pandas as pd
    import numpy as np
    from arbitragelab.codependence import (distance_correlation, angular_distance,
                                           absolute_angular_distance, squared_angular_distance,
                                           kullback_leibler_distance, norm_distance,
                                           get_dependence_matrix)

    # Import dataframe of returns for assets in a portfolio
    asset_returns = pd.read_csv(DATA_PATH, index_col='Date', parse_dates=True)

    # Creates correlation matrices of the returns DataFrame at different time frames
    corr_18 = np.corrcoef(asset_returns.loc['01-01-2018':'01-01-2019'])
    corr_19 = np.corrcoef(asset_returns.loc['01-01-2019':'01-01-2020'])

    asset1 = 'SPY'
    asset2 = 'TLT'

    # Calculate distance correlation between chosen assets
    distance_corr = distance_correlation(asset_returns[asset1], asset_returns[assets2])

    # Calculate angular distance between chosen assets
    angular_dist = angular_distance(asset_returns[asset1], asset_returns[assets2])

    # Calculate absolute angular distance between chosen assets
    angular_dist = absolute_angular_distance(asset_returns[asset1], asset_returns[assets2])

    # Calculate squared angular distance between chosen assets
    angular_dist = squared_angular_distance(asset_returns[asset1], asset_returns[assets2])

    # Calculate angular distance between all assets
    angular_dist_matrix = get_dependence_matrix(data, dependence_method='gnpr_distance')

    # Calculate the Kullback-Leibler distance between two correlation matrices
    kldist = kullback_leibler_distance(corr_18, corr_19)

    # Calculate the Norm distance between two correlation matrices
    dist = norm_distance(corr_18, corr_19)

Presentation Slides
###################

.. image:: images/codep_slides.png
   :scale: 70 %
   :align: center
   :target: https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3512994

.. image:: images/codependence_slides.png
   :scale: 40 %
   :align: center
   :target: https://drive.google.com/file/d/1pamteuYyc06r1q-BR3VFsxwa3c7-7oeK/view

References
##########

* `de Prado, M.L, 2020. Codependence (Presentation Slides). Available at SSRN 3512994. <https://ssrn.com/abstract=3512994>`_
* `Székely, G.J., Rizzo, M.L. and Bakirov, N.K., 2007. Measuring and testing dependence by correlation of distances. The annals of statistics, 35(6), pp.2769-2794. <https://projecteuclid.org/download/pdfview_1/euclid.aos/1201012979>`_
* `Newton, M.A., 2009. Introducing the discussion paper by Székely and Rizzo. The Annals of Applied Statistics, 3(4), pp.1233-1235. <https://arxiv.org/pdf/1010.3575.pdf>`_
* `de Prado, M.L., 2016. Building diversified portfolios that outperform out of sample. The Journal of Portfolio Management, 42(4), pp.59-69. <https://papers.ssrn.com/sol3/papers.cfm?abstract_id=2708678>`_
* `Tumminello, M., Lillo, F. and Mantegna, R.N., 2007. Kullback-Leibler distance as a measure of the information filtered from multivariate data. Physical Review E, 76(3), p.031123. <https://arxiv.org/pdf/0706.0168.pdf>`_
