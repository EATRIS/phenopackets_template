# Dockerfile VERSION = 0.1
# py39phenopackets
## How to build image: go to directory phenopackets_template to have all files in context
# docker build --build-arg BUILD_DATE=$(date -u +'%Y-%m-%dT%H:%M:%SZ') -t niehues/py39phenopackets -t niehues/py39phenopackets:$VERSION -f Docker/Dockerfile .
## Push to registry
# docker push niehues/py39phenopackets:$VERSION
## How to run a container
# docker run -it -p 8888:8888 niehues/py39phenopackets:latest
FROM continuumio/miniconda3:latest

ARG BUILD_DATE

# annotate 
LABEL org.label-schema.schema-version="1.0"
LABEL org.label-schema.build-date=$BUILD_DATE
LABEL org.label-schema.name = "py39phenopackets"
LABEL org.label-schema.description = "EATRIS-Plus FAIRification example: capture phenotype information in Phenopacket JSON file"
LABEL org.label-schema.docker.cmd.help = "docker run -it -p 8888:8888 niehues/py39phenopackets:latest"
LABEL org.label-schema.url="https://github.com/EATRIS-Plus/phenopackets_template"
LABEL org.label-schema.vendor ="EATRIS-Plus - Flagship in Personalised Medicine"

#update
RUN apt update -y && \
    apt upgrade -y  
    
# set working directory
WORKDIR /workspace

# prepare conda environment
COPY ./Docker/environment.yml ./environment.yml
RUN conda env create -f ./environment.yml
SHELL ["conda", "run", "-n", "py39phenopackets", "/bin/bash", "-c"]
RUN python -m ipykernel install --name py39phenopackets --display-name "Python 3.9 (Phenopackets)"

# copy files - notebook, data
COPY ./data ./data
RUN mkdir ./notebooks
COPY ./notebooks/EATRIS-Plus_phenopackets.ipynb ./notebooks/EATRIS-Plus_phenopackets.ipynb

# start jupyter notebook when running container
CMD ["/bin/bash"]
ENTRYPOINT ["conda", "run", "--no-capture-output", "-n", "py39phenopackets", "jupyter", "notebook", "--notebook-dir=/workspace", "--ip=*", "--port=8888", "--no-browser", "--allow-root"]