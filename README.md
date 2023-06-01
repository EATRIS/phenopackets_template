# EATRIS-Plus FAIRification template for phenotype data

EATRIS' flagship project [EATRIS-Plus](https://eatris.eu/projects/eatris-plus/) aims to build further capabilities and deliver innovative scientific tools to support the long-term sustainability strategy of EATRIS as one of Europe’s key European research infrastructures for Personalised Medicine. A key scientific output of the EATRIS-Plus project is a Multi-omics Toolbox available providing resources related to multi-omics technologies, quakity control, data stewardship and integrative analysis to researchers. As part of this toolbox, we provide templates and workflows for multi-omics data FAIRification.

The template in this repository is intended for phenotype data typically complementing multi-omics data in medical research. It converts a tab-separated file to a Phenopackets JSON file.

## Phenotype information 

Phenotype information available for the EATRIS-Plus multi-omics demonstrator cohort includes age, sex, body mass index, blood groups, and clinical haematology measurements.

## Example data

An example phenotype data set is provided in `data/example/synthetic_phenotype.txt`. The tab-separated text file contains one row per sample and multiple columns (param*) with phenotypic characteristics.

| "ID Patient" | "param17377" | "param17374" | "param17371" | "param17401" |
|--------------|--------------|--------------|--------------|--------------|
| "SynthSample0001" | 5.5 | 5.58 | 141 | 0.45 | 
| "SynthSample0002" | 8.1 | 4.92 | 148 | 0.38 |

## FAIRification process

### Mapping to ontology terms

In order to report all elements, values and units in an unambiguous way, the column names and units of the values were mapped to ontology terms. These mappings (term URI and Compact URI, CURIE) are available in `data/ontology_terms/phenotype_variables.csv`. For fields with categorical values, each possible value was mapped to ontology terms available in separate files.

| File | Description |
|------|-------------|
| `data/ontology_terms/phenotype_variables.csv` | Mapping of phenotype columns names |
| `data/ontology_terms/PhenotypicFeature_values.csv` | Mapping of phenotypic feature values (e.g. known disease) |
| `data/ontology_terms/ABO_blood_group_values.csv` | Mapping of ABO blood group values |
| `data/ontology_terms/Rh_blood_group_values.csv` | Mapping of Rhesus blood group values |
| `data/ontology_terms/smoking_values.csv` | Mapping of smoking bahaviour values |

These files contain the original column name or value mapped to ontology terms. Example:

| column_name | original_column_label | column_label | phenopackets_building_block | part_of_phenopackets_building_block | Term URI | Term CURIE | Term label | Unit term URI | Unit term CURIE | Unit term label
|-------------|-|-|-|-|-|-|-|-|-|-|
param17374 | Erythrocytes | Erythrocytes | Measurement | Biosample | https://loinc.org/26453-1/ | LOINC:26453-1 | Erythrocytes [#/volume] in Blood | http://purl.obolibrary.org/obo/NCIT_C67243 | NCIT:C67243 | Trillion Cells per Liter


### Phenopackets

The [GA4GH Phenopackets standard](https://phenopacket-schema.readthedocs.io/en/v2/) provides a way to report structured human and machine-readable phenotypic information about individuals. We use the Python library [`phenopackets`](https://github.com/phenopackets/phenopacket-schema) to create Phenopackets objects from tabular data.

The Jupyter notebook `notebooks/EATRIS-Plus_phenopackets.ipynb` demonstrates the creation of a Phenopackets JSON file using the synthetic data set. The notebook makes use of terms defined in `data/ontology_terms/*`.

The documentation of the phenopacket-schema developed by the [Global Alliance for Genomics and Health (GA4GH)](https://www.ga4gh.org/) can be found at https://phenopacket-schema.readthedocs.io/en/latest/index.html.

## Using Docker container

A docker image is available at https://hub.docker.com/r/niehues/py39phenopackets. It was build from `Docker/Dockerfile`. 

```
docker pull niehues/py39phenopackets:latest
docker run -it -p 8888:8888 niehues/py39phenopackets:latest
```

After starting the container, paste the provided URL in a browser, open and run the Jupyter notebook `notebooks/EATRIS-Plus_phenopackets.ipynb`.
