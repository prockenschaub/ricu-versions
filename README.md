# R `ricu` database versions

This repository contains configurations for various versions of ICU databases supported by the R `ricu` package.

## Introduction
The [ricu package](https://github.com/eth-mds/ricu) in R provides tools and functionalities tailored for ICU databases. This repository aims to offer a collection of configurations for different versions of the included ICU databases, ensuring compatibility and ease of use for researchers and developers working with ICU data in R.

## Configurations
This repository supports configurations for the following versions of ICU databases:

MIMIC III:
- version 1.4
  
MIMIC IV:
- version 1.0
- version 2.0
- version 2.2
  
eICU: 
- version 2.0
  
HiRID: 
- version 1.1.1
  
AUMCdb: 
- version 1.0.2

Navigate to the respective folder for each version to access the configuration files.


## Installation
To use the configurations in this repository, first, ensure you have the `ricu` package installed:

```r
install.packages("ricu")
```

`ricu` usually comes with configurations for the latest database version, e.g., MIMIC IV version 2.2. If you require older version of MIMIC IV, which have a slightly different table structure, you can find those configurations here. Clone this repository to your local machine:

```bash
git clone https://github.com/prockenschaub/ricu-versions.git
cd ricu-versions
```

## Usage

Load the ricu package in your R environment:

``` r
Sys.setenv(RICU_DATA_PATH = "/Users/patrick/datasets/ricu")
library(ricu)
#> 
#> ── ricu 0.5.6 ──────────────────────────────────────────────────────────────────
#> 
#> The following data sources are configured to be attached:
#> (the environment variable `RICU_SRC_LOAD` controls this)
#> 
#> ✔ mimic: 26 of 26 tables available
#> ✔ mimic_demo: 25 of 25 tables available
#> ✔ eicu: 31 of 31 tables available
#> ✔ eicu_demo: 31 of 31 tables available
#> ✔ hirid: 5 of 5 tables available
#> ✔ aumc: 7 of 7 tables available
#> ✔ miiv: 31 of 31 tables available
#> 
#> ────────────────────────────────────────────────────────────────────────────────
```

If you have previously imported tables to `ricu`, your loading message should look something like the above, notifying you that data already exists in `ricu`. If you cannot see any available tables yet, please have a look at the `ricu` documentation first on how to import data into `ricu`.

Once you have `ricu` setup for the latest database versions, you can also add the earlier versions provided here. Note that each version will need to be imported and stored separately:

```r 
old_cfg <- load_src_cfg(name = "miiv-1.0", cfg_dirs = "/path/to/this/repository/miiv/")
import_src(old_cfg$miiv, data_dir = "/path/to/miiv-1.0/source/files") # only required when you first load version 1.0
attach_src(old_cfg$miiv, data_dir = "/path/to/miiv-1.0/fst/files", assign_env = .GlobalEnv)    # by default, same as for import_src 
```

The above code should make a `miiv` object available in your global environment. *Note:* If you use RStudio, the above may lead to your R session grinding to a halt. It is unclear what exactly causes this but it may be related to how RStudio scans the R environment, which may lead to an evaluation of the `miiv` object. In any case, you can use the following work-around: 

```r 
attach_src(old_cfg$miiv, data_dir = "/path/to/miiv-1.0/fst/files")    # no assign env, loading it in the package env
data$miiv
```

`ricu` also contains a data environment, where all data is loaded. This environment is exported but does not seem to get scanned by RStudio, thus avoiding the slowdown. Be aware that if you attached an earlier version while loading `ricu`, e.g., version 2.2, the `miiv` that was created will still point to that version. You can use the following environment variable to define which datasets should be attached on load: 

```r
Sys.setenv(RICU_SRC_LOAD = "mimic,mimic_demo,eicu,eicu_demo,hirid,aumc") # no miiv
```


