# Redshift Evolution of the Dainotti GRB Correlation

**A systematics check on GRB standardization using the Platinum sample.**

Does the slope of the Dainotti relation (log L_X vs. log T*_X) drift with redshift in the Platinum GRB sample, and does accounting for that drift improve a GRB Hubble diagram?

## Summary of findings
- **Baseline fit (full sample, N=50): a = −0.973 (+0.165/−0.167), C0 = 52.10 (+0.62/−0.61), σ_int = 0.605 dex.**
- **No statistically significant redshift evolution detected. A chi-squared consistency test across three equal-number redshift bins gives χ²/dof = 0.696, p = 0.499 — the three bin slopes are mutually consistent within measurement uncertainty.**
- A supplementary 3-point linear drift fit (a(z) = a0 + a1·z) suggests a downward trend, but with only 1 degree of freedom (3 points, 2 fitted parameters), its formal significance is not statistically meaningful — included in the figures for illustration only, not as a claim. The chi-squared test above is the statistically valid result.
- Fitting the relation per redshift bin rather than globally reduces RMS residual scatter in the derived Hubble diagram by 22.1% (0.585 to 0.456 dex) — some practical value in locally-calibrated fits even without a formally significant global trend.

   
## Data

50 GRBs from the Platinum sample: **Cao, Dainotti & Ratra (2022)**, MNRAS 512, 439-454 (arXiv:2201.05245), Table A1.

This work fits the **2-parameter** Dainotti relation (log L_X vs. log T*_X only). The published paper fits a 3-parameter "fundamental plane" relation that also includes peak prompt luminosity (L_peak). Comparisons to the published a, b, C0 values in this notebook are included as context, not as strict validation, given that difference — see the "Literature comparison" cell for details. The nearest true like-for-like comparison for the 2-parameter slope is Dainotti et al. (2010), a ~ -1.06 +/- 0.27.


## Method

1. Parse the published Table A1 values (with asymmetric/symmetric uncertainties) into clean numeric columns.
2. Compute L_X from measured flux, redshift, and a fiducial flat LCDM cosmology (H0 = 70, Om0 = 0.3). Caveat: using a fixed fiducial cosmology to derive L_X and then discussing cosmological implications from those same derived luminosities would be circular if used to constrain cosmological parameters. This project avoids that by comparing relative residual scatter between fits, not deriving new cosmological constraints.
3. Fit the relation via MCMC (emcee) using a D'Agostini likelihood with intrinsic scatter as a free parameter, validated against the full sample as a baseline.
4. Bin the sample into 3 equal-number redshift bins (N=16, 17, 17) and refit independently in each bin.
5. Test bin-to-bin slope consistency via a chi-squared test, and — with explicit caution about its limited statistical power — via a linear drift fit across the 3 bin medians.
6. Compare Hubble-diagram residuals between the global fit and the per-bin fits (Figure 5), the project's main practically relevant result.
   
## Repo contents
- ['dainotti_evolution.ipynb'](dainotti_evolution.ipynb) - full analysis notebook
- [`data/Platinum_sample.csv`](data/Platinum_sample.csv) - parsed Table A1 data
- [`figures/`](figures) - all output figures (PDF + PNG)

   
## Requirements

...
numpy scipy pandas matplotlib astropy emcee corner
...

## Limitations

- Small sample (50 GRBs total, ~16-17 per bin) limits statistical power to detect subtle evolution.
- The 3-bin linear drift fit has only 1 degree of freedom and should not be read as a robust detection - see caveat above.
- This project does not apply the Efron-Petrosian bias-correction method used in the source literature to separate true redshift evolution from selection effects. The binning approach here tests for evolution directly, at the cost of not cleanly separating that from selection bias.
- Fiducial-cosmology dependence in computing L_X (see Method, step 2).
- Fits a 2-parameter relation; the source literature's headline result uses a 3-parameter fit including peak prompt luminosity.
