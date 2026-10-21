# Optical images of MAXI J1750−327 during its 2026 outburst

This repository contains the stacked, astrometrically calibrated optical images of the black hole candidate X-ray transient **MAXI J1750−327** taken in the *B*, *V*, *R*<sub>C</sub>, and *I*<sub>C</sub> bands from 2026 August 20 to October 8 (UT).
The data were used in Mizumoto & Shidatsu (2026, PASJ, submitted).

- Target: MAXI J1750−327 (R.A. = 17:50:03.04, Dec. = −32:42:45.4, J2000; *l* = 357.26°, *b* = −2.77°)
- Epochs: 10 (nine nights in August–September and one stack combining October 6–8)
- Bands: *B*, *V*, *R*<sub>C</sub>, *I*<sub>C</sub> (see "Filters" below for the actual filters)

## Files

The images are FITS files compressed with `fpack` (Rice compression).
Each file is named `YYYYMMDD<band>.fits.fz`, where `YYYYMMDD` is the UT date of the observation and `<band>` is one of `B`, `V`, `R`, and `I`.
The stack combining the data of 2026 October 6, 7, and 8 is named `20261006-08<band>.fits.fz`.

| Date (UT) | *B* | *V* | *R*<sub>C</sub> | *I*<sub>C</sub> |
|---|---|---|---|---|
| 2026-08-20 | 14 | 14 | 14 | 4 |
| 2026-08-22 | 14 | 14 | 14 | 14 |
| 2026-08-24 | 14 | 14 | 14 | 14 |
| 2026-08-27 | 14 | 14 | 14 | 14 |
| 2026-08-28 | 14 | 14 | 14 | 11 |
| 2026-08-29 | 12 | 12 | 12 | 9 |
| 2026-09-03 | 14 | 14 | 14 | 10 |
| 2026-09-06 | 13 | 13 | 13 | 9 |
| 2026-09-10 | 12 | 13 | 12 | 8 |
| 2026-10-07 | 12 | 22 | 19 | 13 |

The numbers give the number of 60 s exposures combined into each stacked image.

## Instrument

- Telescope: RA501, a 0.51 m PlaneWave Corrected Dall-Kirkham (CDK) telescope (focal length 2259 mm, f/4.4) at Siding Spring Observatory, Australia (31°16′24″ S, 149°04′11″ E, 1118 m), operated by RICOH COMPANY, LTD through the remote observation service "RIKORIMO"
- Detector: Player One ZEUS 455M PRO camera with a Sony IMX455 CMOS sensor, cooled to −10 °C
- Binning: 2 × 2, giving a pixel scale of 0.685″ pixel<sup>−1</sup> and a field of view of about 55′ × 36′
- Exposure time: 60 s per frame

## Filters

The *B*, *V*, and *R*<sub>C</sub> images were taken with the Astrodon E-Series Blue, Green, and Red filters, respectively, and the *I*<sub>C</sub> images with the Astrodon Johnson/Cousins Ic filter.
The E-Series filters are designed for color imaging, and their passbands differ from the Johnson–Cousins ones; in particular, the Red filter cuts off sharply at about 700 nm.
The band names in the file names refer to the Johnson–Cousins bands onto which the data were calibrated in the paper, with color terms; **the images themselves are not photometrically calibrated** (see "Notes on photometry").

## Data processing

1. Each 60 s frame was bias-, dark-, and flat-field-corrected, and a bad-pixel map was applied, by the observatory pipeline (MaxIm DL).
2. The frames of each band and night were aligned and average-combined with [ASTAP](https://www.hnsky.org/astap.htm) (ver. 2026-09-15). The data of October 6–8 were combined into a single stack in each band.
3. The astrometric solution of each stacked image was derived with ASTAP and is stored in the FITS header as a TAN-projection WCS (`CRVAL1/2`, `CRPIX1/2`, `CD1_1`–`CD2_2`; `PLTSOLVD = T`).

The pixel values are in ADU and include the offset given by the `PEDESTAL` keyword.
No photometric calibration or extinction correction has been applied.

## Reading the data

The compressed image is stored in the first extension (HDU 1); the primary HDU is empty.
Astropy reads the compressed files directly:

```python
from astropy.io import fits
from astropy.wcs import WCS

with fits.open("20260820V.fits.fz") as hdul:
    data = hdul[1].data
    header = hdul[1].header
wcs = WCS(header)
print(header["DATE-OBS"], header["FILTER"], data.shape)
```

To restore uncompressed FITS files, use `funpack` from [CFITSIO](https://heasarc.gsfc.nasa.gov/fitsio/fpack/):

```sh
funpack 20260820V.fits.fz        # creates 20260820V.fits
```

## Notes on photometry

The following points were taken into account in the analysis of the paper and may be useful for reusing the data.

- **Effective gain.** The frames were average-combined, so the effective gain of a stacked image is `EGAIN` × (number of combined frames), where `EGAIN` (≈ 0.26 e<sup>−</sup> ADU<sup>−1</sup>) is the gain of a single frame given in the header.
- **Crowding.** The target lies in a crowded field toward the Galactic bulge, and the point-spread function varies across the wide field of view. In the paper, comparison stars were restricted to those within 5′ of the target, with reference magnitudes from the Gaia DR3 synthetic photometry in the Johnson–Kron–Cousins system.
- **Color terms.** Because of the filter passbands, color terms are required to convert the instrumental magnitudes onto the Johnson–Cousins system; they are significant in the *B* and *V* bands and negligible in the *R*<sub>C</sub> band.
- ***B* band.** The *B* − *V* color of the target (≈ 1.6) lies outside the color range of the comparison stars with a valid *B*-band standardization in the Gaia DR3 synthetic photometry (*B* − *V* ≤ 1.47), so the standardized *B*-band magnitudes rely on an extrapolation of the color term.
- **Observation time.** Use the mid-time of the stack recorded in the header, not the start time of the first frame.

## Citation

If you use these data, please cite:

> Mizumoto, M., & Shidatsu, M. 2026, PASJ, submitted


## Acknowledgments

The observations were made with the remote observation service "RIKORIMO" provided by RICOH COMPANY, LTD. We thank Sakae Kisara for the observations.

## Contact

Misaki Mizumoto (University of Teacher Education Fukuoka) — mizumoto-m@fukuoka-edu.ac.jp
