---
name: Classify and QC vehicle images
description: Validate and classify car images (car presence, type, angle, crop, exposure, reflections, plate) before or after transformation.
api: Spyne Unified API
base_url: https://api.spyne.ai
operations:
  - POST /auto/classify/v1/image
auth: Bearer token (Authorization header); legacy auth_key body field also accepted
---

# Classify and QC vehicle images

Use this flow to run automated quality control / classification on car images.

## Steps

1. **Authenticate.** Send `Authorization: Bearer <API_KEY>`.

2. **Submit an image.** `POST https://api.spyne.ai/auto/classify/v1/image`
   - Provide the image as `image_file` (upload) or `image_url` (string).
   - Enable the checks you need (booleans): `car_classifier`, `car_type_classifier`,
     `car_shoot_category`, `car_interior_category`, `angle_detection`, `crop_detection`,
     `distance_detection`, `exposure_detection`, `reflection_detection`, `tilt_detection`,
     `window_tint_detection`, `tyre_mud_detection`, `number_plate_detection`.

3. **Read the result.** A `200` returns the classification/validation result for each enabled check.

## Errors
- `401` - "Invalid Auth-key": missing/invalid credential (see errors/spyne-problem-types.yml).

## Notes
Use this as a gate before `POST /api/pv1/merchandise/process/` so only valid, on-angle,
uncropped car images are sent for studio transformation.
