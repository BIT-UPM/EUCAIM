# Brain Glioma Segmenter (BraTS 2025)

This repository contains the Dockerfile for the Brain Glioma Segmenter, compliant with EUCAIM packaging guidelines.

## Tool Description
Inference engine for Brain Glioma MRI Segmentation, developed by the Children's National Hospital Team for BraTS 2025.

## Docker Image
The image is built upon `aparida12/brats2025:gli` and adds necessary metadata and configuration for EUCAIM compliance.

## Usage

```bash
docker run --rm \
    -v /path/to/input:/input \
    -v /path/to/output:/output \
    <registry>/<project>/glioma-segmenter:latest
```

### Inputs
The tool expects input files in `/input`.

### Outputs
The tool writes segmentation results to `/output`.

## Metadata
- **Authors**: Universidad Politécnica de Madrid & Children's National Hospital Team
- **License**: MIT
- **Source**: https://github.com/Pediatric-Accelerated-Intelligence-Lab/BraTS2025
