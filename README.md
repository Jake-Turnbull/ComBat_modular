# ComBat_modular
A modular implementation of ComBat harmonisation and various extensions, with support for feature-wise, similarity-informed empirical Bayes priors.


This repository contains a Python implementation of standard ComBat alongside an experimental extension that constructs local, feature-specific hyperparameters from feature-feature similarity matrices. Similarity can be derived from within-batch data or supplied externally, including spatial proximity information for imaging applications. The code also includes covariate-aware mean modelling, rank-safe covariate pruning, optional CovBat-style covariance correction, and utilities for inspecting prior estimates and harmonisation outputs.
The project is intended for harmonising imaging data where preserving biologically meaningful covariate structure while removing batch effects is critical. It is designed to be transparent, configurable, and easy to compare against baseline ComBat.


## Modular ComBat entrypoint.

    combat_modular(data, batch, mod=None, mean_model='ols', gam_opts=None, prior_mode='global', prior_weight_methods=None, prior_weight_opts=None, parametric=True, DeltaCorrection=True,        UseEB=True, ReferenceBatch=None, RegressCovariates=False, GammaCorrection=True, covbat_mode=False, return_priors=True, **kwargs


This function is a modular wrapper around the existing combat implementation. By default (mean_model='ols' and prior_mode='global') it calls the original combat function to preserve exact baseline behaviour. Experimental modular paths (GAM mean models, local priors) will be implemented here incrementally.

    Parameters (additional options):
        - `mean_model`: 'ols' (default) or 'gam' to fit flexible spline-based covariate effects.
        - `prior_mode`: 'global' (default) or 'local' to enable feature-wise local priors.
        - `prior_weight_methods`: sequence of weight method names (see `_construct_local_priors`).
        - `prior_weight_opts`: dict of options passed to `_construct_local_priors` (method weights, spatial coords, directional params).
    
    Returns:
        - Always returns a dict with keys `bayesdata`, `B_hat`, `priors`, and
          `design_diagnostics`.


## References:

ComBat: Johnson, W. E., Li, C., & Rabinovic, A. (2007). Adjusting batch effects in microarray expression data using empirical Bayes methods. Biostatistics.

Fortin et al., (2018). Harmonization of cortical thickness measurements across scanners and sites. NeuroImage, 167, 104–120. https://doi.org/10.1016/j.neuroimage.2017.11.024

Fortin et al., Harmonization of multi-site diffusion tensor imaging data. NeuroImage, 161, 149–170. https://doi.org/10.1016/j.neuroimage.2017.08.047

ComBat-GAM: Pomponio et al., (2020). Harmonization of large MRI datasets for the analysis of brain imaging patterns throughout the lifespan. NeuroImage, 208, 116450. https://doi.org/10.1016/j.neuroimage.2019.116450

Reference batch ComBat Jacob Turnbull et al., (2026). bioRxiv 2026.05.22.726536; doi: https://doi.org/10.64898/2026.05.22.726536

CovBat: Chen, A. A., Beer, J. C., Tustison, N. J., Cook, P. A., Shinohara, R. T., Shou, H., & Initiative, T. A. D. N. (2022). Mitigating site effects in covariance for machine learning in neuroimaging data. Human Brain Mapping, 43(4), 1179–1195. https://doi.org/10.1002/hbm.25688
