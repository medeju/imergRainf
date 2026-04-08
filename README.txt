README: IMERG Rainfall Processing Pipeline
This repository provides a automated Python workflow to process, aggregate, and extract time-series data from NASA IMERG (GPM) satellite precipitation products.

1. Data Acquisition
All IMERG data must be sourced from the NASA GES DISC portal:

Source: https://disc.gsfc.nasa.gov/

Product Types: The script is compatible with all three IMERG runs:

Early: (Real-time, ~4 hours latency)

Late: (Near real-time, ~14 hours latency)

Final: (Research grade, multi-month latency — Recommended for climate studies)

Format: Ensure you download the NetCDF (.nc4) versions of the daily data.

2. Data Organization
To ensure the script runs correctly, you must organize your downloaded files into a specific directory structure based on the observation year:

Create a root folder (e.g., IMERGL07 or IMERGF).

Inside the root folder, create subdirectories named by the Year.

Place all daily files (365 for standard years, 366 for leap years) inside their respective year folder.

Example Structure:

Plaintext
D:/IMERGL07/
├── 2020/
│   ├── 3B-DAY-L.MS.MRG.3IMERG.20200101-S000000-E235959.V07B.nc4.nc4
│   └── ... (366 files)
├── 2021/
│   ├── 3B-DAY-L.MS.MRG.3IMERG.20210101-S000000-E235959.V07B.nc4.nc4
│   └── ... (365 files)
└── 2022/
    └── ...
3. Script Workflow
The provided Python script executes the following stages:

Part I: Aggregation * Loads yearly folders using xarray.open_mfdataset.

Concatenates multiple years into a single 3D time-series (time, lat, lon).

Calculates Monthly Totals, Annual Totals, and Long-term Climatology.

Part II: Point Extraction

Reads a CSV list of station coordinates (Lat/Lon).

Uses "Nearest Neighbor" selection to extract pixel values at specific ground locations.

Part III: Export

Pivots data into a "Wide Format" matrix (Dates as rows, Stations as columns).

Saves outputs to daily_data.csv and monthly_data.csv.

Part IV: Visualization

Generates a time-series line plot for a sample station.

Produces a 2D spatial map of the mean annual rainfall.

4. Quick Start / Testing
For users new to the script, Sample Data is provided within the repository.

Set your path variable to point to the sample data folder.

Ensure your Python environment has the following dependencies installed:

xarray, dask, netCDF4, pandas, matplotlib

5. Technical Notes
Leap Years: The script automatically detects 366-day years based on the time dimension coordinates.

Spatial Domain: Ensure your downloaded .nc4 files cover the same spatial bounding box to avoid alignment errors during concatenation.

Memory: The script utilizes dask for "lazy loading," meaning it can handle large datasets without exhausting your system RAM until the final computation/export.

Contact: [Your Name/Research Group]

Version: 1.0.0 (April 2026)
