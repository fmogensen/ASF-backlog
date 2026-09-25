# End-user review gate for customer-visible content (copy/legal) before landing and deploy

Feature: an end-user review gate for customer-visible content.

Incident 2026-09-25: botseon's legal pages (contact/DSA, cookies, DPA, privacy, terms) went through code review and CI and went live, but nobody read them as an end user would. The operator: "those legal documents has not been proff read with the eyes of an enduser".

Want (generic, deterministic routing; an LLM only does the reading):
1. Classification: a change touching the product's `customer_paths` content (copy, legal, docs pages, emails, UI strings) is `customer-content`. Content paths come from the product config, e.g. `customer_content_paths`.
2. A new review kind `enduser`: the reviewer brief takes the persona of the product's target user (from product config: `audience:`) and checks leaked internals (ids, TODO, placeholders, spec wording), missing facts, readability, broken links and plain-language gaps. It returns MUST FIX / SHOULD FIX.
3. Gate: a customer-content PR can't land, and a deploy target can't ship it, until the `enduser` review approves. For legal content (`legal_paths`), `approvals` can require the operator's sign-off before prod or site.
4. Views: `asf prod` NOT LIVE / ON PROD rows show the enduser review state.

Acceptance: tests for classification, gate holding a landing and a deploy, the brief rendering with an audience, and the approval path for legal content.
