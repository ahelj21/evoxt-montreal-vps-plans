# Evoxt Montreal VPS: Which of the 11 Plans You Actually Need, What Latency to Expect, and Where the Cheap Tiers Run Out

Evoxt's Montreal location sells 11 plans, from **$3.49/month** for 1 core and 512 MB up to **$95.99/month** for 16 cores and 32 GB. Every one of them is KVM, sits on a 1 Gbps port in a Montreal facility peered at QIX, and ships with a weekly offsite backup plus one IPv6 address. Most readers should start on **VM-1 at $5.99**; the $3.49 box is for people who need a Canadian IP address more than they need a server.

Below is the full plan list, third-party benchmark and latency numbers, a check on the "6.0 GHz" claim, the streaming reports that contradict each other, and the two spots where Evoxt's own numbers need a second look before you pay.

> **Quick pick**
> - **One WordPress site, a backup target, or a small API** → VM-1, $5.99/month (1 core, 2 GB RAM, 20 GB NVMe, 300 GB transfer)
> - **Streaming unlock, a proxy, a bot, a DNS resolver** → VM-0.5, $3.49/month. Unlocking depends on the IP, not the CPU.
> - **Docker with 2–4 services, a small database, staging plus production** → VM-2 at $11.99 or VM-4 at $23.99
> - **Team CI, compiles, a game server with real players** → VM-6 at $29.99 and up
>
> 👉 [See all 11 Montreal plans and current prices](https://bit.ly/EvoXt)

## All 11 Montreal plans, side by side

| Plan | vCPU / RAM | NVMe | Monthly transfer | Price / month | Order |
| --- | --- | --- | --- | --- | --- |
| VM-0.5 | 1 core / 512 MB | 5 GB | 150 GB | $3.49 | [ Deploy VM-0.5](https://bit.ly/EvoXt) |
| VM-0.75 | 1 core / 1 GB | 10 GB | 250 GB | $4.99 | [ Deploy VM-0.75](https://bit.ly/EvoXt) |
| VM-1 | 1 core / 2 GB | 20 GB | 300 GB | $5.99 | [ Deploy VM-1](https://bit.ly/EvoXt) |
| VM-1.5 | 2 cores / 2 GB | 20 GB | 300 GB | $6.95 | [ Deploy VM-1.5](https://bit.ly/EvoXt) |
| VM-2 | 2 cores / 4 GB | 30 GB | 600 GB | $11.99 | [ Deploy VM-2](https://bit.ly/EvoXt) |
| VM-3 | 4 cores / 4 GB | 30 GB | 700 GB | $14.99 | [ Deploy VM-3](https://bit.ly/EvoXt) |
| VM-4 | 4 cores / 8 GB | 60 GB | 1,000 GB | $23.99 | [ Deploy VM-4](https://bit.ly/EvoXt) |
| VM-6 | 8 cores / 8 GB | 60 GB | 1,250 GB | $29.99 | [ Deploy VM-6](https://bit.ly/EvoXt) |
| VM-8 | 8 cores / 16 GB | 80 GB | 2,000 GB | $47.99 | [ Deploy VM-8](https://bit.ly/EvoXt) |
| VM-12 | 16 cores / 16 GB | 80 GB | 2,500 GB | $60.95 | [ Deploy VM-12](https://bit.ly/EvoXt) |
| VM-16 | 16 cores / 32 GB | 100 GB | 4,000 GB | $95.99 | [ Deploy VM-16](https://bit.ly/EvoXt) |

Every row gets the same 1 Gbps port, one IPv4, one IPv6, KVM isolation and the weekly offsite backup. Pricing scales almost linearly with resources, so there is no hidden sweet spot in the middle. What changes is how much transfer you get, and those allowances are tighter than the RAM and disk suggest: 300 GB on VM-1 is roughly 10 GB a day, fine for a text-heavy site and not fine for one serving video or large downloads.

One warning on price. A handful of plan trackers still list VM-0.5 at $2.99 with 500 GB of transfer. Evoxt's own pricing page currently shows **$3.49 and 150 GB**, and the checkout page is the number that gets charged. If a coupon or a legacy rate applies, it will show up in the cart, not in a blog post.

## Where the Montreal box actually sits, and why that matters

This is a real Montreal deployment, not a US node with a Canadian label. The provider's IP space (166.0.106.0/24) geolocates to Montreal, Quebec, under AS149440 Evoxt Sdn. Bhd., with usage type listed as data center. The facility peers at **QIX Montreal** plus multiple Tier 1 carriers, and the port is 1 Gbps.

Third-party latency measurements put Toronto under 10 ms, New York and Boston under 20 ms, and most of the US East Coast under 30 ms. A test VM on the VM-1.5 plan measured 4.27 ms to a Montreal Speedtest target, which is what a locally hosted node should look like.

The data residency angle gets less attention than it deserves. If your workload falls under PIPEDA or Quebec's Law 25, keeping the server and the data in a Canadian facility removes cross-border transfer questions from the conversation. Evoxt bills Canada inside its Standard network group, which is also where the cheapest overage rate lives, $3/TB.

## Performance: the single-core pitch holds, the multi-core numbers are ordinary

Evoxt's marketing leans hard on clock speed, and there is something behind it. The published Geekbench 5 multi-core reference scores for the Standard network:

| Plan | Geekbench 5 multi-core |
| --- | --- |
| VM-0.75 | 1,073 |
| VM-1 | 1,098 |
| VM-2 | 1,940 |
| VM-4 | 3,306 |
| VM-8 | 5,265 |

Those track core count closely, so you are not paying for eight cores and receiving four cores' worth of throughput. Third-party single-core runs are where the gap opens up: a VM-0.5 scored **1,594** on Geekbench 5 single-core, and a VM-1 landed in the 1,400+ range. A same-priced droplet from a large cloud provider typically sits around half that.

A December 2024 test of VM-1.5 recorded Geekbench 6 scores of **2,831 single-core and 5,001 multi-core**, with the OS reporting an AMD EPYC-Genoa processor at 2 cores @ 4491.480 MHz, KVM virtualization, Debian 11 and BBR congestion control. Disk I/O across three runs came in at 1.1, 1.3 and 1.1 GB/s, averaging **1,194.7 MB/s**.

Now the caveat that the sales page compresses into two words. **"Up to 6.0 GHz" is a ceiling, not a spec sheet per VM.** The same page's own comparison graphic (6.0 GHz against 2.4, 2.3 and 2.2 GHz rivals) carries a footnote saying the data was collected in 2020 and may have changed. The system readout on a real VM showed 4,491 MHz. Independent testers describe the single-core frequency as genuinely in that upper range, and that is a fair way to read it: faster than the 2.2–2.4 GHz you get from most shared Xeon fleets, not a guaranteed 6 GHz on every core at every moment.

For workloads that care about one fast thread (compiles, WordPress admin panels, Laravel request handling, a small Minecraft server, a CI runner, a Discord bot), that is the whole reason to look here. For workloads that need many cores at high sustained utilization, this is a small provider's shared fleet and should be benchmarked on your own code before you commit to an annual term.

## What it costs to guess the tier wrong

Overage on the Standard network is **$3/TB**, which is on the low end for the Canadian market. Premium is $12/TB and Premium Plus $24/TB, but Montreal is Standard, so $3 applies. Individual resources can be added without changing plans: **$3/month for an extra IPv4, $3/month for an extra vCore, $2/month for an extra GB of RAM.**

The a-la-carte option sounds like a way to save money. Run the numbers and it usually isn't, and knowing that saves you a support ticket:

- VM-1 is $5.99 for 1 core, 2 GB, 20 GB, 300 GB. Adding a core brings it to $8.99. VM-1.5 gives you the identical 2 cores, 2 GB, 20 GB and 300 GB for **$6.95**. Stepping up is $2 cheaper every month.
- VM-3 is $14.99 for 4 cores, 4 GB, 30 GB, 700 GB. Adding 4 GB of RAM costs $8, so $22.99 total. VM-4 is $23.99 and also doubles the disk to 60 GB and raises transfer to 1,000 GB. One dollar more for a lot more machine.

Where the extras do earn their place is when no plan covers the need: a second IPv4 address for a mail server or a separate SSL endpoint, a one-off top-up when a traffic spike blows past the monthly allowance, or avoiding a rebuild because you would rather not migrate a running box to a new plan. Treat the extras as a patch kit, not a pricing strategy, and pick the plan that matches your workload from the start.

👉 [Start on VM-1 at $5.99/month and resize later](https://bit.ly/EvoXt)

## IP quality and streaming: two reports, two different answers

This is the part where you should not take anyone's word, including mine.

One review of the Montreal node reports native Canadian Netflix, native Disney+, working YouTube Premium, ChatGPT and Amazon Prime Video, with a fraud score of 0. A separate December 2024 test on VM-1.5, run through a standard media-unlock script, returned **Netflix as region MY**, **Disney+ as "Available For [Disney+ MY] Soon"** (not yet usable), YouTube Premium as CA, Amazon Prime as CA, ChatGPT as working, Steam currency as CAD and Google Search captcha-free.

A fresh IP2Location lookup of 166.0.106.91 on 9 November 2026 classifies it as Montreal, Quebec, ISP EVOXT, usage type Data Center/Web Hosting/Transit, **anonymous proxy: No, fraud score: 3**.

So the two reviews disagree on Netflix region, disagree on whether Disney+ works at all, and disagree with a current lookup on the fraud score. All three can be true at once, because IP reputation and catalog assignment move week to week and differ per address inside the same /24.

Practical read: this is a clean-enough data center IP in the right country, and what it unlocks today is not what it will unlock in three months. If a specific catalog is the reason you are buying, take the $3.49 plan monthly, test the exact service you care about in the first hour, and only then think about a longer term. Do not buy a three-year prepay on the strength of a streaming screenshot.

## Latency from Montreal, including from China

Measured from a VM-1.5 in Montreal in December 2024:

| Target | Latency |
| --- | --- |
| Montreal | 4.27 ms |
| Dallas | 39.01 ms |
| Los Angeles | 66.96 ms |
| Amsterdam | 80.58 ms |
| Paris | 84.03 ms |
| Tokyo | 139.73 ms |
| Singapore | 227.98 ms |
| Shanghai | 247.65 ms |

Throughput within North America is close to the port limit: 936/937 Mbps on Speedtest.net, 904/941 to Dallas, 777/945 to Los Angeles, 932/775 to Montreal. Shanghai measured 324 Mbps up and 854 Mbps down, Tokyo 579/975, Singapore 144/532. The Asian numbers are distance and routing, not a throttled port.

For Chinese visitors the routes matter more than the raw figure. Third-party traceroutes describe three very different paths:

- **Unicom** leaves via Beijing, crosses to Frankfurt, then rides HE across the Atlantic; the return goes HE/Zayo to Los Angeles or San Jose and back through AS4837. This is the most direct of the three and the most consistent.
- **Telecom** departs through the Shanghai international gateway to San Jose, crosses the US on HE to Montreal, and returns via Cogent through the UK and Netherlands into 163.
- **Mobile** goes Guangzhou to Los Angeles, then Zayo through Denver, Chicago and Toronto; the return uses HE or Cogent to Los Angeles or Hong Kong into CMI.

Everything lands in the 200–300 ms range with occasional spikes. That rules out game acceleration, real-time voice and anything where a quarter second of lag is felt. It is fine for a backup host, a crawler, a light proxy, an unlock box, or a Canadian-facing site whose actual users are in Canada.

## How it compares with the usual Canadian options

| Provider | Entry plan | RAM / Storage | Transfer | Single-core clock |
| --- | --- | --- | --- | --- |
| Evoxt (VM-0.5) | $3.49/mo | 512 MB / 5 GB | 150 GB | up to 6.0 GHz |
| OVHcloud VPS | about $5/mo, billed in CAD | 2 GB / 20 GB | unmetered, speed-capped | about 2.4 GHz |
| DigitalOcean (Toronto) | $6/mo | 1 GB / 25 GB | 1 TB | about 2.4 GHz |
| Vultr (Toronto) | $6/mo | 1 GB / 25 GB | 1 TB | about 2.4–3.0 GHz depending on the test |
| Hostinger (Canada) | about $5/mo intro | 4 GB / 50 GB | 4 TB | about 3.0 GHz |

Two of these deserve a straight answer rather than a nudge. OVHcloud's unmetered-but-capped bandwidth is the better buy if you plan to push serious sustained traffic, since Evoxt bills by the terabyte. And if you need managed databases, S3-compatible object storage, Kubernetes or a documented compliance portfolio, the larger providers sell those as products; Evoxt sells virtual machines and a control panel. Hostinger's headline price is an intro rate and renews higher, which is worth checking before you compare it to a flat $3.49.

## Who this fits, and who should keep looking

It fits developers and small teams running single-core-bound workloads: compile jobs, WordPress and Laravel sites aimed at Canadian or US East Coast visitors, small game servers, CI runners, bots, mail relays. It fits anyone who wants a Canadian IP with a weekly offsite backup included rather than sold as an add-on, and anyone who would rather pay in Bitcoin, Litecoin, Ethereum or USDT on Tron than hand over card details. Alipay and PayPal are both accepted, which removes the usual friction for buyers in China.

It does not fit four situations. You want a fully managed cPanel stack with phone support. You need SOC 2, ISO 27001 or FedRAMP paperwork; Evoxt does not publish those. You intend to run a gigabit port hot all month. Or you need cloud primitives beyond a VM. The published uptime commitment is 99.99%, and that is the extent of what is on offer; there is no enterprise SLA contract to negotiate.

👉 [Deploy a Montreal VM and test the route for a month](https://bit.ly/EvoXt)

## Questions that come up before ordering

**What can the $3.49 plan actually run?**
512 MB of RAM and 5 GB of NVMe handles a proxy, a bot, a DNS resolver, an unlock box or a monitoring node. A minimal Debian install plus Docker fills a good chunk of that disk. It will not run a database-backed site comfortably, and it will not run a hosting panel.

**Can I install cPanel, aaPanel or BT Panel?**
Evoxt's own VM control panel handles deploys, firewall rules, rescue mode, snapshots and upgrades, and hosting panels are not bundled. You can install one yourself, but cPanel licensing is a separate cost and needs at least 2 GB of RAM to be usable; the 512 MB and 1 GB plans are the wrong shape for it. For a single site, a plain LEMP stack on VM-1 is cheaper and lighter than any panel.

**Is the backup really offsite?**
Evoxt states weekly automatic offsite backups on every plan, restorable from the panel in a few clicks, and a plan tracker describes the schedule as weekly with one offsite copy included, with paid tiers adding daily backups, an on-demand manual backup and five rotation copies priced by VM storage size. Backups do not count against your VM disk. Confirm what your specific plan includes in the panel before you rely on it as your only copy.

**Can I add CPU without switching plans?**
Yes, $3/month per vCore, $2/month per GB of RAM, $3/month per IP. Compare against the next tier first, as the VM-1 and VM-3 examples above show. Upgrading in place is a button in the panel and does not require reinstalling.

**Is Montreal just a US node with a Canadian label?**
No. The IP block geolocates to Montreal, Quebec, the ASN is Evoxt's own, the facility peers at QIX, and measured RTT is about 4 ms to Montreal against 39 ms to Dallas and 67 ms to Los Angeles. Those distances do not happen from a Newark rack.

## Before you click deploy

Match the plan to the job rather than to the price list. An unlock box or a single proxy is VM-0.5 at $3.49. One WordPress site or a backup target is VM-1 at $5.99, which remains the best value per dollar in the lineup. Two to four services with Docker is VM-2 at $11.99 or VM-4 at $23.99. A team running CI, compiles or a populated game server should start at VM-6 and expect to move up.

Two things to settle in the cart, not in a review. First, the final monthly figure after any promotion, since third-party trackers are still circulating older prices and a coupon only counts if it appears at checkout. Second, the refund terms: Evoxt's pricing page does not spell out a refund window, and a third-party tracker lists 7 days for new customers on their first deployment. Pay monthly for the first cycle, watch the transfer counter if your site serves anything heavy, and upgrade once you know the real numbers.

👉 [Check today's Montreal pricing and start with a monthly term](https://bit.ly/EvoXt)
