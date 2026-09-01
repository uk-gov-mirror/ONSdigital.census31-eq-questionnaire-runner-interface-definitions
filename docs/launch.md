# Launch: Respondent Management System to EQ

A respondent must access EQ via an upstream Respondent Management System e.g. Respondent Home. To launch an EQ:

1. Create a set of launch claims. These are required by EQ to authenticate the respondent and provide the necessary data to render the required questionnaire. The claims to use when launching EQ are documented in [Launch Payload](launch_payload_v2.md).
1. Create a JWT token using the launch claims, encrypting the claims with the public key of EQ and signing with the Respondent Management System's private signing key. See [JWT Profile](jwt_profile.md) for details.
1. Create a URL for the EQ `session` endpoint using the JWT as a `token` query string parameter: `https://<eq_domain>/session?token=<JWT>`
1. Return the URL as a `HTTP 302` browser redirect, when the respondent follows this redirect it launches EQ.

## Flushing responses

To flush responses to the downstream systems, a `/flush` endpoint is available.
This endpoint takes a JWT in the same way as `/session` but with a `roles` claim including the value `flusher`.
