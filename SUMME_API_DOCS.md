# SumMe API Documentation

This document provides detailed information about the Summarize API, including the available endpoints, request/response formats, and error codes. The API is currently designed to process text and generate summaries based on the input text. This documentation is only temporary and will be hosted on the API server in the future.

## Base URL

The base URL for the API is:

```
https://medwave.app/api/sum
```

All endpoints should be appended to this base URL to access the API services.

## Authentication

The API does not currently require authentication for access. However, it is recommended to use secure connections (HTTPS) when interacting with the API to protect sensitive data.

In the future, we plan to implement authentication mechanisms to ensure secure access to the API services.

## API Endpoints

### POST /sectionize

This endpoint processes a provided text and organizes it into sections based on predefined titles or custom titles included in the request. It's particularly useful for structuring raw text into a more readable and segmented format.

#### Request

- `text` (string, required): The text to be analyzed and sectioned.
- `titles` (array of strings, optional): A list of custom section titles to be used for segmenting the text. If not provided, a default set of titles will be used.

This should look something like this:

```json
{
  "text": "Dr. Smith has been practicing pediatric medicine for over 20 years. She specializes in treating conditions that affect children. Recently, she published a paper on the effects of early intervention in children with ADHD. Her clinic, located downtown, provides a child-friendly environment that enhances patient visits. Therapy sessions are often conducted with a focus on interactive methods.",
  "titles": ["Biography", "Publications", "Clinic Environment", "Therapy Approach"]
}
```

#### Response

A successful request returns a JSON object containing the segmented text based on the provided titles or default titles.

It will look something like this:

```json
{
  "sections": [
    {
      "title": "Biography",
      "content": "Dr. Smith has been practicing pediatric medicine for over 20 years. She specializes in treating conditions that affect children."
    },
    {
      "title": "Publications",
      "content": "Recently, she published a paper on the effects of early intervention in children with ADHD."
    },
    {
      "title": "Clinic Environment",
      "content": "Her clinic, located downtown, provides a child-friendly environment that enhances patient visits."
    },
    {
      "title": "Therapy Approach",
      "content": "Therapy sessions are often conducted with a focus on interactive methods."
    }
  ]
}
```

#### Example CURL Request

```bash
curl -X POST -H "Content-Type: application/json" -d '{"text": "Dr. Smith has been practicing pediatric medicine for over 20 years. She specializes in treating conditions that affect children. Recently, she published a paper on the effects of early intervention in children with ADHD. Her clinic, located downtown, provides a child-friendly environment that enhances patient visits. Therapy sessions are often conducted with a focus on interactive methods.", "titles": ["Biography", "Publications", "Clinic Environment", "Therapy Approach"]}' https://medwave.app/api/sum/sectionize
```

#### Possible Error Codes

- `422` Unprocessable Entity: The request is missing required parameters or contains invalid data.
- `400 Bad Request`: The request is missing required parameters or contains invalid data.
- `500 Internal Server Error`: An unexpected condition prevented the server from fulfilling the request. Contact the API administrators if this error occurs.

