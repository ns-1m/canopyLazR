
# LiDARforestR

R package to estimate leaf area density (LAD) and leaf area index (LAI) from airborne LiDAR point clouds.

## Information

The theory behind this R package is described in:   

*Kamoske A.G., Dahlin K.M., Stark S.C., and Serbin S.P. 2018. Leaf area density from airborne LiDAR: Comparing sensors and resolutions in a forest ecosystem. In preparation for submission to Forest Ecology and Management.*

### Corresponding Author

Code written by: Aaron G. Kamoske, PhD Student
   
  + [Michigan State University, Department of Geography](http://geo.msu.edu/)      
  + [ERSAM Lab](https://www.ersamlab.com/)   
  + akamoske@gmail.com

### Contributing Authors

Scott C. Stark
   
  + [Michigan State University, Department of Forestry](https://www.canr.msu.edu/for/)      
  + [Tropical Forestry Ecology Lab](https://sites.google.com/site/scottcstarktropicalforest/)   
  + scott.c.stark@gmail.com  
  
Shawn P. Serbin

  + [Brookhaven National Laboratory, Environmental and Climate Sciences Department](https://www.bnl.gov/envsci/bio/serbin-shawn.php)
  + sserbin@bnl.gov
  
## Installation

The easiest way to install `LiDARforestR` is via `install_github` from the `devtools` package:

```
# If you haven't already installed this package and its dependencies
install.packages("devtools")

# If you alread have devtools installed or just installed it
library(devtools)

# Install LiDARforestR from GitHub
install_github("akamoske/LiDARforestR")

# Load the library
library(LiDARforestR)
```

Now all functions should be available.

## Example of usage (after installation)

Once the package is loaded into your R session, this is the an example of how to use the functions in this package
to estimate LAD and LAI:

```
# Convert .laz or .las files into a list of voxelized lidar arrays
laz.data <- laz.to.array(laz.files.path = "./Data/laz_files", 
                         voxel.resolution = 10, 
                         z.resolution = 1)

# Level each voxelized array in the list to mimic a canopy height model
level.canopy <- canopy.height.levelr(lidar.array.list = laz.data)

# Estimate LAD for each voxel in leveled array in the list 
lad.estimates <- machorn.lad(leveld.lidar.array.list = level.canopy, 
                             voxel.height = 1, 
                             beer.lambert.constant = NULL)

# Convert the list of LAD arrays into a single raster stack
lad.raster <- lad.array.to.raster.stack(lad.array.list = lad.estimates, 
                                        laz.array.list = laz.data, 
                                        epsg.code = 32618)

# Create a single LAI raster from the LAD raster stack
lai.raster <- raster::calc(lad.raster, fun = sum, na.rm = TRUE)

# Generate a quick raster plot of the resulting total canopy LAI values for each pixel
plot(lai.raster)
```

![alt text](https://github.com/akamoske/LiDARforestR/blob/master/images/LAI_Raster_20180810.png "Example LAI Raster")

## License

This project is licensed under the GNU GPUv2 License - see the [LICENSE.md](LICENSE.md) file for details

