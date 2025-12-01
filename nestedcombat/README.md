# Nested ComBat (Radiomics Features Harmonization)

This repository provides an implementation of the Nested ComBat harmonization method for radiomics data. It is designed to correct variations in extracted radiomic features due to differences in imaging protocols, centers, or acquisition parameters.

## Tool Description
The tool performs harmonization at the **feature level**, aligning radiomic features across sites or scanners while preserving biological variability. It includes support for:
- **Standard ComBat**
- **Modified ComBat (M-ComBat)** using a reference batch
- **Nested ComBat** to harmonize multiple batch effects sequentially, automatically determining the optimal order


## Usage

```bash
docker run -v /path/to/data:/app/input -v /path/to/output:/app/output NestedComBat
```

### Inputs
The tool expects input files in `/input`:

- `radiomics.csv`:
  - Must include a `PatientID` column and one column per radiomic feature.
- `metadata.csv`:
  - Must include `PatientID`, `Manufacturer`, `Slice Thickness`, `Tube Voltage`, and `Center` columns.
- `config.yaml`:
  - Specifies the harmonization method (`ComBat`, `M-ComBat`, or `Nested ComBat`), batch effects to be corrected, covariates, reference batch (if any), and options to filter out poorly harmonized features.


### Outputs
After harmonization, the tool generates different outputs depending on the selected mode:

### Training Mode
The following files will be saved in the output directory:
- `nested_combat_best_order.csv` – The optimal order of batch effects used in Nested ComBat
- `features_to_harmo_list.pkl` – List of features that were successfully harmonized
- `estimates.pkl` – Estimated parameters used for feature-wise harmonization

### Test Mode
To run the tool in test mode, the following files **must be available** (produced from the training phase):
- `nested_combat_best_order.csv`
- `features_to_harmo_list.pkl`
- `estimates.pkl`
- A new test `radiomics.csv` in the input test folder

The output will include:
- `harmonized_radiomics_test.csv` – The harmonized radiomic features for the test set


## Metadata
- **Authors**: Universidad Politécnica de Madrid
- **License**: MIT
- **Source**: https://github.com/BenitoFar/NestedComBat
