FROM jupyter/scipy-notebook

USER root

RUN chmod -R 777 /home/jovyan
RUN mkdir /usr/share/jupyter
RUN mkdir /usr/share/jupyter/kernels
RUN mkdir /usr/share/jupyter/kernels/IDL
RUN chmod -R 777 /usr/share/jupyter
RUN chmod -R 777 /usr/share/jupyter/kernels
RUN chmod -R 777 /usr/share/jupyter/kernels/IDL
RUN chmod -R 777 /opt/conda/lib/python3.7/site-packages

RUN apt-get update -y && \
    apt-get upgrade -y

    

#RUN apt-get update && \
#    apt-get install -y --no-install-recommends \
#    fonts-dejavu \
#    tzdata \
#    unixodbc \
#    unixodbc-dev \
#    r-cran-rodbc \
#    gfortran \
#    gcc && \
#    rm -rf /var/lib/apt/lists/*

# Fix for devtools https://github.com/conda-forge/r-devtools-feedstock/issues/4
RUN ln -s /bin/tar /bin/gtar

RUN export TMPDIR=/local/scratch/$USER

RUN groupadd -g 1000 slurm
RUN useradd  -m -c "Slurm" -d /var/lib/slurm -u 401 -g slurm  -s /bin/bash slurm
RUN groupadd -g 398 munge
RUN useradd -m -c "Munge" -d /var/lib/munge -u 398 -g munge -s /sbin/nologin munge

USER $NB_UID

LABEL maintainer="Rowan Gaffney <rowan.gaffney@ars.usda.gov>"

RUN conda update conda

RUN conda install -c conda-forge --quiet --yes \
	'dask' \
	'openmpi'\
	'dask-ml' \
	'dask-xgboost' \
	'bokeh' \
	'conda' \
	'mpi4py' \
	'gdal' \
	'git' \
	'pip' \
	'ncurses' \
	'netCDF4' \
	'tqdm' \
	'bottleneck' \
	'zarr' \
	'numcodecs' \
	'xarray' \
	'h5py>=2.9=mpi*' \
	'python-graphviz' \
	'geopandas' \
	'simplegeneric' \
	'rasterio[s3]' \
	'fiona' \
	'spectral' \
	'shapely' \
	'pyshp' \
	'joblib' \
	'psycopg2' \
	'openpyxl' \
	'oauth2client' \
	'PyDrive' \
	'dask-jobqueue' \
	'xgboost' \
	'earthengine-api=0.1.205' \
	'r' \
	'rpy2' \
	'poppler' \
	'intake' \
	'pydap' \
	'holoviews' \
	'geoviews' \
	'panel' \
	'datashader' \
	'streamz' \
	'rasterstats' \
	'pyarrow' \
	's3fs' \
	'gcsfs' \
	'intake-xarray' \
	'intake-esm' \
	'intake-parquet' \
	'intake-sql' \
	'streamz' \
	'hvplot' \
	'selenium ' \
	'h5netcdf' \
	'rioxarray' \
	'xgeo' \
	'xrviz' \
	'pysal' \
	'pyhdf' \
	'rioxarray' \
	'intake-stac' \
	'jupyter-server-proxy' \
	'threddsclient' \
	'nb_conda_kernels'
	
RUN pip install git+https://github.com/intake/gdrivefs

#RUN conda install -c informaticslab --quiet --yes \
#	'intake_geopandas'



#RUN pip install --quiet envipyengine pymodis

RUN fix-permissions /home/$NB_USER
RUN fix-permissions $CONDA_DIR
#RUN conda install -c conda-forge ipywidgets


RUN jupyter labextension install dask-labextension
#RUN jupyter labextension install jupyterlab-server-proxy
RUN jupyter labextension install @pyviz/jupyterlab_pyviz
RUN jupyter labextension update --all

RUN pip install ipyleaflet \
  && jupyter nbextension enable --py --sys-prefix ipyleaflet

RUN jupyter labextension install jupyter-leaflet
#RUN pip install git+https://github.com/pangeo-data/intake-stac

RUN chmod -R 777 /opt/conda/lib/python3.7/site-packages
RUN conda init
