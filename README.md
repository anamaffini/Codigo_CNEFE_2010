# Código CNEFE 2010: An open pipeline for spatializing and categorizing the 2010 CNEFE in Brazil

## Overview
**CNEFE-Faces** is an open-source Python pipeline designed to work with the *Cadastro Nacional de Endereços para Fins Estatísticos* (CNEFE 2010), produced by IBGE.  

The software automates the process of:
1. Reading raw **fixed-width TXT files (FWF)** from the CNEFE 2010 database.
2. Building **standardized geographic identifiers** (`ID_municipio`, `ID_setor`, `ID_face`).
3. Categorizing addresses by their official **usage codes (1–7)**.
4. Refining **“Other establishments” (code 6)** through keyword-based classification using the establishment description.
5. Linking TXT records directly with **IBGE block-face shapefiles**.
6. Exporting results as **GeoPackage (GPKG)** and **CSV** for GIS and urban analysis workflows.

This tool supports researchers in urban planning, geography, demography, and data science who need reproducible, documented, and extensible workflows for socio-spatial analysis.

---

## Input Data
- **CNEFE TXT file (2010)**: fixed-width text file (e.g., `43000340500.TXT`).  
- **Block-face shapefile (IBGE 2010)**: polygon geometry representing *faces de quadra* (e.g., `43000340500_face.shp` + `.dbf`, `.shx`, `.prj`).  

---

## Features
- Implemented in **Python 3** using `pandas`, `geopandas`, and `numpy`.  
- Reads CNEFE TXT files using fixed-width column specifications.  
- Pads codes with leading zeros and constructs unique identifiers:  
  - `ID_municipio` (7 digits)  
  - `ID_setor` (15 digits)  
  - `ID_face` (21 digits)  
- Maps usage codes (`especie_uso`, 1–7) into main categories:  
  - Residential  
  - Public/administrative services  
  - Education  
  - Health  
  - Collective facilities  
  - Other establishments (refined)  
  - Non-residential (others)  
- Refines **Other establishments** (code 6) using keyword classification (`market`, `bakery`, `bar`, `pharmacy`, `workshop`, `warehouse`, etc.).  
- Performs **robust joins** between TXT and SHP by testing multiple candidate columns (`CD_GEO`, `CD_GEOCODI`, etc.) and suffix lengths (14, 21, etc.).  
- Exports results to:  
  - `cnefe_faces.gpkg` (GeoPackage with geometries + attributes)  
  - `cnefe_por_face.csv` (aggregated counts per block face)  

---

## Outputs
- **GeoPackage (GPKG)**: spatial layer with block faces enriched with usage categories.  
- **CSV**: table aggregated by block face with counts per category.  

---

## Quick Start

### Requirements
Create a `requirements.txt` including:
```
pandas
geopandas
numpy
matplotlib
```

### Usage
1. Clone this repository:
   ```bash
   git clone https://github.com/yourusername/cnefe-faces.git
   cd cnefe-faces
   ```

2. Place your **CNEFE TXT** and **face shapefile** in the `data/` folder.

3. Open the Jupyter notebook:
   ```bash
   jupyter notebook CNEFE_FWF_Join.ipynb
   ```

4. Adjust paths in the first cell:
   ```python
   TXT_PATH = Path("data/43000340500.TXT")
   FACE_SHP = Path("data/43000340500_face.shp")
   ```

5. Run all cells. Outputs will be saved in the `output/` folder.

---

## ⚠️ Notes
- Ensure all shapefile components are available (`.shp`, `.shx`, `.dbf`, `.prj`).  
- If the automatic join fails (`0 matches`), manually check which column in the shapefile contains the block-face geocode (e.g., `CD_GEOCODI`) and update the notebook accordingly.  
- The classification dictionary for **Other establishments** (`SUBMAP_OUTROS`) can be extended to capture local variations.

---

## Citation
If you use this software, please cite:

Maffini, A. L. (2025). *Codigo_CNEFE_2010: an open pipeline for spatializing and categorizing the 2010 CNEFE in Brazil*. Journal of Open Source Software (submitted).  

--- 
