---
title: 'Codigo_CNEFE_2010: an open pipeline for spatializing and categorizing the 2010 CNEFE in Brazil.'
tags:
  - Python
  - geoprocessing
  - CNEFE
  - IBGE
  - urban studies
  - census data
authors:
  - name: Ana Luisa Maffini
    orcid: 0000-0001-5334-7073
    affiliation: 1
affiliations:
 - name: Graduate Program in Urban and Regional Planning, Federal University of Rio Grande do Sul (UFRGS), Brazil
   index: 1
date: 2025-09-20
bibliography: paper.bib
---

# Summary

The *Cadastro Nacional de Endereços para Fins Estatísticos* (CNEFE), produced by the Brazilian Institute of Geography and Statistics (IBGE), is the most comprehensive address database in Brazil and serves as the backbone for census operations and spatial analyses. However, its raw fixed-width file (FWF) format and the lack of ready-to-use keys for integration with geographic layers make it difficult to use in urban and social research.

**CNEFE-Faces** is a Python pipeline that automates:
1. Reading CNEFE 2010 TXT files in fixed-width format.
2. Standardizing geographic codes and constructing unique identifiers for block faces (*faces de quadra*).
3. Categorizing address uses, refining “Other establishments” (code 6) with keyword-based classification.
4. Direct integration with IBGE’s block-face shapefiles.
5. Exporting results to GeoPackage (GPKG) and CSV for further use in GIS or Python.

This software enables researchers to replicate spatial analyses of establishments and addresses in any Brazilian municipality, promoting reproducibility and transparency.

# Statement of need

Researchers in urban planning, geography, demography, and data science often need to work with CNEFE, but its raw FWF structure is not user-friendly. Proprietary tools or ad hoc scripts lack reusability and standardization.  

**CNEFE-Faces** addresses this gap by providing open, documented, and extensible code to integrate census data with block-face geometries, enabling studies on socio-spatial inequality, accessibility, urban centralities, and related topics.

# Functionality

- Implemented in **Python 3** using `pandas`, `geopandas`, and `numpy`.
- Compatible with official **IBGE 2010 data** (CNEFE and block-face shapefiles).
- Includes:
  - Reading function via `pd.read_fwf` with official `colspecs`.
  - **Zero-fill** routines and concatenation to build `ID_face` (21 digits).
  - Mapping of `especie_uso` (1–7) to main categories.
  - Refinement of code 6 (“Other establishments”) using regular expressions.
  - Robust suffix-based join between TXT and SHP (testing multiple key lengths).
  - Export to **GPKG** and **CSV**.

# Acknowledgements

We thank IBGE for publicly providing census data (CNEFE and digital boundaries). This project was developed to support research in socio-spatial inequality and urban morphology.

# References
