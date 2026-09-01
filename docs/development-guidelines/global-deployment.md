# Model deployment type guidance for Microsoft Foundry

> [!IMPORTANT]
> This file keeps its historical filename for link stability, but the guidance now focuses on choosing the right **Microsoft Foundry model deployment type** rather than treating "global deployment" as a one-size-fits-all requirement.

## Start with the deployment model, not the legacy migration label

For Microsoft Foundry **Serverless API** deployments, the important decision is usually not "global or not," but **which deployment type best fits the workload's data-processing boundary, performance, and cost profile**.

Use the current Microsoft Learn pages as the source of truth for model support and operational limits:

- [Deployment overview for Microsoft Foundry Models](https://learn.microsoft.com/en-us/azure/foundry/concepts/deployments-overview)
- [Understanding deployment types in Microsoft Foundry Models](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/deployment-types)
- [Foundry Models sold by Azure](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/models-sold-directly-by-azure)
- [Microsoft Foundry Models quotas and limits](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/quotas-limits)
- [Data, privacy, and security for Foundry Models sold by Azure](https://learn.microsoft.com/en-us/azure/foundry/responsible-ai/openai/data-privacy)
- [Model lifecycle and retirements](https://learn.microsoft.com/en-us/azure/foundry/openai/concepts/model-retirements)

Do not hardcode availability, quota, or retirement assumptions in documentation when those Microsoft-managed pages already publish the current values.

## Recommended decision model

### 1. Global Standard is the common default

For most new templates and most general-purpose AI workloads, start with **Global Standard**.

Choose it when you want:

- the broadest default availability
- the lowest-friction starting point for common workloads
- pay-per-token billing
- the most typical first-release path for newly launched models

This is the common default unless you have a specific data boundary, single-region, provisioned-throughput, or asynchronous batch requirement.

### 2. Data Zone Standard is the right choice for processing-boundary requirements

Choose **Data Zone Standard** when the workload must keep inferencing data processing within a supported Microsoft data zone such as **US**, **EU**, or **APAC**.

This is typically the right choice when:

- compliance or policy requires a regional processing boundary broader than one region but narrower than global
- the architecture must align to Microsoft-defined data zone handling instead of any-region processing

### 3. Standard is the right choice for single-region needs

Choose **Standard** when the workload needs processing in one specific Azure region.

This is usually appropriate when:

- a single-region architecture is intentional
- the workload has a regional dependency outside of data-zone semantics
- the compliance requirement is narrower than data-zone processing

Do not assume that the single-region Standard deployment type is the first or most widely available option for a newly released model. Use the availability documentation linked above.

### 4. Provisioned is for reserved throughput and more predictable high-volume traffic

Choose a **Provisioned** deployment type when the main driver is predictable throughput, capacity reservation, or a tighter performance envelope for sustained production traffic.

Provisioned deployments are typically a better fit when:

- you need reserved throughput units
- request volume is large and predictable
- latency consistency matters more than the flexibility of pay-per-token Standard deployments

### 5. Batch is for large asynchronous workloads

Choose **Batch** when the workload is inherently asynchronous and can trade immediacy for cost efficiency.

Batch is usually appropriate when:

- requests do not need an interactive response
- the workload processes large offline jobs
- lower-cost asynchronous execution is more important than immediate completion

## Quick comparison

| Deployment type | Typical default? | Processing boundary | Billing pattern | Use when |
| --- | --- | --- | --- | --- |
| Global Standard | Yes | Any Azure region | Pay-per-token | Most new templates and common workloads |
| Data Zone Standard | Sometimes | Within a supported data zone | Pay-per-token | US/EU/APAC processing-boundary needs |
| Standard | Sometimes | Single region | Pay-per-token | Single-region architectural or compliance needs |
| Provisioned | No | Global, data zone, or regional depending on type | Reserved capacity | Predictable high-volume production traffic |
| Batch | No | Global or data zone depending on type | Async discounted processing | Large non-interactive jobs |

## Data boundary guidance

The Microsoft Foundry documentation distinguishes between:

- **where data is stored at rest**
- **where inferencing data is processed**

When this distinction matters for your scenario, review the current Microsoft privacy and data-processing guidance before choosing a deployment type. In particular:

- **Global** types may process inferencing data in any Azure region.
- **Data Zone** types keep inferencing data processing within the supported Microsoft data zone.
- **Standard** keeps inferencing data processing in the deployment region.

Use the privacy documentation above rather than repository-local assumptions when documenting compliance-sensitive scenarios.

## Template authoring guidance

Template authors **SHOULD** expose deployment type as a parameterized choice instead of baking one SKU into infrastructure with no override path.

For example, a template can:

- set a default environment variable or IaC parameter to `GlobalStandard`
- document when to switch to `DataZoneStandard` or `Standard`
- describe when a production operator should evaluate Provisioned or Batch

Template authors **SHOULD** also:

- keep model name, version, and deployment type configurable independently
- validate the target region or data zone against current model availability docs
- document cost and quota tradeoffs for non-default choices

## Suggested documentation language

When documenting a template, prefer language like this:

> This template defaults to **Global Standard** for common availability and ease of adoption. If your workload has US, EU, or APAC inferencing-boundary requirements, use **Data Zone Standard**. If you need a specific Azure region, use **Standard**. For predictable high-volume traffic, evaluate **Provisioned**. For asynchronous bulk processing, evaluate **Batch**.

This framing is more durable than older guidance that treated global deployment as the single preferred end state for every workload.
