---
layout: page
title: HPAI Spillover
description: Zoonosis Risk 
img: assets/img/zoonosis_risk/waterfowl_in_sac_valley.png
importance: 1
category: work
related_publications: true
---

<div class="row" style="margin-top: 20px; margin-bottom: 20px; margin-left: 10px; margin-right: 10px;">
    <img src="/assets/img/zoonosis_risk/black-necked_stilts_ca_rice_commission.png" 
    alt="Black-necked stilts in California rice field" width="100%" height="100%" longdesc="https://caes.ucdavis.edu/sites/g/files/dgvnsk1721/files/media/images/Black-necked%20stilts%20%28c%29%20Courtesy%20California%20Rice%20Commission.jpg" /> 
</div>


How do natural sources of highly pathogenic avian influenza (HPAI) at the wildlife–agriculture interface influence spillover risk in California's Central Valley?

With each animal that is infected, there is another chance for the influenza to evolve into a virus fit for human-to-human transmission. Creating early warning systems built on a One Health framework–integrating human, animal, and environmental health–are a wise investment to support state biosecurity. One Health surveillance systems can be incorporated into conservation strategies for timely interventions that safeguard human and animal health. 

This project will explore the county in the valley with the highest HPAI detection in wild birds which is Yolo County. Two sources of pathogen transmission will be studied: maintenance hosts (natural reservoirs of the virus) and bridge hosts (birds linking maintenance hosts to target populations). The data will encompass features ranging from seasonal waterfowl migration drivers (land surface temperature and vegetation) to a key agent affecting virus infectivity (surface water temperature). The temporal scope will be isolated to the wintering period for maintenance hosts (mid-October through January). Given characteristics associated with host presence and high infectivity in addition to host daily range, a risk score will be calculated for raster cells based on either similarity or proximity to hosts, resulting in a composite risk raster for the county indicating seasonal spillover risk from a prominent waterfowl habitat.

#### Data Description



#### Data Citation



#### Methods



#### Analysis

Imports

```python
import warnings
warnings.filterwarnings('ignore')

import sys
import os
import time
import pathlib
import zipfile
from getpass import getpass
from glob import glob

import pandas as pd
import geopandas as gpd
import cartopy.crs as ccrs
import holoviews as hv
import hvplot.pandas
hvplot.extension('bokeh')
import cartopy.crs as ccrs
import pygbif.occurrences as occ
```

```python
# Access GBIF

reset_credentials = False
# GBIF needs a username, password, and email
credentials = dict(
    GBIF_USER=(input, ''),
    GBIF_PWD=(getpass, ''),
    GBIF_EMAIL=(input, ''),
)

for env_variable, (prompt_func, prompt_text) in credentials.items():
    # Delete credential from environment if requested
    if reset_credentials and (env_variable in os.environ):
        os.environ.pop(env_variable)
    # Ask for credential and save to environment
    if not env_variable in os.environ:
        os.environ[env_variable] = prompt_func(prompt_text)
```

Set Paths

```python
# Plots
plots_dir = os.path.join(
    # Home directory
    pathlib.Path.home(),
    'Projects',
    # Project directory
    'zoonosis-risk',
    'plots'
)

# Project data directory 
data_dir = os.path.join(
    # Home directory
    pathlib.Path.home(),
    'Projects',
    # Project directory
    'zoonosis-risk',
    'data'
)

# CA boundaries
boundary_dir = os.path.join(
    # Home directory
    pathlib.Path.home(),
    'Projects',
    # Project directory
    'zoonosis-risk',
    'boundary_data'
)

# Boundaries 
county_dir = os.path.join(boundary_dir, 'ca-county-boundaries')
pub_land_dir = os.path.join(boundary_dir, 'ca-public-access-lands')
# Species
gbif_snow_goose_dir = os.path.join(data_dir, 'gbif', 'snow-goose')
gbif_mallard_dir = os.path.join(data_dir, 'gbif', 'mallard')
gbif_red_winged_blackbird_dir = os.path.join(data_dir, 'gbif', 
                                             'red-winged-blackbird')
gbif_savannah_sparrow_dir = os.path.join(data_dir, 'gbif', 'savannah-sparrow')
gbif_house_sparrow_dir = os.path.join(data_dir, 'gbif', 'house-sparrow')
gbif_killdeer_dir = os.path.join(data_dir, 'gbif', 'killdeer')
gbif_rock_pigeon_dir = os.path.join(data_dir, 'gbif', 'rock-pigeon')

os.makedirs(plots_dir, exist_ok=True)
os.makedirs(data_dir, exist_ok=True)

os.makedirs(county_dir, exist_ok=True)
os.makedirs(pub_land_dir, exist_ok=True)
os.makedirs(gbif_snow_goose_dir, exist_ok=True)
os.makedirs(gbif_mallard_dir, exist_ok=True)
os.makedirs(gbif_red_winged_blackbird_dir, exist_ok=True)
os.makedirs(gbif_savannah_sparrow_dir, exist_ok=True)
os.makedirs(gbif_house_sparrow_dir, exist_ok=True)
os.makedirs(gbif_killdeer_dir, exist_ok=True)
os.makedirs(gbif_rock_pigeon_dir, exist_ok=True)
```

