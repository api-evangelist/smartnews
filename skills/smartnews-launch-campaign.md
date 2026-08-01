---
name: Launch a SmartNews ad campaign
description: Authenticate, then create a campaign, ad group, upload media, and create an ad on the SmartNews Ads platform.
api: openapi/smartnews-marketing-openapi.json
operations: [generateAccessToken, getAdAccountsByDeveloperAppId, postCampaigns, postAdGroup, postMedia, postAd]
---

# Launch a SmartNews ad campaign

Use the SmartNews Marketing API (v3, base `https://ads.smartnews.com`) to stand up
a new advertising campaign end to end.

## Auth
1. `generateAccessToken` — `POST /api/oauth/v1/access_tokens` with your developer
   app credentials. The response is a Bearer JWT access token, valid ~24h. Reuse
   it across requests (token issuance is limited to 5 requests/minute per app).
2. Send `Authorization: Bearer <access_token>` on every subsequent call.

## Steps
3. `getAdAccountsByDeveloperAppId` — `GET /api/bm/v1/developer_apps/me/ad_accounts`
   to resolve the `ad_account_id` your app can operate on.
4. `postCampaigns` — `POST /api/ma/v3/ad_accounts/{ad_account_id}/campaigns` to
   create the campaign (objective, budget, schedule).
5. `postAdGroup` — `POST /api/ma/v3/ad_accounts/{ad_account_id}/campaigns/{campaign_id}/ad_groups`
   to create an ad group under the campaign (targeting, bid).
6. `postMedia` — `POST /api/ma/v3/ad_accounts/{ad_account_id}/media_files` to
   upload creative assets and get `media_file_id`s.
7. `postAd` — `POST /api/ma/v3/ad_accounts/{ad_account_id}/ad_groups/{ad_group_id}/ads`
   referencing the uploaded media to create the ad.

## Rules
- Writes are NOT idempotent (no Idempotency-Key). GET before re-POST and handle
  `409 Conflict`. See conventions/smartnews-conventions.yml.
- On `422` the error envelope returns `error.error_fields[]` with the offending
  `field_name` in JSON-path dot notation. See errors/smartnews-problem-types.yml.
- On `429` back off and retry; do not exceed documented limits.
