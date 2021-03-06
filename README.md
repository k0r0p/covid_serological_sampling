# Estimating SARS-CoV-2 seroprevalence and epidemiological parameters with uncertainty from serological surveys

This repository stores code and data associated with the manuscript [Estimating SARS-CoV-2 seroprevalence and epidemiological parameters with uncertainty from serological surveys](https://www.medrxiv.org/content/10.1101/2020.04.15.20067066v1) by Daniel B. Larremore, Bailey K. Fosdick, Kate M. Bubar, Sam Zhang, Stephen M. Kissler, C. Jessica E. Metcalf, Caroline O. Buckee, and Yonatan H. Grad. 

This repository is maintained by [Daniel Larremore](https://larremorelab.github.io/). Questions can be directed to `daniel.larremore@colorado.edu`.

Code is written primarily in `python` but the MCMC engine has been rewritten in `R` because it is, for some reason, faster. 


# Inference of prevalence (one population)

When sensitivity and specificity are known, one can use those values, along with the number of positive and negative test results, to produce posterior estimates of prevalence like this one:
[![Image of Seroprevalence Posterior](https://github.com/LarremoreLab/covid_serological_sampling/blob/master/images/calculator.png)](https://larremorelab.github.io/covid19testgroup)

### Interactive web tool option
This figure can be created and downloaded by using the web-based calculator available at [https://larremorelab.github.io/covid-serology](https://larremorelab.github.io/covid19testgroup). 

### Python option
To perform the same inference in python, a notebook is available at [`codebase/prevalence_onepopulation_workbook.ipynb`](https://github.com/LarremoreLab/covid_serological_sampling/blob/master/codebase/prevalence_onepopulation_workbook.ipynb)

# Inference of prevalence (multiple subpopulations)

When there are multiple subpopulations, it is tempting to simply use the estimation approach above, independently for each subpopulation. However, *especially when the number of samples per bin is small*, this is not a good idea, as the estimates may vary wildly. What can be done?

What we do in the paper, which we provide code for here, is to use a Bayesian hierarchical model where the supopulation seroprevalences share a prior distribution. The mean of that prior is informed entirely by the data, while the variance of that prior is weakly specified by a hyperprior. **In plain English**, the Bayesian approach allows the subpopulations to learn from each other without entraining to the same estimates. 

The notebook [`codebase/prevalence_subpopulations_workbook.ipynb`](https://github.com/LarremoreLab/covid_serological_sampling/blob/master/codebase/prevalence_subpopulations_workbook.ipynb) gives example code. Given data in the form of sensitivity, specificity, and the number of positive and negative samples in each subpopulation, the inference is performed to generate posterior estimates. Note that we are able to produce estimates *albeit with high uncertainty* for the supopulation that was dramatically undersampled. 

![Image of Subpopulation Seroprevalence Posteriors](https://github.com/LarremoreLab/covid_serological_sampling/blob/master/images/subpopulations.png)

# Sample size calculator (one population)

For the moment, the sample size calculation has not been wrapped in its own function to allow one to "solve for `n`". However, if one wanted to compute the number of samples required to produce estimates to within a certain uncertainty, that number would depend on
1. Test sensitivity and specificity.
2. The range of prevalence values that are plausibly expected.
3. The posterior uncertainty spec that is trying to be achieved. 

As a general rule, one could take the plausible prevalence values, choose the one closest to 0.5, and assume that the true seroprevalence is that "worst case" value. Then, use the notebook [`codebase/prevalence_onepopulation_workbook.ipynb`](https://github.com/LarremoreLab/covid_serological_sampling/blob/master/codebase/prevalence_onepopulation_workbook.ipynb) to explore how sample counts affect posterior error (measured by credible interval width).

# Model & Demographic Informed Sample Allocation

MDI stands for *Model & Demographic Informed*. It is a **variance reduction strategy**. In plain language, its goal is to help allocate more samples to those subpopulations whose accurate measurement will be most beneficial. The method is explained in the [manuscript](https://www.medrxiv.org/content/10.1101/2020.04.15.20067066v1), and it produces recommendations for sample allocation. Here is an example of a *hypothetical* sample allocation strategy with the goal of parameterizing a model of disease dynamics in Argentina (using estimates of Argentina's demographic and contact structure). This figure and others like it can be recreated and customized using the Jupyte notebook[`codebase/sample_allocation_MDI_workbook.ipynb`](https://github.com/LarremoreLab/covid_serological_sampling/blob/master/codebase/sample_allocation_MDI_workbook.ipynb)

![Image of MDI Argentina](https://github.com/LarremoreLab/covid_serological_sampling/blob/master/images/MDI_Argentina.png)

# SEIR Simulations

In the paper, we have a figure like this one:
![Image of SEIR Simulation](https://github.com/LarremoreLab/covid_serological_sampling/blob/master/images/SEIR_workbook.png)

This figure can be recreated from scratch using the Jupyter notebook [`codebase/SEIR_workbook.ipynb`](https://github.com/LarremoreLab/covid_serological_sampling/blob/master/codebase/SEIR_workbook.ipynb). In order, this notebook:
1. Simulates hypothetical data. (Real data could be inserted here.)
2. Infers the posterior distribution over seroprevalence using the sensitivity and specificity values chosen.
3. Runs an SEIR simulation forward. (Initial conditions, parameters, and assumptions about immunity could be adjusted here.)
4. Calculates both peak timing and height distributions. (only Height is plotted, but this can be adjusted in plots.)