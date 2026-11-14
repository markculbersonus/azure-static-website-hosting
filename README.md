# Azure Static Website Hosting ☁️

**Live deployment:** https://stlab01mark2026.z14.web.core.windows.net/

A hands-on Microsoft Azure cloud engineering project that deploys a static HTML/CSS website through **Azure Blob Storage**. The project also documents troubleshooting a subscription-level Azure Policy regional restriction.

> **Status:** Live site verified November 13, 2026. The `index.html` in this repository is a clean reconstruction based on the verified page and earlier lab code; it is not a byte-for-byte export of the original deployed file. The optional `404.html` is a repository addition and has **not** been confirmed deployed.

## Architecture

```mermaid
flowchart TD
    U[Visitor's browser] -->|HTTPS| E[Azure static website endpoint]
    E --> W[\$web blob container]
    W --> I[index.html + embedded CSS]
    RG[Resource group: rg-staticwebhosting-mark] -. organizes .-> SA[Storage account: stlab01mark2026]
    SA --> W
    P[Azure Policy: allowed regions] -. constrains deployment .-> SA
```

| Component | Configuration |
| --- | --- |
| Subscription | Azure for Students |
| Resource group | `rg-staticwebhosting-mark` |
| Storage account | `stlab01mark2026` |
| Region | North Central US |
| Performance / redundancy | Standard / LRS |
| Hosting | Azure Blob Storage static website |
| Container | `$web` |
| Default document | `index.html` |
| Error document setting | `404.html` (file upload not verified) |
| Secure transfer | Required (HTTPS) |
| Minimum TLS | 1.2 |
| Blob/container soft delete | Enabled, 7 days each |
| Anonymous blob access | Disabled (static website endpoint remains public) |

## Deployment steps

1. Create an Azure resource group.
2. Create a general-purpose v2 storage account using **Standard** performance and **LRS** redundancy in an allowed region.
3. Under **Data management → Static website**, enable hosting and set `index.html` as the index document. Optionally configure `404.html` as the error document.
4. Open **Data storage → Containers → `$web`**.
5. Upload `index.html` (and `404.html` if using the optional custom error page). Check the HTML blob's content type is `text/html`.
6. Open the **Primary endpoint** URL and verify the page loads.

> Static website hosting serves public content. Do not place secrets, credentials, private files, or sensitive data in `$web`.

## Troubleshooting: regional policy restriction

**Symptom:** Storage account deployment validation failed with `RequestDisallowedByAzure` for the originally selected **East US** region.

**Investigation:** Opened **Azure Policy → Assignments**, inspected the **Allowed resource deployment regions** assignment scoped to Azure for Students, and reviewed the `listOfAllowedLocations` parameter.

**Allowed regions observed:** `francecentral`, `denmarkeast`, `belgiumcentral`, `northcentralus`, `canadacentral`.

**Resolution:** Selected **North Central US**, a region explicitly permitted by the subscription policy, then successfully deployed the storage account and published the website.

**Engineering takeaway:** Subscription governance controls can block deployments even when resource settings are otherwise valid. Inspect the specific error and policy parameters rather than bypassing the policy.

## Security and operations notes

- **HTTPS / TLS:** Secure transfer is required; the storage account's minimum TLS version was set to 1.2.
- **LRS:** Stores multiple copies within a primary regional location; it is not cross-region disaster recovery.
- **Soft delete:** Retains deleted blobs and containers for 7 days; it does not recover a deleted storage account or resource group.
- **Public access:** The static website endpoint is intentionally public; do not confuse this with enabling general anonymous blob container access.
- **Cost:** Storage and requests can consume Azure for Students credits. Review costs and remove unused resources after the lab.

## Skills demonstrated

Microsoft Azure · Azure Blob Storage · Azure Resource Manager · Azure Policy · Cloud Governance · Object Storage · HTML/CSS · HTTPS/TLS · Deployment Troubleshooting

## Interview talking point

> I deployed a static website using Azure Blob Storage. When the deployment failed due to a regional policy restriction, I inspected the subscription-level Azure Policy assignment, identified permitted locations, and redeployed in North Central US. I then configured static website hosting, uploaded the HTML index document to the `$web` container, and verified the live HTTPS endpoint.

## Repository contents

```text
azure-static-website-hosting/
├── README.md
├── index.html
├── 404.html                  # Optional, not confirmed deployed
├── .gitignore
├── docs/
│   └── interview-notes.md
└── screenshots/
    └── README.md
```

## Next improvements

- Add redacted deployment screenshots.
- Upload the optional `404.html` if a custom error page is desired.
- Automate deployment with GitHub Actions using short-lived identity federation rather than stored account keys.
- Add infrastructure-as-code (Bicep or Terraform) after learning the fundamentals.

**Author:** Mark Culberson
