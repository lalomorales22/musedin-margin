# Margin — Specification

## What Margin is

Margin is an open ad network built for agents. An agent that serves an API or a
feed adds one labeled Sponsored line to its answers and earns per impression.
Advertisers buy slots to reach agents where they read. Every slot is reviewed
before it runs, and impressions are counted without tracking who saw them.

## Users

- **Publishers**: agents, or their operators, that serve an API or feed with
  real traffic. They carry the sponsored line in their answers and earn.
- **Advertisers**: people or teams promoting a product, tool, or service to
  agent readers. They buy slots and supply the line.
- **Readers**: the agents, and the humans behind them, that read a publisher's
  answers. A reader must always be able to tell the sponsored line apart from
  content.

## The sponsored line

The sponsored line is the only ad unit. It is one line, at most one per answer,
and always carries the label "Sponsored". A line has five fields: label
("Sponsored"), text (one sentence), link (where the line points), sponsor (who
paid), and slot id (which bought slot it fills). A line never imitates the
publisher's own voice or format, never claims endorsement, and never runs
without its label.

## Data model

- **Slot**: a bought placement. Fields: slot id, advertiser, budget, price per
  impression, start and end, status (pending, live, paused, ended).
- **Line**: the creative itself. Fields: line id, slot id, label, text, link,
  sponsor name, review status.
- **Impression**: one counted serving of a line. Fields: impression id, line
  id, slot id, timestamp. No reader identity is stored: no agent id, IP
  address, or fingerprint.
- **Publisher account**: the publisher's identity, payout address, and
  accrued balance.
- **Payout**: a settled transfer. Fields: payout id, publisher, amount,
  period, transaction reference.

## Review flow

1. An advertiser submits a line for a slot.
2. A reviewer checks it before it runs: is it labeled, truthful, legal, and
   clear of the banned list below?
3. Approved lines go live in their slot. Rejected lines come back with a reason
   and can be resubmitted once fixed.
4. Any edit to a live line sends it back through review before it runs again.
5. A line reported as deceptive is paused while it is re-checked.

## Display rules

The client shows the line apart from content: labeled "Sponsored", visually
separated, never inline as if it were part of the answer. The link target must
match the sponsor named. A publisher that hides the label or blends the line
into content loses the slot.

## Impression counting

An impression counts when the publisher's API returns an answer carrying the
line. Counts are aggregate per line and slot only. No reader is identified or
tracked: no cookies, no per-reader ids, no logging of who asked. Repeat
deliveries of the same answer do not double-count; the publisher reports
served counts and Margin aggregates them per slot.

## Payout model

Advertisers fund slots in advance. Each counted impression accrues the slot's
price per impression to the publisher. No platform fee is set by this spec; if
one is added later, it is published here first. Balances settle on a regular
schedule (weekly in the first version) once a minimum threshold is met, paid
to the payout address on the publisher's account. Impressions later found
invalid, such as bot traffic or mislabeled lines, are reversed before payout,
and the publisher is told why.

## What advertisers may not run

Nothing illegal, no tokens or trading promotions, no impersonation of another
product or person, nothing deceptive, and nothing that hides or weakens the
Sponsored label. Violations end the slot and forfeit its remaining budget.

## First version

The first version ships the line format, the review queue, aggregate
impression counts, and scheduled payouts. Publisher connectors (MCP and HTTP)
and an advertiser dashboard follow once the format holds.
