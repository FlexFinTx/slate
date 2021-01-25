---
title: Flex Hub API Reference

language_tabs:
  - shell
  - javascript

toc_footers:
  - <a href='https://flexfintx.com/'>Join the platform as an organization</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Introduction

## Decentralized Identity

Decentralized Identity, or Self-Sovereign Identity, is a form of digital identity based on blockchain technology. It is a set of emerging technical standards aiming to revolutionize how identity is managed in the modern world, with a focus on privacy and security.

At FlexFinTx, we are building tools for secure digital identity for Africa. We built our core digital identity platform, FlexID, on the [Algorand](https://algorand.com) blockchain, and are building tools for organizations to easily get started using FlexID.

## Flex Hub

The Flex Hub API is a SaaS platform for organizations to manage their organizations on the FlexID platform. The API can be used to manage Decentralized Identifiers (DIDs) owned by the organization, issue out Verifiable Credentials (VCs) to FlexID holders, request holders to present Verifiable Presentations (VPs) to the organization, and revoke previously issued Verifiable Credentials.

# Authorization

Flex Hub uses OAuth2 Client Credentials to allow access to the API. As part of onboarding the FlexID platform, you will be provided with the credentials required to make calls to our authorization provider and receive an access token. The access token is used as an `Authorization` header on all endpoints with the authorization scheme `Bearer`

## Get Access Token

> Request Body

```shell
curl --request POST \
    --url https://flexfintx.us.auth0.com/oauth/token \
    --data '{
        "client_id": "9uKHFvrIwkJGb0U73GRyalb6emEWAf92",
        "client_secret": "X1uYb9hR7KL-NH5RkkJFFhYnH-9hcgR4o-W21FDFfHjzVu8HyY3b3FRmLE0-aj3D",
        "audience": "https://organization.hub.flexfintx.com/v1/",
        "grant_type": "client_credentials"
    }'
```

```javascript
var myHeaders = new Headers();
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
  client_id: "9uKHFvrIwkJGb0U73GRyalb6emEWAf92",
  client_secret:
    "X1uYb9hR7KL-NH5RkkJFFhYnH-9hcgR4o-W21FDFfHjzVu8HyY3b3FRmLE0-aj3D",
  audience: "https://organization.hub.flexfintx.com/v1/",
  grant_type: "client_credentials",
});

var requestOptions = {
  method: "POST",
  headers: myHeaders,
  body: raw,
  redirect: "follow",
};

fetch("https://flexfintx.us.auth0.com/oauth/token", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```

> Sample Response (200 OK)

```json
{
  "access_token": "eyJhbHziOiJSUzI1NiIsInR5cCI6IkpXVCIsImtpZCI6InVEa1lXa0UwOGN4NkZENE9lS01LWiJ9.eyJpc3MiOiJodHRwczovL2ZsZXhmaW50eC51cy5hdXRoMC5jb20vIiwic3ViIjoiOXVLSEZ2ckl3a0pHYjBVNzNHUnlhbGI2ZW1FV0FMMDVAY2xpZW50cyIsImF1ZCI6Imh0dHFwOi8vc2FuZGJveC5odWIuZmxleGZpbnR4LmNvbS92MS8iLCJpYXQiOjE2MDc1MTM2NTgsImV4cCI6MTYwNzYwMDA1OCwiYXpwIjoiOXVLSEZ2ckl3a0pHYjBVNzNHUnlhbGI2ZW1FV0FMMDUiLCJndHkiOiJjbGllbnQtY3JlZGVudGlhbHMifQ.tLlgYdAROgYgFJw6tzdOJrhybQcUcrpswsUsczLFssBx1EHGUTbvBTzrHjWN2QIJ_XyEcAVHPzPyEjOJrVfkZj2l1nsGjZwmU6Gx6tq38FwH70aEVbwZw_xFMuLR2VofJHtk0bIeSkAHe532-yw3tUyPmJlAWIIIPM5DCWARFPNslMw4dqg7WIMHvhXcRattdIfCeN5XOKFBey0brZtI4kMN0-KWsblWx3lOSL90QXoMGav1u2dIgKYafuKhHBSkSae_5OSjhIpDPQ2QuKtmIzBabVRmxWpqotxV-GYoykMWQFZzdTbqGb0eFgO8Y18COvdQ_2v5I_aEFns7GQTfsf",
  "expires_in": 86400,
  "token_type": "Bearer"
}
```

> Sample Response (401 Unauthorized)

```json
{
  "error": "access_denied",
  "error_description": "Unauthorized"
}
```

Authorization endpoint to get the access token required for all organization management requests to the API.

### Request Body

`POST /oauth/token`

| Parameter     | Description                                                          |
| ------------- | -------------------------------------------------------------------- |
| client_id     | Client ID for your organization (Provided during onboarding)         |
| client_secret | Client Secret for your organization (Provided during onboarding)     |
| audience      | Base URL for your Flex Hub Organization (Provided during onboarding) |
| grant_type    | Set to `client_credentials`                                          |

# DIDs

All DID management API's require an `Authorization` header with scheme `Bearer` with an access token obtained from the `Get Access Token` endpoint.

## Create a DID

> Request Body

```shell
curl --request POST \
    --url https://organization.hub.flexfintx.com/v1/dids \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
    --data '{
      "method": "key",
      "keyType": "Ed25519"
    }'
```

```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer <ACCESS_TOKEN>");
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
  method: "key",
  keyType: "Ed25519",
});

var requestOptions = {
  method: "POST",
  headers: myHeaders,
  body: raw,
  redirect: "follow",
};

fetch("https://organization.hub.flexfintx.com/v1/dids", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```

> Sample Response (200 OK)

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    {
      "@base": "did:key:z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE"
    }
  ],
  "id": "did:key:z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE",
  "verificationMethod": [
    {
      "id": "#z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE",
      "type": "Ed25519VerificationKey2018",
      "controller": "did:key:z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE",
      "publicKeyBase58": "CymfNguWwC4bsUTS3E38xBxs1Tg8K8iWCKPtxuSAYBSr"
    },
    {
      "id": "#z6LSdPfuDZn2Dt1Hw5BDLJ3Au1W929rZrBU6zH3mP5nYgCZG",
      "type": "X25519KeyAgreementKey2019",
      "controller": "did:key:z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE",
      "publicKeyBase58": "2iVjhFyA8RHYqgoSoeXDaRHfB1KT9aHx7JL5td91xpnW"
    }
  ],
  "authentication": ["#z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE"],
  "assertionMethod": ["#z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE"],
  "capabilityInvocation": ["#z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE"],
  "capabilityDelegation": ["#z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE"],
  "keyAgreement": ["#z6LSdPfuDZn2Dt1Hw5BDLJ3Au1W929rZrBU6zH3mP5nYgCZG"]
}
```

Generates keys and creates a new DID for the organization based on the provided DID Method type and Cryptographic Key type. Currently only `key` method is supported with key type `Ed25519`.

### Request Body

`POST /dids`

| Parameter | Description                        |
| --------- | ---------------------------------- |
| method    | DID Method to create the DID for   |
| keyType   | Cryptographic key type for the DID |

Returns the DID Document that is created.

Unsupported methods and key types result in `400 Bad Request` errors with meaningful error descriptions.

## Get all DIDs

> Request Body

```shell
curl --request GET \
    --url https://organization.hub.flexfintx.com/v1/dids \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
