# hipages API integration and MCP server: connect hipages to ChatGPT, Claude and any AI agent

**hipages API integration and MCP server by Cotlet.dev: yes, Claude, ChatGPT and other AI agents can work in your hipages account.** hipages has no public API for tradies, and its official ChatGPT app helps homeowners find a tradie, not run your leads or quotes. Cotlet.dev connects your own signed-in hipages business account to any AI agent, so you ask in plain words and the AI answers job leads, sends quotes and follows up for you.

## What you can ask

> Show me today's new hipages leads and draft a reply to each so I am the first to call back. Send nothing yet.

> Rank this week's hipages leads by job size and suburb and flag the vague ones.

> How much did I spend on hipages leads this month, and how many turned into won jobs?

> List quotes from hipages leads with no reply after 3 days and draft a follow-up for each.

> Draft a review request for every customer whose job I finished this week.

## What tradies ask their AI to do in hipages

From tradies' posts on Reddit, Facebook groups, review sites and the hipages help center (checked 25 Nov 2026).

| Job | Ask your AI | Make it repeat |
|---|---|---|
| Reply to new leads first | "Show me today's new hipages leads and draft a reply to each so I am the first tradie to call back. Send nothing yet." | When a lead comes in |
| Sort the leads worth paying for | "Rank this week's hipages leads by job size and suburb and flag the vague ones before I accept any." | Daily |
| Lead spend against jobs won | "Add up what I spent on hipages leads this month and how many became quotes and won jobs." | Monthly |
| Quotes with no answer | "List quotes I sent from hipages leads with no reply after 3 days and draft a follow-up text for each." | Weekly |
| Review requests | "List jobs finished this week and draft a review request for each customer. Send nothing yet." | Weekly |
| Invoices into Xero | "Check that every hipages invoice from this month is in Xero and list the ones that are missing." | Monthly |

Your AI asks you before it sends, buys, replies or changes anything. Payments stay in your hands.

## hipages API

hipages has no API, webhooks or data export for a tradie's own account; it only syncs with Xero, QuickBooks or MYOB (checked 25 Nov 2026). With Cotlet.dev you build your own hipages API: show your AI agent a job once in your signed-in hipages account, save it as an endpoint and call it from n8n, Make, Zapier or your own code. How it works: https://cotlet.dev/integrations/api/hipages

Endpoints you can build in a few minutes (none of them exists until you build it):

- `new-leads`: Time, customer, job, suburb and status of each lead
- `leads-without-quote`: Accepted leads you have not quoted yet, oldest first
- `overdue-invoices`: Invoice, customer, total and days overdue
- `week-appointments`: Time, customer, job and suburb of each visit
- `lead-spend`: Leads bought, their cost and which became won jobs
- `quotes-without-reply`: Quote, customer and days since you sent it
- `add-job-note`: A note saved on the job you name

Call one with your Cotlet API key: `POST https://cotlet.dev/api/v1/custom/new-leads`. One call runs up to 90 seconds, writes wait for your OK, payments are blocked.

## Connect in 2 minutes

Get your API key at https://cotlet.dev/app and export it as `COTLET_API_KEY`.

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

### ChatGPT

Settings → Apps → add connector URL `https://cotlet.dev/mcp/v1`, then sign in with Cotlet.dev. No API key needed.

### Cursor

Copy `.cursor/mcp.json` from this repo into your project:

```json
{
  "mcpServers": {
    "cotlet": {
      "url": "https://cotlet.dev/mcp/v1",
      "headers": {
        "Authorization": "Bearer ${COTLET_API_KEY}"
      }
    }
  }
}
```

## Run it locally (stdio)

This repo is a real MCP server. It speaks MCP over stdio, lists its hipages tools without any key, and forwards every tool call to the hosted Cotlet.dev server. The hosted URL above stays the recommended way. Use this when your client only starts local servers.

