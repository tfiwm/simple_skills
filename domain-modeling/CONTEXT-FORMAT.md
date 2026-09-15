# CONTEXT.md Format

## Structure

The glossary is organized by domain. Each level-2 heading is a domain, with a one-line description of what it covers. Every term sits under exactly one domain.

```md
# {Context Name}

{One or two sentence description of what this context is and why it exists.}

## Ordering

{One or two sentences on what this domain covers.}

**Order**:
A request from a customer to purchase one or more products.
_Avoid_: Purchase, transaction

**Customer**:
A person or organization that places orders.
_Avoid_: Client, buyer, account

## Billing

{One or two sentences on what this domain covers.}

**Invoice**:
A request for payment sent to a customer after delivery.
_Avoid_: Bill, payment request
```

## Rules

- **Assign every term to a domain.** When adding or updating a term, decide first which domain it belongs to, then write it under that heading. Don't place a term where it happens to be convenient.
- **One term, one domain.** If a term seems to belong to two domains, that's a signal, not a tie: either the meanings differ (make two distinct terms) or a boundary line between the domains is wrong. Say which.
- **Describe each domain.** Give every domain heading a one-line description of what it covers. An undefined heading invites dumping.
- **Create a domain deliberately.** A domain heading earns its place with more than one term. If no existing domain fits a new term, stop and either justify a new boundary or ask the user. Never create a `Misc` section: it becomes a dumping ground and defeats the structure.
- **Surface moves.** If a term's domain changes across edits, say so ("Invoice was under Fulfillment, but Billing owns it now"). Don't relocate silently.
- **Be opinionated.** When multiple words exist for the same concept, pick the best one and list the others under `_Avoid_`.
- **Keep definitions tight.** One or two sentences max. Define what it IS, not what it does.
- **Use plain English.** Define the term as you would to a new team member. Avoid jargon, implementation patterns ("aggregate root", "event-sourced"), and multi-clause sentences. Read the definition aloud — if it sounds like a textbook, simplify it.

  Good: "**Order**: A request from a customer to purchase one or more products."
  Avoid: "**Order**: An aggregate root within the ordering context that orchestrates the lifecycle of a purchase transaction via domain events."
- **Only include terms specific to this project's context.** General programming concepts (timeouts, error types, utility patterns) don't belong even if the project uses them extensively. Before adding a term, ask: is this a concept unique to this context, or a general programming concept? Only the former belongs.
