---
title: Statistics and exports
---

# Analyse initiatives and objectives

<!-- md:version 3.0.0 -->

This guide applies from **OSIRIS 3.0.0** (beta). Statistics require access to initiatives. Visible results also depend on your permissions for the linked contents.

## Find contents

Initiative and objective detail pages show their respective linked contents. Advanced searches offer initiative and objective filters and table columns. Initiative relationships include those automatically derived from objectives.

Relationships can be used in existing reporting and export functions. The [reporting section](../reporting/index.md) explains how to design such outputs.

## Open statistics

Click **Statistics** at the top of the initiative or objective overview. On the statistics page, switch between **Initiatives** and **Objectives**. The objective tab appears only when objectives exist.

Choose a reporting year, custom period or reporting date and click **Update**. You can also select a single entry and include or exclude archived entries. The status column describes the current status.

## Chart and tables

- The bar chart shows up to 20 initiatives or objectives with the most linked contents for a selected content type.
- The overview table contains all selected entries, their period, current status and counts by content type.
- Expand **Counted contents** to inspect the individual results.

**Publications** and **activities** are counted separately; the activity column excludes publications. Each item counts once per initiative or objective. Multiple assignments can place it in several rows. Do not add all rows together as though the result were a unique overall count.

## What does the period mean?

Contents are counted when their date or period overlaps the selected reporting period. Proposals use their submission date. For activities with partial dates, the known month or year is used. Contents without an evaluable date are not counted; the page indicates this. Report exclusions are respected.

!!! info "A reporting date is not a previous database state"
    Analysis uses the **currently stored** relationships and data. A publication added today can therefore appear in an analysis for the previous year. A reporting date filters content dates; it does not restore the editing state from that day. Initiative and objective periods are displayed but do not additionally restrict counts of already linked contents.

## Download and reuse

Use the statistics tables' export controls to download the displayed data, including for Excel. **Download analysis data (CSV)** provides counts per initiative or objective and content type, including IDs and the reporting period, for further use in spreadsheets or BI systems.

The CSV download contains aggregated counts rather than complete content records. For those, use the **Counted contents** table or the [OSIRIS API](../../api/initiatives.md). Planning and target-versus-actual comparisons take place outside OSIRIS.

[Overview](index.md)
