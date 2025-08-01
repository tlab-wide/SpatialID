# Spatial ID Dataset for the 23 Wards of Tokyo (ZL25, bldg layer)

This dataset provides spatial identifiers (Spatial IDs) derived from the 2023–2024 CityGML datasets for Tokyo’s 23 special wards. The Spatial IDs are compatible with the format and methodology of the [PLATEAU Spatial ID Generator](https://github.com/Project-PLATEAU/PLATEAU-generator-for-spatialid), a reference implementation developed under the Japanese Digital Agency’s 3D urban model standardisation initiative.

Each identifier corresponds to a single CityGML object from the `bldg` layer of the original CityGML dataset. The output consists of CSV files structured to support downstream indexing, spatial querying, and voxel-based analysis. Identifiers are generated at **zoom level 25**, corresponding to a voxel resolution of approximately **1m × 1m × 1m**.

---

## Dataset Access

Two access methods are provided:

### 1. Compressed archive (ZIP) per ward

Each ZIP archive contains the generated Spatial ID CSV files for a single ward. These archives are hosted under:

```text
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/[ward-name].zip
```

Download ZIP files for all 23 Tokyo wards:

```text
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13101_chiyoda-ku_pref_2023_citygml_2_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13102_chuo-ku_pref_2023_citygml_2_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13103_minato-ku_pref_2023_citygml_2_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13104_shinjuku-ku_pref_2023_citygml_2_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13105_bunkyo-ku_pref_2023_citygml_2_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13106_taito-ku_city_2024_citygml_1_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13107_sumida-ku_city_2024_citygml_1_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13108_koto-ku_pref_2023_citygml_2_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13109_shinagawa-ku_city_2024_citygml_1_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13110_meguro-ku_pref_2023_citygml_2_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13111_ota-ku_pref_2023_citygml_2_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13112_setagaya-ku_pref_2023_citygml_2_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13113_shibuya-ku_pref_2023_citygml_2_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13114_nakano-ku_pref_2023_citygml_2_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13115_suginami-ku_city_2024_citygml_1_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13116_toshima-ku_pref_2023_citygml_2_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13117_kita-ku_pref_2023_citygml_2_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13118_arakawa-ku_pref_2023_citygml_2_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13119_itabashi-ku_pref_2023_citygml_2_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13120_nerima-ku_pref_2023_citygml_2_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13121_adachi-ku_pref_2023_citygml_2_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13122_katsushika-ku_pref_2023_citygml_2_op.zip
https://s3.tlab.cloud/spatialid/tokyo23ku/dl/13123_edogawa-ku_pref_2023_citygml_2_op.zip
```

### 2. Individual CSV File Access (Uncompressed)

In addition to ZIP archives, all Spatial ID CSV files are directly accessible via HTTPS under the following base path:

```
https://s3.tlab.cloud/spatialid/
```

Each file corresponds to a single `.gml` file in the original CityGML dataset and is stored in a parallel folder structure. The CSV files are located in a new `spatialid/` subdirectory alongside the original building data (`bldg/`) and are suffixed with `_zl25.csv`.

#### Example

Original CityGML path:

```
https://s3.tlab.cloud/13101_chiyoda-ku_pref_2023_citygml_2_op/udx/bldg/53394509_bldg_6697_op.gml
```

Corresponding Spatial ID file (CSV):

```
https://s3.tlab.cloud/spatialid/13101_chiyoda-ku_pref_2023_citygml_2_op/udx/bldg/spatialid/53394509_bldg_6697_op_zl25.csv
```

This one-to-one correspondence enables straightforward mapping between `.gml` source files and their associated Spatial ID CSVs in downstream workflows.


## File Structure Comparison

The differences between the original and spatial ID paths are as follows:

```diff
# Original CityGML file
  13101_chiyoda-ku_pref_2023_citygml_2_op/
  └── udx/
      └── bldg/
-         53394509_bldg_6697_op.gml

# Corresponding Spatial ID file
  13101_chiyoda-ku_pref_2023_citygml_2_op/
  └── udx/
      └── bldg/
+         spatialid/
+             53394509_bldg_6697_op_zl25.csv
```



- Spatial ID files are placed in an additional `spatialid/` subdirectory.
- Output filenames are suffixed with `_zl25.csv`, indicating zoom level 25.



## Processing Scope and Methodology

- Only the `bldg` layer was processed.
- The following layers were excluded: `brid`, `dem`, `fld`, `frn`, `htd`, `lsld`, `luse`, `tran`, `ubld`, `urf`, `veg`.
- The structure and semantics of the output closely follow those of the [PLATEAU Spatial ID Generator](https://github.com/Project-PLATEAU/PLATEAU-generator-for-spatialid).
- Each output file provides a mapping between the original `gml_id` and the computed spatial identifier.
- Generation was performed using an internal pipeline, based on `citygml2id.py` logic, using grid type `zfxy` at zoom level 25. Interpolation and merging options were not enabled.



## Technical Specification

| Parameter            | Value        |
|----------------------|--------------|
| Input Format         | CityGML v2.3 |
| Output Format        | CSV          |
| Zoom Level           | 25           |
| Grid Type            | ZFXY         |
| Spatial Resolution   | ~1m³ voxel   |
| Processed Geometry   | `bldg` only  |



## Scope and Exclusions

- This dataset does not include the original CityGML files.
- Only the `bldg` layer was processed; other thematic layers are not included.
- Spatial IDs are output in separate CSVs and are not embedded within the CityGML files.
- The file structure and naming conventions aim for compatibility with automated pipelines, but are not guaranteed to match official government releases exactly.



## Citation and Usage

If you use this dataset in academic publications or software, please cite the repository as follows:

```bibtex
@misc{orsholits2025spatialid, 
  author = {Alex Orsholits and Tsukada Laboratory},
  title = {Spatial ID Dataset for the 23 Special Wards of Tokyo (ZL25)},
  year = {2025}, 
  url = {https://github.com/tlab-wide/SpatialID/},
  note = {Generated July 2025. Derived from publicly available CityGML data from the Project PLATEAU dataset.} 
}
```
