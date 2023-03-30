# Ven&mu;s on AWS

This document describes Ven&mu;s L2A data available on AWS.

## Overview
The Venµs science mission is a joint research mission undertaken by CNES and ISA,
the Israel Space Agency. It aims to demonstrate the effectiveness of high-resolution multi-temporal observation optimised through
Copernicus, the global environmental and security monitoring programme. Venµs was launched from the Centre Spatial Guyanais by a
VEGA rocket, during the night from 2017, August 1st to 2nd. Thanks to its multispectral camera (12 spectral bands in the visible
and near-infrared ranges, with spectral characteristics provided here), it
acquires imagery every 1-2 days over 100+ areas at a spatial resolution of 4 to 5m. This dataset has been converted into Cloud Optimized GeoTIFFs
(COGs). Additionally, SpatioTemporal Asset Catalog metadata are generated in a JSON file alongside the data. This dataset contains
all of the Venus L2A datasets and will continue to grow as the Venus mission acquires new data over the preselected sites.

### Ven&mu;s spectral bands
| Band | Common name | Centre wavelength (nm) |
| :--: | :---------: | :--------------------: |
| B1 | coastal | 424 |
| B2 | coastal | 447 |
| B3 | blue | 492 |
| B4 | green | 555 |
| B5 | yellow | 620 |
| B6 | yellow | 620 |
| B7 | red | 666 |
| B8 | rededge | 702 |
| B9 | rededge | 741 |
| B10 | nir08 | 861 |
| B11 | nir09 | 909 |

## Accessing Ven&mu;s L2A data on AWS

The Ven&mu;s Open Data can be accessed directly in S3. Scenes are organized using the following directory structure:
```
s3://venus-l2a-cogs/{site}/{YYYY}/{MM}/{DD}/VENUS-XS_{YYYYMMDD}-{hhmmss}-000_L2A_{site}_D/
```
where:
* `site`: Collection site
* `YYYY`: Acquisition date's year
* `MM`: Acquisition date's month
* `DD`: Acquisition date's day
* `YYYYMMDD`: Acquisition date
* `hhmmss`: Acquisition time

## Contents
This section describes the assets stored in each directory.

### Open data STAC JSON
The STAC JSON asset contains the STAC metadata associated with the dataset.
This asset is named according to the following naming convention:
```
s3://venus-l2a-cogs/{site}/{YYYY}/{MM}/{DD}/VENUS-XS_{YYYYMMDD}-{hhmmss}-000_L2A_{site}_D/VENUS-XS_{YYYYMMDD}-{hhmmss}-000_L2A_{site}_D_STAC.json
```

