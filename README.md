# DMIT Irvine: Specs, Real Pricing, Restock Reality, and Where It Fits in DMIT's LAX Lineup

Most people who search for "DMIT Irvine" have already seen a spec sheet somewhere — a forum post, a Chinese VPS blog, a Telegram alert — and now they're stuck on the same three questions: what exactly did I just read, is it in stock right now, and is $159 a year actually a good deal for 2 cores and 2GB of RAM.

Those are fair questions, and the answers are more interesting than the usual affiliate copy. Let's go through it properly.

## What "DMIT Irvine" actually refers to

DMIT is a VPS provider that has been running since around 2017, registered in the US, with machines in Los Angeles, Hong Kong and Tokyo. The Irvine plan is one of its Los Angeles products, filed under the LAX region in the cart.

The important thing: "Irvine" is a plan name, not a separate datacenter you pick at checkout. DMIT names its LAX plans after California places — PalmSpring, Irvine, Fontana, Echo — and the product code you'll see on invoices is something like `PVM.LAX.Pro.Irvine` or `LAX.AN4.Pro.Irvine`, depending on which hardware generation the order lands on. The `.Pro` part matters more than the city name. It tells you which routing profile the machine gets.

DMIT splits its LAX network into three profiles:

- **Pro (Premium)** — China Telecom CN2 GIA on the return path, plus premium transit. This is the expensive, low-latency-to-China one.
- **EB (Eyeball)** — CMIN2 and 9929 style routing. A middle step, generally cheaper.
- **T1 (Tier 1)** — international BGP with no China optimisation at all. Cheapest, and honest about it.

Irvine sits in Pro. That single fact explains most of its price.

## DMIT Irvine specs and price

The configuration has been reported consistently across trackers, restock alerts and review sites, and it hasn't changed:

| Item | Irvine |
| --- | --- |
| vCPU | 2 cores |
| RAM | 2 GB |
| Storage | 40 GB SSD |
| Transfer | 3,000 GB / month (bidirectional) |
| Port | 5 Gbps |
| IPv4 | 1 |
| IPv6 | 1 × /64 |
| Over-quota behaviour | Throttled to 4 Mbps, unlimited traffic |
| Billing | $59 / quarter, $159 / year |

Virtualisation is KVM, and the host hardware is AMD EPYC — DMIT has moved its LAX fleet between EPYC 7003, 9004 and 9005 series generations over time, which is why you'll see plan names prefixed AS3, AN4 or AN5.

Two details are easy to skim past and worth slowing down for.

**Traffic counts both directions.** The 3 TB is not 3 TB down plus 3 TB up; it's the sum. If you're running a mirror, a backup target or anything that pushes as much as it pulls, you'll burn through it faster than the number suggests.

**Over-quota means throttling, not suspension.** This is the friendlier behaviour of the two options providers usually offer. DMIT doesn't cut you off when you pass 3 TB — it drops you to 4 Mbps and lets you keep going. For a low-traffic site that occasionally spikes, that's a much better failure mode than a service notice. It is not, however, a substitute for buying more transfer if you regularly run hot.

