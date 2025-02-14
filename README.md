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
    *   `POST /api/users/register`:  Create a new user account and automatically receive an API key.  This is the starting point for using the API.  Requires an `email` parameter.

2.  **Voice Cloning:**
    *   `POST /api/voices/uploads/url`: Get a pre-signed URL to upload your voice training data. Requires `name` and `file_name` parameters.
    *   `POST /api/voices/uploads`: Create a new voice clone using the uploaded data. Requires the `upload_id` parameter.
    *   `GET /api/voices/uploads`: List all your voice training data uploads.
    *   `GET /api/voices`: List all the voice clones associated with your API key.
    *   `GET /api/voices/{voice_id}`: Retrieve information about a specific voice clone. Requires the `voice_id` path parameter.
    *   `POST /api/voices/generations/{voice_id}`: Generate audio using a specific voice clone. Requires the `voice_id` path parameter and a request body with the text to be spoken.
    *   `GET /api/voices/generations/{voice_id}`: List all audio generations for a specific voice clone (supports pagination with `limit` and `offset` parameters). Requires the `voice_id` path parameter.
    *   `GET /api/voices/generations`: List all audio generations (supports pagination with `limit` and `offset` parameters).
    *   `GET /api/voices/generations/{generation_id}`: Retrieve a specific audio generation. Requires the `generation_id` path parameter.

3.  **LoRA Training and Image Generation:**
    *   `POST /api/loras/uploads/url`:  Get a pre-signed URL to upload your LoRA training data (a zip file).  Requires `name`, `trigger_word`, and `file_name` parameters.
    *   `GET /api/loras/uploads`: List all your LoRA uploads.
    *  `POST /api/loras/trainings/{upload_id}`: Start training a LoRA model based on a specific upload. Requires `upload_id` as a path parameter.
    *   `GET /api/loras/trainings/{upload_id}`: Check the status of a specific LoRA training job. Requires the `upload_id` path parameter.
    *   `GET /api/loras/trainings`: List all your LoRA training jobs (supports pagination with `limit` and `offset` parameters).
    *   `POST /api/loras/generations/{upload_id}`: Generate an image using a trained LoRA model.  Requires the `upload_id` path parameter and a request body with the `prompt`.
    *   `GET /api/loras/generations`: List all image generations associated with your API key (supports pagination with `limit` and `offset` parameters).
    *    `GET /api/loras/generations/{generation_id}`: *This endpoint was removed in the openapi.json file.*

4.  **Demo Endpoints (for quick testing):**
    *   `POST /api/demo/tadz`: Generate an image of "Tadz" using a pre-trained model and prompt enhancer.  You just need to provide a basic `prompt`.
    *   `POST /api/demo/pudgy`: Generate an image of a Pudgy Penguin using a pre-trained model and prompt enhancer.  You just need to provide a basic `prompt`.

## Prompt Enhancement

The API includes a built-in prompt enhancer for LoRA image generation.  This feature automatically improves your input prompts to produce higher-quality images.  The prompt enhancer is used by default in the `/api/demo/tadz`, `/api/demo/pudgy`, and `/api/loras/generations/{upload_id}` endpoints.

## Error Handling

The API uses standard HTTP status codes to indicate success or failure.  Common error codes include:

*   `400 Bad Request`:  The request was malformed or missing required parameters.
*   `401 Unauthorized`:  You provided an invalid or missing API key.
*   `404 Not Found`:  The requested resource (e.g., voice clone, LoRA training) was not found.
*   `422 Unprocessable Entity`:  Validation error; the request data was invalid.
*   `500 Internal Server Error`:  An unexpected error occurred on the server.

Error responses will typically include a JSON body with a `detail` field providing more information about the error.

## Example Workflow (LoRA Training and Generation)

1.  **Register:**  Call `POST /api/users/register` with your email to create an account and get an API key.
2.  **Get Upload URL:**  Call `POST /api/loras/uploads/url` with the `name`, `trigger_word`, and `file_name` of your LoRA to get a pre-signed URL.
3.  **Upload Data:**  Upload your zipped training data to the pre-signed URL.
4.  **Start Training:** Call `POST /api/loras/trainings/{upload_id}`.
5.  **Check Status:**  Call `GET /api/loras/trainings/{upload_id}` to monitor the training progress.
6.  **Generate Image:**  Once training is complete, call `POST /api/loras/generations/{upload_id}` with your prompt to generate an image.
7.  **List Generations:** Call `GET /api/loras/generations` to see all your generated images.

## Example Workflow (Voice Cloning and Generation)

1. **Register:** Call `POST /api/users/register` with your email to create an account and get an API key.
2. **Get Upload URL:** Call `POST /api/voices/uploads/url` with `name` and `file_name` to get a pre-signed URL.
3. **Upload Data:** Upload your voice training data to the pre-signed URL.
4. **Create Voice:** Call `POST /api/voices/uploads` with the `upload_id` to create your voice clone.
5. **Generate Audio:** Call `POST /api/voices/generations/{voice_id}` with your text to generate audio.
6. **List Generations:** Call `GET /api/voices/generations` to see all your generated audio files.

