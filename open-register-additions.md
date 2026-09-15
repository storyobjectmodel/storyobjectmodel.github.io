## 9. Story-to-story relationships

`relations[]` ships in 1.0 as it was carried from v0.2: `relation_type` is an
unconstrained string with seven values that are convention only, and `target_story_id`
is optional, which permits a relationship to nothing. An implementer reading the schema
today will invent values, and two implementers will invent different ones.

A full model was worked through by the working group in August and offered for
ratification. It was not taken before the 1.0 cut and is recorded here rather than lost.
It proposed: a governed enum (`FOLLOW_UP`, `DEVELOPS`, `SIDEBAR`, `SPIN_OFF`, `RELATED`,
`CORRECTS`, `SUPERSEDES`, `SAME_STORY`); `target_story_id` required; an explicit
direction rule, where a relationship is read as "this story `relation_type` the target
story" and direction records where it was declared rather than any hierarchy;
withdrawal by state rather than deletion; a back-reference from a relationship to the
assertion that confirmed it; and a rule that only `CORRECTS` and `SUPERSEDES` may give
grounds for a declaration, inform only, one hop at a time, with any walk bounded because
relationships can legitimately form loops.

The open questions, each of which can be resolved additively under the compatibility
policy:

| Question | What turns on it |
|---|---|
| The governed enum itself | Without it, `relation_type` is free text and nothing is interoperable between two implementations. |
| Duplicate resolution | Two stories for one event resolve two different ways. Inside one owning system it is a merge: one story is kept, the other retires to `ARCHIVED` with the reason in the audit, and anything bound to it moves to the survivor keeping its `asset_id` and original timestamps. Across two owning systems there is no merge, only a `SAME_STORY` declaration from each side. Neither path is in the schema today. |
| A proposal does not declare its resolution | A `MATCH` in `assertions[]` carries the target identifier only, so a reviewer confirms without knowing whether they are confirming a merge or a relationship, and a consumer reading `CONFIRMED` cannot tell which to expect. |
| Cross-newsroom referencing form and `story_id` uniqueness scope | `target_story_id` works where identifiers have been exchanged directly. Until the general form is settled, cross-newsroom relationships are not interoperable, and the spec should say so plainly. |
| Cross-domain subscription | A newsroom can only act on another's correction if it is receiving that newsroom's snapshots. This is a conformance-floor question rather than a relations one. |
| The `FOLLOW_UP` and `DEVELOPS` boundary | A desk chooses between the two daily. One sentence each, or merge them, before they become synonyms in practice. |
| Re-proposal of a rejected match | Whether a `REJECTED` assertion on the same target and claim suppresses re-proposal, or whether de-duplicating is the deployment's job. Unstated, a confident matcher re-offers the same relationship every pass. |
| An umbrella or running story | Whether day stories declaring `DEVELOPS` back to a spine is sufficient, or whether the running-story case earns its own value, declared by the child rather than maintained as a list on the parent. |

There is a dependency worth naming: a skills executor cannot currently be recalled on a
relationship at all. The recall advert supports `field` and `field_change` conditions
only. Until a condition kind exists for a restriction carried through an editorial link,
`CORRECTS` is actionable in principle and unreachable in practice.

## 10. Who mints a story

The schema is deliberately silent on which system may create a story, and that silence
has been read both ways. This entry records the position the working group has been
operating to, so that it is either adopted or argued with rather than assumed.

What 1.0 already requires is narrow and unchanged: `story.context` is published by the
story owner, `story_id` is immutable, `sequence_number` increases, and
`originating_system` is re-stamped by whoever publishes, so that a correction is
attributed to the corrector rather than to whoever first minted.

The working position on top of that: minting authority is deployment policy, not a
standard-level rule, and one story has one owner. A newsroom may nominate whatever it
likes as the owner of a given class of story, including a light pass-through that mints
on a high-priority wire under a rule the newsroom wrote. What the position excludes is
an agency or a wire feed minting a story into a newsroom it cannot see into, and any
deployment in which two systems can both mint the same event. A wire arriving is not a
story existing; raw wires do not travel the bus, and a wire settles into a story as
referenced content in `content_refs[]` with its origin on `editorial_source[]`.

The question for the group is whether any of this belongs in the conformance floor.
Today an implementation can satisfy every rule in `conformance.md` and still produce two
stories for one event, and by entry 9 the reconciliation path for that is not in the
schema either.
