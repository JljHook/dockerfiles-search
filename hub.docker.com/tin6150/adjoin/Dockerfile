# Alternate "main" Dockerfile , this has Lucas Bastien Adjoin code,
# which should build ioapi 3.0 and cmaq 4.5, at least cctm ?
# based on Dockerfile.cmaq
# depends on a cascade of 2 other containers
 

##FROM tin6150/lib4cmaq:latest
FROM tin6150/cmaq:latest
MAINTAINER Tin

#RUN  echo "Building CMAQ... this isn't likely working yet" 

# CMAQ 5.2.1 build tutorial: https://github.com/USEPA/CMAQ/blob/5.2.1/DOCS/Tutorials/CMAQ_GettingStarted.md

#### make ioapi 3.1.  i have the source downloaded in the my git repo of CMAQ ####
RUN echo "begining docker build process at " ;\
    date ;\
    mkdir -p Downloads &&  cd Downloads ;\
    git clone --quiet -b tin_521_docker https://tin6150@github.com/tin6150/CMAQ.git ;\
    cd CMAQ ;\
    git log > cmaq.git.log ;\
    #export BIN='Linux2_x86_64gfort'     ;\
    # better to write the setup.sh script to compile cmaq, so that it can be used on actual host or singularity
    # if docker doesn't work
    #./setup.csh 2>&1 | tee setup.log.rst ;\
    echo "setup.csh skipped" > _CMAQ_BUILD_END_ ;\
    echo $? >> _CMAQ_BUILD_END_ ;\

    # Adding a bash script to compile Lucas Adjoin models
		bash ./setup_adjoin.sh  2>&1 | tee setup_adjoin.log.rst ;\
    echo $? > _CMAQ_ADJOIN_BUILD_END_



RUN  cd / \
  && touch    _TOP_DIR_OF_CONTAINER_  \
  && echo  "----"     >> _TOP_DIR_OF_CONTAINER_  \
  && TZ=PST8PDT date  >> _TOP_DIR_OF_CONTAINER_  \
  && echo  "Dockerfile.cmaq_adjoin 2019.0814.2218 (setenv fix, add myFC, compilerString)"  >> _TOP_DIR_OF_CONTAINER_   \
  && echo  "Dockerfile.cmaq_adjoin 2019.0907.1900+ (added adjoin code)"  >> _TOP_DIR_OF_CONTAINER_  



#ENTRYPOINT ["cat", "/Downloads/netcdf-fotran-4.4.5/_END_BUILD_NETCDF_"]
#ENTRYPOINT ["cat", "/Downloads/CMAQ/_CMAQ_BUILD_END_"]
#ENTRYPOINT ["cat", "/_TOP_DIR_OF_CONTAINER_"]

# not defining an ENTRYPOINT will default to a shell inside the container
# otherwise, run with exec bash to get shell:
# docker exec -it tin6150/cmaq -v /home/tin:/home bash 
