# IDEAMAPS Drinking Water Access Deprivation Model (AGILE 2026)

This repository contains the modelling workflow, participatory validation analysis, and sensitivity testing for the AGILE 2026 short paper:

**Turning Geospatial Data into Planning Decisions: Evaluating a Participatory Accessibility Model for Urban Drinking Water**

## Repository Structure
- **Drinking-Water-V1**: baseline model
- **Drinking-Water-V2**: validation-informed threshold recalibration
- **Drinking-Water-V3**: feedback-informed service-weight sensitivity
- **PAR-Analysis**: participatory validation outputs
- **Documentation**: metadata, class definitions, images, and notes

# SETUP
1. Clone the Repository
git clone https://github.com/urbanbigdatacentre/ideamaps-drinking-water-access-dep-agile,
cd ideamaps-healthcare-paper-analysis/data_preparation

2. Create a Virtual Environment
python -m venv .venv
source .venv/bin/activate 

3. Install dependencies
pip install -r requirements.txt

4. Set up the Jupyter Kernel (for VS Code or Jupyter Notebooks)
pip install -U ipykernel
python -m ipykernel install --user --name=.venv

# DATA SOURCES
1. [IDEAMAPS STUDY AREAS](https://github.com/urbanbigdatacentre/ideamaps-models/tree/dev/docs/study-areas)
2. [DONATE WATER DRINKING WATER DATA](https://zenodo.org/records/19184253)
3. [WORLD POP DATA](https://hub.worldpop.org/geodata/summary?id=28031)
4. [Road network data from OpenStreetMap via the Open Route Service API](https://openrouteservice.org)

# RUNNING THE MODEL
Step 1 — Prepare population demand layer
Import the WorldPop 100 m × 100 m gridded population dataset and clip it to the Kano and Lagos study boundaries to represent population demand for drinking water access analysis.

Step 2 — Compute the OD Matrix
Calculate travel times and distances from each grid centroid to EmOC facilities using the [OpenRouteService (ORS) Matrix API](https://openrouteservice.org)

    - Option A: Public ORS API
    OPENROUTESERVICE_API_KEY = 'your_api_key'
    api_key = os.getenv('OPENROUTESERVICE_API_KEY')
    client = openrouteservice.Client(key=api_key)

    - Option B: Local ORS Instance (Docker)
    See [ORS documentation](https://github.com/GIScience/openrouteservice/tree/main) for setup instructions.

Step 3 — Apply the E2SFCA Method
    - Define catchment areas per facility using a travel-time threshold
    - Compute supply-to-demand ratios for each facility
    - Aggregate accessibility scores for each demand grid cell

# DATA AVAILABILITY
Large raster, matrix, and intermediate spatial files are excluded from this repository due to size constraints. Source datasets are available via WorldPop and IDEAMAPS / DonateWater links provided in the documentation.

# LIMITATIONS AND ASSUMPTIONS
1. Population data (WorldPop 2020) provides high-resolution, UN-adjusted estimates based on census counts. However, it may not fully capture recent urban growth, particularly in informal settlements, and it does not provide demographic disaggregation (e.g., age, gender).
2. The Donate Water Nigeria waterpoint dataset has limited spatial coverage, and some variables, including affordability and queueing conditions, were recorded only descriptively; consequently, these parameters could not be quantitatively incorporated into the modelling framework.
3. Not all informal water vendors or small-scale distribution points were captured in the dataset.
4. Service reliability was approximated where direct monitoring data were missing.
5. Walking path alone was used to estimate travel times.
6. Travel time estimates rely on routing assumptions and may not always capture seasonal variability (e.g., flooding).
7. Local validation was limited, as only one community in Kano and one community in Lagos were able to validate the result.
The waterpoint dataset was cleaned to remove outliers, incomplete records, and points outside the study area, which may have reduced representativeness at the edges of the dataset.

If you require further information about the data sources and methodology used to derive these thresholds, please email us via the [IDEAMAPS network](admin@ideamapsnetwork.org) or [Oluwatimilehin Adenike Shonowo](o.shonowo.1@research.gla.ac.uk) at the [Urban Big Data Centre, University of Glasgow](https://www.ubdc.ac.uk).
