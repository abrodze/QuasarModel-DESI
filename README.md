# QuasarModel-DESI
Files accompanying [Brodzeller et al. (2023)](https://ui.adsabs.harvard.edu/abs/2023AJ....166...66B/abstract). Included in this repository are the quasar templates, SDSS redshift catalog used for training the templates, and an updated HIZ template that accounts for lya mean transmission. The latter model is discussed in Bault et al. (2024). All template files are compatable with [redrock](https://github.com/desihub/redrock). LOZ/HIZv1.0 produced the standard quasar classifications for DESI year one data release. HIZv1.1 refined the z>1.6 DESI year one redshifts for Lya forest cosmology studies, redshift catalog to be released TBD. 

Contact abrodze@gmail.com for questions about this repository.

# Files

**rrtemplate-qso-LOZ.fits** - low redshift (0.05<z<1.6) quasar templates presented in Brodzeller+23

**rrtemplate-qso-HIZv1.0.fits** - high redshift (1.4<z<7) quasar templates presented in Brodzeller+23

**rrtemplate-qso-HIZv1.1.fits** - Upgraded high redshift (1.4<z<1.7) quasar templates, trained on identical sample as above HIZ but individual spectra were corrected
                          for Lya effective optical depth via Kamble et al. (2020); redshift performance described in Bault et al. (2024)

**trainingsample.fits** - catalog of SDSS spectra used to train the above templates; data model described below

# Example plotting for model components

To plot the individual eigenspectra from each file in python:
```
from astropy.io import fits
import numpy as np
import matplotlib.pyplot as plt

with fits.open('rrtemplate-qso-LOZ.fits') as h:
    
    wave = 10**(h[0].header['CRVAL1'] + np.arange(h[0].header['NAXIS1'])*h[0].header['CDELT1']) #wavelength stored in log lambda format
    vectors = h['BASIS_VECTORS'].data #eigenspectra
    #redshifts = h['REDSHIFTS'].data #redshift coverage of model for running redrock

for i in range(6):
    
    plt.plot(wave, vectors[i]/np.median(vectors[i]), label=f'vector #{i}')
    
plt.xlabel(r'rest-frame wavelength [$\AA$]')
plt.ylabel('flux desnity')
plt.legend()
plt.show()
````

# Data Model trainingsample.fits

| Column name | # | Description |
| - | -: | - |
| plate       | 0 | SDSS spectroscopic plate number |   
| mjd         | 1 | Modified Julian day of the spectroscopic observation |   
| fiberid     | 2 | Spectroscopic fiber number |   
| SNR         | 3 | mean signal-to-noise ratio per pixel of spectrum calculated using only lambda_RF(Z_PCA) > 1216 AA |
| Z_PCA       | 4 | PCA redshift derived by the redvsblue algorithm, provided in the SDSS DR16 quasar catalog | 
| Z_FIN       | 5 | Redshift used to train LOZ/HIZ quasar templates; derivation described in Section 3.2 of Brodzeller et al. (2023) | 
| Z_REFINE    | 6 | Redshift determined when Lya forest is masked. If not -1, this redshift is used instead of Z_FIN | 

