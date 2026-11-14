# dedupendnote-testdata

Input files for the unit and integration tests of
[DedupEndNote](https://github.com/globbestael/DedupEndNote).

## Layout

| Folder | In git? | Purpose |
|---|---|---|
| `unit/` | yes | Fixtures for the unit tests |
| `integration/` | yes | Fixtures for the integration tests (`@SpringBootTest`) |
| `validation/` | no (gitignored) | Manually validated truth files for the validation tests; large, local only |
| `other/` | no (gitignored) | Interesting files that belong to no test: copies from the production server, files sent by users, ... |

Files written by the tests next to their inputs (`*_deduplicated.txt`, `*_mark.txt`) are gitignored.

## Which version do the tests use?

The DedupEndNote repository pins one commit of this repository in its file `testdata.ref`.
CI checks out exactly that commit. Locally run, in the DedupEndNote repository:

    bin/sync-testdata.sh

The tests fail fast if your local copy is *behind* the pinned commit, and only warn if it is ahead.

## Changing a fixture

1. Change and commit the file here, and push to `main` (direct pushes, no PRs).
2. In the DedupEndNote branch that needs it, put the new commit SHA in `testdata.ref`.

Never force-push or rewrite history here: older pinned commits must stay reachable.

## Where to clone

The tests look for the data in the directory given by the system property `dedup.testdata.dir`,
then the environment variable `DEDUP_TESTDATA_DIR`, then `~/dedupendnote_input_files`.

The maintainer uses `~/dedupendnote_input_files` as the working copy of this repository, so that
`validation/` and `other/` sit next to the tracked folders. That folder is therefore **not** a
normal clone:

- **Never run `git clean -x` / `-fdx` there.** It deletes the ignored `validation/` and `other/`
  folders, which exist in no git repository (plain `git clean -fd` is safe).
- Back `validation/` and `other/` up separately.

## Files are public

This repository is public. Only add files that may be published; the records come from public
bibliographic databases such as PubMed.
