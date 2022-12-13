# rces-final-project

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/pangeo-data/pangeo-docker-images/2022.09.21?urlpath=git-pull%3Frepo%3Dhttps%253A%252F%252Fgithub.com%252Fparsellsx%252Frces-final-project%26urlpath%3Dlab%252Ftree%252Frces-final-project%252Ffinal_project_multiyear.ipynb%26branch%3Dmain)

In this project, I am examining the implied meridional moist static energy transport in the atmosphere using [MERRA-2](https://gmao.gsfc.nasa.gov/reanalysis/MERRA-2/) reanalysis data from 1980 through 2019. My research outside of class is on energy transport in the atmosphere, and this project is my first step down that road. 

## Background

The atmosphere, along with the ocean, is constantly transporting a huge amount of energy to different parts of the world. Much of this energy is transported meridionally (i.e., north-south), as opposed to east-west. The reason for this is that Earth receives different amounts of energy from the Sun at different latitudes; e.g., the polar regions receive less sunlight (measured in watts per square meter; W/m<sup>2</sup>) on average over a year than the tropics do. If there were no way for the Earth to horizontally distribute energy, we would see enormous temperature gradients between the equator and the poles due to this differential heating, as is the case on the Moon. Since the Moon has no significant atmosphere or ocean, any horizontal heat transport that occurs must do so via conduction through rock, which is much less efficient than convection of a fluid like air or water. Because Earth has an atmosphere and an ocean, which are fluids, its equator-to-poles temperature gradient creates a pressure gradient, which drives a circulation. In this way, Earth’s atmosphere is able to transport large enough quantities of energy to significantly flatten the meridional temperature gradient.

Moist static energy (MSE) is a useful quantity for keeping track of energy flow through the atmosphere because it takes into account three different forms that the energy can take on: thermal energy (temperature), gravitational potential energy (height), and latent heat (moisture). An equation for MSE is given below. 

$$S = C_pT+gz+L_vq.$$

This project was inspired in part by some work that one of my advisors, Spencer Hill, did early in the semester. He diagnosed the MSE transport in the [ERA5](https://www.ecmwf.int/en/forecasts/datasets/reanalysis-datasets/era5) reanalysis dataset and found something unexpected – namely that the energy-flux equator (EFE; i.e., the latitude at which the net meridional MSE transport crosses zero) for the total MSE transport was offset from the EFE for the mean meridional circulation (MMC; i.e., the Hadley cell) component only, and this offset was about five degrees of latitude, enough to put the two EFEs in different hemispheres. This was surprising since the dominant paradigm has been that the location of the EFE for the total flux should be dominated by the MMC, with minimal contributions from stationary and transient eddies (which are the other two components). Because of this result, I started to look into whether the same offset exists in the MERRA-2 reanalysis, because if the answer was no, then we might have discovered a fluke (error) in ERA5, and if the answer was yes, then it likely wasn’t a fluke and rather we might stand to learn something new about the importance of eddies in setting the location of the total MSE EFE. This project is the first step towards determining that offset in MERRA-2. In future work I will calculate the actual (i.e., not implied) MSE transport and decompose it into the MMC and eddy components.

## Methodology

I used data on evaporation, shortwave and longwave radiative net fluxes at the Earth’s surface and the top of the atmosphere (TOA), and sensible heat flux at the surface to calculate the net vertical energy transport into each atmospheric column (i.e., grid cell of the reanalysis). Assuming steady state (i.e., no heating or cooling anywhere), that means all of the net flux vertically into each column must flow horizontally out of the column. Taking the zonal mean, I obtained the meridional energy flux and ultimately obtained a figure similar to Fig. 4a of [Donohoe et al. (2020)](https://journals.ametsoc.org/view/journals/clim/33/10/jcli-d-19-0797.1.xml). More details are given in the final_project_multiyear.ipynb notebook.

## Data

The data I used come from the [MERRA-2](https://gmao.gsfc.nasa.gov/reanalysis/MERRA-2/) reanalysis, in particular the dataset called “M2TMNXINT,” located [here](https://disc.gsfc.nasa.gov/datasets/M2TMNXINT_5.12.4/summary?keywords=M2TMNXINT). I downloaded this dataset over all space and time available, but I only downloaded the variables I needed – these were:

- convective_rainfall
- evaporation_from_turbulence
- large_scale_rainfall
- sensible_heat_flux_from_turbulence
- snowfall
- surface_net_downward_longwave_flux
- surface_net_downward_shortwave_flux
- toa_net_downward_shortwave_flux
- upwelling_longwave_flux_at_toa

I then uploaded these filtered versions of the data files to a Google Cloud Storage bucket, located [here](https://console.cloud.google.com/storage/browser/rces-data;tab=objects?prefix=&forceOnObjectsSortingFiltering=false). This is how I interface with the data in final_project_multiyear.ipynb.
