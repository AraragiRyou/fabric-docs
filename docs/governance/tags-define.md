---
title: Create and manage a set of tags
description: "Learn how to create and manage a set of tags in Microsoft Fabric."
author: paulinbar
ms.author: painbar
ms.service: fabric
ms.subservice: governance
ms.topic: how-to #Don't change
ms.date: 09/Z6/2024

#customer intent: As a Fabric admin, I want to create and manage a set of tags so that data creators and data consumers can use them to better manage and find data.

---

# Create and manage a set of tags

This article describes how to create and manage a set of tags in Microsoft Fabric. The target audience is Fabric administrators who want to create and manage such a set in their organization.

For more information about tags, see [Tags in Microsoft Fabric](./tags-overview.md).

[Introduce and explain the purpose of the article.]
Can create one or more tags at a time.
You can delete or rename tags.
When you delete a tag it gets removed from an items it was applied to.
When you rename a tag it gets renamed on any item it was applied to.


## Prerequisites

You must have a Fabric administrator role or higher to be able to create, rename, or delete tags.

## Create a set of tags

This section shows how to create a set of one or more tags.

1. Open the [admin portal](../admin/admin-center.md#how-to-get-to-the-admin-portal) and select **Tags (preview)**.

    The Tags tab opens. All currently defined tags are listed on the tab. (is there a limit on the number of tags that can be defined in the tenant?)

1. Select **+ New tag**. If no tags have been defined yet, select **Add your first tags**.

1. In the dialog that appears, provide names for your new tags. Use comma-separated names to create more than one tag at a time. You can create as many tags as you want - there is no limit on the number of tags you can create.

    :::image type="content" source="./media/tags-define/create-tags.png" alt-text="Screenshot showing how to create two new tags.":::

    Tag names can contain:
    * Letters
    * Numbers
    * Spaces (not in the beginning of the tag)
    * Special characters (can't be exclusively special characters, must also contain letters (or numbers?))

1. When done, select **Create**. The new tags will be added to the list of tags.

## Rename a tag

1. Open the [admin portal](../admin/admin-center.md#how-to-get-to-the-admin-portal) and select **Tags (preview)**.

1. Open the options menu of the tag you want to rename and select **Rename**.

    :::image type="content" source="./media/tags-define/rename-tag.png" alt-text="Screenshot showing how to rename a tag.":::

1. Change the name as desired and then select **Rename**. The new name is reflected wherever the tag is applied.

## Delete a tag from the set of tags

1. Open the [admin portal](../admin/admin-center.md#how-to-get-to-the-admin-portal) and select **Tags (preview)**.

1. Open the options menu of the tag you want to rename and select **Delete**.

    :::image type="content" source="./media/tags-define/delete-tag.png" alt-text="Screenshot showing how to delete a tag from the set of tags.":::

1. Select **Delete**. The tag is deleted from the set of defined tags, and is removed from all items where it is applied.

## Related content

* [Tags overview](tags-overview.md)
* [Apply tags to items](tags-apply.md)
* [Monitor tag use](tags-monitor.md)