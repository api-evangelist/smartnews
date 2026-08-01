---
name: Report on SmartNews ad performance
description: Authenticate and pull performance metrics from the SmartNews Insights API at the account, campaign, ad-group or ad layer.
api: openapi/smartnews-marketing-openapi.json
operations: [generateAccessToken, getInsightsV3, getAggregatedInsightsV3]
---

# Report on SmartNews ad performance

## Auth
1. `generateAccessToken` — `POST /api/oauth/v1/access_tokens`; use the Bearer JWT.

## Steps
2. `getInsightsV3` — `GET /api/ma/v3/ad_accounts/{ad_account_id}/insights/{layer}`
   where `layer` is the reporting granularity (account / campaign / ad_group / ad).
   Pass date range and metric/breakdown params.
3. `getAggregatedInsightsV3` — `GET /api/ma/v3/ad_accounts/{ad_account_id}/aggregated_insights/{layer}`
   for pre-aggregated totals over the period.

## Rules
- List/report responses paginate with `page` + `page_size`; read the `pagination`
  wrapper to page through. See conventions/smartnews-conventions.yml.
- Handle `429` with retry/backoff.
