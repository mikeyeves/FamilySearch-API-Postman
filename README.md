# FamilySearch API — Postman Collection

A Postman (v2.1) collection covering the **FamilySearch API**: 205 operations from the official OpenAPI reference, plus OAuth 2.0 + PKCE helper requests and worked genealogy-research examples.

**Import link:** `https://raw.githubusercontent.com/mikeyeves/FamilySearch-API-Postman/main/FamilySearch_API.postman_collection.json`
(Postman → Import → Link). Also import one of the environment files.

## Files
| File | What it is |
|---|---|
| `FamilySearch_API.postman_collection.json` | The collection (29 folders, 220 requests) |
| `FamilySearch_Beta.postman_environment.json` | apibeta / identbeta hosts (sample data, available to any app key) |
| `FamilySearch_Production.postman_environment.json` | api / ident hosts (needs production approval) |

## Getting access
1. Create a FamilySearch account and register an app at https://www.familysearch.org/developers/ to get an **App Key** (`client_id`).
2. Beta access is turned on automatically. **Production** requires acceptance into the Compatible Solution Program (app review).
3. Register a redirect URI. To use Postman's callback, register `https://oauth.pstmn.io/v1/callback`.

## Authenticating
**Option A: helper requests** (folder `00 · Authentication`)
1. *Build login URL* generates a PKCE verifier/challenge and shows the sign-in link.
2. Sign in in a browser. Copy the `code` from the redirect into `auth_code`.
3. *Exchange code for access token* saves `access_token`. Every request then uses `Bearer {{access_token}}`.
4. *Unauthenticated session token* needs no login. It works for **Places and Standardize Date** only.

**Option B: Postman's built-in OAuth 2.0**
- Grant type: Authorization Code (With PKCE), SHA-256.
- Auth URL: `{{ident_base}}/oauth2/v3/authorization`
- Token URL: `{{ident_base}}/oauth2/v3/token`
- Client ID: `{{client_id}}`. No client secret. Send client credentials in the body.

Access tokens expire after inactivity (24-hour maximum). Authorization codes are single-use and expire in 5 minutes.

## Folders
| Folder | Requests |
|---|---|
| 00 · Authentication | 6 |
| ★ Research · Bukovsky / Barna (1905) | 11 |
| Agent | 1 |
| Artifacts | 2 |
| Change History | 8 |
| Comments | 3 |
| Dates | 1 |
| Discussions | 7 |
| Groups | 5 |
| Matches | 10 |
| Memories | 6 |
| Names | 3 |
| Notes | 16 |
| Pedigrees | 2 |
| Person | 7 |
| Personas | 5 |
| Persons | 22 |
| Places | 14 |
| Portraits | 4 |
| Relationships | 4 |
| Relationships: Child and Parents | 11 |
| Relationships: Couple | 10 |
| Search | 1 |
| Source Box | 11 |
| Sources | 21 |
| Trees | 13 |
| Users | 7 |
| Utilities | 1 |
| Vocabularies | 8 |

Every request description links to its page in the official reference.

## Tips
- **Accept headers:** most reads use `application/x-fs-v1+json`. Search, matches and change history use `application/x-gedcomx-atom+json`.
- **Tree search** (`/tree/search`):
  - Fields: `q.givenName`, `q.surname`, `q.birthLikeDate`, `q.birthLikePlace`, `q.residencePlace`, `q.spouseGivenName`, …
  - Modifiers: `.exact`, `.require`, `.from` / `.to`.
  - Fuzzy surname: append `~`. Paging: `count` ≤ 100, `offset + count` < 5000.
- **Results table:** search and match requests include a Postman Visualizer table with links to each tree person.
- **X-Reason:** write requests send an `X-Reason` header taken from `{{change_reason}}`.
- **Throttling:** watch for HTTP 429 and wait for the `Retry-After` time.

## Limitations
- **Historical record search** (indexed records such as Ellis Island passenger lists or census) is **not** in FamilySearch's public API reference. From the API, use **record hints**: `/tree/persons/{pid}/matches?collection=records`.
- **Ordinances and records-collection endpoints** are restricted. They aren't included.
- **Beta data:** beta returns sample data only. Real tree persons (e.g. `K1XH-LPL`) resolve only on production.

## Related collections
- [NYC Historical Vital Records](https://github.com/mikeyeves/NYC-Historical-Vital-Records-Postman)
- [NYC Open Data](https://github.com/mikeyeves/NYC-Open-Data-Postman)
- [Unified Census ED Finder](https://github.com/mikeyeves/Unified-Census-ED-Finder-Postman)
- [Federal Register + National Archives Catalog](https://github.com/mikeyeves/US-Federal-Register-National-Archives-Catalog-APIs)
- [Hebcal](https://github.com/mikeyeves/Hebcal-Jewish-Calendar-APIs)

*Unofficial. Not affiliated with or endorsed by FamilySearch International. Generated from the public API reference.*
