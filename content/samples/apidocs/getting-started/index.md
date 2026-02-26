---
title: Getting started
date: 2025-12-15
bread: false
toc: true
---
The following guide is aimed to help Armadillo corporate employees get started with the Armadillo API Sandbox.


# Prerequisites

To successfully follow this guide, you'll need:
- Fresh Armadillo account.
- Latest version of curl installed.


# Get access

1. Request Admin access for your newly created Armadillo account from the Armadillo Product team (request template ID NNT-1304). Along with the Admins permissions, the Product team will provide you with the user secret.

    > [!NOTE] Never share your user secret with anyone.

2. Having received your user secret, obtain your access token. To do that via curl, replace `USER_ID` and `USER_SECRET` in the below snippet with your encrypted credentials and send it:

    ```
        curl -v -X POST "https://sb-env.armadillo.pub/api/v1/oauth2/token" \
        -u "USER_ID:USER_SECRET" \
        -H "Content-Type: application/json" \
        -d "grant_type=user_credentials"
    ```

3. Receive a response with the access token (`access_token`) and its TTL in minutes (`token_ttl`):

    ```json
    {
        "access_token": "ACESS_TOKEN",
        "token_type": "Bearer",
        "user_id": "USER_ID",
        "token_ttl": 30,
        "created_at": "ISO 8601 UTC TIMESTAMP"
    }
    ```

    >[!NOTE] Use the `access_token` with every API request. If the `access_token` expired, request a new one by repeating steps 2 and 3.


# Send test request

1. Go to "Get Trips" endpoint.

2. Follow examples to send a request.

3. Check the response. If it's an error, refer to the "Errors" section of the endpoint or to Armadillo API overview > Errors. 
