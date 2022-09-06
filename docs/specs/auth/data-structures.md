# Data Structures

## Metadata

Metadata is a set of parameters used to identify each participant in a session and/or pairing which are provided by the consumer for the client to broadcast to its peer

```jsonc
{
  "name": string,
  "description": string,
  "url": string,
  "icons": [string],
  "redirect": { // Optional
    "native": string, // Optional
    "universal": string, // Optional
  }
}
```

## Request Params

```typescript
interface RequestParams {
  chainId: string;
  domain: string;
  nonce: string;
  aud: string;
  type?: string;
  nbf?: string;
  exp?: string;
  statement?: string;
  requestId?: string;
  resources?: string[];
}
```

## Respond Params

```typescript
type RespondParams = ResultResponse | ErrorResponse;
```

## Payload Params (partial Cacao)

Used for requester to authenticate wallet

```typescript
interface PayloadParams {
  type: string; // same as Cacao Header type (t)
  chainId: string;
  domain: string;
  aud: string;
  version: string;
  nonce: string;
  iat: string;
  nbf?: string;
  exp?: string;
  statement?: string;
  requestId?: string;
  resources?: string[];
}
```

## Response

```typescript
type Response = Cacao | ErrorResponse;
```

## Pending Request

```typescript
interface PendingRequest {
  id: number;
  payloadParams: PayloadParams;
  message: string;
}
```

## Cacao Header (CAIP-70)

```typescript
interface CacaoHeader {
  t: string;
}
```

## Cacao Payload (CAIP-70)

```typescript
interface CacaoPayload {
  iss: string;
  domain: string;
  aud: string;
  version: string;
  nonce: string;
  iat: string;
  nbf?: string;
  exp?: string;
  statement?: string;
  requestId?: string;
  resources?: string[];
}
```

## Cacao Signature (CAIP-70)

```typescript
interface CacaoSignature {
  t: string;
  s: string;
  m?: string;
}
```

## Cacao (CAIP-70)

```typescript
interface Cacao {
  header: CacaoHeader;
  payload: CacaoPayload;
  signature: CacaoSignature;
}
```

## Result Response

```typescript
interface ResultResponse {
  id: number;
  signature: CacaoSignature;
}
```

## Error Response

```typescript
interface ErrorResponse {
  id: number;
  error: {
    code: number;
    message: string;
  };
}
```