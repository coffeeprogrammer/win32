---
Description: This topic is not current. For the most current information, see the Print Schema Specification.
ms.assetid: 80fdc8f1-4fda-4102-9b27-16d9acb4d077
title: Supporting PrintTicket Deltas
ms.topic: article
ms.date: 05/31/2018
---

# Supporting PrintTicket Deltas

This topic is not current. For the most current information, see the [Print Schema Specification](https://www.microsoft.com/whdc/xps/printschema.mspx).

The PrintTicket Provider Interface has methods that can be used to make incremental changes to an existing PrintTicket. The incremental PrintTicket changes can be specified in a partial PrintTicket that is known as a PrintTicket delta. A revised PrintTicket is created by merging a PrintTicket delta with an existing PrintTicket. See the forthcoming PrintTicket Provider Interface document for more information about methods involving PrintTicket deltas.

When a PrintTicket delta is processed, the following steps must be performed.

1.  Identify Feature or ParameterInit instances that are common to both the existing PrintTicket (the target PrintTicket) and the PrintTicket delta.

    -   For each Feature common to both the target PrintTicket and the PrintTicket delta, replace the Feature in the target PrintTicket by the corresponding Feature in the PrintTicket delta.

    -   For each ParameterInit common to both the target PrintTicket and the PrintTicket delta, replace the ParameterInit in the target PrintTicket by the corresponding ParameterInit in the PrintTicket delta.

2.  Copy all remaining Feature and ParameterInit instances in the PrintTicket delta to the target PrintTicket.

3.  If your conflict resolution algorithm allows priorities to be specified in the PrintTicket itself, you may choose to elevate the priorities of the Feature and ParameterInit instances named in the PrintTicket delta.

4.  Perform PrintTicket validation as described in [PrintTicket Validation Checklist](printticket-validation-checklist.md).

## Related topics

<dl> <dt>

[Print Schema Specification](https://www.microsoft.com/whdc/xps/printschema.mspx)
</dt> </dl>

??

??