Load Utility Functions

```python
def normalize_occurrences(df):
    """
    Normalize occurrence data for given species.

    Args:
    occ_df (pandas.DataFrame): DataFrame of species occurrence.

    Returns:
    norm_occ_df (pandas.DataFrame): DataFrame of normalized species occurrence.
    """
    
    occ_df = (
        df
        # For each month
        .groupby(['host_name', 'month'])
        # count the number of occurrences
        .agg(occurrences=('host_name', 'count'))
    )

    # Drop rare observations (possible misidentification)
    occ_df = occ_df[occ_df["occurrences"] > 1]

    # Take the mean by month
    mean_occurrences_by_month = (
        occ_df
        .groupby(['month'])
        .mean()
    )

    occ_df['norm_occurrences'] = (
        occ_df
        / mean_occurrences_by_month
    )

    return occ_df 
```
```python
def load_gbif(gbif_csv):
    """
    Load occurrence data from GBIF CSV.

    Args:
    gbif_csv (str): A path to downloaded species CSV.

    Returns:
    (pandas.DataFrame): A species occurrence DataFrame.
    """
    return pd.read_csv(
        gbif_csv, 
        delimiter='\t',
        index_col='gbifID',
        on_bad_lines='skip',
        usecols=['gbifID', 'order', 'species', 'month', 'day', 'year', 
                 'decimalLatitude', 'decimalLongitude']
    )
```
```python
def download_gbif(species, gbif_credentials, species_dict, 
                  period, site_bounds):
    """
    Download occurrence data for given species.

    Args:
    species (str): Input host species name. 
    gbif_credentials (dict): GBIF API credentials.
    species_dict (dict): Host species GBIF metadata.
    period (str): Temporal range of observations.
    site_bounds (list): Site boundary coordinates.

    Returns:
    gbif_path (str): A path to downloaded species CSV.
    """
    
    gbif_dir = species_dict[species]['DATA PATH']
    species_key = species_dict[species]['GBIF SPECIES']
    species_download_key = species_dict[species]['GBIF DOWNLOAD']
    site_latitude = f'{site_bounds[1]},{site_bounds[3]}'
    site_longitude = f'{site_bounds[0]},{site_bounds[2]}'
    
    # Only download once
    gbif_pattern = os.path.join(gbif_dir, '*.csv')
    if not glob(gbif_pattern):
        # Submit query to GBIF
        gbif_query = occ.download([
            f"speciesKey = {species_key}",
            f"eventDate = {period}",
            f"decimalLatitude = {site_latitude}",
            f"decimalLongitude = {site_longitude}",
            "hasCoordinate = TRUE",
        ],
        user=gbif_credentials['GBIF_USER'][1], 
        pwd=gbif_credentials['GBIF_PWD'][1], 
        email=gbif_credentials['GBIF_EMAIL'][1])

        # Only download once
        if not species_download_key in os.environ:
            os.environ[species_download_key] = gbif_query[0]

            # Wait for the download to build
            wait = occ.download_meta(
                    os.environ[species_download_key])['status'] 
            while not wait=='SUCCEEDED':
                wait = occ.download_meta(
                        os.environ[species_download_key])['status'] 
                time.sleep(5)

            # Download GBIF data
            download_info = occ.download_get(
                os.environ[species_download_key], 
                path=gbif_dir)

            # Unzip GBIF data
            with zipfile.ZipFile(download_info['path']) as download_zip:
                download_zip.extractall(path=gbif_dir)

    # Find the extracted .csv file path
    gbif_path = glob(gbif_pattern)[0]
    
    return gbif_path
```

Sites

```python
# Set up California County Boundaries - Yolo (2024) path
yolo_path = os.path.join(county_dir, 'ca_county.geojson')

# Load in the county data
yolo_county_gdf = gpd.read_file(yolo_path)

# Site plot
yolo_boundary_plt = (
    yolo_county_gdf
    .to_crs(ccrs.Mercator())
    .hvplot(
        title='Yolo County',
        line_color='#ffb403', fill_color=None,
        line_width=3,
        crs=ccrs.Mercator(), tiles='EsriImagery',
        frame_width=600, frame_height=650)
)
```

```python
# Set up California Department of Fish and Wildlife (CDFW) Public Access Lands path
pub_land_path = os.path.join(pub_land_dir, 'ca_public_access_land.geojson')

# Load in the Yolo Bypass Wildlife Area data
yolo_bypass_gdf = gpd.read_file(pub_land_path)

# Nested habitat 
(
    yolo_boundary_plt * yolo_bypass_plt
).opts(
    title='Yolo County and Bypass Wildlife Area',
    width=600, height=650
)
```

<div class="row" style="margin-top: 20px; margin-bottom: 20px; margin-left: 10px; margin-right: 10px;">
    <img src="/assets/img/zoonosis_risk/yolo_county_and_bypass.png" alt="Yolo County and Bypass Wildlife Area" width="70%" height="70%" /> 
</div>

Surface Water Temperature (Habitat Inlet)

