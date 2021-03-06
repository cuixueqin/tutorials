Install the MAgPIE model and all software/data required
================
Anastasis Giannousakis (<giannou@pik-potsdam.de>)
25 April, 2019

HOW TO INSTALL
--------------

There are two ways to download the MAgPIE model. If you have git installed clone the model from <https://github.com/magpiemodel/magpie.git> If you have no git installed you can alternatively download MAgPIE as a zip file from <https://github.com/magpiemodel/magpie/archive/master.zip>.

MAgPIE requires *GAMS* (<https://www.gams.com/>) including licenses for the solvers *CONOPT* and (optionally) *CPLEX* for its core calculations. As the model benefits significantly from recent improvements in *GAMS* and *CONOPT4* it is recommended to work with the most recent versions of both. Please make sure that the GAMS installation path is added to the PATH variable of the system:

-   the easiest way to add is by simply checking the "Use advanced installation mode" box at the beginning of the installation. At a later step you have to tick again a checkbox that adds the GAMS path to your PATH variable
-   you can also edit your computer's advanced settings and add the GAMS path to the PATH variable manually (applies also if GAMS is installed but not included in PATH).

This tutorial shows how to check and add variables to your PATH variable: <https://www.youtube.com/watch?v=5P9EDJwfXBo>

Please add the GAMS training license you have been provided (gamslice.txt) by saving the file to your GAMS local folder. Under Windows something like `C:\Program Files (x86)\GAMS\24.3`

In addition *R* (<https://www.r-project.org/>) is required for pre- and postprocessing and run management (needs to be added to the user's PATH variable as well). It is recommended to install also RSudio (<https://www.rstudio.com>).

For R, some packages are required to run MAgPIE. All except of one (`gdxrrw`) are either distributed via the offical R CRAN or via a separate repository hosted at PIK (PIK-CRAN). Before proceeding PIK-CRAN should be added to the list of available repositories via:

``` r
options(repos = c(CRAN = "@CRAN@", pik = "https://rse.pik-potsdam.de/r/packages"))
```

The `gdxrrw` package has to be downloaded directly from GAMS via

``` r
download.file("https://support.gams.com/_media/gdxrrw:gdxrrw_1.0.2.zip",
              "gdxrrw_1.0.2.zip")
install.packages("gdxrrw_1.0.2.zip",repos = NULL)
```

It can be the case that the `R` installation reqiures a more up-to-date version of `gdxrrw` package. In that case, check on the webpage <https://support.gams.com/gdxrrw:interfacing_gams_and_r> for adequate package version and eventual additional information and then proceed with download and install.

In some cases it can happen that `gdxrrw` does not return an error message during installation but also did not install properly. To verify a successful installation try to load the package via `library(gdxrrw)`.

------------------------------------------------------------------------

If loading of the package fails you need to install the package from source. Under Windows this requires to install Rtools (<https://cran.r-project.org/bin/windows/Rtools/>) and to add it to the PATH variable. After that you can run the following lines of code:

``` r
download.file("https://support.gams.com/_media/gdxrrw:gdxrrw_1.0.2.tar.gz",
              "gdxrrw_1.0.2.tar.gz")
install.packages("gdxrrw_1.0.2.tar.gz",repos = NULL, type="source")
```

------------------------------------------------------------------------

After that all remaining packages can be installed via `install.packages`

``` r
pkgs <- c("ggplot2",
          "curl",
          "gdx",
          "magclass",
          "madrat",
          "mip",
          "lucode",
          "magpie4",
          "magpiesets",
          "lusweave",
          "luscale",
          "goxygen",
          "shinyresults")
install.packages(pkgs)
```

For post-processing model outputs *Latex* is required (<https://www.latex-project.org/get/>). To be seen by the model it also needs to be added to the PATH variable of your system.

Lastly, please download MAgPIE from here: <https://github.com/magpiemodel/magpie/archive/master.zip>

If the following lines of code are executed withour error, then you are all set!

``` r
system("gams")
library(gdxrrw)
library(magpie4)
print("")
if(.Platform$OS.type == "unix") {
  system("which pdflatex")
} else {
  system("where pdflatex")
}
```

NOTE: If the model fails to start from the Windows console, try starting it from within RStudio.
