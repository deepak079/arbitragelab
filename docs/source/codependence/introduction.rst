.. _codependence-introduction:

============
Introduction
============

.. raw:: html

    <div style="position: relative;
                padding-bottom: 56.25%;
                margin-bottom: 5%;
                height: 0;
                overflow: hidden;
                max-width: 100%;
                height: auto;">

        <iframe src="https://www.youtube.com/embed/YyMouLPj2QA"
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

This module includes implementations of codependence metrics. According to Lopez de Prado:

"Two random variables are codependent when knowing the value of one helps us determine the value of the other.
This should not be confounded with the notion of causality."

Pearson correlation coefficient is the most famous and widely used measure of codependence, however, it has some drawbacks.

.. warning::

    Pearson correlation suffers from 3 major drawbacks:

    1) It captures linear effects, but if two variables have strong non-linear dependency (squared or abs for example) Pearson correlation won't find any pattern between them.
    2) Correlation is not a distance metric: it does not satisfy non-negativity and subadditivity conditions.
    3) Financial markets have non-linear patterns, which Pearson correlation fails to capture.

Pearson correlation is not the only way of measuring codependence. There are alternative and more modern measures of codependence,
which are described in the parts of this module.

.. note::
   For some methods in this module, it’s discussed whether they are true metrics.
   According to Arkhangel'skii, A. V. and Pontryagin, L. S. (1990), **General Topology I**:
   A metric on a set :math:`X` is a function (called a distance):

   .. math::
      d: X \times X \rightarrow [0,+ \infty) ;   x, y, z \in X

   for which the following three axioms are satisfied:

   1. :math:`d(x, y) = 0 \iff x = y` — identity of indiscernibles;

   2. :math:`d(x,y) = d(y,x)` — symmetry;

   3. :math:`d(x,y) \le d(x,z) + d(z,y)` — triangle inequality;

   and these imply :math:`d(x,y) \ge 0` — non-negativity.

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

* `Lopez de Prado, M., 2020. Codependence (Presentation Slides). Available at SSRN 3512994. <https://ssrn.com/abstract=3512994>`_
