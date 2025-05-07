# CurateX: Decentralized Content Curation Protocol

**CurateX** is a decentralized content curation platform built on the **Stacks Layer 2** blockchain, leveraging Bitcoin's security guarantees. It incentivizes community-driven content discovery through reputation-weighted voting, direct STX gratuity rewards, and transparent, decentralized moderation.

## Overview

CurateX empowers communities to surface high-quality information by decentralizing the processes of content submission, evaluation, and promotion. It incorporates an immutable audit trail via Bitcoin finality while remaining scalable and cost-efficient thanks to Stacks L2.

### Key Features

* **Reputation-Based Voting**: Users earn or lose credibility based on the quality of their appraisals.
* **STX-Powered Incentives**: Submitters pay a submission fee in STX, and creators can be directly rewarded via gratuities.
* **Decentralized Moderation**: Community flagging with administrator oversight helps surface and moderate problematic content.
* **Topic-Based Categorization**: Content is organized into categories curated by the protocol administrator.
* **Sybil Resistance**: Voting power is constrained by participant credibility, reducing the influence of spam or low-effort accounts.
* **Bitcoin Finality**: Security is rooted in Bitcoin via the Stacks chain.

## Smart Contract Architecture

### Constants & Configuration

* `PROTOCOL_ADMINISTRATOR`: Principal with special access control
* `MIN_HYPERLINK_LENGTH`: Ensures valid URLs
* `MAX_UINT`: Upper bound for safe computation
* `submission-charge`: STX fee for content submission (adjustable)
* `content-topics`: Max 10 administrator-defined topic categories

### Storage

* `curated-items`: All content entries, including metadata and interaction stats
* `participant-appraisals`: User-submitted upvote/downvote records
* `participant-credibility`: Reputation score based on voting behavior

## Public Functions

### Content Submission

```clojure
(contribute-item (headline) (hyperlink) (topic)) → (ok item-identifier)
```

Submits new content. Charges `submission-charge` STX. Validates topic and hyperlink. Adds item to the curation pool.

### Appraisal / Voting

```clojure
(appraise-item (item-identifier) (appraisal)) → (ok true)
```

Appraise an item as `1` (upvote) or `-1` (downvote). Affects both the item's appraisal score and appraiser's credibility.

### Reward Content Creators

```clojure
(reward-originator (item-identifier) (gratuity-amount)) → (ok true)
```

Send STX directly to a content creator as a tip. Updates internal reward tally.

### Flagging Content

```clojure
(flag-item (item-identifier)) → (ok true)
```

Report problematic content. Originators cannot flag their own posts.

## Read-Only Queries

* `retrieve-item-details`: Get all metadata and metrics for a specific item.
* `retrieve-participant-appraisal`: See a user's vote on an item.
* `retrieve-aggregate-submissions`: Count of all submissions.
* `retrieve-participant-credibility`: View a user's reputation score.
* `get-item-ids`: List of item IDs (max 10).
* `retrieve-top-items`: Top-rated items filtered by non-negative appraisals.

## Administrative Controls

Only callable by the protocol administrator.

* `adjust-submission-charge`: Update the STX fee for submissions.
* `expunge-item`: Remove flagged or inappropriate content.
* `introduce-topic`: Add a new content category (max 10 total).

## Security Considerations

* **Sybil Resistance**: Reputation scores constrain influence.
* **Immutable Audit Trail**: Historical records are preserved transparently.
* **Bitcoin Security**: Finality of state changes is anchored to Bitcoin.
* **Overflow Protection**: Constants and logic guard against arithmetic exploits.
* **Access Control**: Only the administrator can modify critical parameters or remove content.

## Future Enhancements

* **DAO Governance** for administrator role rotation
* **Topic Moderators** and decentralized topic creation
* **NFT-based content certification**
* **On-chain metadata indexing and search**

## Deployment

Built for Clarity smart contracts on the Stacks 2.0 blockchain. Compatible with:

* **Stacks.js**
* **Clarinet** for local testing
* **Stacks Explorer** for deployment verification
