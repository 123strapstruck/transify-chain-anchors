# Transify Chain Anchors

Daily published root-hash anchors for Transify's transaction hash chain.

## What this is

Once every 24 hours, Transify computes a SHA-256 root hash over the current
chain tail (block number + hash) of every active workspace on the platform,
sorted deterministically by workspace ID, and commits it here as a dated
JSON file.

This repository exists so that Transify's chain state on a given date is
witnessed by infrastructure Transify does not control. A root hash
committed here cannot be altered after the fact without that change being
visible in this repo's own commit history — independent of Transify's
database.

## File format

One file per day, at `anchors/YYYY-MM-DD.json`:

```json
{
  "date": "2026-07-02",
  "rootHash": "a3f8e1...",
  "workspaceCount": 214,
  "publishedAt": "2026-07-02T00:03:11.000Z"
}
```

- `rootHash` — SHA-256 hex digest of the sorted `{ workspaceId,
  lastAssignedBlockNumber, lastAssignedHash }` array across all workspaces
  with at least one recorded transaction that day.
- `workspaceCount` — number of workspaces included in that day's root.
- `publishedAt` — UTC timestamp of publication.

## What this proves

That Transify's aggregate chain state existed, unchanged, as of the commit
date shown by GitHub — independent of anything Transify itself asserts
after the fact.

## What this does not (yet) prove

This is a platform-wide aggregate root, not a per-workspace inclusion
proof. It does not currently let you confirm that one specific workspace's
chain head was part of a given day's root without trusting Transify's own
statement of which entries went in. A Merkle-tree structure with
per-workspace inclusion proofs is a planned improvement.

## Who publishes this

Transify's own Cloud Functions, on a fixed daily schedule, via an
automated commit. No manual edits are made to files in this repository.
