---
name: Transform a vehicle into studio-grade media
description: Submit a vehicle's raw images/video to Spyne and retrieve AI-transformed studio images, 360 spins, and feature videos.
api: Spyne Unified API
base_url: https://api.spyne.ai
operations:
  - POST /api/pv1/merchandise/process/
  - GET get-media
  - GET getreadyimages
  - webhook sku.done
auth: Bearer token (Authorization header) from the Spyne Console (Developer Hub > API Keys)
---

# Transform a vehicle into studio-grade media

Use this flow to turn a dealership's raw vehicle photos/video into merchandising-ready media.

## Prerequisites
- A Spyne API key. Generate it in the Console: Developer Hub > API Keys > Generate Key (shown once).
- Publicly reachable URLs for the input images and/or video.

## Steps

1. **Authenticate.** Send `Authorization: Bearer <API_KEY>` on every request.

2. **Submit the job.** `POST https://api.spyne.ai/api/pv1/merchandise/process/`
   - Identify the vehicle with one of `vin`, `stockNumber`, or `registrationNumber` (optionally `dealerId`).
   - Choose outputs in `media`: `image` (studio images), `spin` (360), `featureVideo`.
   - Provide inputs in `mediaInput.imageData[]` (image URLs) and/or `mediaInput.videoData[]`.
   - Tune with `processingDetails` (e.g. `backgroundId`, `numberPlateLogo`, `bannerId`, `image.backgroundType`, `spin.spinFrameCount`, `featureVideo.templateId`).
   - Capture the returned `dealerVinId`.

3. **Wait for completion (async).** Processing is asynchronous. Either:
   - Register a webhook callback URL with your Spyne contact and handle the `sku.done` event (payload carries `project_id`, `sku_id`, `sku_status`, and an `image_data[]` array of `output_image` URLs; verify the `signature` JWT), OR
   - Poll the retrieve endpoints (Retrieve Media / GET Images) until the SKU status is `done`.

4. **Collect outputs.** Read `output_image` (and `lowres_output`) URLs per image, ordered by `frame_no`. Images are categorized `Exterior` / `Interior` / `Miscellaneous`.

## Errors
- `401` - missing/invalid Bearer token (see errors/spyne-problem-types.yml).
- `400` - malformed request or invalid parameters.
