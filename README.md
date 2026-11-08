# Power Players MCP connectors

Power Players ([powerplayers.app](https://powerplayers.app)) is a civic platform where people rate US officials and candidates, track their ballot, contact their representatives and give to campaigns. These Model Context Protocol (MCP) connectors bring it into AI assistants such as Claude, ChatGPT, Grok, Perplexity and Le Chat.

The servers are hosted by Power Players. This repository holds the public listing material only: this README, the MCP Registry entry (`server.json`), the logo and the license. The server code is not published here.

## Endpoints

All three use Streamable HTTP.

| Connector | URL | Sign-in | What it does |
|---|---|---|---|
| Public civic data | `https://powerplayers.app/api/mcp/v1` | None | Ten read-only tools: search people, citizen ratings, trends, comparisons, voter-registration links, quote, embed and share helpers. |
| Your account | `https://powerplayers.app/api/mcp/account` | OAuth 2.1 (Clerk, PKCE, dynamic client registration) | Acts for a signed-in user: rate people, manage a ballot, email officials, Power stakes and boosts, donor-profile prep. Every write shows a preview and waits for the user's confirmation. |
| Power Players for Offices | `https://powerplayers.app/api/mcp/office` | OAuth 2.1, same sign-in | For officials, campaign managers and staff their office granted access: constituent sentiment, review replies, updates, town halls, AMA answers, staff access. |

OAuth clients discover the authorization server from the protected-resource metadata:

- `https://powerplayers.app/.well-known/oauth-protected-resource/api/mcp/account`
- `https://powerplayers.app/.well-known/oauth-protected-resource/api/mcp/office`

Other discovery files: `https://powerplayers.app/.well-known/mcp.json`, `https://powerplayers.app/.well-known/mcp/server.json` and `https://powerplayers.app/llms.txt`.

## Connect

- **Claude**: Settings > Connectors > Add custom connector, then paste a URL above.
- **ChatGPT**: Settings > Apps & Connectors (developer mode) > Create, then paste a URL above.
- **Grok, Perplexity, Le Chat and other MCP clients**: add a custom remote MCP server with a URL above.
- **Claude Code**: `claude mcp add --transport http powerplayers https://powerplayers.app/api/mcp/v1`

For the account and office connectors, the assistant opens a sign-in page; after signing in, call `link_account` and open the one-time link (valid 10 minutes) to connect the assistant to your Power Players account.

## Tools

Titles and MCP annotations (`readOnlyHint`, `destructiveHint`, `idempotentHint`, `openWorldHint`) are served in `tools/list`.

| Tool | Title | Connectors | Kind |
|---|---|---|---|
| `search_people` | Search people | all three | Read |
| `get_person` | Get person | all three | Read |
| `get_citizen_rating` | Get citizen rating | all three | Read |
| `get_rating_trend` | Get rating trend | all three | Read |
| `compare_people` | Compare people | all three | Read |
| `get_voter_registration_info` | Get voter registration links | all three | Read |
| `get_quote_ready_stat` | Get quote-ready stat | all three | Read |
| `get_embed_code` | Get embed code | all three | Read |
| `get_share_card` | Get share card | all three | Read |
| `get_press_kit` | Get press kit | all three | Read |
| `link_account` | Link Power Players account | account, office | Write (adds) |
| `get_my_account` | Get my account status | account | Read |
| `get_my_ratings` | Get my ratings | account | Read |
| `get_my_representatives` | Find my representatives | account | Read |
| `rate_person` | Rate a person | account | Write (adds) |
| `update_my_rating` | Update my rating | account | Write (changes, removes or spends) |
| `get_contact_options` | Get contact options | account | Read |
| `get_person_brief` | Get person brief | account | Read |
| `list_my_assistant_actions` | List assistant actions | account | Read |
| `get_call_info` | Get call info | account | Read |
| `send_email_to_official` | Email an official | account | Write (sends email) |
| `get_my_ballot` | Get my ballot | account | Read |
| `add_to_my_ballot` | Add to my ballot | account | Write (adds) |
| `update_my_ballot_entry` | Update ballot entry | account | Write (changes, removes or spends) |
| `remove_from_my_ballot` | Remove from my ballot | account | Write (changes, removes or spends) |
| `get_leaderboard` | Get leaderboard | account | Read |
| `get_leaderboard_summary` | Get leaderboard summary | account | Read |
| `get_my_power` | Get my Power balance | account | Read |
| `get_my_stakes` | Get my stakes | account | Read |
| `list_open_races` | List open races | account | Read |
| `get_race_market` | Get race market | account | Read |
| `stake_on_race` | Stake Power on a race | account | Write (changes, removes or spends) |
| `list_boost_catalog` | List boost catalog | account | Read |
| `get_boosts_on_person` | Get boosts on person | account | Read |
| `buy_boost` | Buy a boost | account | Write (changes, removes or spends) |
| `get_my_donor_profile_status` | Get donor profile status | account | Read |
| `prepare_my_donor_profile` | Prepare donor profile | account | Write (adds) |
| `get_my_offices` | Get my offices | office | Read |
| `get_constituent_sentiment` | Get constituent sentiment | office | Read |
| `get_rating_trends_vs_peers` | Compare rating with peers | office | Read |
| `list_rating_comments` | List rating comments | office | Read |
| `get_feedback_briefing` | Get feedback briefing | office | Read |
| `reply_to_review` | Reply to a review | office | Write (changes, removes or spends) |
| `post_update` | Post a public update | office | Write (adds) |
| `create_town_hall` | Create a town hall | office | Write (adds) |
| `update_town_hall` | Update a town hall | office | Write (changes, removes or spends) |
| `answer_ama_question` | Answer an AMA question | office | Write (changes, removes or spends) |
| `list_team` | List office team | office | Read |
| `grant_staff_access` | Grant staff access | office | Write (adds) |
| `update_staff_permissions` | Update staff permissions | office | Write (changes, removes or spends) |
| `revoke_staff_access` | Revoke staff access | office | Write (changes, removes or spends) |
| `update_office_profile` | Update office profile | office | Write (changes, removes or spends) |

## Giving to campaigns

Power Players makes donating to campaigns as easy as possible. The connectors never take a payment: an assistant can show who represents you and prepare the FEC donor details a contribution needs, and you check, certify and give on powerplayers.app.

## Ratings are not polls

Citizen ratings are voluntary, crowdsourced opinions. They are not polls and are not statistically representative. Every rating response includes its sample count and source date.

## Privacy and terms

- Privacy policy: https://powerplayers.app/privacy (section 14 covers AI assistants and connectors)
- Terms: https://powerplayers.app/terms
- Support: support@powerplayers.app

## License

The files in this repository are released under the MIT License (see `LICENSE`). Public Power Players data returned by the connectors is licensed CC BY 4.0; the logo is a Power Players trademark.
