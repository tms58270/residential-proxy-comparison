# residential proxy provider: how to pick one without overpaying for traffic you can't actually use

If you're comparing residential proxy providers, the short answer is: most lists will sell you raw IP pools by the gigabyte, but that's only half the decision. Before you pick a provider, you need to know how you'll actually consume those IPs — through a proxy endpoint you manage yourself, or through a scraping API that handles rotation, CAPTCHAs and browser rendering for you. The two routes bill in completely different units (per GB vs. per request), and picking the wrong billing model is the single most expensive mistake in this market.

This article covers both. It compares the established residential proxy providers on verified pricing, then walks through ScraperAPI — a provider that packages residential IPs behind an API — with its full plan list, real per-request costs, and the credit multipliers that most reviews never mention.

> **Quick take:** If your team writes scrapers and you just want to swap an API key for your proxy plumbing, ScraperAPI's Hobby plan at **$49/month** (100,000 credits, 20 concurrent threads) is the realistic starting point, and the free tier gives you 1,000 credits to test with. If you need to route arbitrary traffic through residential IPs yourself, a per-GB provider like Bright Data (from ~$2.50/GB) or IPRoyal (from ~$1.75/GB) fits better. 👉 [See ScraperAPI's full plan list and current pricing](https://www.scraperapi.com/pricing?fp_ref=coupons)

## What a residential proxy provider actually does

A residential proxy provider routes your requests through real home or mobile IPs instead of datacenter addresses. Because the traffic exits from an IP that belongs to an ordinary consumer connection, target sites treat it like a regular visitor — which is why residential proxies are the default choice for price monitoring, SERP tracking, ad verification and market research. With automated traffic making up 51% of all web requests in 2024 according to the Imperva Bad Bot Report, anti-bot defenses are aggressive enough that datacenter IPs alone rarely survive.

Two things vary enormously between providers, and they matter more than raw IP counts:

1. **How you access the IPs.** Some providers hand you proxy endpoints (host, port, username, password) and you manage sessions, rotation and retries in your own code. Others — ScraperAPI is the clearest example — hide the proxy layer behind a single API call and also handle JavaScript rendering, CAPTCHA solving and automatic retries.
2. **How you pay.** Per-GB billing charges for bandwidth whether or not a request succeeds. Per-request (credit) billing charges only for successful responses, but the cost per request can vary wildly by target site — more on that below, because it's where the real surprises live.

## ScraperAPI's full plan lineup

ScraperAPI runs on a credit system: one standard page costs 1 credit, and every paid plan includes unlimited bandwidth, JS rendering, premium proxy access, CAPTCHA handling and automatic retries. Here's everything currently shown on their pricing page:

| Plan | Monthly price | Annual (per month) | Credits / month | Concurrent threads | Subscribe |
| --- | --- | --- | --- | --- | --- |
| Hobby | $49 | $44.10 | 100,000 | 20 | [ View Hobby plan pricing](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Startup | $149 | $134.10 | 1,000,000 | 50 | [ View Startup plan pricing](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Business | $299 | $269.10 | 3,000,000 | 100 | [ View Business plan pricing](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Scaling | $475 | $427.50 | 5,000,000 | 200 | [ View Scaling plan pricing](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Professional | $975 | $877.50 | 10,500,000 | 300 | [ View Professional plan pricing](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Advanced | $1,975 | $1,777.50 | 21,500,000 | 500 | [ View Advanced plan pricing](https://www.scraperapi.com/pricing?fp_ref=coupons) |
| Enterprise | Custom | Custom | 22,000,000+ | 500+ | [ Talk to the enterprise team](https://www.scraperapi.com/contact-sales/?fp_ref=coupons) |

Annual billing takes 10% off every plan. There's also a free tier: signing up gets you 1,000 free credits with up to 5 concurrent connections, and a 7-day trial bumps that to 5,000 credits without asking for a credit card. For perspective on what a credit buys: Hobby's 100,000 credits at $49 works out to roughly $0.49 per thousand standard pages — about half a cent per page at volume, if you're scraping targets that cost 1 credit. That per-request number will matter when we get to the multipliers.

## The credit multipliers nobody puts in their comparison tables

This is the part most reviews skip, and it directly changes which plan you should buy.

ScraperAPI's standard pool already includes millions of proxies from over a dozen ISPs, and for most sites those 1-credit requests are all you'll pay. But three things multiply your cost per request:

- **Target-site pricing.** A standard page costs 1 credit. Amazon costs 5. Google and Bing (including subdomains) cost 25. LinkedIn costs 30. So a Hobby plan's 100,000 credits is either ~100,000 ordinary pages or ~4,000 Google searches — a 25x difference that depends entirely on what you're scraping.
- **The premium residential/mobile pool.** For hard-to-scrape targets, ScraperAPI maintains a private pool of residential and mobile IPs, open to all paid users. Requests through it (`premium=true`) bill at **10x** the normal rate — 10 credits per successful request, or 25 credits if you also enable JavaScript rendering.
- **The ultra-premium pool.** For really tough targets like LinkedIn, `ultra_premium=true` costs **30 credits** per request, or **75** with rendering. This level is paid-plans only, requests through it are cached by default, and note that custom headers don't survive `ultra_premium=true` — even with `keep_headers=true`, custom headers get dropped.

Two practical consequences. First, if your whole project is Google SERP scraping, do the math before choosing a plan: Startup's 1M credits is about 40,000 Google requests at 25 credits each, not a million. Second, use the `max_cost` parameter per scrape and the Domain Cost Estimator in the dashboard — both let you check the exact credit cost of any URL before you burn through your monthly quota. This is genuinely useful tooling that raw per-GB proxy providers don't offer, because they don't know or care what's on the other end of your request.

## How ScraperAPI compares to traditional residential proxy providers

Here's where honesty matters: ScraperAPI doesn't hand you raw residential IP pools the way classic providers do. You can't configure a proxy endpoint in your own tooling — access is API-only. That's a dealbreaker for some setups and a relief for others. The comparison looks like this (verified prices from current provider pages, per GB for rotating residential):

| Provider | Entry price | IP pool | Notable traits |
| --- | --- | --- | --- |
| Bright Data | from ~$2.50/GB (PAYG $4/GB) | 400M+, ~195 countries | Largest network, Web Unlocker tooling, steepest learning curve |
| Oxylabs | ~$2.00–$6.00/GB tiered | 175M+, 195+ countries | Enterprise SLAs, 0.41s average response, 99.95% success rate |
| Decodo (ex-Smartproxy) | ~$2.00/GB | 115M+ | Easiest onboarding, 3-day free trial |
| IPRoyal | ~$1.75/GB | 64M+, ~27 countries | Pay-as-you-go, traffic doesn't expire |
| SOAX | ~$1.50–$3.00/GB | 155M+ | City/ISP-level targeting |
| Webshare | ~$1.40/GB | 80M+ | Free tier (10 proxies, 1GB/month), cheapest entry |
| DataImpulse | ~$0.80–$1/GB | 90M+ | Lowest raw per-GB price |
| **ScraperAPI** | **$49/mo for 100k requests** | **40M+ IPs, 50+ countries (residential + datacenter + mobile)** | **Per-request billing, CAPTCHA and rendering built in, API-only access** |

The honest read: if you need raw IP access — custom proxy tooling, non-HTTP protocols, session control your code already handles — a per-GB provider is the right shape, and IPRoyal or Webshare are the budget-friendly entries while Bright Data and Oxylabs are the enterprise-grade ones. If your goal is data extraction (get me Amazon product data, SERP results, Walmart listings) and you'd rather not maintain proxy rotation logic at all, ScraperAPI's model removes a whole category of work. Independent testing context is worth knowing here too: Proxyway's 2026 market research found residential proxies succeed 95–99% against unprotected endpoints but drop to a 74% median against protected targets — meaning the anti-bot layer bundled into ScraperAPI's API isn't decoration; it's the part that does the heavy lifting on hard sites.

Worth flagging as well: this market has had real supply-chain problems. In July 2026, the FBI and Google jointly disrupted NetNut's residential network after finding much of its ~2-million-device pool was built on compromised consumer devices. Provider sourcing and compliance statements are worth checking on any provider you shortlist — ScraperAPI states full CCPA and GDPR compliance on its site.

## Which ScraperAPI plan fits which scenario

The plan ladder is mostly about volume and concurrency, with two features that split at specific tiers:

- **Geotargeting.** Hobby and Startup are both limited to US and EU targeting. From Business up ($299/month), you get country-level geotargeting across the full 50+ country footprint. If your targets sit outside the US/EU, no amount of extra credits on the lower tiers will fix that — Business is the actual minimum.
- **Analytics retention.** Hobby and Startup only show the last 30 days of stats. Business and above keep unlimited history.
- **Overage options split across tiers.** Despite appearing in every plan's feature list, **pay-as-you-go** billing (keep scraping past your limit at a fixed per-credit rate, capped by a monthly spend limit you set) is actually only available on **Scaling, Professional, Advanced and Enterprise**. On the lower tiers, the equivalent safety net is **auto-upgrade** — an opt-in feature available on Hobby, Startup, Business, Scaling and Professional that automatically moves your account to the next plan when credits run out. The two options overlap on Scaling and Professional; Advanced gets pay-as-you-go only, so if you want the spending-cap model at the highest tiers, that's where it lives.

So the decision rules are fairly clean: testing or side projects → the free tier, then Hobby if it sticks. Regular scraping up to ~1M standard pages per month → Startup. Production workloads needing country targeting outside US/EU → Business at minimum. High-volume pipelines where running dry mid-month would hurt → Scaling or above, where pay-as-you-go gives you a spending-capped safety net. If you're choosing between Business ($299, 3M credits) and Scaling ($475, 5M credits), the extra $176 buys 2M credits plus the overage option — at roughly $0.000099 vs $0.000095 per standard request, the per-credit economics are nearly identical, so the real question is whether you need 200 concurrent threads.

Annual billing saves 10% across the board — $58.80/year on Hobby, up to $2,370/year on Advanced. 👉 [Check the annual billing prices on ScraperAPI's pricing page](https://www.scraperapi.com/pricing?fp_ref=coupons)

## Common questions

**Can I test residential proxies before paying?**
Yes. The free plan includes 1,000 credits per month, and adding `premium=true` routes requests through the residential/mobile pool on paid plans — with the 7-day trial giving you 5,000 credits, enough to test against your actual target sites rather than a demo page. 👉 [Start the free 7-day trial (5,000 credits, no credit card)](https://www.scraperapi.com/signup?fp_ref=coupons)

**What happens if I exceed my plan's concurrent thread limit?**
You get a 429 response and the request is turned away rather than queued — no extra charge, but no queuing either. Size your concurrency to the plan, or move up a tier.

**Does ScraperAPI work against Cloudflare and DataDome?**
It's built to bypass them, but successful bypasses add 10 credits per request. Third-party reviews note that the most aggressive anti-bot systems can still cause blocks, so testing on your specific targets during the trial is the only reliable way to know your real cost per page.

**Is there a money-back guarantee?**
No published refund policy could be verified, so treat the 7-day free trial (5,000 credits, no credit card required) as the risk-free evaluation window. Since no card is needed to trial, there's no charge to worry about until you actively pick a plan.

## Before you commit

Run this quick checklist against your own project: if you're scraping fewer than ~50,000 standard pages a month and your targets aren't on the premium-cost domain list, the free tier and Hobby plan are probably all you'll ever need — don't talk yourself into Startup. If more than a quarter of your requests hit Google, Bing, or LinkedIn, re-price your project in multiplied credits before picking a tier, because the plan that "fits your volume" on standard pages will run out four to thirty times faster on those domains. Check whether your targets fall outside US/EU geotargeting before committing to Hobby or Startup — country-level targeting starts at Business. And if what you actually wanted was a raw residential IP pool to plug into your own proxy manager, skip the API-bundled providers entirely and go with a per-GB supplier instead.

For everyone in the first camp, 👉 [start ScraperAPI's free 7-day trial with 5,000 credits](https://www.scraperapi.com/signup?fp_ref=coupons) and test your real targets before spending a dollar.
