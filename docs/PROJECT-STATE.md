# Project State

**Last Updated:** 2026-04-08

## Current Phase
- Phase: Localization & Integration
- Focus: English translation and Factory.ai Droid harness configuration
- Progress: Complete — all content translated and droids configured

## Last Session
- Date: 2026-04-08
- What Done: Translated entire repo from Portuguese to English, created 5 Factory.ai droid configs, renamed directories/files to English, removed Portuguese README
- See: `docs/Handoffs/handoff-001.md`

## Next Steps
1. Commit all changes to git
2. Update docs/plans/ to reflect English structure (optional)
3. Test droid invocations in Factory.ai session

## Blockers
- None

## Decisions Made This Session
- Embedded key knowledge directly in droid prompts for optimal AI usage
- Used English equivalents for all Portuguese terminology
- Maintained dual compatibility (Claude Code plugin format + Factory.ai droid format)

## Files To Know
- `.factory/droids/` — 5 Factory.ai droid configurations (delphi-expert, delphi-auditor, delphi-writer, delphi-spec-writer, delphi-tester)
- `skills/delphi-audit/` — Technical audit skill (formerly delphi-laudo)
- `skills/delphi-tests/` — DUnitX testing skill (formerly delphi-testes)
- `skills/delphi-ignore/` — .claudeignore management skill (formerly delphi-claudeignore)
- `skills/delphi-standards/references/` — 5 English reference docs for coding standards
