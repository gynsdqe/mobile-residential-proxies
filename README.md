# mobile vs residential proxies: how to choose by target, budget and ban rate, and what residential bandwidth really costs

Most people searching this question are not shopping for a specific provider yet. They are trying to work out how much money they have to spend to stop getting blocked. That is the useful way to frame it, because the technical difference between mobile and residential proxies is smaller than the price difference. Both route your traffic through someone's real internet connection. Both come from devices owned by actual people. The gap is which network those devices sit on, how much you pay per gigabyte, and how much protection that buys you.

Short version: residential covers the large majority of jobs at a fraction of the cost. Mobile earns its premium on a narrow set of targets that specifically filter for carrier networks. Buying mobile for everything is the most common way to waste budget on a scraping project.

## The two-minute answer

| Question | Residential | Mobile |
| --- | --- | --- |
| Where the IPs come from | Home broadband ISPs | Mobile carriers (3G/4G/5G) |
| What a site sees | An ordinary household connection | A carrier ASN, usually behind carrier-grade NAT |
| Typical price model | Per GB, or per IP with unlimited traffic | Per GB, and a higher rate |
| Ban resistance | High, but shared IPs get burned over time | Highest, because blocking one IP can cut off thousands of subscribers |
| Geo-targeting | Finer: continent, country, city, often ISP or ZIP level | Coarser, carrier gateways may exit in a different city than the device |
| Speed | Usually steadier, dependent on one home's upstream | Varies with signal, cell congestion and tower handoffs |
| Best for | Scraping at volume, price monitoring, SERP tracking, ad verification on desktop | Account management, carrier-gated pricing, mobile app testing |

If your work is scraping, monitoring or research, residential is the default and mobile is the exception you add later. If your work is running accounts on platforms that treat cellular traffic as more trustworthy, or you need to see what a page serves to a specific carrier in a specific region, mobile is the point of the exercise.

## What a site actually reads from your IP

Before a page loads a single line of JavaScript, the server has already made a few cheap decisions about your request. It has looked up the ASN, the owner of the autonomous system your IP belongs to. It has checked the reverse DNS, which for server ranges tends to resolve to machine-generated hostnames. It has counted how many unrelated sessions left through that address in the last minute. And it has pulled whatever reputation history that IP range carries from previous automation.

None of those checks involve your browser. That is why the proxy class matters on its own, and why a perfectly coherent browser profile will not rescue a datacenter IP on a strict target.

Residential exits read as consumer broadband: ISP ASN, subscriber-style reverse DNS, no obvious hosting signature. Mobile exits go further. Carriers put a large number of real subscribers behind a small pool of public addresses through carrier-grade NAT, and the addresses rotate on the carrier's own schedule as devices move between towers. That churn is normal subscriber traffic, not a tell. And an anti-bot system that blocks a mobile IP takes a real risk, because the same address is legitimately used by thousands of paying customers in that region.

That single asymmetry explains most of the price gap.

## Cost per gigabyte is where the decision actually lands

Residential bandwidth has become a commodity. Budget providers now sell in the $1 to $2 per GB range, mid-market sits around $3 to $5, and premium networks with bigger pools and unblocker tooling run higher. Mobile capacity is genuinely scarcer because the provider is paying for SIM cards, data plans and hardware whether you use them or not.

The historical numbers are worth knowing because they show where the market went. Mobile traffic used to be priced around $40/GB at its peak and has since fallen into single digits on several networks. Even so, mobile usually still sits above residential in any given provider's catalogue, and it is billed on the same data-based model, so a 100 GB crawl that costs you a couple of hundred dollars on residential can cost several times that on mobile.

The practical way to think about it is cost per accepted record, not cost per GB:


proxy traffic + browser runtime + parsing work + rejected requests


A cheap residential route that gets blocked constantly and forces retries is expensive. A mobile route that returns clean results on the first attempt can be cheaper per usable row even at three times the bandwidth rate. Where teams go wrong is applying the premium route to the whole crawl instead of the handful of endpoints that genuinely need it.

## The speed argument is less settled than vendors claim

You will find providers arguing both directions. Some position cellular as faster, pointing at 5G latency and carrier-managed traffic that does not compete with a household's peak-hour congestion. Others point out that mobile latency depends on radio conditions, signal strength, cell congestion and tower handoffs, and that no provider can control those.