```python
"""
USGS Daily Values Service
Station: Cache C Outflow From Settling Basin NR Woodland CA - 11452900 
About: Inlet to Yolo Bypass Wildlife Area
Time period: 10-20-2024 to 01-31-2025
Metric: Temperature, water, degrees Celsius
"""

USGS_DAILY_URL = (
    'https://nwis.waterservices.usgs.gov/nwis/iv/?'
    'sites=11452900&agencyCd=USGS&'
    'startDT=2024-10-20T00:00:00.000-07:00&'
    'endDT=2025-01-30T23:59:59.999-08:00&'
    'parameterCd=00010&format=json'
)

usgs_daily_water = pd.read_json(USGS_DAILY_URL)
```

```python
# Transform dates

usgs_daily_water_time_series = usgs_daily_water.iloc[1]
dwts_values = usgs_daily_water_time_series.value[0]['values'][0]['value']
dwts_df = pd.DataFrame(dwts_values)

dwts_df = dwts_df.assign(
    datetime_date=lambda x: pd.to_datetime(x['dateTime']))

dwts_df['date'] = dwts_df.datetime_date.apply(lambda x: pd.to_datetime(
                                                x.strftime('%Y-%m-%d')))
```

```python
# Plot daily surface water temperatures

dwts_df_subset = dwts_df[['date', 'value']]
dwts_df_subset['water_temperature'] = dwts_df_subset[
                                        'value'].astype(float)
dwts_df_subset.drop('value', axis=1, inplace=True)

daily_water_temps = dwts_df_subset.groupby('date').agg(
    Minimum=('water_temperature', 'min'),
    Maximum=('water_temperature', 'max'),
    Average=('water_temperature', 'mean')
)

daily_water_temps.hvplot.line(
    label='Daily Surface Water Temperature (Yolo Bypass Wildlife Area Inlet)',
    xlabel='Date', ylabel='Temperature',
    height=500, width=900,
    legend='top_right', group_label='Temperature (°C)'
)
```

<div class="row" style="margin-top: 20px; margin-bottom: 20px; margin-left: 10px; margin-right: 10px;">
    <img src="/assets/img/zoonosis_risk/YBWA_inlet_daily_surface_water_temperature.png" alt="Yolo Bypass Wildlife Area Inlet Daily Surface Water Temperature" width="80%" height="70%" /> 
</div>

Hosts

```python
HOSTS = {
    'Snow Goose': {
        'DATA PATH': gbif_snow_goose_dir,
        'GBIF SPECIES': '2498167',
        'GBIF DOWNLOAD': 'GBIF_DOWNLOAD_KEY_2498167'
    },
    'Mallard': {
        'DATA PATH': gbif_mallard_dir,
        'GBIF SPECIES': '9761484',
        'GBIF DOWNLOAD': 'GBIF_DOWNLOAD_KEY_9761484'
    },
    'Red-winged Blackbird': {
        'DATA PATH': gbif_red_winged_blackbird_dir,
        'GBIF SPECIES': '9409198',
        'GBIF DOWNLOAD': 'GBIF_DOWNLOAD_KEY_9409198'
    },
    'Savannah Sparrow': {
        'DATA PATH': gbif_savannah_sparrow_dir,
        'GBIF SPECIES': '5231142',
        'GBIF DOWNLOAD': 'GBIF_DOWNLOAD_KEY_5231142'
    },
    'House Sparrow': {
        'DATA PATH': gbif_house_sparrow_dir,
        'GBIF SPECIES': '5231190',
        'GBIF DOWNLOAD': 'GBIF_DOWNLOAD_KEY_5231190'
    },
    'Killdeer': {
        'DATA PATH': gbif_killdeer_dir,
        'GBIF SPECIES': '2480320',
        'GBIF DOWNLOAD': 'GBIF_DOWNLOAD_KEY_2480320'
    },
    'Rock Pigeon': {
        'DATA PATH': gbif_rock_pigeon_dir,
        'GBIF SPECIES': '2495414',
        'GBIF DOWNLOAD': 'GBIF_DOWNLOAD_KEY_2495414'
    }
}
```

```python

```

```python

```

```python

```

```python

```

```python

```

```python

```

#### Discussion



#### References

Jordahl, K., Van den Bossche, J., Fleischmann, M., Wasserman, J., McBride, J., Gerard, J., … Leblanc, F. (2024). *geopandas/geopandas: v1.0.1* (Version 1.0.1) [Computer software]. Zenodo. https://doi.org/10.5281/zenodo.12625316 

Python Software Foundation. (2024). *Python* (Version 3.13.2) [Computer software]. https://docs.python.org/release/3.12.6 

Rudiger, P., Liquet, M., Signell, J., Hansen, S. H., Bednar, J. A., Madsen, M. S., … Hilton, T. W. (2024). *holoviz/hvplot: Version 0.11.2* (Version 0.11.2) [Computer software]. Zenodo. https://doi.org/10.5281/zenodo.13851295 

The pandas development team. (2024). *pandas-dev/pandas: Pandas* (Version 2.2.3) [Computer software]. Zenodo. https://doi.org/10.5281/zenodo.3509134
