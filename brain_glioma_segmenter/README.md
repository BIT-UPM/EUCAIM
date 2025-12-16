# **Brain Glioma Segmentation (BraTS-GLI)**

This tool performs Pre- and Post-Treatment Adult Glioma segmentation (BraTS-GLI).

This tool was developed as part of the Brain Tumor Segmentation (BraTS) 2025 Challenge.
The inference engine is dockerized to allow for a plug and play approach for segmentation of NifTi data.

## **Installation**

### For EUCAIM Users:

1. Make sure you have Docker installed (version 25 or higher).

2. Log in to the EUCAIM Harbor registry using your user credentials (retrieved from your user profile in the registry):

    ```bash
    docker login harbor.eucaim.cancerimage.eu -u <your_user> -p <your_token>
    ```

3. Pull the prebuilt Docker image from the EUCAIM registry:

    ```bash
    docker pull harbor.eucaim.cancerimage.eu/processing-tools/brain-glioma-segmenter:v1.0.0
    ```

4. Run the Docker container as explained in the **Usage** section below.

  **Note:** You do **not** need to build the Docker image locally. The image is already available and ready to use from the EUCAIM Harbor registry.

## Folder Structure Overview

The input folder should follow a similar structure as shown below including the naming convention. Do not add any other files or folders into the input directory. `XX` can be anything. Ensure that the image is preprocessed using the same pipeline and steps applied in the corresponding BraTS 2024 subtask preprocessing.

```
input_folder/
│
└───XX-XX-XX-XX
│   │   XX-XX-XX-XX-t1n.nii.gz
│   │   XX-XX-XX-XX-t1c.nii.gz
│   │   XX-XX-XX-XX-t2w.nii.gz
│   │   XX-XX-XX-XX-t2f.nii.gz
```

## **Usage**

Once the docker image is pulled, you must first perform a **one-time setup** to download the necessary model weights, and then you can run the inference.

### Step 1: One-Time Weight Download and Setup

The model weights must be downloaded to a persistent location that the container can access. This step requires **internet access** and is performed once.

1. **Create a persistent directory** for the weights on your host machine and ensure it is writable by the container's non-root user:

    ```bash
    mkdir -p $(pwd)/eucaim_weights
    chmod 777 $(pwd)/eucaim_weights
    ```

2. **Run the container with network enabled** (`--network=bridge` or simply omitting `--network none`) and mount the writable directory to the expected location (`/mlcube_inferer/checkpoints/`):

    ```bash
    docker run --rm --gpus=all --memory=16G --shm-size 4G \
        -v $(pwd)/eucaim_weights:/mlcube_inferer/checkpoints/:rw \
        -v <inpdir>:/input/:ro -v <outdir>:/output/:rw \
        harbor.eucaim.cancerimage.eu/processing-tools/brain-glioma-segmenter:v1.0.0
    ```

    *This will download the `all_weights.zip` file into the local `eucaim_weights` folder and then likely fail the segmentation due to missing input, but the necessary weights are now saved.*

### Step 2: Running Inference (Offline)

For subsequent and actual inference runs, you can now use the original command with the required `--network none` flag, mounting the **pre-downloaded** weights.

```bash
docker run --rm --network none --gpus=all --memory=16G --shm-size 4G \
        -v $(pwd)/eucaim_weights:/mlcube_inferer/checkpoints/:ro \
        -v <inpdir>:/input/:ro -v <outdir>:/output/:rw \
        harbor.eucaim.cancerimage.eu/processing-tools/brain-glioma-segmenter:v1.0.0
```

where:

- `<inpdir>` is the directory with all input MRI NifTi files (extn .nii.gz; .nii will be ignored) that need to be segmented.
- `<outdir>` is the directory where all outputs are stored (`<inpdir>` should not be `<outdir>`).
- **`$(pwd)/eucaim_weights`** is the persistent folder created in Step 1 containing the downloaded model weights. Note that the mount is now **read-only** (`:ro`) for the official inference run.

## Citations

If you use and/or refer to this software in your research, please cite the following papers:

- Capellán-Martín, D., Jiang, Z., Parida, A., Liu, X., Lam, V., Nisar, H., ... & Linguraru, M. G. (2023). Model ensemble for brain tumor segmentation in magnetic resonance imaging. In International Challenge on Cross-Modality Domain Adaptation for Medical Image Segmentation (pp. 221-232). Cham: Springer Nature Switzerland. https://doi.org/10.1007/978-3-031-76163-8_20

- Jiang, Z., Capellán-Martín, D., Parida, A., Tapp, A., Liu, X., Ledesma-Carbayo, M. J., ... & Linguraru, M. G. (2024). Magnetic Resonance Imaging Feature-Based Subtyping and Model Ensemble for Enhanced Brain Tumor Segmentation. arXiv preprint arXiv:2412.04094. https://doi.org/10.48550/arXiv.2412.04094

- Jiang, Z., Capellán-Martín, D., Parida, A., Liu, X., Ledesma-Carbayo, M. J., Anwar, S. M., & Linguraru, M. G. (2024, May). Enhancing generalizability in brain tumor segmentation: Model ensemble with adaptive post-processing. In 2024 IEEE International Symposium on Biomedical Imaging (ISBI) (pp. 1-4). IEEE. https://doi.org/10.1109/ISBI56570.2024.10635469

## Disclaimer

This software is provided without any warranties or liabilities and is intended for research purposes only. It has not been reviewed or approved for clinical use by the Food and Drug Administration (FDA) or any other federal or state agency.

## Metadata
- **Authors**: Universidad Politécnica de Madrid & Children's National Hospital Team
- **License**: MIT
- **Source**: https://github.com/Pediatric-Accelerated-Intelligence-Lab/BraTS2025
