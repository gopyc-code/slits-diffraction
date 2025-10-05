# Point to Continuous: Double Slit Diffraction Autoencoder
<img width="1200" height="600" alt="slit-diffraction" src="https://github.com/user-attachments/assets/d06b35a2-0763-4b8e-a1b8-7c26638e18ff" />

The repository contains code and data for translating noisy detector hits (discrete electron-impact coordinates) into smooth continuous diffraction patterns predicted by wave optics. The core idea is to train an autoencoder that denoises and converts a rasterised point-cloud image (what a detector registers) into a continuous intensity image (theoretical profile). This is useful for removing detector noise and recovering physically plausible continuous distributions from sparse point data.

## Project Preview
Examples in the repo show side-by-side triplets:
- rasterised detector image (points)
- continuous theoretical target (sinc envelope x interference)
- autoencoder reconstruction (denoised continuous)

Image size: **64x64**. Latent vector length: **64** (chosen because the 1D continuous profile is sampled at 64 x-values).

## Dataset
Synthetic dataset [diffraction_point_and_continuous.npz](https://mega.nz/file/GiIHWJzQ#W28fxXs3qhYMRPCLuO_TIpRsErOyaIGvcQAIiOw5kGA) of 1000 paired images:
- `X_points` — rasterised point images, shape `(1000, 64, 64)` float32
- `X_cont`   — continuous theoretical images, shape `(1000, 64, 64)` float32
- `params`   — array with `(slit_width, slit_sep, L, wavelength)` per sample

## Pretrained model
Download the model pre-trained for 30 epochs from mega.nz:
- Autoencoder: [point2cont_autoencoder.keras](https://mega.nz/file/HqJiWATa#18CDB5bWKksXElhRk0j95tGv50edUe6U0XgmOlrD0sM)
- Decoder: [point2cont_decoder.keras](https://mega.nz/file/G2oWwTCa#LHxMu1Ea4rxVix6tDLskCJrlQaI7DPZg8K6F-shmtxw)
- Encoder: [point2cont_encoder.keras](https://mega.nz/file/qvYVgBaJ#FlBJKU4xfEhYvYeuMNnTal5FOOC7bAwUqq7mc2O-No4)

## Features
- Generates point clouds sampled from a 1D diffraction intensity PDF (sinc envelope × interference), rasterises them and applies gaussian smoothing to simulate detector imaging.
- Generates continuous target images using the same analytic formula (sinc × cos²), normalised and vertically smoothed.
- Paired dataset for supervised denoising: `point image` to `continuous image`.
- Convolutional autoencoder: encoder (Conv2D ×3 + pooling + Dense latent), decoder (Dense + Conv2DTranspose ×3 + sigmoid).
- Trained with MSE loss to reconstruct continuous images from point images.

## Results

Convergence reported as good during experiments. The final MSE is 0.0149.
