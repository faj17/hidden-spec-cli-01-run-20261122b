# mend — one patch tool for every AI edit format

`mend` applies an edit that an AI model wrote to a tree of text files. Models write edits
in four formats, and `mend` reads all four to one contract (`SPEC.md`):

- unified diffs (often with wrong line numbers),
- SEARCH/REPLACE blocks,
- V4A patches,
- text-editor tool calls as JSON.

It finds the lines an edit names at the first of three matching tiers — exact, trailing
whitespace ignored, leading and trailing whitespace ignored — and changes every file or
none, printing what it changed or why it failed.

## Run

Node 22, standard library only, nothing to install:

```
node main.ts [--format=NAME] [--check] [--strict] [PATCH]
```

`PATCH` is the patch file, or `-` / absent for standard input. `--format` is `auto`
(default), `unified`, `search-replace`, `v4a` or `json`. `--check` validates without
writing. `--strict` matches lines exactly (tier 0 only).

Exit 0 prints one line per changed path (`A`/`D`/`M`, `R old -> new` for moves) and
`ok: <n> files`. Exit 1 lists each failed edit and `failed: <k> of <n> edits`, and
writes nothing. Exit 2 is a usage, patch-read, format or parse error, one line.

## Files

- `main.ts` — the CLI.
- `runner.ts` — the kit's trial runner (unchanged); `main.ts` hands a trial over to it
  when `MAKEFUN_ENV_URL` is set.
- `check.ts` — runs the public practice set: `node check.ts <practice.json>`.
- `SPEC.md` — the whole contract.
- `LICENSE` — MIT.

## Practice score

100 of 100 practice checks pass (practice score 1000), verified with the kit's
`check.ts` against `program/generator/practice.json`.
