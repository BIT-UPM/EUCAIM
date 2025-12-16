# **Intracranial Meningioma Segmentation (BraTS-MEN)**

This tool performs Pre-Treatment intracranial Meningioma segmentation (BraTS-MEN).

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
    docker pull harbor.eucaim.cancerimage.eu/processing-tools/intracranial-meningioma-segmenter:v1.0.0
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

Once the docker image is pulled, you must first perform a **one-time setup** to ensure the output directory has the correct permissions, and then you can run the inference.

### Setup Prerequisites

Before running the container, you **must** create the directory for the output on the host machine and ensure it is writable by all users (the container's `appuser` cannot write to directories owned by `root`).

1. **Create and Set Permissions for Output:**

    ```bash
    mkdir -p output_dir
    chmod 777 output_dir
    ```

    > **IMPORTANT:** If you encounter a `Permission denied` error on `chmod 777 output_dir`, the directory was incorrectly created by `root` (e.g., in a previous failed run). You must delete the folder (`sudo rm -rf <folder>`) and then re-run the `mkdir` command above to ensure your host user owns it, which is essential for running this tool without `sudo`.

### Running Inference

You can now run the inference using the command below, ensuring absolute paths are used for input/output.

```bash
docker run --rm --network none --gpus=all --memory=16G --shm-size 4G \
        -v $(pwd)/input_dir:/input/:ro \
        -v $(pwd)/output_dir:/output/:rw \
        harbor.eucaim.cancerimage.eu/processing-tools/intracranial-meningioma-segmenter:v1.0.0
```

where:

- **`$(pwd)/input_dir`** is the absolute path to your input data directory.
- **`$(pwd)/output_dir`** is the absolute path to your output directory.

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
