FROM nfcore/base
LABEL authors="Kevin Menden" \
      description="Docker image containing all requirements for nf-core/cageseq pipeline"

COPY environment.yml /
RUN conda env create -f /environment.yml && conda clean -a
ENV PATH /opt/conda/envs/nf-core-cageseq-1.0dev/bin:$PATH
