[![npm version](https://badge.fury.io/js/@tmaguire%2Fazure-ad-verify-token.svg)](https://badge.fury.io/js/@tmaguire%2Fazure-ad-verify-token)
[![CI](https://github.com/tmaguire/azure-ad-verify-token/workflows/CI/badge.svg)](https://github.com/tmaguire/azure-ad-verify-token/actions/workflows/ci.yml)
[![codecov](https://codecov.io/gh/tmaguire/azure-ad-verify-token/branch/main/graph/badge.svg?token=D6Y449JK0D)](https://codecov.io/gh/tmaguire/azure-ad-verify-token)

# Azure AD/Entra ID Verify Token

Verify JWTs issued by Azure Active Directory (Azure B2C), Entra ID (organisational accounts and B2B workflows), and Microsoft Account (MSA). This is collectively referred to as the Microsoft Identity Platform which forms part of Microsoft Entra.

> This fork is maintained by [@tmaguire](https://github.com/tmaguire) - this module was originally created by [@justinlettau](https://github.com/justinlettau).

# Table of Contents

- [Features](#features)
- [Installation](#installation)
- [Usage](#usage)
- [References](#references)
- [Development](#development)

# Features

- 🎉 **Verify JWTs** issued by the Microsoft Identity Platform (including Azure AD B2C).
- 🚀 Automatically use the **rotated public keys** from Entra ID.
- 💪 Written in **TypeScript**.
- ♻️ **Configurable cache** for public keys.

# Installation

```bash
npm install @tmaguire/azure-ad-verify-token --save
```

# Usage

## Verify

```ts
import { verify, VerifyOptions } from '@tmaguire/azure-ad-verify-token';

// Azure B2C example
const options: VerifyOptions = {
	jwksUri:
		'https://contoso.b2clogin.com/contoso.onmicrosoft.com/b2c_1_signupsignin1/discovery/v2.0/keys',
	issuer:
		'https://contoso.b2clogin.com/contoso.onmicrosoft.com/v2.0/',
	audience: '99d1275c-e805-483f-b832-600f8130829c',
};

// Entra ID example
// const options: VerifyOptions = {
// 	jwksUri:
// 		'https://login.microsoftonline.com/contoso.onmicrosoft.com/discovery/v2.0/keys',
// 	issuer:
// 		'https://sts.windows.net/contoso.onmicrosoft.com/',
// 	audience: 'api://99d1275c-e805-483f-b832-600f8130829c',
// };

verify(token, options)
	.then((decoded) => {
		// verified and decoded token
		console.log(decoded);
	})
	.catch((error) => {
		// invalid token
		console.error(error);
	});
```

### Verify options

| Property   | Type     | Description                                             |
| ---------- | -------- | ------------------------------------------------------- |
| `jwksUri`  | `string` | `jwk_uri` value obtained from metadata endpoint.        |
| `issuer`   | `string` | `issuer` value obtained from metadata endpoint.         |
| `audience` | `string` | Application ID of the application accessing the tenant. |

### Example metadata endpoints

#### Common endpoints

- https://login.microsoftonline.com/common/.well-known/openid-configuration
- https://login.microsoftonline.com/common/discovery/keys

#### Organisational endpoints

- https://login.microsoftonline.com/{tenantId}/.well-known/openid-configuration
- https://login.microsoftonline.com/{tenantId}/discovery/v2.0/keys

> Replace `{tenantId}` with either your Tenant ID (GUID from Entra ID Portal) or one of your verified domains.

#### Azure B2C endpoints

- https://{tenantName}.b2clogin.com/{tenantId}/{policyName}/v2.0/.well-known/openid-configuration
- https://{tenantName}.b2clogin.com/{tenantId}/{policyName}/discovery/v2.0/keys

> Replace `{tenantId}` with either your Tenant ID (GUID from Entra ID Portal) or one of your verified domains, replace `{tenantName}` with your B2C tenant endpoint name, and replace `{policyName}` with the relevant policy that has issued the token you want to verify.

## Configuration

```ts
import { setConfig } from '@tmaguire/azure-ad-verify-token';

setConfig({
	cacheLifetime: 12 * (60 * 60 * 1000), // 12 hours
});
```

### Configuration options

| Property        | Type     | Description                                  | Default |
| --------------- | -------- | -------------------------------------------- | ------- |
| `cacheLifetime` | `number` | Number of milliseconds to cache public keys. | 1 hour  |

# References

- [Overview of tokens in Azure Active Directory B2C](https://docs.microsoft.com/en-gb/azure/active-directory-b2c/tokens-overview)
- [Microsoft identity platform access tokens](https://docs.microsoft.com/en-gb/azure/active-directory/develop/access-tokens)
- [RSA Key Converter](https://superdry.apphb.com/tools/online-rsa-key-converter)

# Development

```bash
npm install
npm run build
```
