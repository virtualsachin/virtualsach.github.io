---
title: "The Surgery: Migrating Distributed Firewalls from NSX-V to T"
date: 2023-08-10
description: "Performing a high-stakes migration of thousands of micro-segmentation rules with zero outages."
tags: ["VMware", "NSX-T", "Migration", "Security", "Terraform"]
---

> "In a migration, the data is easy to move. The policy (Security) is the hard part. It's like performing open-heart surgery while the patient is running a marathon."

## The Context

We were migrating our "Internal Hosting Platform" on IBM Cloud. The control plane had to evolve from the deprecated **NSX-V** to the modern **NSX-T**.

But here was the catch: The data plane—live customer traffic—could not drop. We had to swap the security brain without the body noticing.

## The Challenge: The DFW Matrix

We had thousands of **Distributed Firewall (DFW)** micro-segmentation rules.

* Allow App-A to DB-B
* Block Web-C to Internal-D

These rules were defined using NSX-V constructs (Security Groups based on legacy IDs). Re-writing them manually into NSX-T's new policy model would take a team of 10 people six months. We didn't have the time or the headcount.

## The Solution: The Translation Layer

We didn't write rules; we wrote a **Translator**.

We built a custom **Terraform translation layer** (aided by Python scripting) that:

1. **Read** the NSX-V rules via API.
2. **Mapped** the legacy Object IDs to new NSX-T Tags.
3. **Generated** the new NSX-T Policy configs programmatically.

This allowed us to "Migration as Code." We could run dry-runs, verify the translation logic, and then apply it in bulk.

## The Outcome

* **Precision:** 100% rule fidelity. No security holes opened.
* **Downtime:** Zero significant outages during the cutover.
* **Legacy:** Successfully retired the NSX-V controllers, ending a 5-year era.

It proved that with the right automation strategy, even the scariest "Surgery" can be routine.
