# v3 → v4 API map: effect/Encoding

Generated from the official Effect `migration/v3-to-v4.md` (API diff + curated annotations). Each entry is `v3 symbol -> v4 replacement: note`.

## `effect/Encoding`

- `Encoding.DecodeException` -> `effect/encoding/EncodingError#EncodingError`: Use the unified error class with kind Decode.

- `Encoding.DecodeExceptionTypeId` -> `effect/encoding/EncodingError#EncodingErrorTypeId`: Decode and encode failures now share one marker.

- `Encoding.EncodeException` -> `effect/encoding/EncodingError#EncodingError`: Use the unified error class with kind Encode.

- `Encoding.EncodeExceptionTypeId` -> `effect/encoding/EncodingError#EncodingErrorTypeId`: Decode and encode failures now share one marker.

- `Encoding.decodeBase64` -> `effect/encoding/Base64#decode`: Import the Base64 helpers from their direct subpath.

- `Encoding.decodeBase64String` -> `effect/encoding/Base64#decodeString`: Import the Base64 helpers from their direct subpath.

- `Encoding.decodeBase64Url` -> `effect/encoding/Base64Url#decode`: Import the Base64Url helpers from their direct subpath.

- `Encoding.decodeBase64UrlString` -> `effect/encoding/Base64Url#decodeString`: Import the Base64Url helpers from their direct subpath.

- `Encoding.decodeHex` -> `effect/encoding/Hex#decode`: Import the Hex helpers from their direct subpath.

- `Encoding.decodeHexString` -> `effect/encoding/Hex#decodeString`: Import the Hex helpers from their direct subpath.

- `Encoding.decodeUriComponent` -> `Result.try`: Wrap decodeURIComponent in Result.try and map failure to EncodingError, or decode Schema.StringFromUriComponent.

- `Encoding.encodeBase64` -> `effect/encoding/Base64#encode`: Import the Base64 helpers from their direct subpath.

- `Encoding.encodeBase64Url` -> `effect/encoding/Base64Url#encode`: Import the Base64Url helpers from their direct subpath.

- `Encoding.encodeHex` -> `effect/encoding/Hex#encode`: Import the Hex helpers from their direct subpath.

- `Encoding.encodeUriComponent` -> `Result.try`: Wrap encodeURIComponent in Result.try and map failure to EncodingError, or encode Schema.StringFromUriComponent.

- `Encoding.isDecodeException` -> `effect/encoding/EncodingError#isEncodingError`: Use the unified guard and test kind === Decode when decode-only narrowing is required.

- `Encoding.isEncodeException` -> `effect/encoding/EncodingError#isEncodingError`: Use the unified guard and test kind === Encode when encode-only narrowing is required.
