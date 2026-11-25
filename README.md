# ScraperAPI Geotargeting: How Do You Set country_code, Which Plans Unlock Germany or Japan, and Is the $299 Business Upgrade Worth It? (Full Plan and Credit Breakdown)

ScraperAPI geotargeting is one parameter. Add `country_code=de` (or any two-letter ISO code) to a request and ScraperAPI routes it through a proxy in that country, so amazon.de returns the German catalog and German prices instead of the US version.

The catch is plan gating. Hobby ($49/mo) and Startup ($149/mo) accept only `us` and `eu`, and `eu` is a general European pool, not a country. Country-level targeting starts at Business: $299/mo, or $269.10/mo on annual billing, with 3 million credits and 100 concurrent threads. That single fact decides most purchase decisions, so it belongs at the top.

What follows covers how the parameter works in API and proxy mode, which countries each plan unlocks, what a geotargeted request really costs in credits, and the places where the feature stops helping.

> **Quick pick**
> - Scraping US sites, or Europe as a region → **Hobby, $49/mo** ($44.10 annual), 100,000 credits.
> - Need Germany, Japan, Brazil, or any other named country → **Business, $299/mo** ($269.10 annual), 3M credits, 69 countries.
> - Need a rare market or residential IPs → **Business or above with `premium=true`**, 150+ countries, +10 credits per request.
>
> 👉 [Compare ScraperAPI plans and current pricing](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## All ScraperAPI plans at a glance

| Plan | Price (monthly / annual per month) | Credits per month | Concurrent threads | Geotargeting | Purchase |
| --- | --- | --- | --- | --- | --- |
| Free Trial | $0 for 7 days | 5,000 (one-time) | 5 | Check the trial page | [ Start the free trial](https://www.scraperapi.com/?fp_ref=coupons) |
| Hobby | $49 / $44.10 | 100,000 | 20 | US & EU only | [ Get the Hobby plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Startup | $149 / $134.10 | 1,000,000 | 50 | US & EU only | [ Get the Startup plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Business | $299 / $269.10 | 3,000,000 | 100 | Country-level; Premium Geo with `premium=true` | [ Get the Business plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Scaling | $475 / $427.50 | 5,000,000 | 200 | Country-level; Premium Geo; pay-as-you-go overage | [ Get the Scaling plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Professional | $975 / $877.50 | 10,500,000 | 300 | Country-level; Premium Geo; overage | [ Get the Professional plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Advanced | $1,975 / $1,777.50 | 21,500,000 | 500 | Country-level; Premium Geo; overage | [ Get the Advanced plan](https://www.scraperapi.com/pricing/?fp_ref=coupons) |
| Enterprise | Custom | 22M+ | 500+ | Country-level plus countries on request | [ Talk to sales about Enterprise](https://www.scraperapi.com/?fp_ref=coupons) |

Every paid plan drops 10% on annual billing. Professional and Advanced are the two Growth tiers launched in May 2026, and both currently carry a limited-time signup bonus (250K extra credits on Professional, 500K on Advanced) that should not be treated as permanent pricing.

## How country_code works, in the API and in proxy mode

Set `country_code` to the ISO 3166-1 alpha-2 code for the country you want, and ScraperAPI handles proxy selection and rotation. You never touch a proxy list.


https://api.scraperapi.com/?api_key=YOUR_KEY&url=https://www.amazon.de/dp/XXXX&country_code=de


If you already route traffic through ScraperAPI as a proxy rather than calling the API directly, the country goes into the proxy username:


scraperapi.country_code=de:YOUR_API_KEY@proxy-server.scraperapi.com:8001


Async batch jobs accept `country_code` as well, so a multi-market price sweep can mix `de`, `jp`, and `br` in the same submission. One setting per request, standard two-letter codes (`us`, `uk`, `de`, `jp`, `br`, `in`), and ScraperAPI's own failover kicks in if the first IP is blocked: a failed geotargeted request escalates to a residential IP from the same region automatically, without you changing a parameter.

## Which countries each plan actually unlocks

The official documentation is blunt about the tiers. Hobby and Startup get `us` and `eu`. Everything else needs Business or Enterprise, which covers 69 countries as of the current docs, including Germany, the UK, France, Japan, Brazil, India, Australia, Mexico, Singapore, South Korea, the UAE, Saudi Arabia, and South Africa. Enterprise customers can request countries outside that list through support.

The `eu` code deserves emphasis because it's the most common misunderstanding on cheap plans. It is a general European proxy pool, not a way to look German or French. If a retailer shows different pricing per EU country, `eu` will not reliably give you the German storefront.

Above country-level sits Premium Geo. Add `premium=true` alongside `country_code` and requests go through residential and mobile pools covering more than 150 countries, including markets like Sri Lanka, Bolivia, Cambodia, Kazakhstan, Mongolia, Nepal, and Uruguay. The flag costs +10 credits per request. ScraperAPI positions this at Business and above.

Two hard limits:

- **No city-level targeting.** You cannot ask for Chicago or inner London. The single exception is ZIP code targeting on the Amazon US domain, documented separately.
- **Country routing is not a guarantee of a local IP.** The official FAQ says requests are routed to the target country's pool, but a 100% country match isn't promised, and success rates wobble in less common markets.

## What a geotargeted request costs in credits

`country_code` itself adds nothing. Credits come from what you stack on top of it.

| Request type | Credits |
| --- | --- |
| Standard page | 1 |
| Amazon | 5 |
| Google / Bing SERP | 25 |
| LinkedIn | 30 |
| Cloudflare / DataDome protected site | +10 |
| `premium=true` | +10 |
| `render=true` | +10 |
| `ultra_premium=true` (mobile IPs) | +30 |
| `premium=true` + `render=true` | 25 total |
| `ultra_premium=true` + `render=true` | 75 total |

Take the case most price-monitoring teams run: `country_code=de` with `render=true` against amazon.de. That's 5 credits for Amazon plus 10 for rendering, so **15 credits per successful request**. Hobby's 100,000 credits buy roughly 6,600 of those, not 100,000. On Business, 3 million credits buy about 200,000.

The per-credit price gap is where Business justifies itself for volume work. Business works out to roughly $0.10 per 1,000 credits; Hobby is roughly $0.49 per 1,000. On that same 15-credit request, you're paying around $0.0074 on Hobby versus $0.0015 on Business.

SERP scraping is the other trap. A Business plan sounds like 3 million requests. At 25 credits per Google SERP call, it's about 120,000 SERP pulls per month. Ten countries means a tenth of that per market.

Before committing, price your actual URLs. ScraperAPI exposes a cost endpoint:


https://api.scraperapi.com/account/urlcost?api_key=YOUR_KEY&url=YOUR_TARGET_URL&render=true


Every response also carries an `sa-credit-cost` header showing what the request really deducted. Run three or four of your real targets through it during the trial and multiply out; that number, not the credit headline, is your monthly bill.

👉 [See Business plan pricing and credit allowances](https://www.scraperapi.com/pricing/?fp_ref=coupons)

## The billing rule that catches people below Scaling

Hobby, Startup, and Business have no pay-as-you-go overflow. When credits run out mid-cycle, scraping stops until renewal or an upgrade. Credits also don't roll over; unused credits expire at renewal.

There is a middle path the pricing page mentions but the plan table doesn't: auto-upgrade is opt-in on Hobby, Startup, Business, Scaling, and Professional. Turn it on and running dry bumps you to the next tier instead of locking you out. True pay-as-you-go at a fixed per-credit rate, capped by a monthly limit you set, starts at Scaling ($475/mo) and continues through Professional, Advanced, and Enterprise.

If your pipeline can't tolerate a mid-month stop and you don't want an automatic jump to the next tier's bill, Scaling is the honest entry point, not Business. Concurrency works the same way: exceed your thread limit and you get a 429, not a surcharge.

## Where geotargeting stops helping

Geotargeting fixes what the site thinks you are. It doesn't fix a site that blocks scrapers regardless of origin. An independent benchmark cited in third-party coverage of the tool reported 0% success rates on Instagram and Twitter/X and complete failure on Booking.com. If those are your targets, no `country_code` value saves the job.

Latency is ordinary: average response times land around 5 to 7 seconds, and georouting itself adds very little on top.

The tool also isn't built for hyperlocal work. City-level and carrier-level targeting aren't offered, so ad verification that needs a specific metro, or local SEO checks at postal level outside Amazon US, require a different provider.

## How it compares with other geotargeting options

- **Bright Data** runs a much larger pool (150M+ IPs across 195 countries) with city-level and carrier-level targeting, and managed scraping starts around $500/mo. It's the strongest option on granularity and the heaviest to configure.
- **Oxylabs** offers fine geographic control including ISP-level targeting, priced and positioned for enterprise buyers.
- **ScrapingBee** includes country-level geotargeting on every paid plan from about $49/mo, which makes it cheaper to reach single-country targeting than ScraperAPI's $299 threshold. Its proxy pool and country list are smaller.
- **Scrape.do** includes standard country targeting on paid tiers from around $29/mo, with fewer regions and a smaller IP pool.

ScraperAPI's position: no city or carrier granularity, but country-level routing across 69 documented countries plus residential fallback, without an enterprise contract. If single-country access on a small budget is the whole requirement, ScrapingBee or Scrape.do get you there for less. If you also want JS rendering, anti-bot bypass, and structured data in one API, the comparison shifts.

## FAQ

**Does country_code cost extra credits?**
No. The parameter is free. Costs come from `premium=true` (+10), `render=true` (+10), `ultra_premium=true` (+30), and domain-based pricing like Amazon's 5 credits or Google's 25.

**Can I target a city or a ZIP code?**
Only country-level. Amazon US requests support ZIP code targeting; nothing else does.

**Does proxy mode support geotargeting?**
Yes, via `scraperapi.country_code=de` in the proxy username.

**Can I test geotargeting before paying?**
The free trial runs 7 days with 5,000 credits and no credit card. Trial geotargeting coverage varies by account, so confirm the codes you need in the dashboard before you rely on it.

**What happens when I hit my credit limit?**
On Hobby, Startup, and Business, scraping pauses until renewal or upgrade unless auto-upgrade is enabled. On Scaling and above, pay-as-you-go continues up to your cap.

**How is ScraperAPI rated?**
Third-party platforms score it 4.9/5 for ease of use on Capterra and 4.5/5 on Trustpilot across 42 reviews. The recurring complaint in those reviews is difficulty predicting real costs before the first bill, which is exactly what the `urlcost` endpoint and `sa-credit-cost` header are for.

## Which plan to buy

- **US only, or Europe as a region** → Hobby at $49/mo. Paying $299 for `country_code=de` when your targets are American is wasted money.
- **One or more named countries** → Business at $299/mo, or $269.10/mo annually. The 69-country list covers most commercial markets.
- **Rare markets or residential IPs needed** → Business or above plus `premium=true`. Budget +10 credits per request on top of the base cost.
- **High volume where a mid-month cutoff is unacceptable** → Scaling at $475/mo for pay-as-you-go overage.
- **150+ countries at scale** → Professional or Advanced, and treat the current bonus credits as a limited-time extra.

Two things to do before you enter card details. First, run your real target URLs through the `urlcost` endpoint or read the `sa-credit-cost` header on trial requests, so you know your true cost per successful scrape rather than per credit. Second, decide whether the 10% annual discount is worth locking in a year, which depends entirely on whether that credit math holds once rendering and premium proxies are stacked.

👉 [Check current ScraperAPI plans and pick the tier that matches your country list](https://www.scraperapi.com/pricing/?fp_ref=coupons)
