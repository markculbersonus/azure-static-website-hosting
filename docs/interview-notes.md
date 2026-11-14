# Technical Interview Notes

## Explain the architecture
A resource group logically organizes Azure resources. The StorageV2 storage account stores blobs, and enabling static website hosting creates the special `$web` container and exposes a public website endpoint. The browser requests `index.html` over HTTPS. No virtual machine is needed for this static site.

## STAR story — Azure Policy restriction
- **Situation:** Azure rejected an attempted storage account deployment to East US.
- **Task:** Identify the deployment blocker and complete the lab without changing organizational governance rules.
- **Action:** Examined `RequestDisallowedByAzure`, inspected Azure Policy assignments and the allowed-locations parameter, then selected North Central US.
- **Result:** Storage account was deployed and the static site was verified live over HTTPS.

## Questions to prepare for

**What is a resource group?** A logical container for managing related Azure resources and applying controls at a shared scope.

**What is a block blob?** An Azure object storage blob type suited to files such as HTML, CSS, images, and documents.

**Does LRS replace backups?** No. LRS provides local redundancy; it does not protect against every deletion or region-wide disaster. Soft delete addresses certain accidental deletion scenarios.

**Why did the region matter?** An Azure Policy assignment at subscription scope restricted deployments to approved locations.

**Why can the website be public while anonymous blob access is disabled?** The static website endpoint is a separate public serving mechanism for `$web` content; it does not require enabling general anonymous blob access.

**What are limitations of Blob Storage static websites?** They serve static assets and client-side JavaScript, but do not execute server-side application code. Advanced routing and custom domain HTTPS often require additional services.

## Accurate résumé bullet
Deployed an Azure Blob Storage static website with Standard LRS storage, HTTPS and seven-day soft-delete settings; diagnosed and resolved a subscription-level Azure Policy regional restriction to complete deployment in an approved region.