```json
{"mcpServers":{"hipages":{"command":"uvx","args":["hipages-mcp"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Or with Docker:

```bash
docker build -t hipages-mcp .
docker run -i --rm -e COTLET_API_KEY hipages-mcp
```

Tools: `hipages_guide`, `hipages_status`, `hipages_open`, `hipages_read`, `hipages_click`, `hipages_type`, `hipages_close`. Without `COTLET_API_KEY` a tool call answers: "Set COTLET_API_KEY. Get a key at https://cotlet.dev/app".

## Command line

Python 3.10+, nothing else to install. From PyPI:

```bash
pip install hipages-mcp        # or run it once: uvx hipages-mcp connect
hipages-mcp connect
```

Or straight from this repo:

```bash
export COTLET_API_KEY="your-key"
python3 hipages_agent.py connect   # ready config for Claude Code, Cursor, ChatGPT
python3 hipages_agent.py status    # is hipages ready in your Cotlet.dev account
python3 hipages_agent.py guide     # hipages screens and sign-in link
python3 hipages_agent.py read      # open hipages, read the page, close the browser
python3 hipages_agent.py tools     # list Cotlet.dev tools
python3 hipages_agent.py serve     # start the stdio MCP server (same as no argument)
```

## Questions from hipages users

**Does hipages have an API for tradies?**

hipages has no public API for tradies (checked 25 Nov 2026). Its business app only syncs invoices and customer details with Xero, QuickBooks or MYOB. Cotlet.dev works in your own signed-in hipages account, so your AI agent can read job leads, reply to customers and send quotes.

**How do I connect hipages to ChatGPT?**

Add Cotlet.dev as an app in ChatGPT (Settings → Apps, connector URL `https://cotlet.dev/mcp/v1`), sign in to hipages once in your Cotlet.dev Cloud Browser, and ask ChatGPT in plain words. No code and no hipages API key.

**Is there a way to use Claude with hipages?**

Yes. In Claude Code run the one-line `claude mcp add` command above; in the Claude app add `https://cotlet.dev/mcp/v1` as a custom connector. Claude then works in your own hipages account.

**Is there a hipages MCP server?**

hipages has no official MCP server for business accounts. Cotlet.dev is a hosted MCP server that works in your hipages account for you, 24/7, with Claude, ChatGPT, Cursor, Codex, n8n, Make and Zapier.

**Does hipages have an official ChatGPT app?**

Yes. The official hipages app in ChatGPT helps homeowners find tradies, see profiles and post jobs. It doesn't manage a business's leads or quotes. Cotlet.dev works in your own hipages account, so your AI agent can do that part.

**Can AI reply to hipages leads before other tradies do?**

hipages sends a lead to several businesses, so the first reply matters. With Cotlet.dev your AI reads new leads in your signed-in hipages for business account, drafts a reply for each and sends it after your OK.

**How do I see what I spend on hipages leads against the jobs I win?**

hipages has no export of your leads or charges (checked 25 Nov 2026). Your AI reads your leads, quotes and billing in your account and adds up spend, quotes sent and jobs won for the month.

## Find it on