```

```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer <ACCESS_TOKEN>");

var requestOptions = {
  method: "GET",
  headers: myHeaders,
  redirect: "follow",
};

fetch("https://organization.hub.flexfintx.com/v1/dids", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```

> Sample Response (200 OK)

```json
[
  {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      {
        "@base": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"
      }
    ],
    "authentication": ["#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"],
    "assertionMethod": ["#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"],
    "capabilityDelegation": [
      "#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"
    ],
    "capabilityInvocation": [
      "#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"
    ],
    "keyAgreement": ["#z6LSejq2UEYZFuEWuCohJCk1tEJtch4XuKCK4zToNMsy6qBZ"],
    "id": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
    "verificationMethod": [
      {
        "id": "#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
        "type": "Ed25519VerificationKey2018",
        "controller": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
        "publicKeyBase58": "5u3wGZwUxbwgGEFFbpnZQjfyBzNcAVTcgWvDhvzgquPP"
      },
      {
        "id": "#z6LSejq2UEYZFuEWuCohJCk1tEJtch4XuKCK4zToNMsy6qBZ",
        "type": "X25519KeyAgreementKey2019",
        "controller": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
        "publicKeyBase58": "44erwvjhASWmopRvmZE4Ze6QmYXRCi2AC1k7suESPTQo"
      }
    ]
  }
]
```

Returns the DID Documents of all DIDs owned by the organization.

### Request Body

`GET /dids`

## Delete a DID

> Request

```shell
curl --request DELETE \
    --url https://organization.hub.flexfintx.com/v1/dids/did:key:z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE \
    --header 'Authorization: Bearer <ACCESS_TOKEN>'