👉 [Check current configuration and availability for LAX.Pro.Irvine](https://www.dmit.io/aff.php?aff=12046)

## Why it keeps selling out

Irvine is a limited plan. DMIT releases it in batches rather than running it as permanent inventory, and third-party trackers describe it as being sold out for most of any given year, with periodic restocks. One widely-circulated restock happened on 9 June 2025, when DMIT also brought back PalmSpring.

That model has a practical consequence for anyone reading this: if the order page currently shows no stock, that's normal and not a sign the product is discontinued. The usual workaround people use is putting the standard LAX.Pro line to work instead — different hardware generation, same routing philosophy, always available.

It's also worth checking which hardware generation you're getting before you compare prices. DMIT has listed identical-spec Pro plans at different prices across AS3, AN4 and AN5 platforms, so two spec sheets that look contradictory can both be correct. The cart price is the authoritative one.

## Irvine vs PalmSpring

These two get mentioned in the same breath constantly, and they're genuinely close — same 2 cores, same 2 GB, same 40 GB SSD, same 4 Mbps throttle, both Pro routing.

|  | Irvine | PalmSpring |
| --- | --- | --- |
| vCPU / RAM | 2 / 2 GB | 2 / 2 GB |
| Storage | 40 GB SSD | 40 GB SSD |
| Port | 5 Gbps | 2 Gbps |
| Monthly transfer | 3,000 GB | 2,000 GB |
| Price | $159 / year | $100 / year |

PalmSpring is roughly 37% cheaper and gives up 1 Gbps of port speed and 1 TB of monthly transfer for it. The honest read: if your workload is a personal site, a small app backend or a proxy node that moves a few hundred gigabytes a month, the port speed difference will never affect you, and $59/year is real money. Irvine's extra headroom only pays for itself if you're actually pushing enough traffic to notice the ceiling — video delivery, large file hosting, or anything with bursty peaks.

So the decision isn't really "which one is better." It's whether you can name a specific workload that will use the extra 1 Gbps. If you can't, PalmSpring is the better buy, and Irvine is a plan you're buying because you like having room.

👉 [See the current price on DMIT's LAX.Pro line](https://www.dmit.io/aff.php?aff=12046)

## Routing: what the CN2 GIA label buys you, and the IPv6 catch

For the Pro profile in Los Angeles, the advertised paths are:

- **China Telecom** — outbound via AS4809 (CN2 GIA), return via CN2 GIA
- **China Unicom** — outbound direct via AS4837, return via CN2 GIA
- **China Mobile** — outbound via Hong Kong Mobile (AS58453), return via CN2 GIA

The return path is the part that matters most for an interactive site, and it's uniformly CN2 GIA across all three carriers. Latency to mainland China from a well-connected LA machine on CN2 GIA typically lands in a different bracket than standard international BGP — that difference is the entire product.

The catch is IPv6. DMIT has publicly explained that AS4809's domestic IPv6 footprint isn't built out, so its LAX Pro plans don't actually carry CN2 on the v6 side. Reported behaviour is that IPv6 on LAX Pro performs closer to plain AS4134, and one third-party summary describes the IPv6 return routing as CMIN2 rather than CN2. Either way, the practical point is the same: **on LAX Pro, IPv4 is the optimised path and IPv6 is not.** If your application resolves over v6 by default — and plenty do — you may be getting worse performance than the Pro label implies without realising it.

That's not a reason to avoid the plan. It's a reason to know what you're buying.

## The full LAX.Pro lineup, and where Irvine sits

Irvine is the cheapest entry point into the Pro profile at annual billing. Everything above it scales linearly in RAM, storage and transfer.

Prices below are monthly and vary by hardware generation, since DMIT lists AS3, AN4 and AN5 variants of the same tiers at different rates. Where a generation isn't shown, it hasn't been listed for that tier.

| Plan | vCPU | RAM | SSD | Transfer / Port | Throttle | AS3 | AN4 | AN5 | Order |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Irvine (limited) | 2 | 2 GB | 40 GB | 3 TB @ 5 Gbps | 4 Mbps | — | — | — | $159/yr |
| PalmSpring (limited) | 2 | 2 GB | 40 GB | 2 TB @ 2 Gbps | 4 Mbps | — | — | — | $100/yr |
| Pro.TINY | 1 | 2 GB | 20 GB | 1 TB @ 1 Gbps | 4 Mbps | $10.90 | $12.90 | $14.90 | [Order](https://www.dmit.io/cart.php?region=los-angeles&generation=as3&network=premium&product=lax.as3.pro.tiny&aff=12046) |
| Pro.Pocket | 2 | 2 GB | 40 GB | 1.5 TB @ 4 Gbps | 4 Mbps | $16.90 | $18.90 | $21.90 | [Order](https://www.dmit.io/cart.php?region=los-angeles&generation=as3&network=premium&product=lax.as3.pro.pocket&aff=12046) |
| Pro.STARTER | 2 | 2 GB | 80 GB | 3 TB @ 10 Gbps | 4 Mbps | $34.90 | $38.90 | $43.90 | [Order](https://www.dmit.io/cart.php?region=los-angeles&generation=as3&network=premium&product=lax.as3.pro.starter&aff=12046) |
| Pro.MINI | 4 | 4 GB | 80 GB | 5 TB @ 10 Gbps | 8 Mbps | $62.98 | $72.99 | $79.90 | [Order](https://www.dmit.io/cart.php?region=los-angeles&generation=as3&network=premium&product=lax.as3.pro.mini&aff=12046) |
| Pro.MICRO | 4 | 4 GB | 160 GB | 7 TB @ 10 Gbps | 8 Mbps | $87.90 | $102.90 | $110.90 | [Order](https://www.dmit.io/cart.php?region=los-angeles&generation=as3&network=premium&product=lax.as3.pro.micro&aff=12046) |
| Pro.MEDIUM | 6 | 8 GB | 160 GB | 15 TB @ 10 Gbps | 10 Mbps | $199.90 | $239.90 | $289.90 | [Order](https://www.dmit.io/cart.php?region=los-angeles&generation=as3&network=premium&product=lax.as3.pro.medium&aff=12046) |
| Pro.LARGE | 8 | 16 GB | 320 GB | 25 TB @ 10 Gbps | 10 Mbps | $459.90 | $499.90 | — | [Order](https://www.dmit.io/aff.php?aff=12046) |
| Pro.GIANT | 12 | 24 GB | 640 GB | 50 TB @ 10 Gbps | 10 Mbps | $929.99 | $1,009.90 | — | [Order](https://www.dmit.io/aff.php?aff=12046) |

Irvine's real competition inside this table isn't the tiers above it — it's PalmSpring below it, and Pro.Pocket next to it. Pocket runs $16.90/month on AS3 with 40 GB storage, which is comparable hardware at a much lower commitment. What Pocket doesn't give you is Irvine's 5 Gbps port or 3 TB of monthly transfer. If you're paying annually and expect the machine to grow into its allowance, Irvine absorbs that; Pocket doesn't.

## The other two LAX profiles

Pro isn't the only thing DMIT sells in Los Angeles, and for some workloads it's the wrong purchase entirely.

**LAX.Eyeball (EB)** runs CMIN2 and 9929 style routing — cheaper than Pro, still China-aware, but without the CN2 GIA return path. DMIT has historically let EB stock run thin, so it's often listed as sold out while other lines are available.

| EB Plan | vCPU | RAM | SSD | Transfer / Port | Price |
| --- | --- | --- | --- | --- | --- |
| EB.TINY | 1 | 2 GB | 20 GB | 1.5 TB @ 2 Gbps | $10.90/mo |
| EB.Pocket | 2 | 2 GB | 40 GB | 3 TB @ 4 Gbps | $16.90/mo |
| EB.STARTER | 2 | 2 GB | 80 GB | 5 TB @ 10 Gbps | $34.90/mo |
| EB.MINI | 4 | 4 GB | 80 GB | 10 TB @ 10 Gbps | $62.90/mo |
| EB.MICRO | 4 | 4 GB | 160 GB | 14 TB @ 10 Gbps | $87.90/mo |
| EB.MEDIUM | 6 | 8 GB | 160 GB | 30 TB @ 10 Gbps | $199.90/mo |

**LAX.Tier 1** carries no China optimisation at all. Return traffic routes whatever way the internet decides, and DMIT prices accordingly. Over-quota throttling here is far gentler — typically 100 Mbps to 500 Mbps rather than 4 Mbps.

| T1 Plan | vCPU | RAM | SSD | Transfer / Port | Price |
| --- | --- | --- | --- | --- | --- |
| T1.WEE | 1 | 1 GB | 20 GB | 1 TB @ 4 Gbps | $36.90/yr |
| T1.TINY | 1 | 1 GB | 20 GB | 2 TB @ 4 Gbps | $6.90/mo |
| T1.STARTER | 2 | 2 GB | 40 GB | 4 TB @ 10 Gbps | $12.90/mo |
| T1.MINI | 2 | 4 GB | 80 GB | 8 TB @ 10 Gbps | $21.90/mo |
| T1.MICRO | 4 | 4 GB | 120 GB | 16 TB @ 10 Gbps | $32.90/mo |

One caveat worth knowing if you go hunting for T1 pricing: DMIT has also listed a separate higher-volume Tier 1 series using codes like V2C2G, V2C4G and V4C4G at different rates. If the T1 numbers you find elsewhere don't match this table, you're probably looking at that series.

Beyond Los Angeles, DMIT also runs Hong Kong and Tokyo regions with their own Pro/EB/T1 splits — those are separate product lines, not upgrades of a LAX machine, and you can't migrate between regions after purchase. Worth checking before you commit to LAX.

## Discounts and what you'll actually pay

DMIT doesn't maintain a public coupon page. Third-party trackers have reported the same thing: no standing public discount code.

There have been codes, but they've come with narrow conditions. One that circulated during the June 2025 restock window — `2025-EXISTING-CUSTOMER-10OFF` — was restricted to existing customers spending above roughly $3.33/month and excluded the T1 series, and its stated validity ran to 30 June 2025. It is not a current offer. If you see it quoted as an active coupon somewhere, that source is out of date.

The pricing that matters is therefore the annual rate. On Pro plans, annual billing is where the real discount lives — Pocket at $14.90/month becomes $159.98/year, which works out to a little under 11 months' worth. Irvine and PalmSpring skip monthly billing entirely and are sold on quarterly or annual terms only.

## Refunds, IP replacement and payments

DMIT's refund terms are more specific than most hosts in this segment, and they're worth reading before you order rather than after.

**Full refund** — available within 3 days of purchase, provided you've used no more than 30 GB of transfer. Transaction fees charged by the payment gateway are deducted.

**Partial refund** — available within 30 days. The refundable amount is calculated from what you actually paid; if the value of transfer you've already used is higher, the calculation works off the remaining transfer instead of remaining time.

**No refund** — a list that includes a third refund on the same product series by the same person, anything related to (D)DoS activity, "the network isn't good enough," IP geolocation complaints, and chargebacks filed in violation of the terms.

Payments are handled through Alipay, WeChat Pay, PayPal and cryptocurrency, and Chinese-language support is available — which goes some way to explaining why CN2 GIA plans from a US-registered company have such a following among users in mainland China.

## Who should buy it, and who shouldn't

Irvine makes sense if you need a machine in the US with a genuinely optimised return path to China, you expect to sit somewhere in the 2–3 TB monthly transfer range, and you'd rather pay once a year than think about it again. The 5 Gbps port is generous for the price bracket, and the throttle-not-suspend policy is a real safety net.

It doesn't make sense if you're on a tight budget and can't point to a workload that will use the extra port speed — PalmSpring covers that case for $59 less per year. It also doesn't make sense if most of your traffic is IPv6-heavy, given how the v6 path behaves on LAX Pro. And if your audience is entirely in Europe or North America, you're paying a premium for routing you'll never use; the Tier 1 line is the rational purchase there, at roughly a twelfth of Irvine's monthly equivalent.

## Quick answers

**Is DMIT Irvine in stock?** Availability is released in batches. Check the order page directly — a sold-out status is normal and not a discontinuation.

**Is Irvine a separate datacenter?** No. It's a plan in DMIT's LAX.Pro series, named after a California city, hosted in the Los Angeles region.

**Can I switch to Hong Kong or Tokyo later?** No. Regions are separate products and don't migrate.

**What happens if I exceed 3 TB?** You're throttled to 4 Mbps and keep unlimited traffic — no suspension.

**Does the CN2 GIA routing apply to IPv6?** No. The optimised path is IPv4. IPv6 on LAX Pro performs materially worse, which DMIT has confirmed.
