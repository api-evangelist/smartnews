---
name: Build and target a SmartNews custom audience
description: Create a custom audience, upload its member id list, and attach it to an ad group for targeting.
api: openapi/smartnews-marketing-openapi.json
operations: [generateAccessToken, createCustomAudience, postAudienceIdListFile, getCustomAudience, getAdGroupsByCustomAudience]
---

# Build and target a SmartNews custom audience

## Auth
1. `generateAccessToken` — `POST /api/oauth/v1/access_tokens`; use the Bearer JWT.

## Steps
2. `createCustomAudience` — `POST /api/ma/v3/ad_accounts/{ad_account_id}/custom_audiences`
   to create the audience container.
3. `postAudienceIdListFile` — `POST /api/ma/v3/ad_accounts/{ad_account_id}/audience_id_list_files`
   to upload the hashed member id list that populates the audience.
4. `getCustomAudience` — `GET /api/ma/v3/ad_accounts/{ad_account_id}/custom_audiences/{custom_audience_id}`
   to poll until the audience is ready/sized.
5. `getAdGroupsByCustomAudience` —
   `GET /api/ma/v3/ad_accounts/{ad_account_id}/custom_audiences/{custom_audience_id}/ad_groups`
   to see which ad groups already target it; attach via the ad-group targeting on
   create/update.

## Rules
- Audience population is asynchronous — poll `getCustomAudience` rather than
  assuming immediate availability.
- Validation failures return `error.error_fields[]`. See errors/smartnews-problem-types.yml.
