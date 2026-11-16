# Lambda Labs Alternatives: What Vultr Charges for H100 and B200, When a $1.99 H100 Is a Bad Deal, and How Billing Really Compares (Full GPU Plan Table)

Vultr advertises its 8-GPU HGX H100 node from $2.30 per GPU-hour, and its own public plan API lists that node at $23.92/hour ($2.99 per GPU) with a preemptible rate of $18.40/hour ($2.30 per GPU). Both figures sit under Lambda's $3.99/hr H100 SXM. Vultr is the better fit for teams that want a self-serve multi-GPU cluster with InfiniBand and no reservation paperwork; it is the wrong shop for a 24GB consumer card or a scale-to-zero inference endpoint.

> **Quick pick:** If you are renting H100 SXM capacity by the node, start on Vultr's HGX H100 plan (advertised from $2.30/GPU-hr, $2.99/GPU on-demand per its plan API) and confirm the rate you'll actually be billed before you deploy. New accounts can apply a $300 credit code first.
> 👉 [Vultr $300 code credit for new accounts](https://bit.ly/VuLtr)

## Vultr's GPU lineup and what each plan actually costs

Vultr sells GPU capacity two ways: Cloud GPU instances (1 to 8 GPUs per instance) and bare metal GPU, with clusters available on both. Its H100 page lists 34 cloud data center locations across six continents and cites ISO and SOC 2+ compliance.

| Plan | GPU and included specs | Advertised price | What to know | Link |
| --- | --- | --- | --- | --- |
| Cloud Compute (vc2-1c-1gb) | 1 vCPU, 1 GB RAM, 25 GB SSD, 1 TB transfer | $5.00/mo ($0.007/hr) | Cheapest entry point; CPU only, useful for a control node or a test box | [ Vultr plans](https://bit.ly/VuLtr) |
| NVIDIA L40S 48GB | 16 vCPU, 180 GB RAM, 1.20 TB storage, 10 TB bandwidth per GPU | $1.67/GPU-hr | Same per-GPU rate at 1, 2, 4 and 8 GPUs | [ Vultr L40S plans](https://bit.ly/VuLtr) |
| NVIDIA A40 48GB | 24 vCPU, 120 GB RAM, 1.40 TB storage, 15 TB bandwidth per GPU | $1.71/GPU-hr | 4-GPU configuration totals $6.849/hr | [ Vultr A40 plans](https://bit.ly/VuLtr) |
| NVIDIA A100 80GB PCIe | 12 vCPU, 120 GB RAM, 1.40 TB storage, 10 TB bandwidth per GPU | $2.40/GPU-hr | Full 8-GPU PCIe node totals $19.178/hr | [ Vultr A100 plans](https://bit.ly/VuLtr) |
| HGX A100 80GB SXM (8-GPU node) | 8× SXM A100 in one node | from $2.80/GPU-hr | "Starting at" figure; confirm the self-serve on-demand rate | [ Vultr GPU plans](https://bit.ly/VuLtr) |
| HGX H100 80GB SXM (8-GPU node) | 27 vCPU and 239.25 GB RAM per GPU share; 216 vCPU, 1,914 GB RAM, 640 GB GPU memory per node | from $2.30/GPU-hr advertised; $2.99/GPU on-demand per Vultr's plan API | Sold as an 8-GPU node. Preemptible rate $18.40/hr per node | [ Vultr H100 pricing](https://www.vultr.com/products/cloud-gpu/nvidia-h100?ref=9738262-9J) |
| HGX B200 192GB (8-GPU node) | 31 vCPU and 353.25 GB RAM per GPU share; 248 vCPU, 2,826 GB RAM per node | $3.50/GPU-hr on the product page; Vultr's plan API lists $68.00/hr per node ($8.50/GPU) | The two numbers disagree by more than 2x. Treat $3.50 as unconfirmed | [ Vultr GPU plans](https://bit.ly/VuLtr) |

Read the H100 and B200 rows carefully before you budget anything. Vultr's H100 page prints "Starting at $2.300 / Per hour," but the same site's bare metal section says starting-from prices reflect pre-paid contract terms, and the product page's Cloud GPU section calls its pricing on-demand. The public plan API resolves the ambiguity in the least flattering direction for the headline: the 8-GPU H100 node is $23.92/hr at the standard rate with on-demand deployment flagged off, and $18.40/hr preemptible. A preemptible H100 at $2.30/GPU-hr is still cheaper than Lambda's $3.99, but it is a different product from an on-demand reservation, and jobs that get interrupted do not finish.

The B200 line is worse. A $3.50/GPU-hr B200 would undercut Lambda's $6.69 by nearly half; $8.50 would beat it by nothing at all. Until Vultr's page and its API agree, plan on the higher number.

If you want to see which of these plans is deployable on-demand at your target region today:
👉 [Open Vultr's HGX H100 plan page](https://www.vultr.com/products/cloud-gpu/nvidia-h100?ref=9738262-9J)

## Why teams leave Lambda in the first place

Three facts explain most of the migration traffic.

**The price went up and the spot option disappeared.** Lambda's H100 SXM on-demand rate is $3.99/hr as of August 2026, up from $2.99/hr in mid-2025, a 33% increase tied to the Microsoft capacity deal announced in November 2025. Lambda no longer offers spot instances at all. Every instance is a persistent VM billed at full on-demand rates whether you use it or not.

**There is no serverless layer.** Lambda sells instances and clusters. No scale-to-zero, no per-request billing, no autoscaling inference endpoint. If you are serving a production API with uneven traffic, you pay for idle hours by design.

**The cluster floor is 16 GPUs for two weeks, arranged through sales.** That is a sensible minimum for foundation-model training and an absurd one for a team that needs four nodes for a weekend. Vultr's positioning here is the clean contrast: Vultr Clusters run on both Cloud GPU and Bare Metal, connect over NVIDIA InfiniBand, and Vultr states there is "no need for reservations or special requests," with provisioning through the console or API and scheduling via Slurm or Kubernetes. Whether that holds at the capacity you need is a question for the region, not the marketing page, but the purchasing model is self-serve by default.

Lambda still wins on two things worth naming. Egress is free, which matters if you move datasets between clouds monthly. And Lambda Stack is a maintained ML software distribution that saves real setup time if you would otherwise build and version your own container.

## How Vultr compares with the other Lambda alternatives

Prices below are on-demand, per GPU-hour, and move constantly. Verify at the provider before you commit.

| Provider | H100 SXM on-demand | Why you'd pick it | The catch |
| --- | --- | --- | --- |
| Lambda Labs | $3.99 | Maintained Lambda Stack, free egress | 8-GPU SXM minimum, no serverless, 16-GPU/2-week cluster floor |
| Vultr | from $2.30 advertised; $2.99 per plan API | Self-serve InfiniBand clusters, 34 locations, bare metal GPU available | Headline price is a floor, not a rate; no consumer cards |
| Runpod | $3.29 Secure ($2.89 PCIe); $1.99 Community | Per-second billing, serverless with scale-to-zero, 31 regions | Community tier has no SLA |
| Vast.ai | roughly $1.49 to $1.87 from verified hosts | Widest catalog, 68+ GPU types across 40+ data centers | Marketplace with no uptime guarantee; hosts can reclaim instances |
| DigitalOcean Gradient | $1.99 | Preinstalled PyTorch, TensorFlow, Jupyter and CUDA 11.8+; HIPAA-eligible, SOC 2 | Five GPU families, far narrower than a marketplace |
| Nebius | $2.15 to $3.85 (B200 $3.95 to $7.15) | EU residency in Finland and Paris, zero egress fees | Smaller catalog |
| Hyperstack | from $2.40 (NVLink $1.95) | Single-tenant isolation, per-minute billing | Enterprise-oriented sales process |

The pattern: Lambda's $3.99 is now mid-market. Marketplaces undercut it by more than half, managed platforms land 25–50% below it, and Lambda's remaining edge is software convenience plus free egress, not price.

## The billing trap nobody puts in the comparison table

Headline rates hide the unit. Lambda bills per minute, but rounds H100 SXM work to 8-GPU minimums. Run a 47-minute single-GPU fine-tune on Lambda and you are charged for 8 GPUs × 47 minutes at $3.99, about **$25.00**. The same 47 minutes on Runpod Secure at $3.29/hr per second comes to **$2.58**. The rate difference is 21%; the minimum makes it nearly 10x.

Hourly billing has a smaller version of the same problem. A 47-minute job billed by the hour costs 60 minutes, roughly 28% more than the work you did. That compounds fast if you spin up ten instances for a 20-minute inference batch.

Vultr's model has a different trap, and it is the one that eats credits. Vultr's billing documentation states that charges begin when a server is deployed and continue while the server is **stopped**; you have to destroy the instance to stop billing. A month is capped at 672 hours. If you claim a $300 credit, deploy an 8-GPU H100 node to test a training script, then "stop" it and walk away for a week, the meter keeps running.

## Pick by workload

**Model fits in 24GB.** Lambda has nothing below $0.79/hr and no consumer cards, L40S, RTX 6000 Ada or A6000. Vultr's cheapest GPU is the L40S at $1.67/GPU-hr, still a 48GB card. For 24GB work, the marketplaces are cheaper: Vast.ai lists RTX 3060 through B200, and TensorDock published RTX 4090 floors around $0.35/hr.

**Training that needs more than one node, but not 16 GPUs for a fortnight.** This is Vultr's clearest structural advantage over Lambda. Vultr Clusters are self-serve over InfiniBand with Slurm or Kubernetes, no reservation required, and the same cluster tooling works on bare metal. Deploy the node size you need, destroy it when the run ends.
👉 [Check Vultr's H100 node pricing and availability](https://www.vultr.com/products/cloud-gpu/nvidia-h100?ref=9738262-9J)

**Serving inference with uneven traffic.** You need scale-to-zero. Runpod and Modal both publish serverless options with per-request or per-second billing; Lambda offers nothing in this category. Check the cold-start numbers yourself, since published figures vary between sources.

**EU data residency or single tenancy.** Nebius offers Finland and Paris regions with zero egress fees. Hyperstack sells single-tenant deployments, which matters when multi-tenant infrastructure is a compliance finding rather than a preference. Vultr's 34 locations are useful for latency; compliance posture differs per provider and needs reading, not assuming.

## What to confirm before you enter a card number

**Which H100 price you're getting.** Advertised $2.30, API $2.99, preemptible $18.40/hr per node. Confirm whether your workload can tolerate preemption before you take the lower number.

**The B200 number.** Page says $3.50/GPU-hr, API says $8.50. Do not sign a contract against $3.50 until someone at Vultr confirms it in writing.

**Egress.** Lambda's free egress is real and worth hundreds of dollars a month on large dataset transfers. Net it against any cheaper hourly rate rather than comparing GPU rates in isolation.

**Credit terms.** Vultr's coupons page lists code FLY300VULTR for $300 free credit, alongside $200, $250 and a deposit-match offer. All are new customers only, apply to select products only, and cannot be combined with other offers. Vultr's promotional-code documentation requires a linked card or PayPal, allows one promotional code per user, and states credits are subject to expiration and are non-transferable. Third-party write-ups of the $300 offer report a 30-day expiry on unused credit, so plan to spend it inside a month rather than park it.

**Whether you need to destroy, not stop.** Charges accrue on stopped servers. Build the destroy step into your test script.

## The decision rule

Renting H100 SXM by the node with multi-node scaling and no sales call: Vultr, at $2.99/GPU-hr on-demand per its plan API, with the preemptible rate as an option only for interruptible work. Needing the absolute lowest H100 rate and able to handle a host disappearing: a marketplace. Needing per-request inference billing: neither Lambda nor Vultr's documented GPU plans, so look at Runpod or Modal. Needing 16+ GPUs for weeks with a maintained software stack: Lambda remains reasonable, and its free egress and Lambda Stack are the reasons.

Apply the credit before your first deploy, not after:
👉 [Claim the Vultr $300 code credit](https://bit.ly/VuLtr)
