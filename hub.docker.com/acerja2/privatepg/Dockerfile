FROM acerja2/powergrid-dev:latest

# Run everything in /root
WORKDIR /root

# Looks like we lost the PGI environmental variable
ENV PGI=/opt/pgi
RUN echo $PGI; \
	ls "${PGI}/linux86-64/2018/cuda/9.1/include"

ARG GITHUBTOKEN=983877d3d082851c3560ddbee074b124bdf46150

RUN git clone https://acerjanic:$GITHUBTOKEN@github.com/acerjanic/PrivatePG.git; \
    cd PrivatePG; \
    git fetch; \
    git checkout master; \
    mkdir build; \
    cd build; \
    cmake ../ -DCMAKE_CXX_COMPILER=pgc++ -DCMAKE_INSTALL_PREFIX=/opt/PowerGrid; \
    make -j4; make install

# For singularity compatibility
RUN ldconfig





