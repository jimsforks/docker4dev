FROM rocker/verse:3.6.3

MAINTAINER "Sébastien Rochette" sebastien@thinkr.fr

# RUN apt-get update && apt-get upgrade --yes

# Update Rstudio Server to 1.4.106
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    file \
    git \
    libapparmor1 \
    libcurl4-openssl-dev \
    libedit2 \
    libssl-dev \
    lsb-release \
    psmisc \
    procps \
    python-setuptools \
    sudo \
    wget \
    libclang-dev \
    libclang-3.8-dev \
    libobjc-6-dev \
    libclang1-3.8 \
    libclang-common-3.8-dev \
    libllvm3.8 \
    libobjc4 \
    libgc1c2 \
  && RSTUDIO_URL="https://s3.amazonaws.com/rstudio-ide-build/server/debian9/x86_64/rstudio-server-1.4.106-amd64.deb" \
  && wget -q $RSTUDIO_URL \
  && dpkg -i rstudio-server-*-amd64.deb \
  && rm rstudio-server-*-amd64.deb

# geospatial
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
    lbzip2 \
    libfftw3-dev \
    libgdal-dev \
    libgeos-dev \
    libgsl0-dev \
    libgl1-mesa-dev \
    libglu1-mesa-dev \
    libhdf4-alt-dev \
    libhdf5-dev \
    libjq-dev \
    liblwgeom-dev \
    libpq-dev \
    libproj-dev \
    libprotobuf-dev \
    libnetcdf-dev \
    libsqlite3-dev \
    libssl-dev \
    libudunits2-dev \
    netcdf-bin \
    postgis \
    protobuf-compiler \
    sqlite3 \
    tk-dev \
    unixodbc-dev
  
# Set MRAN
RUN echo "options(repos = list(CRAN = 'https://mran.revolutionanalytics.com/snapshot/2019-08-05/'))" >> /usr/local/lib/R/etc/Rprofile.site

# Install versions available on MRAN at the correct date
RUN R -e "install.packages('remotes')"
RUN R -e 'll <- installed.packages()[,1] ; ll <- ll[-which(lapply(ll, packageDescription, field="Priority") == "base")] ; for(l in ll){try(remotes::install_version(l))}'
# RUN R -e "update.packages(ask = FALSE)"

# renv
RUN R -e "remotes::install_github('rstudio/renv', ref = '0.9.3-63')"

# golem and deps
RUN R -e "remotes::install_github(repo = 'ThinkR-open/golem', ref = 'v0.1')"

# spatial pkg
RUN R -e "install.packages(c('Rcpp', 'units', 'sf', 'lwgeom'))"
# RUN R -e "remotes::install_github(repo = 'r-spatial/lwgeom', ref = '1cd8e1bac5c3ae8ce56ef5368b8a87efa00e716d')"
  
# Directory for supplemental packages
# RUN R -e "dir.create('/home/rstudio/library/')"
# RUN echo ".libPaths(c('/home/rstudio/library', .libPaths())); \
#  ">> /home/rstudio/.Rprofile
# RUN chmod a+rwx /home/rstudio/.Rprofile 
