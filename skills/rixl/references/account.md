# Users, members and access

Who the caller is, who else is in the organization, and what they are allowed to do.

`user_id` and `user.user_id` appear across these operations and identify the caller, so leave them
out. `org_id` and `user.org_id` default to the caller's organization in the same way.

## The caller

| Operation | Params |
|-----------|--------|
| `auth.v1.UserService.GetUser` | none |
| `auth.v1.UserService.GetUserInfo` | none |
| `auth.v1.EmailService.GetUserEmailStatus` | none |
| `auth.v1.PasskeyService.ListPasskeys` | none |
| `auth.v1.OTPService.GetOTPStatus` | none |
| `auth.v1.ProvidersService.ListProviders` | none |
| `auth.v1.BlogService.GetBlogSubscription` | none |

`GetUser` returns the account record. `GetUserInfo` returns the OIDC style claims. Use `GetUser`
unless the user asks for claims.

## Members

| Operation | Params |
|-----------|--------|
| `auth.v1.MembershipService.ListMemberships` | `limit`, `offset`, `state` |
| `auth.v1.MembershipService.ListOrganizationMembers` | `limit`, `offset` |
| `auth.v1.MembershipService.ListMembershipApplications` | `limit`, `offset`, `state` |
| `auth.v1.MembershipService.CheckMembership` | `org_id` |
| `auth.v1.MembershipService.GetInternalMembershipInfo` | `org_id` |

`ListMemberships` is the orgs the caller belongs to. `ListOrganizationMembers` is the people in one
org. They sound alike and answer opposite questions.

These page with `limit` and `offset`, not `pagination.limit` and `pagination.offset`.

## Access policies

| Operation | Params |
|-----------|--------|
| `auth.v1.PolicyService.ListPolicies` | none |
| `auth.v1.PolicyService.GetPolicy` | `policy_id` |
| `auth.v1.PolicyService.ListPolicyAttachments` | `policy_id` |
| `auth.v1.PolicyService.ListUserPolicies` | `member_id` |
| `auth.v1.PolicyService.ListPermissionRegistry` | `types`, `limit`, `offset` |

To answer "what can this person do", get their `member_id` from `ListOrganizationMembers`, then call
`ListUserPolicies`. `ListPolicyAttachments` goes the other way and lists who holds a given policy.
`ListPermissionRegistry` is the catalogue of every permission that exists.

## Domains and credentials

| Operation | Params |
|-----------|--------|
| `auth.v1.DomainService.GetDomainStatus` | `org_id` |
| `auth.v1.DomainService.GetDomainAutoJoin` | `org_id` |
| `apikeys.v1.ApiKeyService.ListApiKeys` | `pagination.limit`, `pagination.offset` |
| `clientauth.v1.ClientCredentialService.ListClientCredentials` | `pagination.limit`, `pagination.offset` |

`ListApiKeys` returns key metadata, never a key value. Auto-join controls whether someone with a
verified email on the custom domain joins the org automatically.