```

```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer <ACCESS_TOKEN>");

var requestOptions = {
  method: "DELETE",
  headers: myHeaders,
  redirect: "follow",
};

fetch(
  "https://organization.hub.flexfintx.com/v1/dids/did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
  requestOptions
)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```

Deletes a DID managed by the organization.

### URL Parameters

`DELETE /dids/:id`

| Parameter | Description                  |
| --------- | ---------------------------- |
| id        | The DID identifier to delete |

## Resolve a DID

> Request

```shell
curl --request GET \
    --url https://organization.hub.flexfintx.com/v1/dids/did:key:z6MkrS2hxw9xGjZ4yyJ8inzyoHWrq2wyj1xrtLJpoBQBTQEE \
    --header 'Authorization: Bearer <ACCESS_TOKEN>'
```

```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer <ACCESS_TOKEN>");

var requestOptions = {
  method: "GET",
  headers: myHeaders,
  redirect: "follow",
};

fetch(
  "https://organization.hub.flexfintx.com/v1/dids/did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
  requestOptions
)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```

> Sample Response (200 OK)

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v1",
    {
      "@base": "did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C"
    }
  ],
  "id": "did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
  "verificationMethod": [
    {
      "id": "#z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
      "type": "Ed25519VerificationKey2018",
      "controller": "did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
      "publicKeyBase58": "EvtEQdUgGL8nc5izdL5X6aremUwNRGhmuqS6k5CmVbEp"
    },
    {
      "id": "#z6LStpYjLUfF46hW9UMbgDQXJgBbWvBDoWHX4rvb1uca8JXX",
      "type": "X25519KeyAgreementKey2019",
      "controller": "did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
      "publicKeyBase58": "J9NZpArNxdym45yq9ZtZz5y7fme76u7NBtCuXSy3Qvkm"
    }
  ],
  "authentication": ["#z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C"],
  "assertionMethod": ["#z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C"],
  "capabilityInvocation": ["#z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C"],
  "capabilityDelegation": ["#z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C"],
  "keyAgreement": ["#z6LStpYjLUfF46hW9UMbgDQXJgBbWvBDoWHX4rvb1uca8JXX"]
}
```

Resolves the given DID based on it's DID Method to return the DID Document. The DID Document contains additional information related to the DID.

### URL Parameters

`GET /dids/:id`

| Parameter | Description                   |
| --------- | ----------------------------- |
| id        | The DID identifier to resolve |

# Credentials

All Credential management API's require an `Authorization` header with scheme `Bearer` with an access token obtained from the `Get Access Token` endpoint.

## Create a Credential

> Request Body

```shell
curl --request POST \
    --url https://organization.hub.flexfintx.com/v1/credentials \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
    --data '{
      "@context": ["https://www.w3.org/2018/credentials/v1", "https://schema.org"],
      "type": ["VerifiableCredential", "AlumniCredential"],
      "credentialSubject": {
        "id": "did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
        "givenName": "John",
        "familyName": "Doe",
        "alumniOf": "Example University",
      },
      "issuer": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
      "saveToBank": true,
      "isRevocable": true,
    }'
```

```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer <ACCESS_TOKEN>");
myHeaders.append("Content-Type", "application/json");

var raw = JSON.stringify({
  "@context": ["https://www.w3.org/2018/credentials/v1", "https://schema.org"],
  type: ["VerifiableCredential", "AlumniCredential"],
  credentialSubject: {
    id: "did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
    givenName: "John",
    familyName: "Doe",
    alumniOf: "Example University",
  },
  issuer: "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
  saveToBank: true,
  isRevocable: true,
});

var requestOptions = {
  method: "POST",
  headers: myHeaders,
  body: raw,
  redirect: "follow",
};

fetch("https://organization.hub.flexfintx.com/v1/credentials", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```

> Sample Response (200 OK)

