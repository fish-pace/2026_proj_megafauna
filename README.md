# 2026_proj_megafauna

Find relationships between PACE satellite data and cetacean distribution in the Mid-Atlantic Bight

**Folder Structure**


* `contributor_folders` This is a staging area where our team members practiced!
* `final_notebooks` Our final notebook for the time spent on Fish-PACE is here
* `data` These were datasets that we used, or downloaded

## Project Name

NEFSC Marine Mammals and MOANA: Linking PACE Phytoplankton Community Data to Cetacean Distribution

## One-line Description

Using NASA PACE satellite data (MOANA phytoplankton products) to characterize the ecological niches of cetacean species observed during NEFSC Mid-Atlantic offshore surveys.

## Planning

* Initial idea: Explore whether PACE-derived phytoplankton community composition (prokaryote abundances) can reveal habitat partitioning among co-occurring cetacean species better than traditional chlorophyll-a alone
* Slack channel: fp25_proj_megafauna


## Collaborators

| Name                | Role                |
|---------------------|---------------------|
| Liz Ferguson        | Project Facilitator |
| Ana Vaz             | Participant         |
| James King          | Participant         |

## Background

Traditional cetacean habitat models rely on physical oceanographic variables (SST, depth, chlorophyll-a) that may not capture fine-scale ecological differences between species. NASA's PACE satellite provides novel phytoplankton community composition data through the MOANA product (L4M), resolving Synechococcus, Prochlorococcus, and picoeukaryote abundances at 4 km / 8-day resolution. These prokaryote groups represent different trophic and oceanographic regimes, and may better characterize the prey environments of cetaceans with distinct foraging ecologies.

## Goals

1. Extract and visualize PACE MOANA phytoplankton data for the Mid-Atlantic Bight study area (35.5–41°N, 71.5–77°W)
2. Match MOANA data (plus chlorophyll-a, SST, carbon, and bathymetry) to georeferenced cetacean observation records from NEFSC offshore surveys
3. Characterize and compare environmental niches across species using statistical tests, niche overlap analysis, PCA, and hierarchical clustering
4. Determine whether PACE prokaryote community data reveals habitat partitioning not visible in traditional productivity metrics

## Datasets

| Dataset | Source | Description |
|---------|--------|-------------|
| NEFSC Mid-Atlantic Cetacean Survey | OBIS / SEAMAP (dataset 2368) | Georeferenced sightings of cetaceans with group size, Dec 28–Mar 28 |
| PACE OCI MOANA L4M (8-day, 4 km) | NASA Earthdata (`PACE_OCI_L4M_MOANA`) | Synechococcus, Prochlorococcus, and picoeukaryote cell concentrations |
| PACE OCI Chlorophyll-a | NASA Earthdata | Chlorophyll-a concentration (mg m⁻³) |
| PACE OCI Particulate Organic Carbon | NASA Earthdata | Phytoplankton carbon (mg m⁻³) |
| PACE OCI Sea Surface Temperature | NASA Earthdata | SST (°C) |
| GEBCO 2025 Bathymetry | GEBCO | 15-arc-second gridded elevation/depth |
| NEFSC Bathymetric Contours | NEFSC | Shapefiles for 50, 100, 200, 500, 1000, 2000 m isobaths |

Species analyzed: Fin whale, Humpback whale, Common minke whale, Sperm whale, North Atlantic right whale, Blue whale, Beaked whales (full dataset); primary niche analyses focused on the four species with the largest sample sizes (Fin, Humpback, Sperm, Common minke).

## Workflow/Roadmap

**Section 1 – MOANA Visualization and Data Extraction**
- Authenticate with NASA Earthdata via `earthaccess`
- Search and load PACE MOANA L4M granules for the study bounding box
- Visualize spatial distributions of Synechococcus, Prochlorococcus, and picoeukaryotes
- Overlay NEFSC cetacean observations on phytoplankton maps
- Plot latitudinal abundance profiles for prokaryote groups
- Extract 3×3 pixel box statistics (mean, median, std, min, max, IQR) at each observation point across all survey dates; merge with existing chlorophyll data → `NEFSC_Offshore_Obs_With_Chl_and_MOANA.csv`

**Section 2 – Correlations: Group Size and Phytoplankton Variables**
- Pearson and Spearman correlations between cetacean group size and each MOANA variable, by species
- Scatter plots, box plots by group-size category, and Spearman correlation heatmaps

**Section 3 – Bathymetry**
- Load GEBCO NetCDF and NEFSC isobath shapefiles
- Map bathymetric contours with cetacean observations
- Overlay chlorophyll and bathymetry
- Extract depth at each observation point via nearest-neighbor interpolation → `NEFSC_Offshore_Obs_With_Chl_MOANA_Depth.csv`
- Visualize depth distributions per species (box plots, violin plots, histograms, depth-zone stacked bars)

**Section 4 – Environmental Niche Comparison**
- Multi-variable niche analysis using all available environmental predictors: chlorophyll-a, chlorophyll variability, Synechococcus, Prochlorococcus, picoeukaryotes, phytoplankton carbon, SST, depth, and latitude
- Kruskal-Wallis tests for significant inter-species differences, followed by Mann-Whitney U post-hoc pairwise tests with Bonferroni correction
- Niche overlap quantified via histogram overlap index for each species pair × variable combination
- Visualization suite: distribution histograms, box/violin plots, 2D environmental space scatter plots, niche overlap heatmaps, radar charts, dot plots, biological vs. physical variable comparison panels, PCA biplots, hierarchical clustering dendrogram

## Results/Findings

- Prokaryote community composition (especially Synechococcus and picoeukaryotes) reveals strong niche partitioning that is not apparent from chlorophyll-a or particulate carbon alone.
- Sperm whales show near-zero overlap with all other species on Synechococcus and picoeukaryotes, consistent with their offshore, deep-diving habitat.
- Fin and Humpback whales show the highest niche overlap across most variables (shelf-edge generalists), while Common minke whales show zero Prochlorococcus overlap (coastal specialist).
- Carbon overlap is consistently higher than prokaryote overlap across species pairs, confirming that total biomass metrics mask ecologically meaningful differences in phytoplankton community structure.
- PCA and hierarchical clustering broadly confirm the Sperm whale as an outlier in multivariate environmental space, with Fin and Humpback grouping together.
- Environmental niche indicates different use by species within offshore region.

## Lessons Learned

- There is so much potential for MOANA!

## References

- OBIS SEAMAP NEFSC dataset: https://seamap.env.duke.edu/dataset/2368
- NASA Earthdata PACE MOANA product: https://search.earthdata.nasa.gov
- GEBCO 2025 Bathymetric Grid: https://www.gebco.net
- earthaccess Python library: https://earthaccess.readthedocs.io
