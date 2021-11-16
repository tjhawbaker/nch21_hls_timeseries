# NASA 2021 Cloud Hackathon Project

Todd Hawbaker, Jodi Riegle, and Kehan Yang

## Project description
A cloud-deployable framework to identify burned areas in the harmonized Landsat Sentinel-2 data, and possibly other remotely sensed data sources.  Basically, extending what we've done with Landsat to HLS data (Hawbaker et. al., 2020).  The end product we are planning is code or a container that users could run in the cloud to generate a time series of burned area products for their area of interest.

Hawbaker, T.J, M.K. Vanderhoof, G.L. Schmidt, Y. Beal, J.J. Picotte, J.D. Takacs, J.T. Falgout, and J.L. Dwyer. 2020. The Landsat Burned Area algorithm and products for the conterminous United States. Remote Sensing of Environment, 244, 111801, https://doi.org/10.1016/j.rse.2020.111801

## HLS data description
https://lpdaac.usgs.gov/news/release-of-harmonized-landsat-and-sentinel-2-hls-version-20/

## Tasks
1. Build standardize tools to query and acquire links to HLS data from the EarthData library.  See https://nasa-openscapes.github.io/2021-Cloud-Hackathon/tutorials/02_Data_Discovery_CMR-STAC_API.html for examples.  Code developed is in query_and_acquire_HLS_data.ipynb  Download time for 2021 HLS data for tile 13TDE is 331 s.
2. Create a rioxarray Dataset consisting of DataArrays for bands shared by Sentinel and Landsat, with date and time information.  See https://nasa-openscapes.github.io/2021-Cloud-Hackathon/tutorials/05_Data_Access_Direct_S3.html.
  Shared bands include: Fmask, blue, green, red, nir, swir1, swir2, sun zenith and azimuth angles, view zenith and azimuth angles.
  See read_hls_data.ipynb
3. Generate cloud-free masks from the Fmask bands and replace masked areas in the Dataset with Null values.
4. Use band math to add spectral indices to the Dataset.  See read_hls_data.ipynb
5. Applying a moving window to the time series of spectral indices (e.g., mean NDVI over the past 30 days).
6. Calculate difference between spectral indices for each scene and the moving window time series.
7. Apply a classifier to the spectral index differences, save the results.

## Performance tests
1. File read speed from EC2 disk vs. S3 bucket
2. Spectral index calculation vs. n processors
3. Temporal aggregation vs. n processors
