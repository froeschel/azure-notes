## Security

### Authentication and Authorization
* Identities can be manged and access can be controlled with Azure AD.
* Azure AD has different SKUs: Free, Premium P1 and Premium P2.
* Azure AD can be used to authenticate users for your application.
* Applications canbe set up to use Azure AD as identity provider.
* Azure AD supports MFA.
* OpenID Connect for authentication and OAuth2 for authentication.
* Shared Access Signatures (SAS) grant limited access to resources. Three types [User, Service and Account].
* SAS keys can be genereted in PowerShell, CLI or .NET.
* RBAC is used to control access to resources.
* Role assignment: attaching a role to a user, group, service principal, or managed identity at a particular scope for the purpose of granting access.
* There are some build in roles e.g.: Owner, Contributor, Reader, User Access Administrator.
* Role assignments can be made in the portal using the IAM blade.
* *

### Secure Cloud Solutions
* Pricing Standard and Premium.
* Azure Key Vault API: Azure Key Vault can manage secrets, keys and certificates. When using C# one can access keys by using the `KeyClient` class.
* Key Vault is secured by Azure AD. It is possible to assign permissions to a Key Vault. Permission are managed in 'Access policies'.
* In Functions you can reference the Key Vault by using an environment variable in format `@Microsoft.KeyVault(SecretUri=copied identifier for the secret)`. 
* Managed identities: Used for giving access to to resources (e.g. KeyVault) without having to handle credentials.
* Managed identities are free to use.
* System assigned: Enable a managed identity directly on a service instance.
* User assigned: Identity created by the user and assined to different resources. 
