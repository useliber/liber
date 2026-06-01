# Contributing to LIBER

LIBER is an early-stage, pre-final project (first edition: 1 June 2026), stewarded by
Przemysław Zieliński and Krzysztof Luty. The best time to shape a format is before it sets —
so the most valuable thing you can offer right now is **honest, specific feedback**.

## The fastest ways to help

1. **React to the format.** Read [SPEC.md](./SPEC.md) and the example in
   [`library/database-design-patterns.liber.md`](./library/database-design-patterns.liber.md).
   Does the `.liber` shape make sense for *your* domain? What's missing? What's awkward?
   Open an issue labeled `format-feedback`.

2. **Request a decision landscape.** Is there a "which X should I choose" decision you
   wish existed as a `.liber` file? Open an issue labeled `library-request` describing the
   domain and the options you'd expect to see.

3. **Correct something.** Found a trade-off that's wrong, an outdated claim, a broken link,
   or anything overclaimed? That's the most useful issue of all. Open it.

## Contributing a `.liber` file

A good `.liber` file earns its existence by containing decision knowledge an agent
**cannot** find in three web searches. Before you write one:

- **Map a decision, not a topic.** `database-selection` is a decision; `databases` is a category.
- **Present a landscape, never a verdict.** Options with trade-offs per context — never "use this one." Neutrality is an invariant.
- **Cite at least two independent sources.** And don't let vendor docs be the *only* evidence for an option.
- **Be honest about confidence.** Maximum is `0.95`. `null` ("I didn't assess") is more honest than a made-up number.
- **Keep research confidence and adoption data strictly separate.** Never average them.
- **Date it.** `verified: YYYY-MM-DD`. Knowledge that rots in silence is worse than no knowledge.

Validate your frontmatter against the schema in [SPEC.md](./SPEC.md) (Appendix A) before
opening a pull request. Any standard YAML frontmatter parser (`gray-matter`,
`python-frontmatter`) should read it without custom tooling — if it needs a custom parser,
something is off.

## Pull requests

- One decision landscape per file. If `options` would exceed ~30, split into focused sub-domain files.
- Use `kebab-case.liber.md` filenames and `snake_case` YAML fields.
- Include a short note in the PR describing the sources you used and your confidence reasoning.

## Code of conduct

Be kind, be specific, assume good faith. We're building something useful together, early.

## License

By contributing, you agree that your contributions to the format and library content are
licensed under **CC BY-SA 4.0**, consistent with the rest of this repository (see [LICENSE](./LICENSE)).
