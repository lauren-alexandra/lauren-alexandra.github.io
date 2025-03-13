---
layout: page
title: Blue Oak
description: Habitat Suitability
img: assets/img/habitat_suitability/bo_card.png
importance: 1
category: work
related_publications: true
---

<div class="row" style="margin-top: 20px; margin-bottom: 20px; margin-left: 10px; margin-right: 10px;">
    <img src="/assets/img/habitat_suitability/blue-oak-woodland.png" alt="Blue Oak Woodland" width="100%" height="100%" longdesc="https://calscape.org/storage/app/species_images/calphotos/images/0000_0000_0113_0949.jpeg" /> 
</div>


The blue oak (*Quercus douglasii*) is a deciduous [drought-tolerant](https://calscape.org/Quercus-douglasii-(Blue-Oak)) tree endemic to California. Blue oaks can be identified by their [blue-green foliage](https://oaks.cnr.berkeley.edu/oak-tree-species-id-ecology/), slightly to deeply lobed leaves, textured and pale gray bark, typical height between 20-60 ft., and natural settings of dry, rocky, and somewhat acidic to neutral soils. Acorns germinate in fall, appear in winter, and are propagated by species like the California Scrub-Jay which can [plant up to 3,300 oaks each year](https://www.allaboutbirds.org/guide/California_Scrub-Jay/lifehistory) (Tallamy, 2021). Most [seasonal development](https://www.fs.usda.gov/database/feis/plants/tree/quedou/all.html#114) occurs during March through May when water uptake is high and air temperatures rise. 

Across North America, oaks support more life than any other tree due to their caterpillar abundance. Caterpillars are necessary to sustain terrestrial food webs: they are a key survival food for baby birds and also support larger animals like bears. Within California, oak woodlands have one of the highest levels of biodiversity in the state, [sustaining over 5,000 plant and animal species](https://www.sacvalleycnps.org/oak-woodland/#:~:text=Habitat%20Values,and%20streams%20for%20anadromous%20fishes). The blue oak alone is estimated to [host approximately 170 moth and butterfly species](https://calscape.org/plant/Quercus-douglasii-(Blue-Oak)/host). This species can be found among [plant communities](https://calscape.org/Quercus-douglasii-(Blue-Oak)) such as chaparral, foothill woodland, and oak woodland at [elevations](https://www.srs.fs.usda.gov/pubs/misc/ag_654/volume_2/quercus/douglasii.htm) of 500-2000 ft. in the north and up to 5000 ft. in the south. The blue oak is dispersed throughout the state including in the central Sierra Nevada [Eldorado National Forest](https://www.fs.usda.gov/main/eldorado/about-forest/about-area) and the central Coast and Transverse Ranges in [Los Padres National Forest](https://www.fs.usda.gov/main/lpnf/about-forest).

#### Data Description

[National Forest System Land Units](https://data.fs.usda.gov/geodata/edw/datasets.php?xmlKeyword=forest) includes proclaimed national forest, purchase unit, national grassland, land utilization project, research and experimental area, national preserve, and other land area. An NFS Land Unit is nationally significant classification of Federally owned forest, range, and related lands that are administered by the USDA Forest Service or designated for administration through the Forest Service. 

[MACAV2-METDATA DAILY/MONTHLY](https://www.reacchpna.org/thredds/reacch_climate_CMIP5_macav2_catalog2.html) is a downscaled climate dataset that covers CONUS at 4 kilometers (1/24 degree) resolution. Temporal coverage encompasses historical model output (1950-2005) and projections (2006-2099) for climate normals represented as monthly averages of daily values. 

Probabilistic Remapping of SSURGO ([POLARIS](https://www.usgs.gov/publications/polaris-properties-30-meter-probabilistic-maps-soil-properties-over-contiguous-united)) soil properties is a dataset of 30‐m probabilistic soil property maps over the contiguous United States (CONUS). The mapped variables over CONUS include soil texture, organic matter, pH, saturated hydraulic conductivity, Brooks‐Corey and Van Genuchten water retention curve parameters, bulk density, and saturated water content.

The [SRTM 1 Arc-Second Global](https://doi.org/10.5066/F7PR7TFT) product offers global coverage of void filled elevation data at a resolution of 1 arc-second (30 meters). The Shuttle Radar Topography Mission (SRTM) was flown aboard the space shuttle Endeavour February 11-22, 2000.

#### Data Citation

Abatzoglou, J. T. (2017). *REACCH Climatic Modeling CMIP5 MACAV2-METDATA DAILY/MONTHLY Catalog* [Data set]. https://www.reacchpna.org/thredds/reacch_climate_CMIP5_macav2_catalog2.html

Chaney, N. W., Minasny, B., Herman, J. D., Nauman, T. W., Brungard, C. W., Morgan, C. L. S., McBratney, A. B., Wood, E. F., & Yimam, Y. (2019, February 5). POLARIS soil properties: 30-m probabilistic maps of soil properties over the contiguous United States. *Water Resources Research, 55*(4). https://doi.org/10.1029/2018WR022797

NASA. (2024). *SRTM 1 Arc-Second Global* [Data set]. https://doi.org/10.5066/F7PR7TFT

USDA Forest Service. (2024). *National Forest System Land Units* [Data set]. https://data.fs.usda.gov/geodata/edw/edw_resources/shp/S_USA.NFSLandUnit.zip

#### Methods

Downscaled climate models were compared by site under the RCP 4.5 emissions [scenario](https://climatetoolbox.org/tool/Future-Climate-Scenarios), examining winter and summer mean temperatures (and change relative to historical by °F) as well as winter precipitation (and percent change relative to the historical value). The land units [shapefile](https://data.fs.usda.gov/geodata/edw/edw_resources/shp/S_USA.NFSLandUnit.zip) was extracted and read into a GeoDataFrame with geopandas and subset for site locations. [earthaccess](https://earthaccess.readthedocs.io/en/latest/) (a library for NASA's Search API) retrieved digital elevation models by dataset ([SRTMGL1](https://doi.org/10.5066/F7PR7TFT)) which were scoped to spatial bounds collected from the unit sites. Mean soil data was selected for pH, depth, and location through [POLARIS](https://www.usgs.gov/publications/polaris-properties-30-meter-probabilistic-maps-soil-properties-over-contiguous-united).

Data granules were processed into soil and elevation rasters through masking, scaling, cropping, and merging. The [xarray-spatial](https://docs.xarray.dev/en/stable/index.html) aspect function calculated aspect in degrees for each cell in a site elevation raster. Projected climate scenarios (medium and high emissions) for a monthly average of daily maximum near-surface air temperature in 2096-2099 were obtained from [MACAV2]((https://climate.northwestknowledge.net/MACA/index.php)), scoped to site boundaries, assigned a longitude coordinate range aligned with existing rasters, and converted from Kelvin to Fahrenheit. Sites and corresponding attributes were subsequently plotted using the [hvplot API](https://hvplot.holoviz.org/) and exported to rasters.

Given optimal characteristics for each site variable, a habitat suitability score was identified for each site leveraging a fuzzy Gaussian function to assign scores for each raster cell based on proximity to ideal site values. For each site and medium emissions scenario, variable rasters were harmonized across resolution and projection, suitability layers calculated and multiplied, and then combined into a final raster.

#### Analysis

Imports


```python
import warnings
warnings.simplefilter(action='ignore', category=FutureWarning)

import os
import pathlib
import zipfile
from glob import glob
import tqdm as notebook_tqdm

import pandas as pd
import numpy as np
import geopandas as gpd
import rioxarray as rxr
from rioxarray.merge import merge_arrays
import xarray as xr
import xrspatial

import matplotlib.pyplot as plt
import hvplot.pandas
import hvplot.xarray
import cartopy.crs as ccrs
import geoviews as gv

import earthaccess

from utils import *
```

Set Paths

```python
# Plots
plots_dir = os.path.join(
    # Home directory
    pathlib.Path.home(),
    'Projects',
    # Project directory
    'habitat-suitability',
    'plots'
)

# Datasets
datasets_dir = os.path.join(
    # Home directory
    pathlib.Path.home(),
    'Projects',
    # Project directory
    'habitat-suitability',
    'datasets'
)

# Project data directory 
data_dir = os.path.join(
    # Home directory
    pathlib.Path.home(),
    'Projects',
    # Project directory
    'habitat-suitability',
    'data'
)

# Define directories for data
land_units_dir = os.path.join(data_dir, 'usfs-national-lands')
eldorado_elevation_dir = os.path.join(data_dir, 'srtm', 'eldorado')
los_padres_elevation_dir = os.path.join(data_dir, 'srtm', 'los_padres')

os.makedirs(plots_dir, exist_ok=True)
os.makedirs(datasets_dir, exist_ok=True)
os.makedirs(data_dir, exist_ok=True)
os.makedirs(eldorado_elevation_dir, exist_ok=True)
os.makedirs(los_padres_elevation_dir, exist_ok=True)
```

Load Sites

```python
# Only extract once
usfs_pattern = os.path.join(land_units_dir, '*.shp')

if not glob(usfs_pattern):
    usfs_zip = f'{datasets_dir}/S_USA.NFSLandUnit.zip'

    # Unzip data
    with zipfile.ZipFile(usfs_zip, 'r') as zip:
        zip.extractall(path=land_units_dir)

# Find the extracted .shp file path
usfs_land_path = glob(usfs_pattern)[0]

# Load USFS land units from shapefile
usfs_land_units_gdf = (
    gpd.read_file(usfs_land_path)
)

# Obtain units with location name
valid_units = usfs_land_units_gdf.dropna(subset=['HQ_LOCATIO'])

# Select only CA units
all_ca_units = valid_units[valid_units['HQ_LOCATIO'].str.contains('CA')]
```

```python
earthaccess.login(strategy="interactive", persist=True)
```

```python
# Search for Digital Elevation Models

ea_dem = earthaccess.search_datasets(keyword='SRTM DEM', count=15)
for dataset in ea_dem:
    print(dataset['umm']['ShortName'], dataset['umm']['EntryTitle'])
```

    NASADEM_SHHP NASADEM SRTM-only Height and Height Precision Mosaic Global 1 arc second V001
    NASADEM_SIM NASADEM SRTM Image Mosaic Global 1 arc second V001
    NASADEM_SSP NASADEM SRTM Subswath Global 1 arc second V001
    C_Pools_Fluxes_CONUS_1837 CMS: Terrestrial Carbon Stocks, Emissions, and Fluxes for Conterminous US, 2001-2016
    SRTMGL1 NASA Shuttle Radar Topography Mission Global 1 arc second V003
    GEDI01_B GEDI L1B Geolocated Waveform Data Global Footprint Level V002
    GEDI02_B GEDI L2B Canopy Cover and Vertical Profile Metrics Data Global Footprint Level V002
    NASADEM_HGT NASADEM Merged DEM Global 1 arc second V001
    SRTMGL3 NASA Shuttle Radar Topography Mission Global 3 arc second V003
    SRTMGL1_NC NASA Shuttle Radar Topography Mission Global 1 arc second NetCDF V003
    SRTMGL30 NASA Shuttle Radar Topography Mission Global 30 arc second V002
    GFSAD30EUCEARUMECE Global Food Security-support Analysis Data (GFSAD) Cropland Extent 2015 Europe, Central Asia, Russia, Middle East product 30 m V001
    GFSAD30SACE Global Food Security-support Analysis Data (GFSAD) Cropland Extent 2015 South America product 30 m V001
    GFSAD30SEACE Global Food Security-support Analysis Data (GFSAD) Cropland Extent 2015 Southeast and Northeast Asia product 30 m V001
    CMS_AGB_NW_USA_1719 Annual Aboveground Biomass Maps for Forests in the Northwestern USA, 2000-2016


```python
# Los Padres National Forest

los_padres_gdf = all_ca_units.loc[
    all_ca_units['NFSLANDU_2'] == 'Los Padres National Forest'
]
# Create folium map instance
los_padres_gdf.explore()
```

<div class="row" style="margin-top: 20px; margin-bottom: 20px; margin-left: 10px; margin-right: 10px;">
    <img src="/assets/img/habitat_suitability/Los-Padres-National-Forest-Boundary.png" alt="Los Padres National Forest" width="70%" height="70%" /> 
</div>


```python
# Eldorado National Forest

eldorado_gdf = all_ca_units.loc[
    all_ca_units['NFSLANDU_2'] == 'Eldorado National Forest'
]

eldorado_gdf.explore()
```
<div class="row" style="margin-top: 20px; margin-bottom: 20px; margin-left: 10px; margin-right: 10px;">
    <img src="/assets/img/habitat_suitability/Eldorado-National-Forest-Boundary.png" alt="Eldorado National Forest" width="70%" height="70%" /> 
</div>


Retrieve Soils

```python
# Set site parameters

soil_prop = 'ph'
soil_prop_stat = 'mean'
# cm (the minimum depth for blue oaks is 1-2 ft)
soil_depth = '30_60'
```

```python
eldorado_soil_da = download_polaris('eldorado', eldorado_gdf, soil_prop, 
                                    soil_prop_stat, soil_depth,
                                    'Eldorado-National-Forest',
                                    'Eldorado National Forest',
                                    data_dir, plots_dir)
```

<div class="row" style="margin-top: 20px; margin-bottom: 20px; margin-left: 10px; margin-right: 10px;">
    <img src="/assets/img/habitat_suitability/Eldorado-National-Forest-Soil.png" alt="Eldorado National Forest Soil" width="100%" height="100%" /> 
</div>

```python
los_padres_soil_da = download_polaris('los_padres', los_padres_gdf, soil_prop, 
                                      soil_prop_stat, soil_depth,
                                      'Los-Padres-National-Forest',
                                      'Los Padres National Forest',
                                      data_dir, plots_dir)
```

<div class="row" style="margin-top: 20px; margin-bottom: 20px; margin-left: 10px; margin-right: 10px;">
    <img src="/assets/img/habitat_suitability/Los-Padres-National-Forest-Soil.png" alt="Los Padres National Forest Soil" width="100%" height="100%" /> 
</div>


Select Digital Elevation Model and Calculate Aspect

```python
eldorado_elev_da = download_topography(
                        'eldorado', eldorado_gdf,
                        'Eldorado-National-Forest', 
                        'Eldorado National Forest',
                        eldorado_elevation_dir, data_dir, plots_dir
                    )
```
<div class="row" style="margin-top: 20px; margin-bottom: 20px; margin-left: 10px; margin-right: 10px;">
    <img src="/assets/img/habitat_suitability/Eldorado-National-Forest-Elevation.png" alt="Eldorado National Forest Elevation" width="100%" height="100%" /> 
</div>
<div class="row" style="margin-top: 20px; margin-bottom: 20px; margin-left: 10px; margin-right: 10px;">
    <img src="/assets/img/habitat_suitability/Eldorado-National-Forest-Aspect-Degrees.png" alt="Eldorado National Forest Aspect" width="100%" height="100%" /> 
</div>

```python
los_padres_elev_da = download_topography(
                        'los_padres', los_padres_gdf, 
                        'Los-Padres-National-Forest',
                        'Los Padres National Forest',
                        los_padres_elevation_dir, data_dir, plots_dir
                    )
```
<div class="row" style="margin-top: 20px; margin-bottom: 20px; margin-left: 10px; margin-right: 10px;">
    <img src="/assets/img/habitat_suitability/Los-Padres-National-Forest-Elevation.png" alt="Los Padres National Forest Elevation" width="100%" height="100%" /> 
</div>
<div class="row" style="margin-top: 20px; margin-bottom: 20px; margin-left: 10px; margin-right: 10px;">
    <img src="/assets/img/habitat_suitability/Los-Padres-National-Forest-Aspect-Degrees.png" alt="Los Padres National Forest Aspect" width="100%" height="100%" /> 
</div>


Projected Climate

```python
# Set climate parameters

emissions_scenario = 'rcp45'
climate_models = ['CanESM2', 'MIROC-ESM-CHEM', 'CNRM-CM5', 'GFDL-ESM2M']
mid_century = [2036, 2041, 2046, 2051, 2056, 2061] # 2036-2066 
late_century = [2066, 2071, 2076, 2081, 2086, 2091] # 2066-2096 
time_periods = [mid_century, late_century]
```

Eldorado Projected Climate (RCP 4.5)

```python
# Mid century 

download_climate('eldorado', eldorado_gdf,
                emissions_scenario, climate_models, mid_century,
                'eldorado_mid_century', data_dir)

# Late century

download_climate('eldorado', eldorado_gdf,
                emissions_scenario, climate_models, late_century,
                'eldorado_late_century', data_dir) 
```

Los Padres Projected Climate (RCP 4.5)

```python
# Mid century 

download_climate('los padres', los_padres_gdf,
                emissions_scenario, climate_models, mid_century,
                'los_padres_mid_century', data_dir)

# Late century

download_climate('los padres', los_padres_gdf,
                emissions_scenario, climate_models, late_century,
                'los_padres_late_century', data_dir)
```

Identify Suitability

```python
# Optimal values for species for each variable
# Variables: elevation, temperature, aspect, soil pH
optimal_values = [1500.0, 90.0, 0.0, 6.75]

# Tolerance ranges (acceptable deviation) for each variable
tolerance_ranges = [1500.0, 20.0, 65.0, 0.75]
```

Eldorado National Forest Suitability 

```
# CanESM2 (Warm and wet)

build_habitat_suitability_model('eldorado', 'mid_century', 'CanESM2',
                                optimal_values, tolerance_ranges, data_dir, 
                                'eld_mc_CanESM2_suitability')

build_habitat_suitability_model('eldorado', 'late_century', 'CanESM2',
                                optimal_values, tolerance_ranges, data_dir, 
                                'eld_lc_CanESM2_suitability')

# MIROC-ESM-CHEM (Warm and dry)

build_habitat_suitability_model('eldorado', 'mid_century', 'MIROC-ESM-CHEM',
                                optimal_values, tolerance_ranges, data_dir, 
                                'eld_mc_MIROC-ESM-CHEM_suitability')

build_habitat_suitability_model('eldorado', 'late_century', 'MIROC-ESM-CHEM',
                                optimal_values, tolerance_ranges, data_dir, 
                                'eld_lc_MIROC-ESM-CHEM_suitability')

# CNRM-CM5 (Cool and wet)

build_habitat_suitability_model('eldorado', 'mid_century', 'CNRM-CM5',
                                optimal_values, tolerance_ranges, data_dir, 
                                'eld_mc_CNRM-CM5_suitability')

build_habitat_suitability_model('eldorado', 'late_century', 'CNRM-CM5',
                                optimal_values, tolerance_ranges, data_dir, 
                                'eld_lc_CNRM-CM5_suitability')

# GFDL-ESM2M (Cool and dry)

build_habitat_suitability_model('eldorado', 'mid_century', 'GFDL-ESM2M',
                                optimal_values, tolerance_ranges, data_dir, 
                                'eld_mc_GFDL-ESM2M_suitability')

build_habitat_suitability_model('eldorado', 'late_century', 'GFDL-ESM2M',
                                optimal_values, tolerance_ranges, data_dir, 
                                'eld_lc_GFDL-ESM2M_suitability')
```

<div class="row" style="margin-top: 20px; margin-bottom: 20px; margin-left: 10px; margin-right: 10px;">
    <img src="/assets/img/habitat_suitability/Eldorado-National-Forest-Suitability-Late-Century-CanESM2.png" alt="Eldorado National Forest Suitability Late Century CanESM2" width="100%" height="100%" /> 
</div>

Los Padres National Forest Suitability

```python
# CanESM2 (Warm and wet)

build_habitat_suitability_model('los_padres', 'mid_century', 'CanESM2',
                                optimal_values, tolerance_ranges, data_dir, 
                                'lp_mc_CanESM2_suitability')

build_habitat_suitability_model('los_padres', 'late_century', 'CanESM2',
                                optimal_values, tolerance_ranges, data_dir, 
                                'lp_lc_CanESM2_suitability')

# MIROC-ESM-CHEM (Warm and dry)

build_habitat_suitability_model('los_padres', 'mid_century', 'MIROC-ESM-CHEM',
                                optimal_values, tolerance_ranges, data_dir, 
                                'lp_mc_MIROC-ESM-CHEM_suitability')

build_habitat_suitability_model('los_padres', 'late_century', 'MIROC-ESM-CHEM',
                                optimal_values, tolerance_ranges, data_dir, 
                                'lp_lc_MIROC-ESM-CHEM_suitability')

# CNRM-CM5 (Cool and wet)

build_habitat_suitability_model('los_padres', 'mid_century', 'CNRM-CM5',
                                optimal_values, tolerance_ranges, data_dir, 
                                'lp_mc_CNRM-CM5_suitability')

build_habitat_suitability_model('los_padres', 'late_century', 'CNRM-CM5',
                                optimal_values, tolerance_ranges, data_dir, 
                                'lp_lc_CNRM-CM5_suitability')

# GFDL-ESM2M (Cool and dry)

build_habitat_suitability_model('los_padres', 'mid_century', 'GFDL-ESM2M',
                                optimal_values, tolerance_ranges, data_dir, 
                                'lp_mc_GFDL-ESM2M_suitability')

build_habitat_suitability_model('los_padres', 'late_century', 'GFDL-ESM2M',
                                optimal_values, tolerance_ranges, data_dir, 
                                'lp_lc_GFDL-ESM2M_suitability')
```

<div class="row" style="margin-top: 20px; margin-bottom: 20px; margin-left: 10px; margin-right: 10px;">
    <img src="/assets/img/habitat_suitability/Los-Padres-National-Forest-Suitability-Late-Century-CanESM2.png" alt="Los Padres National Forest Suitability Late Century CanESM2" width="100%" height="100%" /> 
</div>


#### Discussion

Habitat suitability for mid and late 21st century was assessed for a medium emissions scenario (RCP 4.5) using four diverging climate models: CanESM2 (Warm/Wet), MIROC-ESM-CHEM (Warm/Dry), CNRM-CM5 (Cool/Wet), and GFDL-ESM2M (Cool/Dry). Species tolerance and optimal ranges for elevation (1,500 ± 1,500 meters), temperature (90 ± 20 °F), aspect (0 ± 65 degrees), and soil (6.75 ± 0.75 pH) were evaluated in site suitability scores. Given the oak's accommodating tolerance for maximum temperature fluctations, model differences were minimal between plots and 30-year periods. However, this flexibility does not indicate a robustness to extreme seasonal changes. Blue oaks are generally equipped to withstand drier conditions compared to other native trees, abundant in areas with less conducive soils where they often cohabitate with pines. Nonetheless, the onset of a [severe drought diminishes the threshold of site tolerance](https://oaks.cnr.berkeley.edu/wp-content/uploads/2020/04/Swiecki-Bernhardt-Oak-Mortality-4.2-2020.pdf). 

By the end of the century, the average annual maximum temperatures are [projected to grow by 7.5°F to 10.9°F](https://www.fs.usda.gov/Internet/FSE_DOCUMENTS/fseprd985002.pdf). Intense precipitation events are projected as well, with frequencies of [pronounced dry seasons and whiplash events](https://doi.org/10.1038/s41558-018-0140-y) expanding by over 50% across the state with a notable change in Southern California: a ~200% increase in extreme dry seasons, a ~150% increase in extreme wet seasons, and a ~75% increase in year-to-year whiplash. Historically blue oak mortality has been typically attributed to decay fungi such as canker rots and root-rotting decay fungi. Pathogens like *Phytophthora ramorum*, the cause for sudden oak death, respond faster than trees to drought followed by high precipitation events, accelerating disintegration. The combination of disease and long-term climate stress contributes to raised levels of mortality years after drought. Futhermore, although blue oaks have evolved in an environment where fires occur regularly and can survive low-to-moderate intensity fires, the threat of [conifer encroachment](https://oaks.cnr.berkeley.edu/conifer-encroachment/) subjects oaks to more canopy fire risk in a warmer climate.

**Sites**

Los Padres National Forest is [vulnerable to floods](https://www.fs.usda.gov/Internet/FSE_DOCUMENTS/fseprd497638.pdf) and debris flows after extreme precipitation events, in particular after recurrent wildfires, due to the forest’s position in the Coast and Transverse mountain ranges in relation to the coast. Sediment flow in Southern California has a history of high variability under El Niño Southern Oscillation (ENSO) events and has led to the destruction of oak riparian habitat. Eldorado National Forest is similarly projected to experience extreme climate variability between wet and dry years. Toward the end of the century at the lowest and highest elevations in the Sierra Nevada, warming temperatures will lead to greater evaporation and a [projected ~15% reduction in fuel and soil moisture](https://www.fs.usda.gov/Internet/FSE_DOCUMENTS/fseprd985002.pdf). This scenario indicates greater drought likelihood and impacts across native plant communities. Heightened fire weather conditions will generate [shifts in blue oak mortality rates](https://www.fs.usda.gov/Internet/FSE_DOCUMENTS/fseprd985002.pdf) as well as post-fire germination. Moreover a shift in oak mortality affects fire activity. Blue oak sites with [south-facing aspects](https://esajournals.onlinelibrary.wiley.com/doi/full/10.1002/ecs2.3558) in particular demonstrate lower potential for accumulated drainage and thus the highest level of mortality compared to other aspects. Sites like these are especially at risk for greater dead woody surface fuels, enhancing the probability of larger fires over longer periods.

#### References


Barrett, A., Battisto, C., J. Bourbeau, J., Fisher, M., Kaufman, D., Kennedy, J., … Steiker, A. (2024). *earthaccess* (Version 0.12.0) [Computer software]. Zenodo. https://doi.org/10.5281/zenodo.8365009

California Native Plant Society (CNPS). (n.d.). Blue oak. https://calscape.org/Quercus-douglasii-(Blue-Oak)

Collins, B., Hetzel, J. T., & Metzger, T. C. (2024). *xarray-spatial* (Version 0.4.0) [Computer software]. GitHub. https://github.com/makepath/xarray-spatial/releases/tag/v0.4.0

Estes, B. & Gross, S. (2020). *Eldorado National Forest climate change trend summary.* USDA Forest Service. https://www.fs.usda.gov/Internet/FSE_DOCUMENTS/fseprd985002.pdf

Huesca, M., Ustin, S. L., Shapiro, K. D., Boynton, R., & Thorne, J. H. (2021, June 16). Detection of drought-induced blue oak mortality in the Sierra Nevada Mountains, California. Ecosphere, 12(6). https://doi.org/10.1002/ecs2.3558

Hunter, J. D. (2024). *Matplotlib: A 2D graphics environment* (Version 3.9.2) [Computer software]. Zenodo. https://zenodo.org/records/13308876

Jordahl, K., Van den Bossche, J., Fleischmann, M., Wasserman, J., McBride, J., Gerard, J., … Leblanc, F. (2024). *geopandas/geopandas: v1.0.1* (Version 1.0.1) [Computer software]. Zenodo. https://doi.org/10.5281/zenodo.12625316 

Molinari, N., Sawyer, S., & Safford, H. (n.d.). *A summary of current trends and probable future trends in climate and climate-driven processes in the Los Padres National Forest and neighboring lands.* USDA Forest Service. https://www.fs.usda.gov/Internet/FSE_DOCUMENTS/fseprd497638.pdf

Point Blue Conservation Science. (n.d.). *California oak planting guide.* https://www.pointblue.org/wp-content/uploads/2023/06/Planting-Oaks-Guide-Version-1.pdf

Python Software Foundation. (2024). *Python* (Version 3.12.6) [Computer software]. https://docs.python.org/release/3.12.6 

Rudiger, P., Liquet, M., Signell, J., Hansen, S. H., Bednar, J. A., Madsen, M. S., … Hilton, T. W. (2024). *holoviz/hvplot: Version 0.11.0* (Version 0.11.0) [Computer software]. Zenodo. https://doi.org/10.5281/zenodo.13851295 

Sacramento Valley Chapter (CNPS). (n.d.). *Oak woodland.* https://www.sacvalleycnps.org/oak-woodland/#:~:text=Habitat%20Values,and%20streams%20for%20anadromous%20fishes

Snow, A. D., Scott, R., Raspaud, M., Brochart, D., Kouzoubov, K., Henderson, S., … Weidenholzer, L. (2024). *corteva/rioxarray: 0.18.1 Release* (Version 0.18.1) [Computer software]. Zenodo. https://doi.org/10.5281/zenodo.4570456

Swain, D. L., Langenbrunner, B., Neelin, J. D., & Hall, A. (2018). Increasing precipitation volatility in twenty-first-century California. *Nature Climate Change, 8*(5), 427. https://doi.org/10.1038/s41558-018-0140-y

Swiecki, T. & Bernhardt, E. (2020). *Recent increases in blue oak mortality.* Phytosphere Research. https://oaks.cnr.berkeley.edu/wp-content/uploads/2020/04/Swiecki-Bernhardt-Oak-Mortality-4.2-2020.pdf

Tallamy, D. W. (2021). *The nature of oaks: The rich ecology of our most essential native trees.* Timber Press.

The pandas development team. (2024). *pandas-dev/pandas: Pandas* (Version 2.2.2) [Computer software]. Zenodo. https://doi.org/10.5281/zenodo.3509134

UC Oaks. (n.d.). *Blue oak-foothill pine woodland & wildlife habitat*. https://oaks.cnr.berkeley.edu/blue-oak-foothill-pine-woodland-wildlife-habitat

UC Oaks. (n.d.). *Coastal oak woodland & wildlife habitat.* https://oaks.cnr.berkeley.edu/coastal-oak-woodland/

UC Oaks. (n.d.). *Conifer encroachment.* https://oaks.cnr.berkeley.edu/conifer-encroachment/

UC Oaks. (2024, July 1). *Oak tree species ID & ecology.* https://oaks.cnr.berkeley.edu/oak-tree-species-id-ecology

University of California Merced (n.d.). *Future Climate Scenarios*. The Climate Toolbox. https://climatetoolbox.org/tool/Future-Climate-Scenarios

USDA Forest Service. (n.d.). *About the area*. https://www.fs.usda.gov/main/eldorado/about-forest/about-area

USDA Forest Service. (n.d.). *About the forest*. https://www.fs.usda.gov/main/lpnf/about-forest

USDA Forest Service. (n.d.). *Quercus douglasii*. https://www.fs.usda.gov/database/feis/plants/tree/quedou/all.html

USDA Southern Research Station. (n.d.). *Quercus douglasii*. https://www.srs.fs.usda.gov/pubs/misc/ag_654/volume_2/quercus/douglasii.htm

