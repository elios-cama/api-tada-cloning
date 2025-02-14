
# Ta-da API Documentation

This document provides an overview of the Ta-da API, which allows you to interact with the Ta-da AI voice cloning and image generation platform.  This API is designed for developers who want to integrate Ta-da's features into their own applications.

## Authentication

Most API endpoints require an API key for authentication.  You can obtain an API key by registering on the Ta-da platform.  The API key should be included in the `X-API-Key` header of each request:

```
X-API-Key: YOUR_API_KEY
```

## Core Functionalities

The Ta-da API provides the following core functionalities:

1.  **User Registration and API Key Generation:**
    *   `/api/register`:  Create a new user account and automatically receive an API key.  This is the starting point for using the API.

2.  **Voice Cloning:**
    *   `/api/voices`:  Create a new voice clone.  You'll need to provide one or more audio files (`.wav` format) of the voice you want to clone, along with a name for the voice.
    *   `/api/voices/{voice_id}`: Retrieve information about a specific voice clone you've created.
    *   `/api/voices`:  List all the voice clones associated with your API key.
    *   `/api/voices/{voice_id}/generations`: Generate audio using a specific voice clone. You provide the text you want the voice to speak.
    *   `/api/voices/{voice_id}/generations`: List all audio generations for a specific voice clone.
    *   `/api/voices/generations/{generation_id}`: Retrieve a specific audio generation by its ID.
    *   `/api/voices/generations`: List all audio generations associated with your API key.

3.  **LoRA Training and Image Generation:**
    *   `/api/loras/upload-url`:  Get a pre-signed URL to upload your LoRA training data (a zip file) to Supabase Storage.  This is the first step in training a custom LoRA model.  You'll need to provide a `name`, `trigger_word`, and `file_name` for your LoRA.
    *   `/api/loras`:  After uploading your data, use this endpoint to initiate the LoRA training process.  You'll need to provide the `upload_id` returned by the upload process.
    *   `/api/loras/uploads/{upload_id}`: Get information about a specific LoRA upload.
    *   `/api/loras/uploads`: List all your LoRA uploads.
    *   `/api/loras/{upload_id}/trainings`: Start training a LoRA model based on a specific upload.
    *   `/api/loras/trainings`: List all your LoRA training jobs.
    *   `/api/loras/trainings/{training_id}`: Check the status of a specific LoRA training job.
    *   `/api/loras/generate/{training_id}`: Generate an image using a trained LoRA model.  You'll need to provide the `training_id` and a `prompt` for the image.
    *   `/api/loras/generations`: List all image generations associated with your API key.
    *   `/api/loras/generations/{generation_id}`: Retrieve a specific image generation by its ID.

4.  **Demo Endpoints (for quick testing):**
    *   `/api/demo/tadz`: Generate an image of "Tadz" using a pre-trained model and prompt enhancer.  You just need to provide a basic `prompt`.
    *   `/api/demo/pudgy`: Generate an image of a Pudgy Penguin using a pre-trained model and prompt enhancer.  You just need to provide a basic `prompt`.

## Prompt Enhancement

The API includes a built-in prompt enhancer for LoRA image generation.  This feature automatically improves your input prompts to produce higher-quality images.  The prompt enhancer is used by default in the `/api/demo/tadz`, `/api/demo/pudgy`, and `/api/loras/generate/{training_id}` endpoints.

## Error Handling

The API uses standard HTTP status codes to indicate success or failure.  Common error codes include:

*   `400 Bad Request`:  The request was malformed or missing required parameters.
*   `401 Unauthorized`:  You provided an invalid or missing API key.
*   `404 Not Found`:  The requested resource (e.g., voice clone, LoRA training) was not found.
*   `422 Unprocessable Entity`:  Validation error; the request data was invalid.
*   `500 Internal Server Error`:  An unexpected error occurred on the server.

Error responses will typically include a JSON body with a `detail` field providing more information about the error.

## Example Workflow (LoRA Training and Generation)

1.  **Register:**  Call `/api/register` with your email to create an account and get an API key.
2.  **Get Upload URL:**  Call `/api/loras/upload-url` with the `name`, `trigger_word`, and `file_name` of your LoRA to get a pre-signed URL.
3.  **Upload Data:**  Upload your zipped training data to the pre-signed URL.
4.  **Start Training:**  Call `/api/loras` with the `upload_id` from the previous step to start training.
5.  **Check Status:**  Call `/api/loras/trainings/{training_id}` (using the ID returned from step 4) to monitor the training progress.
6.  **Generate Image:**  Once training is complete, call `/api/loras/generate/{training_id}` with your prompt to generate an image.
7.  **List Generations:** Call `/api/loras/generations` to see all your generated images.

## Example Workflow (Voice Cloning and Generation)

1.  **Register:** Call `/api/register` with your email to create an account and get an API key.
2.  **Create Voice Clone:** Call `/api/voices` with your audio files and a name for the voice.
3.  **List Voice Clones:** Call `/api/voices` to see all your created voices.
4.  **Generate Audio:** Call `/api/voices/{voice_id}/generations` with the `voice_id` and the text you want to generate.
5.  **List Generations:** Call `/api/voices/generations` to see all your generated audio files.

## Example Workflow (LoRA Training and Generation)
1. **Register:** Call `/api/register` with your email to create an account and get an API key.
2. **Get Upload URL:** Call `/api/loras/upload-url` with name, trigger_word, and file_name to get a pre-signed URL.
3. **Create LoRA Upload:** Call `/api/loras` with your upload data to register the training files.
4. **List LoRA Uploads:** Call `/api/loras/uploads` to see all your uploaded training data.
5. **Train LoRA:** Call `/api/loras/{upload_id}/trainings` to start training your custom model.
6. **Check Training Status:** Call `/api/loras/trainings/{training_id}` to monitor progress.
7. **Generate Image:** Call `/api/loras/generate/{training_id}` with your prompt to create an image.
8. **List Generations:** Call `/api/loras/generations` to see all your generated images.