- [hipages on Cotlet.dev](https://cotlet.dev/integrations/mcp/hipages?utm_source=github&utm_medium=readme&utm_campaign=hipages&utm_content=find)
- [PyPI package hipages-mcp](https://pypi.org/project/hipages-mcp/)
- [Odel](https://odel.app/modules/cotlet/hipages)
- [mcpbeat](https://mcpbeat.com/mcp-servers/cotlet/hipages/)
- [Step-by-step guide (PDF) on Internet Archive](https://archive.org/details/cotlet-connect-hipages-to-ai-agents-guide)

## Also works with

- [ServiceSeeking MCP](https://github.com/cotlet-connect/serviceseeking-mcp)
- [Mangomint MCP](https://github.com/cotlet-connect/mangomint-mcp)
- [Wanderlog MCP](https://github.com/cotlet-connect/wanderlog-mcp)
- [SmugMug MCP](https://github.com/cotlet-connect/smugmug-mcp)
- [Garmin Connect MCP](https://github.com/cotlet-connect/garmin-mcp)
- [Housecall Pro MCP](https://github.com/cotlet-connect/housecall-pro-mcp)
- [Medium MCP](https://github.com/cotlet-connect/medium-mcp)
- [Crexi MCP](https://github.com/cotlet-connect/crexi-mcp)
- [DistroKid MCP](https://github.com/cotlet-connect/distrokid-mcp)
- [LinkedIn MCP](https://github.com/cotlet-connect/linkedin-mcp)
- [EstimateOne MCP](https://github.com/cotlet-connect/estimateone-mcp)
- [Google Search Console MCP](https://github.com/cotlet-connect/google-search-console-mcp)
- [Lichess MCP](https://github.com/cotlet-connect/lichess-mcp)
- [Strava MCP](https://github.com/cotlet-connect/strava-mcp)
- [StreamElements MCP](https://github.com/cotlet-connect/streamelements-mcp)
- [Meetup MCP](https://github.com/cotlet-connect/meetup-mcp)
- [AccuRanker MCP](https://github.com/cotlet-connect/accuranker-mcp)
- [GetYourGuide MCP](https://github.com/cotlet-connect/getyourguide-mcp)
- [Idealista MCP](https://github.com/cotlet-connect/idealista-mcp)
- [Riverside MCP](https://github.com/cotlet-connect/riverside-mcp)
- [Udemy MCP](https://github.com/cotlet-connect/udemy-mcp)
- [Faire MCP](https://github.com/cotlet-connect/faire-mcp)
- [Teachable MCP](https://github.com/cotlet-connect/teachable-mcp)
- [Vagaro MCP](https://github.com/cotlet-connect/vagaro-mcp)
- [Buildertrend MCP](https://github.com/cotlet-connect/buildertrend-mcp)
- [Zoopla MCP](https://github.com/cotlet-connect/zoopla-mcp)
- [Dribbble MCP](https://github.com/cotlet-connect/dribbble-mcp)
- [Hostelworld MCP](https://github.com/cotlet-connect/hostelworld-mcp)
- [Mashvisor MCP](https://github.com/cotlet-connect/mashvisor-mcp)
- [Rent Manager MCP](https://github.com/cotlet-connect/rent-manager-mcp)
- [ThriveCart MCP](https://github.com/cotlet-connect/thrivecart-mcp)
- [Fresha MCP](https://github.com/cotlet-connect/fresha-mcp)
- [Booksy MCP](https://github.com/cotlet-connect/booksy-mcp)
- [BookingKoala MCP](https://github.com/cotlet-connect/bookingkoala-mcp)
- [Milanote MCP](https://github.com/cotlet-connect/milanote-mcp)
- [Partiful MCP](https://github.com/cotlet-connect/partiful-mcp)
- [Contractor Foreman MCP](https://github.com/cotlet-connect/contractor-foreman-mcp)
- [Pixieset MCP](https://github.com/cotlet-connect/pixieset-mcp)
- [GlossGenius MCP](https://github.com/cotlet-connect/glossgenius-mcp)
- [ZenMaid MCP](https://github.com/cotlet-connect/zenmaid-mcp)
- [AroFlo MCP](https://github.com/cotlet-connect/aroflo-mcp)
- [Simpro MCP](https://github.com/cotlet-connect/simpro-mcp)
- [Workiz MCP](https://github.com/cotlet-connect/workiz-mcp)
- [Kickserv MCP](https://github.com/cotlet-connect/kickserv-mcp)
- [Service Autopilot MCP](https://github.com/cotlet-connect/service-autopilot-mcp)
- [Mindbody MCP](https://github.com/cotlet-connect/mindbody-mcp)
- [DaySmart MCP](https://github.com/cotlet-connect/daysmart-mcp)
- [Acuity Scheduling MCP](https://github.com/cotlet-connect/acuity-scheduling-mcp)
- [Jane App MCP](https://github.com/cotlet-connect/jane-app-mcp)
- [FieldEdge MCP](https://github.com/cotlet-connect/fieldedge-mcp)
- [Phorest MCP](https://github.com/cotlet-connect/phorest-mcp)
- [DINGG MCP](https://github.com/cotlet-connect/dingg-mcp)
- [Invoice Simple MCP](https://github.com/cotlet-connect/invoice-simple-mcp)
- [PhotoShelter MCP](https://github.com/cotlet-connect/photoshelter-mcp)
- [RazorSync MCP](https://github.com/cotlet-connect/razorsync-mcp)
- [ShootProof MCP](https://github.com/cotlet-connect/shootproof-mcp)
- [Invoice2go MCP](https://github.com/cotlet-connect/invoice2go-mcp)
- [Appointy MCP](https://github.com/cotlet-connect/appointy-mcp)
- [Contractor+ MCP](https://github.com/cotlet-connect/contractor-plus-mcp)
- [Petpooja MCP](https://github.com/cotlet-connect/petpooja-mcp)
- [Teachers Pay Teachers MCP](https://github.com/cotlet-connect/teachers-pay-teachers-mcp)
- [Bloom MCP](https://github.com/cotlet-connect/bloom-mcp)
- [MyJobQuote MCP](https://github.com/cotlet-connect/myjobquote-mcp)
- [MyBuilder MCP](https://github.com/cotlet-connect/mybuilder-mcp)
- [Yelp for Business MCP](https://github.com/cotlet-connect/yelp-for-business-mcp)
- [Rosie MCP](https://github.com/cotlet-connect/rosie-mcp)
- [Roofr MCP](https://github.com/cotlet-connect/roofr-mcp)
- [FieldPulse MCP](https://github.com/cotlet-connect/fieldpulse-mcp)
- [YourTradebase MCP](https://github.com/cotlet-connect/yourtradebase-mcp)
- [JobNimbus MCP](https://github.com/cotlet-connect/jobnimbus-mcp)
- [Ascora MCP](https://github.com/cotlet-connect/ascora-mcp)
- [eLocal MCP](https://github.com/cotlet-connect/elocal-mcp)
- [All apps connected through Cotlet.dev](https://github.com/cotlet-connect)

---

More: https://cotlet.dev/integrations/mcp/hipages?utm_source=github&utm_medium=readme&utm_campaign=hipages

Start free: https://cotlet.dev/signup?app=hipages&utm_source=github&utm_medium=readme&utm_campaign=hipages
