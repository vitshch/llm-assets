## Context
As a product owner, I want to list all available buckets with which I can interact, meaning retrieving documents.

## Description
The goal is to allow a consumer (web ui or application) to retrieve a list of all available buckets. This will allow the consumer to know which buckets are available for interaction, such as retrieving documents.

The API will return list of buckets with their names and any relevant metadata. The consumer can then use this information to select a bucket for further operations, such as retrieving documents.

This information is only available in specific `get /buckets` endpoint. The consumer will not be able to retrieve documents from the buckets, only the list of available buckets.

Security should allow access for roles `ADMIN`, `CONSUMER` and `PROVIDER`.
Audience: `documents`, Scope: `buckets:get`

## Technical Notes
- The endpoint will be implemented as a GET request to `/buckets`.
- The response will be a JSON array of bucket objects, each containing the bucket name and relevant metadata.
- The endpoint will be secured to allow access only for users with roles `ADMIN`, `CONSUMER`, and `PROVIDER`.
### Request
- `GET /buckets`
- Headers:
 - `Authorization: Bearer <token>` (required)
 - `Content-Type: application/json` (optional)
### Response 
- `200 OK` with a JSON array of bucket objects, each containing the bucket name and relevant metadata.
- Payload:
  - ```json
      [
        {
          "name": "bucket1",
          "metadata": {
            "createdBy": "user1",
            "createdAt": "2024-01-01T12:00:00Z"
          }
        },
        {
          "name": "bucket2",
          "metadata": {
            "createdBy": "user2",
            "createdAt": "2024-02-01T12:00:00Z"
          }
        }
      ]
    ```
- `403 Forbidden` if the user does not have the required role.

## Acceptance Criteria
[] Given a user with role `ADMIN`, `CONSUMER`, or `PROVIDER`, when they send a GET request to `/buckets`, then they should receive a `200 OK` response with a JSON array of available buckets.
[] Given a user without the required role, when they send a GET request to `/buckets`, then they should receive a `403 Forbidden` response.
[] Given a user with the required role, when they send a GET request to `/buckets`, then the response should include the bucket name and relevant metadata for each available bucket.
[] Given a user with the required role, when they send a GET request to `/buckets`, then the response should be in JSON format and contain an array of bucket objects.
