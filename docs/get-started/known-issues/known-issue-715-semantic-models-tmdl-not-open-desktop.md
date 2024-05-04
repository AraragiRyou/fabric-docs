---
title: Known issue - Semantic models created with TMDL can't be opened in Desktop
description: A known issue is posted where Semantic models created with TMDLand edited on the web can't be opened in Desktop.
author: mihart
ms.author: jessicamo
ms.topic: troubleshooting
ms.date: 05/02/2024
ms.custom: known-issue-715
---

# Known issue - Semantic models created with TMDL can't be opened in Desktop

If you create a semantic model with Tabular Model Definition Language (TMDL), you can edit it on the web. If you try to open that semantic model in Power BI Desktop, the semantic model doesn't open. Power BI Desktop can't open the semantic model for editing because it doesn’t recognize the keyword **cultureInfo** due to a TMDL language update.

**Status:** Open

**Product Experience:** Power BI

## Symptoms

When you try to open the semantic model in Power BI Desktop or export using Fabric Git integration, the opening or export fails. You receive an error message similar to: "Unsupported object type - cultureInfo isn't a supported property in the current context."

## Solutions and workarounds

To manually fix the issue, open each of the TMDL files. Rename the **culture** TMDL object type name from **cultureInfo** to **culture**.

## Next steps

- [About known issues](https://support.fabric.microsoft.com/known-issues) 