```json
{
  "@context": [
    "https://www.w3.org/2018/credentials/v1",
    "https://schema.org",
    "https://w3id.org/vc-revocation-list-2020/v1"
  ],
  "id": "dc557bfe-4257-4a5a-b1f8-f3cd8dadd10a",
  "type": ["VerifiableCredential", "AlumniCredential"],
  "issuer": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
  "issuanceDate": "2021-01-25T00:59:15.857Z",
  "credentialSubject": {
    "id": "did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
    "givenName": "John",
    "familyName": "Doe",
    "alumniOf": "Example University"
  },
  "credentialStatus": {
    "id": "https://sandbox.hub.flexfintx.com/v1/revocations/list/74268682-176c-427f-ace2-b0b88d415bd8#1",
    "type": "RevocationList2020Status",
    "revocationListIndex": 1,
    "revocationListCredential": "https://sandbox.hub.flexfintx.com/v1/revocations/list/74268682-176c-427f-ace2-b0b88d415bd8"
  },
  "proof": {
    "type": "Ed25519Signature2018",
    "created": "2021-01-25T00:59:15.926Z",
    "jws": "eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..63hx3XYtdJc2jG3lkjayZrwPSV-8N5_KWvoSP5H5d7rwy0hgkE1ptkIlcyy9fAo0G_mUFCb7I1ylNREKP92cDg",
    "proofPurpose": "assertionMethod",
    "verificationMethod": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"
  }
}
```

Creates a new Verifiable Credential signed by the organization's DID that is specified in the `issuer` field. Any fields under `credentialSubject` are allowed as long as they exist as a `https://schema.org/` specification.

### Request Body

`POST /credentials`

| Parameter         | Description                                                                                                           |
| ----------------- | --------------------------------------------------------------------------------------------------------------------- |
| @context          | The JSON-LD context for the credential                                                                                |
| type              | Type of the Verifiable Credential                                                                                     |
| credentialSubject | Specific details contained within the credential. `id` field must be the DID of the receiver/holder of the credential |
| issuer            | A DID owned by the organization used to sign the credential                                                           |
| saveToBank        | `true` if the created credential should be stored in the bank, `false` if not                                         |
| isRevocable       | `true` if the credential can be revoked later, `false` if not                                                         |

Returns the signed Credential

Unsupported or invalid request bodies result in `400 Bad Request` errors with meaningful error descriptions.

## Get all Credentials

> Request Body

```shell
curl --request GET \
    --url https://organization.hub.flexfintx.com/v1/credentials \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
```

```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer <ACCESS_TOKEN>");

var requestOptions = {
  method: "GET",
  headers: myHeaders,
  redirect: "follow",
};

fetch("https://organization.hub.flexfintx.com/v1/credentials", requestOptions)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```

> Sample Response (200 OK)

```json
[
  {
    "@context": [
      "https://www.w3.org/2018/credentials/v1",
      "https://schema.org",
      "https://w3id.org/vc-revocation-list-2020/v1"
    ],
    "type": ["VerifiableCredential", "AlumniCredential"],
    "id": "46831e51-0e7d-47b9-b1e2-36ebf93bd95d",
    "issuer": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
    "issuanceDate": "2020-11-30T18:22:49.378Z",
    "credentialSubject": {
      "id": "did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
      "givenName": "John",
      "familyName": "Doe",
      "alumniOf": "Example University"
    },
    "credentialStatus": {
      "id": "https://sandbox.hub.flexfintx.com/v1/revocations/list/74268682-176c-427f-ace2-b0b88d415bd8#0",
      "type": "RevocationList2020Status",
      "revocationListIndex": 0,
      "revocationListCredential": "https://sandbox.hub.flexfintx.com/v1/revocations/list/74268682-176c-427f-ace2-b0b88d415bd8"
    },
    "proof": {
      "type": "Ed25519Signature2018",
      "created": "2020-11-30T18:22:49.402Z",
      "jws": "eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..tPH_SGm65wGlD8KXKos_fGozAqXB3xSoTVAvhyJaudi8pvTehWAnHcVNxFr42DWf39Y1P7FenEW8rMu_EFASDQ",
      "proofPurpose": "assertionMethod",
      "verificationMethod": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"
    }
  },
  {
    "@context": [
      "https://www.w3.org/2018/credentials/v1",
      "https://schema.org"
    ],
    "type": ["VerifiableCredential", "AlumniCredential"],
    "id": "1758b393-f701-4844-b223-09fa95d2dc6b",
    "issuer": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
    "issuanceDate": "2021-01-25T01:07:11.455Z",
    "proof": {
      "type": "Ed25519Signature2018",
      "created": "2021-01-25T01:07:11.467Z",
      "jws": "eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..GZKL35qIn0njHGYnbnyXpMkJ8ey3NusjZnobA9A0XlB5TOQggyikuJonvGmi4WYoKD2lVNJ66VMbe6K58rU5CQ",
      "proofPurpose": "assertionMethod",
      "verificationMethod": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"
    }
  }
]
```

