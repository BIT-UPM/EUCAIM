# Pediatric Brain Tumor Segmenter (BraTS 2025)

This repository contains the Dockerfile for the Pediatric Brain Tumor Segmenter, compliant with EUCAIM packaging guidelines.

## Tool Description
Inference engine for Pediatric Brain MRI Segmentation, developed by Universidad Politécnica de Madrid & Children's National Hospital Team for BraTS 2025.

## Docker Image
The image is built upon `aparida12/brats2025:peds` and adds necessary metadata and configuration for EUCAIM compliance.

## Usage

```bash
docker run --rm \
    -v /path/to/input:/input \
    -v /path/to/output:/output \
    <registry>/<project>/pediatric-segmenter:latest
```

### Inputs
The tool expects input files in `/input`.

### Outputs
The tool writes segmentation results to `/output`.

## Metadata
- **Authors**: Universidad Politécnica de Madrid & Children's National Hospital Team
- **License**: MIT
- **Source**: https://github.com/Precision-Medical-Imaging-Group/BraTS2025
