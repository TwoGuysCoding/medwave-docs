# Speech-to-Text API Documentation

This document provides detailed information about the Speech-to-Text (S2T) API, including the available endpoints, request/response formats, and error codes. The API is currently designed to transcribe spoken words from audio files into text using the Deepgram Nova-2 model. This documenatation is only temporary and will be hosted on the API server in the future.

## Base URL

The base URL for the API is:

```
https://medwave.app/api/kid/s2t
```

All endpoints should be appended to this base URL to access the API services.

## Authentication

The API does not currently require authentication for access. However, it is recommended to use secure connections (HTTPS) when interacting with the API to protect sensitive data. 

In the future, we plan to implement authentication mechanisms to ensure secure access to the API services.

## API Endpoints
Currently, the API supports the following endpoints:

### **RESTful endpoints:**

### `/ping` - Health Check Endpoint

This endpoint is used to check the health status of the API. It returns a simple string response to indicate that the API is up and running.

#### Method: GET

#### URL: `/ping`

#### Payload:
None required.

#### Query Parameters:
None required.

#### Headers:
None required.

#### Response:

A successful request returns a JSON object containing the message "Server is running".

It will look something like this:

```python
"Server is running"
```

#### Example CURL Request:

```bash
curl -X GET https://medwave.app/api/kid/s2t/ping
```

#### Possible Error Codes:

- `403`: Forbidden - The server understood the request, but is refusing to fulfill it. This usually happens when the request is missing required parameters or headers. Should not happen here.
- `405`: Method Not Allowed - The method used in the request is not supported for the endpoint. Check the method and try again. This shows usually only if you accidentaly use a POST request instead of a GET request.
- `500`: Internal Server Error - The server encountered an unexpected condition that prevented it from fulfilling the request. If this shows, contact us immediately.

### `/transcribe_poc` - Audio Transcription Service

This endpoint leverages the power of the Deepgram Nova-2 model to transcribe spoken words from audio files into text. It's designed to handle audio data seamlessly, providing a robust transcription service that supports various languages.

#### Method: POST

#### URL: `/transcribe_poc`

#### Payload:

- **audio_data** (`UploadFile`): The audio file you want to transcribe. Please upload as a `.wav` file to ensure compatibility and accuracy in transcription. Other files were not tested and may not work.

**NOTE**: The field name for the audio file must be `audio_data` for the API to process the file correctly.

#### Query Parameters:
None required.

#### Headers:
- `Content-Type`: Must be `multipart/form-data` to accommodate the file upload process.

#### Response:

A successful request returns a JSON object containing the transcription results along with additional metadata about the transcription:

- **text** (`string`): The transcribed text from the audio file.
- **duration** (`float`): The total duration of the audio file in seconds.

It will look something like this:

```json
{
    "text": "Hello, how are you doing today?",
    "duration": 5.0
}
```

#### Example CURL Request:

```bash
curl -X POST -F "audio_data=@path_to_your_audio_file.wav" https://medwave.app/api/kid/s2t/transcribe_poc
```

#### Example Python Script:

```python
import requests

url = "https://medwave.app/api/kid/s2t/transcribe_poc"

files = {'audio_data': open('path_to_your_audio_file.wav', 'rb')}
response = requests.post(url, files=files)

print(response.json())
```

#### Possible Error Codes:

- `400`: Bad Request - The server cannot or will not process the request due to an apparent client error. This usually happens when the request is missing required parameters or headers. Check the request and try again.
- `403`: Forbidden - The server understood the request, but is refusing to fulfill it. This usually happens when the request is missing required parameters or headers. Not likely to happen here.
- `422`: Unprocessable Entity - The server understands the content type of the request entity (hence a 415 Unsupported Media Type status code is inappropriate), and the syntax of the request entity is correct (thus a 400 Bad Request status code is inappropriate) but was unable to process the contained instructions. This happens often when you make a request with wrong body parameters or headers. Check the request and try again.
- `500`: Internal Server Error - The server encountered an unexpected condition that prevented it from fulfilling the request. If this shows, contact us immediately. 

#### Security Note:
Be aware that this endpoint records the IP address of the requester and stores the uploaded files in a user-specific directory.

#### Tips for Optimal Use:
 - Ensure the audio file is clear with minimal background noise for best results.
 - Verify the file format before uploading to avoid errors in processing.
 - For lengthy audios, consider breaking them into smaller segments to optimize processing times and resource usage.


## Contact

If you have any questions or need further assistance, please contact us [here](jakub.m.muszynski@gmail.com)