Returns the credentials issued by the organization. Credentials that were not saved to the bank will be returned without the `credentialSubject` field.

### Request Body

`GET /credentials`

## Get all persisted Credentials

```shell
curl --request GET \
    --url https://organization.hub.flexfintx.com/v1/credentials/persisted \
    --header 'Authorization: Bearer <ACCESS_TOKEN>' \
```

```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer <ACCESS_TOKEN>");

var requestOptions = {
  method: "GET",
  headers: myHeaders,
  redirect: "follow",
};

fetch(
  "https://organization.hub.flexfintx.com/v1/credentials/persisted",
  requestOptions
)
  .then((response) => response.text())
  .then((result) => console.log(result))
  .catch((error) => console.log("error", error));
```

> Sample Response (200 OK)

```json
[
  {
    "@context": [
      "https://www.w3.org/2018/credentials/v1",
      "https://w3id.org/vc-revocation-list-2020/v1"
    ],
    "type": ["VerifiableCredential", "RevocationList2020Credential"],
    "id": "https://sandbox.hub.flexfintx.com/v1/revocations/list/74268682-176c-427f-ace2-b0b88d415bd8",
    "issuer": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
    "issuanceDate": "2020-11-30T18:21:38.105Z",
    "credentialSubject": {
      "id": "https://sandbox.hub.flexfintx.com/v1/revocations/list/74268682-176c-427f-ace2-b0b88d415bd8#list",
      "type": "RevocationList2020",
      "encodedList": "H4sIAAAAAAAAA-3BMQEAAADCoPVPbQsvoAAAAAAAAAAAAAAAAP4GcwM92tQwAAA"
    },
    "proof": {
      "type": "Ed25519Signature2018",
      "created": "2020-11-30T18:21:38.106Z",
      "jws": "eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..4cjdI8CvBb2kEl9cARWs4__kFtkkMOwcmxYbMf1vacj1mEM0khC7NUTfQgH4uOXd07SulS3xRIsYbQjGpcIJDg",
      "proofPurpose": "assertionMethod",
      "verificationMethod": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"
    }
  },
  {
    "@context": [
      "https://www.w3.org/2018/credentials/v1",
      "https://schema.org",
      "https://w3id.org/vc-revocation-list-2020/v1"
    ],
    "type": ["VerifiableCredential", "AlumniCredential"],
    "id": "dc557bfe-4257-4a5a-b1f8-f3cd8dadd10a",
    "issuer": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am",
    "issuanceDate": "2021-01-25T00:59:15.857Z",
    "credentialSubject": {
      "id": "did:key:z6MktP9Gzsj7bsdFiaZhJu3MwgQeb4DDq9x8brM2aMAnQp2C",
      "givenName": "John",
      "familyName": "Doe",
      "alumniOf": "Example University"
    },
    "credentialStatus": {
      "id": "https://sandbox.hub.flexfintx.com/v1/revocations/list/74268682-176c-427f-ace2-b0b88d415bd8#1",
      "type": "RevocationList2020Status",
      "revocationListIndex": 1,
      "revocationListCredential": "https://sandbox.hub.flexfintx.com/v1/revocations/list/74268682-176c-427f-ace2-b0b88d415bd8"
    },
    "proof": {
      "type": "Ed25519Signature2018",
      "created": "2021-01-25T00:59:15.926Z",
      "jws": "eyJhbGciOiJFZERTQSIsImI2NCI6ZmFsc2UsImNyaXQiOlsiYjY0Il19..63hx3XYtdJc2jG3lkjayZrwPSV-8N5_KWvoSP5H5d7rwy0hgkE1ptkIlcyy9fAo0G_mUFCb7I1ylNREKP92cDg",
      "proofPurpose": "assertionMethod",
      "verificationMethod": "did:key:z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am#z6MkjMJyrpBvJ9S9Nj5xHPkQFqDy1ZeTaNhyNXq9YCxhm8Am"
    }
  }
]
```

Returns the credentials issued by the organization that were saved to the bank. All returned credentials from this endpoint will include the `credentialSubject` field.

### Request Body

`GET /credentials/persisted`

## Get Credential by ID

> Request Body