### Surface Reflectance
Surface reflectance assets contain integer-coded surface reflectances for each of the 12 [Ven&mu;s spectral bands](#venμs-spectral-bands), corrected for atmospheric effects, including adjacency effects.

Surface reflectance assets are Cloud Optimized GeoTIFFs (COGs), coded as 16-bit unsigned integers, with a nodata value of 0.

To obtain reflectance values from these files one must multiply by 0.0001 and subtract 0.1, i.e.
```
reflectance = 0.0001 * DN - 0.1
```
where `DN` is the 16-bit unsigned integer value in the file.

Surface reflectance assets are named according to the following naming convention:
```
s3://venus-l2a-cogs/{site}/{YYYY}/{MM}/{DD}/VENUS-XS_{YYYYMMDD}-{hhmmss}-000_L2A_{site}_D/VENUS-XS_{YYYYMMDD}-{hhmmss}-000_L2A_{site}_C_V3-1_SRE_{band}.tif
```
where:
* `site`: Collection site
* `YYYY`: Acquisition date's year
* `MM`: Acquisition date's month
* `DD`: Acquisition date's day
* `YYYYMMDD`: Acquisition date
* `hhmmss`: Acquisition time
* `band`: Band number, prefixed with `B` (for example, `B8`)

### Flat Reflectance
Flat reflectance assets contain integer-coded surface reflectances for each of the 12 [Ven&mu;s spectral bands](#venμs-spectral-bands), corrected for atmospheric effects, including adjacency effects and slope effects. Compared to surface reflectance, flat reflectance assets suppress apparent reflectance variations due to the terrain orientation with respect to the sun.

Flat reflectance assets are Cloud Optimized GeoTIFFs (COGs), coded as 16-bit unsigned integers, with a nodata value of 0.

To obtain reflectance values from these files one must multiply by 0.0001 and subtract 0.1, i.e.
```
reflectance = 0.0001 * DN - 0.1
```
where `DN` is the 16-bit unsigned integer value in the file.

Flat reflectance assets are named according to the following naming convention:
```
s3://venus-l2a-cogs/{site}/{YYYY}/{MM}/{DD}/VENUS-XS_{YYYYMMDD}-{hhmmss}-000_L2A_{site}_D/VENUS-XS_{YYYYMMDD}-{hhmmss}-000_L2A_{site}_C_V3-1_FRE_{band}.tif
```
where:
* `site`: Collection site
* `YYYY`: Acquisition date's year
* `MM`: Acquisition date's month
* `DD`: Acquisition date's day
* `YYYYMMDD`: Acquisition date
* `hhmmss`: Acquisition time
* `band`: Band number, prefixed with `B` (for example, `B8`)


### Atmospheric and Biophysical Parameters
The Atmospheric and Biophysical parameters asset contains the atmospheric variables used to correct for atmospheric effects.

Atmospheric and and Biophysical parameters assets are Cloud Optimized GeoTIFFs (COGs), each containing two bands coded as 8-bit unsigned integers, each with a nodata value of 0:
1. Band 1 contains water vapour content, in g/cm^2, obtained by multiplying the values by 0.05, i.e. `water vapour = 0.05 * DN`, where `DN` is the 8-bit unsigned integer value
2. Band 2 contains aerosol optical thickness, obtained by multiplying the values by 0.005, i.e. `aerosol optical thickness = 0.005 * DN`, where `DN` is the 8-bit unsigned integer value.

This asset is named according to the following naming convention:
```
s3://venus-l2a-cogs/{site}/{YYYY}/{MM}/{DD}/VENUS-XS_{YYYYMMDD}-{hhmmss}-000_L2A_{site}_D/VENUS-XS_{YYYYMMDD}-{hhmmss}-000_L2A_{site}_C_V3-1_ATB_XS.tif
```
where:
* `site`: Collection site
* `YYYY`: Acquisition date's year
* `MM`: Acquisition date's month
* `DD`: Acquisition date's day
* `YYYYMMDD`: Acquisition date
* `hhmmss`: Acquisition time

### Thumbnail
The Thumbnail asset contains a quicklook of the image, formatted as JPEG.

This asset is named according to the following naming convention:
```
s3://venus-l2a-cogs/{site}/{YYYY}/{MM}/{DD}/VENUS-XS_{YYYYMMDD}-{hhmmss}-000_L2A_{site}_D/VENUS-XS_{YYYYMMDD}-{hhmmss}-000_L2A_{site}_C_V3-1_QKL_ALL.jpg
```
where:
* `site`: Collection site
* `YYYY`: Acquisition date's year
* `MM`: Acquisition date's month
* `DD`: Acquisition date's day
* `YYYYMMDD`: Acquisition date
* `hhmmss`: Acquisition time

### Quality Mask
The Quality Mask asset defines areas in the product that contain blackfill (nodata) or are filled with content such as cloud. Each band in the Quality Mask represents a particular aspect as described below

| Band Number | Description | Definition |
|-------------|-------------|------------|
| 0 | No data | If 2, indicates that the area contains nodata<br/>If 1, indicates that the area does not contain nodata |
| 1 | Cloud | If 2, indicates that the area contains cloud<br/>If 1, indicates that the area does not contain cloud<br/>If 0, indicates that the area contains nodata |
| 2 | Haze | If 2, indicates that the area contains haze<br/>If 1, indicates that the area does not contain haze<br/>If 0, indicates that the area contains nodata |
| 3 | Cloud Shadow | If 2, indicates that the area contains cloud shadow<br/>If 1, indicates that the area does not contain cloud shadow<br/>If 0, indicates that the area contains nodata |
| 4 | Thin Cirrus | If 2, indicates that the area contains thin cirrus<br/>If 1, indicates that the area does not contain thin cirrus<br/>If 0, indicates that the area contains nodata |
| 5 | Snow | If 2, indicates that the area contains snow<br/>If 1, indicates that the area does not contain snow<br/>If 0, indicates that the area contains nodata |
| 6 | Water | If 2, indicates that the area contains water<br/>If 1, indicates that the area does not contain water<br/>If 0, indicates that the area contains nodata |

This asset is named according to the following naming convention:
```
s3://venus-l2a-cogs/{site}/{YYYY}/{MM}/{DD}/VENUS-XS_{YYYYMMDD}-{hhmmss}-000_L2A_{site}_D/VENUS-XS_{YYYYMMDD}-{hhmmss}-000_L2A_{site}_C_V3-1_QUALITY_MASK.tif
```
where:
* `site`: Collection site
* `YYYY`: Acquisition date's year
* `MM`: Acquisition date's month
* `DD`: Acquisition date's day
* `YYYYMMDD`: Acquisition date
* `hhmmss`: Acquisition time

### Detailed Processing Data
Auxiliary data assets are provided in a subdirectory of the scene directory named `DATA`. The following files are included:
* Job processing information (`*_JPI_ALL.xml`)
* Solar angles (`*_SOL_ALL.tif`)
* Viewing angles (`*_VIE_ALL.tif`)
* Useful image information (`*_UTI_ALL.xml`)

### Detailed Masks
Detailed masks are provided in a subdirectory of the scene directory named `MASKS`. The following files are included:
* Cloud and shadows mask (`*_CLM_XS.tif`)
* Edge (nodata) mask (`*_EDG_XS.tif`)
* Interpolated water vapour and aerosol optical thickness pixel mask (`*_IAB_XS.tif`)
* Geophysical mask (`*_MG2_XS.tif`)
* Interpolated pixel mask (`_PIX_XS.tif`)
* Saturated pixel mask (`_SAT_XS.tif`)
* Useful image mask (`_USI_XS.tif`)
