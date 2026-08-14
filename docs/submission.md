# Submission: EQ to Downstream Ingestion Service

When a user submits (a survey response or feedback):

1. The submitted data is serialised as JSON. See [Submission Payload](submission_payload_v2.md) for details of the structure.
1. The JSON is encrypted with the public key of the downstream service and signed with the EQ private key for downstream verification. See [JWT Profile][jwt_profile].
1. The encrypted and signed payload is stored in a Google Cloud Storage (GCS) sbucket

## Google Cloud Storage Details

The GCS object ID used when storing the submission depends on the type of submission:

- For `surveyresponse` objects, the object ID is the response's `tx_id`.
- For `feedback` objects, the object ID is a uniquely generated UUID.

The GCS response object contains associated [metadata][gcs_metadata] which can be used in a Pub/Sub messaging strategy for further event driven processes (e.g. receipting and triggering ingestion flow). These properties are stored in cleartext.

- Metadata will always contain a `tx_id`, `case_id` and `questionnaire_id`
- Additional metadata may be added

Example v2 GCS metadata:

```json
{
  "tx_id": "7e916adc-1f36-4f1e-81f9-57a4ab4ad183",
  "case_id": "f48e8790-f591-4086-9f6d-98c8642d96cb",
  "questionnaire_id": "1234567890"
}
```

[gcs_metadata]: https://cloud.google.com/storage/docs/viewing-editing-metadata "GCS Metadata"
[jwt_profile]: jwt_profile.md "JWT Profile Definition"
