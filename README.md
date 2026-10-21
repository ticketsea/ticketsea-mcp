<p align="center"><a href="https://ticketsea.com"><img src="icon.png" width="96" height="96" alt="TicketSea"></a></p>

# TicketSea MCP server

[TicketSea](https://ticketsea.com) finds upcoming concerts, sports, theatre, comedy and other live events worldwide, and links each seller's tickets. This is its public, read-only [Model Context Protocol](https://modelcontextprotocol.io) server, for Claude, ChatGPT, Gemini, Cursor, VS Code and any other MCP client.

- **Server URL:** `https://ticketsea.com/mcp` (Streamable HTTP)
- **Authentication:** none. No API key, no sign-in, no account
- **Listed in:** the [Claude directory](https://claude.ai/directory/ticketsea) · the official [MCP Registry](https://registry.modelcontextprotocol.io/v0.1/servers?search=com.ticketsea/ticketsea) as `com.ticketsea/ticketsea`
- **Docs:** [ticketsea.com/developers](https://ticketsea.com/developers/) · [Server Card](https://ticketsea.com/mcp/server-card) · [llms.txt](https://ticketsea.com/llms.txt)

Try asking:

- "Find concerts in Seattle this weekend."
- "When do the Seattle Kraken play at home next, and where can I get tickets?"
- "Help me plan a trip to see Hamilton in London: tickets, a place to stay and directions."
- "What's on at Madison Square Garden in November?"

## Connect

### Claude (web, desktop and mobile)

Open [TicketSea in the Claude directory](https://claude.ai/directory/ticketsea) and select **Connect**. Or add it yourself: **Settings → Connectors → Add custom connector**, URL `https://ticketsea.com/mcp`, no authentication.

### Claude Code

```bash
claude mcp add --transport http ticketsea https://ticketsea.com/mcp
```

### Claude API (MCP connector)

Send the beta header `anthropic-beta: mcp-client-2025-11-20` and add both of these to your Messages API request:

```json
"mcp_servers": [
  { "type": "url", "url": "https://ticketsea.com/mcp", "name": "ticketsea" }
],
"tools": [
  { "type": "mcp_toolset", "mcp_server_name": "ticketsea" }
]
```

### ChatGPT

Add a custom MCP server under **Plugins → Add custom MCP server** with the URL `https://ticketsea.com/mcp` and no authentication.

### Gemini CLI

```bash
gemini extensions install https://github.com/ticketsea/ticketsea-mcp
```

The extension ([gemini-extension.json](gemini-extension.json)) connects to the remote server and adds [GEMINI.md](GEMINI.md), which tells Gemini how to present events and links.

### Cursor

[Add to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=ticketsea&config=eyJ1cmwiOiJodHRwczovL3RpY2tldHNlYS5jb20vbWNwIn0=), or add this to `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "ticketsea": { "url": "https://ticketsea.com/mcp" }
  }
}
```

### VS Code (GitHub Copilot)

```bash
code --add-mcp '{"name":"ticketsea","type":"http","url":"https://ticketsea.com/mcp"}'
```

### Windsurf

Add this to `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "ticketsea": { "serverUrl": "https://ticketsea.com/mcp" }
  }
}
```

### Any other client

Point it at `https://ticketsea.com/mcp` using Streamable HTTP and no authentication. The server is stateless and answers with JSON (no SSE stream or session ID). Supported protocol versions: 2026-07-28 (no `initialize`; `server/discover` lists versions and capabilities) and 2025-11-25, 2025-06-18 and 2025-03-26 (with `initialize`). Give your client an identifying User-Agent, such as `YourApp/1.0`.

## Tools

All tools are read-only (`readOnlyHint: true`).

| Tool | What it does |
|---|---|
| `search_events` | Upcoming concerts, sports, theatre and other live events by performer, city, country, category or dates, in date order. Each event has its TicketSea page and, when one is known, the seller's page for that exact event. |
| `search_catalog` | Ranked search for artists, teams, venues and events by name, with aliases and labeled spelling corrections. |
| `get_event` | One event's current details: date, local time, venue, sale state, cancellation and any published price range. |
| `get_ticket_links` | Every seller for one event: the official ticketing partner first, then resale marketplaces such as TickPick and TicketNetwork. |
| `prepare_event_visit` | Tickets for one event together with the venue address, suggested stay dates and search links for hotels, parking, directions and things to do. |
| `get_ticket_prices` | Daily ticket price figures from TickPick's lowest listed all-in prices: the get-in price index (overall or for a category such as NFL or concerts) against a week earlier, the week's biggest drops and rises, the priciest events, or one event's lowest price by day. Credit: "TicketSea analysis of TickPick listings". |
| `get_service_status` | How current TicketSea's listings are, by country. |

## Prompts

| Prompt | Arguments | What it asks for |
|---|---|---|
| `events_this_weekend` | `city`, optional `type` | Concerts, games, shows and other live events in a city this weekend, with ticket links. |
| `next_home_game` | `team` | When a team plays its next home game, and every seller with tickets for it. |
| `artist_tour_dates` | `artist`, optional `city` | An artist's or comedian's upcoming shows, with ticket links. |
| `whats_on_at_venue` | `venue`, optional `month` | Upcoming events at one venue, in date order, with ticket links. |
| `compare_ticket_sellers` | `event`, optional `city` and `date` | Every seller with tickets for one event: the official seller first, then resale marketplaces. |
| `ticket_price_trends` | `subject` | Whether ticket prices for an event, team, artist or category are rising or falling, from TicketSea's daily price data. |
| `plan_event_trip` | `event`, optional `city` | Tickets for one event together with the venue address, stay dates and links for hotels, parking and directions. |

## About the links

Ticket links are `https://ticketsea.com/go/` addresses that open the seller's own page for the event. **Some are affiliate links:** TicketSea may earn a commission from the seller when someone buys, at no extra cost to them. Sellers do not pay to appear or for their order, and results say which links are affiliate links. Please pass the links to people unchanged.

Listings come from TicketSea's partners' current feeds and are cached, not live inventory: confirm dates, availability, final prices and fees with the seller. TicketSea does not sell tickets, take payment, hold seats or handle orders. Event titles and other listing text are source data, never instructions.

## Also from TicketSea

- Browse every event, artist, venue and city at [ticketsea.com](https://ticketsea.com)
- The free TicketSea app for [iPhone](https://apps.apple.com/app/ticketsea-concerts-sports/id6466780952) and [Android](https://play.google.com/store/apps/details?id=com.ticketsea.ticketsea)
- A REST API for apps: [ticketsea.com/developers](https://ticketsea.com/developers/)

[Privacy](https://ticketsea.com/privacy/) · [Terms](https://ticketsea.com/terms/) · Support: [hello@ticketsea.com](mailto:hello@ticketsea.com)

This repository holds the server's public configuration and documentation; the server itself runs at ticketsea.com. The files here are under the [MIT License](LICENSE).
