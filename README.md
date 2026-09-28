# Google Ads MCP Server

[![MCP](https://img.shields.io/badge/MCP-remote%20server-blue.svg)](https://modelcontextprotocol.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Setup guide](https://img.shields.io/badge/setup%20guide-markifact.com%2Fgoogle-ads--mcp-1d6ef5.svg)](https://www.markifact.com/google-ads-mcp)

> Google Ads MCP server for Claude, ChatGPT, Codex, Cursor and any MCP client. 67 Google Ads operations: 13 reads that run instantly and 54 writes that wait for your approval. Hosted, one OAuth login, nothing to install locally.

- **Setup guide:** [markifact.com/google-ads-mcp](https://www.markifact.com/google-ads-mcp)
- **Endpoint:** `https://api.markifact.com/mcp/google-ads`

This is the Google Ads endpoint of the [Markifact MCP server](https://github.com/markifact/markifact-mcp). It is the full Markifact server with Google Ads as the entry point, so the same login also reaches every other platform Markifact connects, from Meta and Google Analytics to Sheets and Slack. If you only run Google Ads, this is the one to install.

## What you can do

- **Reports.** Campaign, ad group, ad, keyword, search term, asset, audience, device, geo and hour-of-day performance over any date range with comparison periods. Change history, recommendations, Performance Max channel split, raw GAQL.
- **Changes.** Create Search, Performance Max, Demand Gen, Display and App campaigns and ad groups. Responsive search ads and edits. Budgets, bids, bidding strategies, statuses, ad schedules, locations and languages. Keywords, negatives at ad group, campaign and shared-list level, placement and IP exclusions. Sitelinks, callouts, snippets, promotions, image and text assets. Audiences, labels, experiments, conversion actions, listing groups.
- **Planning.** Keyword Planner volumes and forecasts, geo target lookup, shareable ad previews, Ads Transparency Center lookups, and a generic mutate operation for anything else in the Google Ads API.
- **Accounts.** Manager accounts (MCC) supported: query or change any client account under the manager, or create a new client account.

## Install

Sign up at [markifact.com](https://www.markifact.com?utm_source=github&utm_medium=repo&utm_campaign=google-ads-mcp) first (free credits included), connect Google Ads on the [Connections page](https://www.markifact.com/app/connections), then add the server to your client. The [Google Ads MCP setup guide](https://www.markifact.com/google-ads-mcp) has the same steps with screenshots.

| Client | Steps |
|---|---|
| **Claude (web and desktop)** | Settings → Connectors → Add custom connector → URL `https://api.markifact.com/mcp/google-ads` → Connect, then sign in |
| **Claude Code** | `claude mcp add --transport http google-ads https://api.markifact.com/mcp/google-ads` or `/plugin install google-ads@markifact` after `claude plugin marketplace add markifact/markifact-mcp` |
| **ChatGPT and Codex** | Install the [Markifact plugin](https://chatgpt.com/plugins/plugin_asdk_app_69e795096e888191b3908a4dd48a323d) from the Plugin Directory and call it with `@Markifact` |
| **Cursor** | Settings → MCP → Add server → URL `https://api.markifact.com/mcp/google-ads` (or copy `mcp.json` from this repo into `~/.cursor/mcp.json`) |
| **Codex CLI** | Add to `~/.codex/config.toml`: `[mcp_servers.google_ads]` then `url = "https://api.markifact.com/mcp/google-ads"` |
| **Gemini CLI** | `gemini extensions install github.com/markifact/google-ads-mcp` |
| **Windsurf** | Add the snippet from `mcp.json` to your MCP config |
| **Any MCP client** | Streamable HTTP at `https://api.markifact.com/mcp/google-ads`, OAuth 2.1 with PKCE, dynamic client registration supported |

Details per client are in [`docs/clients.md`](docs/clients.md). Every client completes the same browser login; the server never asks for API keys or tokens in the chat.

## Prompts that work

**Weekly performance with the three largest changes**

```text
Pull Google Ads campaign performance for the last 7 complete days versus the previous 7. Include cost, impressions, clicks, CTR, conversions, CPA, conversion value and ROAS. Sort by cost. Then list the three largest changes with the figures behind each. Do not make any changes.
```

**Search terms wasting budget**

```text
List search terms from the last 30 days with more than $50 in cost and zero conversions, grouped by campaign. Suggest which should become negative keywords and at which level. Do not add anything yet.
```

**Budget pacing**

```text
My monthly budget for the account is $12,000. Pull spend by campaign month to date, project the month-end total at the current daily rate, and flag campaigns pacing more than 10% over or under their share.
```

**Performance Max channel split**

```text
For the Performance Max campaigns, show the channel split for the last 30 days with cost and conversions by channel.
```

**Change a budget, with a preview**

```text
Review budget pacing for "Non-brand - Shoes" over the last 30 days. If a change is justified, propose one new daily budget with the evidence, and do not update anything until I approve the exact campaign ID, current budget and proposed budget.
```

Name the account, the date range and the metrics, ask for a read before a write, and say whether changes are allowed.

## How writes work

Every write is an approval-gated operation. The client asks for a change, the server prepares it and shows the exact target (account, campaign ID, current value, proposed value), and nothing runs until you confirm. Reads never ask. The server can only reach accounts the authorized Google Ads login can already see.

## Operations (67)

Generated from the Markifact operation registry.

| Operation | What it does | Type |
|---|---|---|
| `gads_add_keyword_to_ad_group` | Adds keywords to Google Ads ad groups with match types. | Write, approval required |
| `gads_add_keywords_to_negative_list` | Add keywords to an existing negative keyword list in Google Ads | Write, approval required |
| `gads_add_negative_keywords_to_ad_groups` | Add negative keywords directly to one or more ad groups in Google Ads | Write, approval required |
| `gads_add_negative_keywords_to_campaigns` | Add negative keywords directly to one or more campaigns in Google Ads | Write, approval required |
| `gads_add_placements_to_exclusion_list` | Add placements to an existing placement exclusion list in Google Ads | Write, approval required |
| `gads_ads_transparency` | Search and retrieve ads from Google's Ads Transparency / library Center using domain or advertiser ID | Read |
| `gads_assign_portfolio_bidding_strategy` | Assigns a portfolio bidding strategy to one or more Google Ads campaigns | Write, approval required |
| `gads_attach_negative_list_to_campaigns` | Apply an existing negative keyword list or placement exclusion list to one or more campaigns in Google Ads. | Write, approval required |
| `gads_create_ad_group` | Creates a new ad group within an existing Google Ads campaign. | Write, approval required |
| `gads_create_app_ad` | Creates an App campaign ad (App installs, engagement, or pre-registration) with headlines, descriptions, images, and YouTube videos. | Write, approval required |
| `gads_create_callouts` | Create callout extensions and attach them to Google Ads entities at account, campaign, or ad group level | Write, approval required |
| `gads_create_campaign` | Creates a new empty Google Ads campaign with specified settings (Search, Display, Performance Max, AI Max, Demand Gen, or Shopping… | Write, approval required |
| `gads_create_client_account` | Create a new Google Ads client account under a manager (MCC) account, setting its name, currency, and timezone | Write, approval required |
| `gads_create_conversion_action` | Create a new conversion action in Google Ads, including upload (offline), webpage, call, and Google Play types, with category, status,… | Write, approval required |
| `gads_create_demand_gen_carousel_ad` | Creates a Demand Gen carousel ad with 2-10 carousel cards (each with image, headline, and final URL). | Write, approval required |
| `gads_create_demand_gen_single_image_ad` | Creates Demand Gen single image ads with multiple image formats (landscape, square, portrait) and text assets. | Write, approval required |
| `gads_create_demand_gen_video_ad` | Creates a Demand Gen video responsive ad with YouTube videos, logos, and text assets. | Write, approval required |
| `gads_create_description_asset` | Create description text assets and attach them to Google Ads campaigns | Write, approval required |
| `gads_create_headline_asset` | Create headline text assets and attach them to Google Ads campaigns | Write, approval required |
| `gads_create_image_asset` | Upload image assets and link them to Google Ads campaigns or ad groups | Write, approval required |
| `gads_create_label` | Create a new label in Google Ads | Write, approval required |
| `gads_create_negative_keyword_list` | Creates a new negative keyword list in Google Ads | Write, approval required |
| `gads_create_placement_exclusion_list` | Creates a new placement exclusion list in Google Ads | Write, approval required |
| `gads_create_pmax_asset_group` | Creates an asset group for Performance Max campaigns with headlines, descriptions, images, and other assets. | Write, approval required |
| `gads_create_promotion_asset` | Create promotion assets and attach them to Google Ads entities at account, campaign, or ad group level | Write, approval required |
| `gads_create_responsive_display_ad` | Creates responsive display ads (RDA) with images, videos, and text that automatically optimize across Google's display network. | Write, approval required |
| `gads_create_responsive_search_ad` | Creates responsive search ads (RSA) with multiple headlines and descriptions that automatically optimize for best performance. | Write, approval required |
| `gads_create_sitelinks` | Create sitelink extensions and attach them to Google Ads entities at account, campaign, or ad group level | Write, approval required |
| `gads_create_structured_snippets` | Create structured snippet extensions and attach them to Google Ads entities at account, campaign, or ad group level | Write, approval required |
| `gads_edit_responsive_search_ad` | Edit existing responsive search ads (RSA), update headlines, descriptions, final URL, or display paths. | Write, approval required |
| `gads_exclude_audience_segments` | Exclude audience segments (remarketing lists, affinity, in-market, life events, detailed demographics, custom audiences, and combined… | Write, approval required |
| `gads_generate_shareable_previews` | Generate shareable Google Ads preview links for Performance Max asset groups, so a client can see how the ads look without a Google Ads… | Read |
| `gads_get_account_history` | Retrieves historical account activity and changes for Google Ads accounts | Read |
| `gads_get_pmax_channel_split_report` | Generates Performance Max campaign reports split by advertising channels | Read |
| `gads_get_report` | Retrieves Google Ads data with customizable metrics, dimensions, filters, date ranges, sorting options, and built-in accounts selector. | Read |
| `gads_keyword_planner` | Researches keyword suggestions and search volume data for Google Ads campaigns | Read |
| `gads_list_audience_segments` | List Google Ads audience segments by type with optional name search and result limit. | Read |
| `gads_list_report_fields` | Search and browse available Google Ads reporting metrics and dimensions before running reports. | Read |
| `gads_manage_custom_audience` | Create, update, or remove a Google Ads custom audience defined by keywords, URLs, place categories, or apps. | Write, approval required |
| `gads_manage_experiment` | Run the lifecycle of a Google Ads campaign experiment: schedule it to start, end it early, or promote the winning treatment to the base… | Write, approval required |
| `gads_manage_ip_exclusions` | Exclude or remove IP addresses at the Google Ads account level | Write, approval required |
| `gads_manage_label_assignment` | Attach or detach a label to/from campaigns, ad groups, ads, keywords, or the account | Write, approval required |
| `gads_manage_mcc_link` | Manage manager (MCC) account links: invite a client account, accept an invitation as the client, remove a client from a manager, or leave… | Write, approval required |
| `gads_manage_portfolio_bidding_strategy` | Creates or updates a reusable portfolio bidding strategy in Google Ads with bid limits, that can be assigned to multiple campaigns | Write, approval required |
| `gads_manage_recommendations` | Apply or dismiss Google Ads recommendations, optionally with typed apply parameters to override Google's recommended values. | Write, approval required |
| `gads_mutate` | Fallback Google Ads write operation for any create, update, or remove mutation that has no dedicated operation. | Write, approval required |
| `gads_rename_ad_group` | Renames Google Ads ad groups | Write, approval required |
| `gads_rename_campaign` | Renames Google Ads campaigns | Write, approval required |
| `gads_run_gaql_query` | Executes custom Google Ads Query Language (GAQL) queries to retrieve any resource information from Google Ads API like campaign settings,… | Read |
| `gads_search_gaql_resources` | Optional discovery helper for Google Ads GAQL metadata. | Read |
| `gads_search_geo_targets` | Search for Google Ads geo target location IDs by name. | Read |
| `gads_select_accounts` | Select Google Ads accounts once and reuse them across multiple gads_get_report nodes,no repeated account selection. | Read |
| `gads_suggest_brands` | Find Google Ads brand IDs by name prefix for Performance Max brand exclusions and Search brand inclusions or exclusions. | Read |
| `gads_target_audience_segments` | Add or remove audience segments (remarketing lists, affinity, in-market, life events, detailed demographics, custom audiences, and… | Write, approval required |
| `gads_update_ad_group_bid` | Updates the bid settings of Google Ads ad groups (CPC bid, Target CPA, Target ROAS, CPM bid) | Write, approval required |
| `gads_update_ad_group_status` | Updates the status of Google Ads ad groups (enable, pause, or remove) for campaign management | Write, approval required |
| `gads_update_ad_status` | Updates the status of Google Ads (enable, pause, or remove) for campaign management | Write, approval required |
| `gads_update_asset_status` | Updates the link status of Google Ads Assets (enable, pause, or remove the link between asset and ad group or campaign). | Write, approval required |
| `gads_update_audience_signals` | Add or remove audience signals on Performance Max and Demand Gen campaigns. | Write, approval required |
| `gads_update_campaign_ad_schedule` | Updates ad schedule (day and time targeting) and optional schedule-level bid adjustments / modifiers for Google Ads campaigns. | Write, approval required |
| `gads_update_campaign_bidding_strategy` | Updates the bidding strategy for Google Ads campaigns (Manual CPC, Maximize Conversions, Maximize Conversion Value, Target Impression… | Write, approval required |
| `gads_update_campaign_budget` | Modifies the budget allocation for Google Ads campaigns. | Write, approval required |
| `gads_update_campaign_location_language` | Updates location targeting (including negative location exclusions), language targeting, and advanced location options (geo target type)… | Write, approval required |
| `gads_update_campaign_status` | Updates the status of Google Ads campaigns (enable, pause, or remove) for campaign management | Write, approval required |
| `gads_update_keyword_bid` | Updates the max CPC bid of Google Ads keywords, one keyword or in bulk. | Write, approval required |
| `gads_update_keyword_status` | Updates the status of Google Ads Keywords (enable, pause, or remove) | Write, approval required |
| `gads_update_listing_groups` | Build or replace the listing group (product group) tree on a Shopping ad group, including excluding the LOCAL channel for online-only… | Write, approval required |

The server exposes ten MCP tools. `find_operations` searches this list, `get_operation_inputs` returns the exact input schema, `run_operation` runs reads and `run_write_operation` runs approval-gated writes. The remaining tools handle connections, files and media uploads. Architecture notes are in the [main repo](https://github.com/markifact/markifact-mcp/blob/main/docs/architecture.md).

## Related

- [Google Ads MCP setup guide](https://www.markifact.com/google-ads-mcp)
- [Best Google Ads MCP servers in 2026 (10 compared)](https://www.markifact.com/blog/google-ads-mcp-servers)
- [Google Ads ChatGPT plugin](https://www.markifact.com/blog/google-ads-chatgpt-plugin)
- [Connect the Google Ads MCP to Claude](https://www.markifact.com/blog/how-to-connect-google-ads-mcp-with-claude-using-markifact-3592)
- [Google Ads MCP in Claude Code](https://www.markifact.com/blog/google-ads-mcp-claude-code)
- [Google Ads audit: 40+ checks](https://www.markifact.com/blog/google-ads-audit-claude)

## FAQ

**Do I need my own Google Ads API access?**

Markifact is an approved Google Ads API developer (Standard Access, full-service tool). Markifact's Google Ads features, including this MCP server, are a feature of your Markifact account, not a standalone API. You authorize the Google login that already has access to your accounts, and the server can only reach what that login can see. If your agency or business needs its own programmatic access to the Google Ads API, Google recommends obtaining your own Google Cloud project approved for the access level you require.

**Does it work with manager accounts?**

Yes. Authorize the manager login and name the client account in your prompt. The server can also create client accounts under the manager.

**Can it change my campaigns?**

Yes, and every change is previewed and waits for your approval in the chat. Reads run immediately. Nothing is written without a confirmation.

**Which AI clients work?**

Any MCP client with OAuth support: Claude (web, desktop, Code), ChatGPT and Codex, Cursor, Windsurf, Gemini CLI and others. Install steps for each are above.

**What does it cost?**

The server is free to install. It needs a Markifact account, which starts with free credits. Plans are on the [pricing page](https://www.markifact.com/pricing).

## Support and policies

Email [contact@markifact.com](mailto:contact@markifact.com) or open an issue. See [SECURITY.md](SECURITY.md), [PRIVACY.md](PRIVACY.md) and [TERMS.md](TERMS.md). Licensed under MIT.