Both are describing something real. Residential speed depends on one home's upstream connection, which is usually fine and occasionally terrible. Mobile speed depends on the radio environment, which can be excellent on a well-served 5G cell and painful on a congested one. If your task is latency-sensitive and you cannot tolerate variance, residential or a static ISP proxy will disappoint you less often than mobile.

For bulk crawling, throughput usually matters more than raw latency anyway, and that favours residential pools, which can handle more parallel requests without the extra network variability.

## Sessions: rotation is for crawling, stickiness is for logins

Two different jobs, two different requirements.

Crawling wants rotation so no single IP accumulates enough request velocity to look suspicious. Logging in wants stickiness, because an account that changes city mid-session tends to get flagged. Mobile sticky windows tend to run shorter because the carrier can reassign your address without warning. Residential gives you both modes, and static ISP proxies go further by holding one address indefinitely.

One thing that trips people up: a sticky IP does not preserve your cookies. Session continuity comes from your client keeping the same browser context, not from the proxy holding the same address.

## Geo-targeting is the quiet dealbreaker

Residential addresses map to fixed locations, so pools can be targeted at continent, country, city, and often down to ASN or ZIP level. Mobile is looser, because carrier traffic exits through regional gateways. A phone sitting in one town can end up with an IP that geolocates somewhere else entirely. What you get instead with mobile is carrier-level targeting: which network, which region.

If your deliverable is "top Google results for a London postcode", mobile is the wrong tool. If your deliverable is "does this campaign render correctly for an O2 subscriber in Manchester", mobile is the only tool.

## Choosing by use case

| Task | Start with | Switch only when |
| --- | --- | --- |
| High-volume scraping | Rotating residential | Your test sample shows the target filters carrier ASNs |
| Price monitoring by region | Residential, pinned to country or city | Pricing is gated behind a mobile-only surface |
| SEO and SERP tracking | Residential with fixed geography | The search experience you are studying is carrier-specific |
| Ad verification | Residential by market | The campaign is mobile-only |
| Multi-account management | Mobile or dedicated ISP | Ban rates on residential prove unacceptable |
| Mobile app and API testing | Mobile | Never, this is the requirement |
| Long multi-page sessions | Sticky residential or static ISP | A carrier-origin session is an explicit product requirement |

The pattern: residential until evidence justifies the upgrade. Run the same job through both stacks, compare success rate and cost per accepted request, and route by task from there.

## Where 9Proxy fits into this decision

9Proxy is a residential proxy network, not a mobile one. That makes it relevant to the residential half of this comparison, and it is worth being straight about the limitation: if your target genuinely requires carrier IPs, no amount of residential quality will substitute. You will need a mobile provider, and 9Proxy does not sell that.

What it does sell is residential access, billed in two different ways, with a pool the vendor advertises at 20 million-plus IPs across 90-plus countries and HTTP/HTTPS plus SOCKS5 support.

The pricing model is the interesting part, because it is a one-off balance top-up rather than a monthly subscription. There is no recurring charge, and unused IPs from IP-based packages do not expire. GB-based packages carry a 180-day validity window, with enterprise tiers removing the expiry entirely.

That structure suits uneven workloads. If you scrape hard for two weeks and then go quiet for a month, you are not paying for an idle subscription.

### The full current lineup

The vendor raised prices on IP-based and bundle packages on 1 June 2026. GB-based pricing was left untouched in that change. Here is the complete current structure.

**Residential by IP** (pay per IP, unlimited bandwidth per IP, unused IPs never expire)

| Package | Per IP | Total | Purchase |
| --- | --- | --- | --- |
| 100 IPs | $0.24 | $24 | Get 100 IPs |
| 500 IPs | $0.14 | $72 | Get 500 IPs |
| 1,000 IPs + 500 bonus | $0.084 | $126 | Get 1,500 IPs |
| 2,500 IPs | $0.084 | $210 | Get 2,500 IPs |
| 5,000 IPs | $0.072 | $360 | Get 5,000 IPs |
| 15,000 IPs | $0.048 | $720 | Get 15,000 IPs |
| 25,000 IPs | $0.035 | $863 | Get 25,000 IPs |
| 50,000 IPs | $0.029 | $1,438 | Get 50,000 IPs |
| 100,000 IPs | $0.023 | $2,300 | Get 100,000 IPs |
| 200,000 IPs | $0.021 | $4,140 | Get 200,000 IPs |
| 500,000 IPs | $0.018 | $8,625 | Get 500,000 IPs |

**Residential by GB** (180-day validity)

