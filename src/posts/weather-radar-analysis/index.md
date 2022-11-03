---
title: 'Xarray and weather radar analysis - A success story'
date: '2022-11-XX'
authors:
  - name: Maxwell Grover
    github: mgrover1
  - name: Kai Mühlbauer
    github: kmuehlbauer
---

![Openadar Science logo](https://raw.githubusercontent.com/openradar/openradar.github.io/main/_static/openradar_logo.svg)

## Who are we?

The [Openradar Community](https://openradarscience.org/) is aimed to foster the use and the collaboration on weather radar related open source software ([BAMS: The Emergence of Open Source Software for the Weather Radar Community](https://doi.org/10.1175/BAMS-D-13-00240.1)). Many affiliated packages are accessible via Python. For more than eight years the openradar community is providing short courses on several weather radar related topics at major conferences (eg. [ERAD2022](https://openradarscience.org/erad2022/README.html)).

## What problem we are trying to solve?

One major difficulty to this time is handling the plethora of radar data file formats. At least two standards emerged within the last decades, [OPERA ODIM_H5](https://www.eumetnet.eu/activities/observations-programme/current-activities/opera/) and [CfRadial1](https://ncar.github.io/CfRadial/). Out of those 2 standards [WMO JET-OWR](https://community.wmo.int/governance/commission-membership/commission-observation-infrastructure-and-information-systems-infcom/standing-committee-measurements-instrumentation-and-traceability-sc-mint/joint-expert-team-operational-weather-radar-jet-owr) is distilling a new standard, FM301 ([WMO CF Extensions](https://community.wmo.int/activity-areas/wis/wmo-cf-extensions)).

Within the openradar community the consensus was that a close collaboration that benefits the entire community can only be maximized through joint projects. So the idea of a common software project [xradar](http://xradar.readthedocs.io/) whose only task is to read and write radar data was born. The data import should include as many available data formats as possible, but the data export should be limited to the above recognized standards. 


## How does Xarray help?

Within the community Xarray was already used to conveniently hold radar data in memory (see [wradlib xarray backends](https://docs.wradlib.org/en/stable/notebooks/fileio/wradlib_xarray_backends.html)). The closeness of the Xarray Datamodel using Dataset and DataArray to the major radar data standards (based on netCDF4/HDF5) made Xarray the ideal package to base on. Any software package that uses Xarray in any way will then be able to directly use the described data model and thus quickly and easily import and export radar data.     

## Why did we choose Xarray?

Beside the already mentioned similarities a major point is cloud readiness and inherent multiprocessing capabilities. Also the multitude of available Xarray-based packages targeting different aspects of scientific problems is another huge advantage. 

## xradar

1. Install it in your environment using either
   - [conda](https://anaconda.org/conda-forge/xradar) (`conda install -c conda-forge xradar`)
   - or [pip](https://pypi.org/project/xradar/) (`pip install xradar`)
2. Open up a notebook and use the imports shown below!

```python
import os
import xarray as xr
import xradar as xd
# retrieval of data files
from urllib.parse import urljoin
from urllib.request import urlretrieve
```

### Fetch and Open CfRadial1 data

```python
def fetch_cfradial1_file():
    fname = "cfradial1_data.nc"
    if not os.path.exists(fname):
        base_url = "https://raw.githubusercontent.com/wradlib/wradlib-data/main/netcdf/"
        filename = "cfrad.20080604_002217_000_SPOL_v36_SUR.nc"
        url = urljoin(base_url, filename)
        urlretrieve(url, filename=fname)
    return fname

filename = fetch_cfradial1_file()
dtree = xd.io.open_odim_datatree(filename, first_dim="auto")
display(dtree)
```

### Georeference data

```python
ds = xd.georeference.get_x_y_z(dtree["sweep_0"].ds)
display(ds)
```

### Plot data

TBC.

## Conclusions

In this blog post we demonstrated how `xradar` can be utilized to import weather radar data into a covenient structure based on Xarray and xarray-datatree. We showed visualization and export.