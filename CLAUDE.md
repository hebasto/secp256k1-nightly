# Conventions for this repository

## Git

- Always set both the commit author and the committer to `GingerAgent <345745614+GingerAgent@users.noreply.github.com>`, whether or not the request says so. Never mention the authorship in the PR description or anywhere else; it follows from the commit metadata.
- Name branches `YYMMDD-topic`, e.g. `261019-gcc-snapshot`, and base them on the current `main`.
- Push to the `YYMMDD-topic` branch and open the PR from it, even when the session assigns a `claude/...` branch; this convention wins over the session default.
- Before pushing to an existing PR branch or editing a PR's title or description, read the PR's current state; GitHub accepts both on a closed PR without complaint. Do this every time, as a PR may have been closed or merged since it was last looked at.
- A merged or closed PR and its branch are left untouched. Follow-ups go on a new branch from `main` with a new PR.
- Rewriting history on an unmerged PR branch is fine (`git push --force-with-lease`).
- Fold small follow-ups (review fixes, tweaks on the same lines) into the commit they belong to instead of adding new commits; a new commit is for a distinct change.

## Workflows

- Invoke GNU Make as `gmake`.
- Pin runner labels explicitly (e.g. `ubuntu-26.04`, `ubuntu-26.04-arm`) for jobs that build on the host. Jobs that run in a `container:` or boot a VM via vmactions use `ubuntu-latest`, since the host image does not affect the build.
- Nightly workflows chain on `Manager` via `workflow_run` and report to CDash. `openbsd.yml` is a convenient starting point for a new workflow, but layout may differ per platform; what every CDash job must satisfy is:
  - it builds `bitcoin-core/secp256k1` master, or a PR merge ref while a platform fix is pending upstream, and reports the underlying master commit to CDash via `ctest_update()`; the commit ID is read from the pax global header of the source tarball, as the source tree does not record it;
  - it sets `CTEST_SUBMIT_URL` to `https://cdash.fish.foo/submit.php?project=secp256k1` and `CTEST_NIGHTLY_START_TIME` to `00:00:00 UTC` in its CTest script, as the source tree ships no `CTestConfig.cmake`;
  - its `ctest_update()` uses the shared form, `set(CTEST_UPDATE_COMMAND not-run/git)` plus `CTEST_UPDATE_VERSION_OVERRIDE`, never `CTEST_GIT_COMMAND` or `CTEST_UPDATE_VERSION_ONLY`: a real git would list local modifications and report a patched tree as an update error;
  - it starts the `Nightly` group only when chained from `Manager`, and `Experimental` otherwise;
  - configure, build, and test failures each fail the job (`ctest_test()` needs the `RETURN_VALUE` check);
  - the submit step runs with `if: always()`;
  - the job name and `CTEST_BUILD_NAME` are identical and name every matrix dimension (release, `static` or `shared`) and any disabled module or feature.
- `README.md` mirrors the workflows: the releases column lists the matrix entries, the notes column names what a job covers beyond the defaults or does not build or test, and toolchain versions appear where the notes pin them. A change to any of those in a workflow updates the matching README row in the same commit; dropping a feature from a job name or `CTEST_BUILD_NAME` drops it from the README too.
- Attach the compiler's `-v` output as a CDash note (`CTEST_NOTES_FILES`) only where the toolchain is not fixed by the build name: compiler snapshots, rolling releases, and compilers from third-party package repositories. For pinned OS releases, CMake's compiler identification in the configure output is enough.