| Package | Per GB | Total | Purchase |
| --- | --- | --- | --- |
| 5 GB | $3.00 | $15 | Get 5 GB |
| 50 GB + 5 GB bonus | $2.10 | $105 | Get 55 GB |
| 100 GB | $1.50 | $150 | Get 100 GB |
| 200 GB | $1.00 | $200 | Get 200 GB |
| 1,000 GB | $0.80 | $800 | Get 1,000 GB |
| 2,000 GB | $0.75 | $1,500 | Get 2,000 GB |

**Enterprise GB** (no expiry)

| Package | Per GB | Total | Purchase |
| --- | --- | --- | --- |
| 3,000 GB | $0.72 | $2,160 | Get 3,000 GB |
| 6,000 GB | $0.70 | $4,200 | Get 6,000 GB |
| 10,000 GB | $0.68 | $6,800 | Get 10,000 GB |

**Bundles** (IPs plus bandwidth in one top-up)

| Bundle | Total | Purchase |
| --- | --- | --- |
| 100 IPs + 5 GB | $30 | Get the Starter bundle |
| 1,500 IPs + 50 GB | $180 | Get the Popular bundle |
| 5,000 IPs + 500 GB | $720 | Get the Pro bundle |

Two things stand out in that table. The 1,000 IP tier quietly ships with 500 bonus IPs, which puts it at the same effective rate as the 2,500 IP tier, and it is the cheapest entry into the sub-$0.10 range. And the bundle tiers are priced below what the same IPs and GB would cost separately, which matters if you are running mixed workloads where some sessions need to hold and others need to rotate.

### How the residential product actually runs

9Proxy splits residential into two models, and they are set up differently.

IP-based packages run through the 9Proxy desktop app, which handles local port forwarding. Each IP stays live for a few hours up to roughly 24 hours, and each forwarded IP counts as one use. There is no natural rotation, though the platform supports an auto-rotation proxy that changes exits at intervals you set on selected ports. Bandwidth during that active window is unlimited.

GB-based packages generate endpoints directly from the dashboard with unlimited generations, authenticated by username and password or an IP whitelist. Rotation switches the exit automatically per request; sticky mode holds until the session time you configure runs out. This is the model that fits bulk crawling and geo-distributed requests.

If you work from a laptop and want a handful of stable IPs for account work, the IP model is the one you want. If you are running automation at volume, the GB model is cheaper to operate. The bundle tiers exist because plenty of people end up needing both. You can 👉 start a 9Proxy account here and the invite code in the link is applied at signup.

### What to check before you pay

Two caveats worth knowing going in, both of which come from the vendor's own documentation and public support responses.

The service runs through a dedicated application rather than handing you a plain IP:port list. That is a deliberate design choice, and it is also the single most common source of complaints from buyers who expected to paste a list into a third-party tool. If your workflow requires raw IP credentials, check how the app integrates with your stack first.

Second, granular targeting is a known friction point. 9Proxy advertises targeting down to country, city, ZIP and ISP level, and its third-party review scores sit around 3.9 out of 5 in proxy directories. On Trustpilot, where the brand currently sits at 2 out of 5, reviewers have reported city selections resolving to a different city and slower SOCKS speeds. The vendor's public replies point users to its refund policy for proxies that are completely non-functional and to a reuse feature for IPs from the previous 24 hours. Read that policy at checkout rather than after.

That is not a reason to walk away. It is a reason to test on your own targets with the smallest package before committing to a 50,000 IP tier. Budget residential networks in the $1 to $2 per GB band are legitimate and perfectly capable on tier-one and tier-two targets; they are not built for the hardest, most aggressively defended endpoints, and no pricing page will tell you which category your target falls into.

## Matching the answer back to your target

Run the decision in this order. Identify what your target actually filters on. If it does not care about ASN, cheap residential handles it. If it blocks datacenter ranges but nothing more, residential still handles it. If it specifically distrusts non-cellular traffic, or your work involves carrier-gated pricing, mobile-only surfaces or multi-account behaviour on mobile-first platforms, then mobile is a real requirement and you should budget for it.

Then look at volume. Mobile capacity is limited and priced accordingly, so keep it pointed at the endpoints that need it. Residential takes everything else, and a pay-per-IP plan with unlimited bandwidth is often the cheaper structure for steady session work while a GB plan wins on high-rotation scraping.

The mistake to avoid is paying mobile rates across a whole crawl because one difficult template forced your hand. Route by task, measure accepted records rather than raw requests, and the cheapest provider turns out to be the one that stops you from retrying.
