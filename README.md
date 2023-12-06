# QuasarModel-DESI
Quasar Models from Brodzeller et al. (2023) and updated HIZ template that accounts for lya mean transmission. Files are compatable with [redrock](https://github.com/desihub/redrock). LOZ/HIZ_init will run in redrock main branch, but HIZ (lya optical depth upgraded model) requires customized redrock tag. See **TODO: ADD LINK** for more details. Contact abrodze@gmail.com for questions about this repository.

# Files

**rrtemplate-qso-LOZ.fits** - low redshift (0.05<z<1.6) quasar templates presented in Brodzeller+23

**rrtemplate-qso-HIZ_init.fits** - high redshift (1.4<z<7) quasar templates presented in Brodzeller+23

**rrtemplate-qso-HIZ.fits** - Upgraded high redshift (1.4<z<1.7) quasar templates, trained on identical sample as above HIZ but individual spectra were corrected
                          for Lya effective optical depth via Kamble et al. (2020); redshift performance described in Bault et al. (2024)

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
