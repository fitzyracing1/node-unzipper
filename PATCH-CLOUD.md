# patch-cloud build of unzipper@0.12.5

This tarball was built by patch-cloud from [fitzyracing1/node-unzipper@patch-cloud](https://github.com/fitzyracing1/node-unzipper/tree/patch-cloud),
a fork of [ZJONSSON/node-unzipper](https://github.com/ZJONSSON/node-unzipper). It is **not** an official upstream release.
Package name and version are unchanged from upstream (`unzipper@0.12.5`, base `aaf77f0 (unzipper@0.12.5 release commit)`).

## Fix

Move @aws-sdk/client-s3 from devDependencies to dependencies so Open.s3_v3 (and bundlers that statically resolve lib/Open/index.js) can find it after a normal install.

- Upstream PR: https://github.com/ZJONSSON/node-unzipper/pull/331
- Upstream issue: https://github.com/ZJONSSON/node-unzipper/issues/330
- Credit: @zachstence (PR #331 author; Co-authored-by in the fork commit)

## Exactly what changed vs upstream

- package.json: added "@aws-sdk/client-s3": "^3.620.0" to dependencies; removed it from devDependencies. No source changes.
- Trade-off: installing unzipper now also installs the AWS SDK v3 S3 client (and its dependencies).
- Added this `PATCH-CLOUD.md` and a `_patchCloud` field in `package.json` (metadata only).

## Verification

- Clean production install (no devDependencies): require.resolve('@aws-sdk/client-s3') from unzipper succeeds and unzipper.Open.s3_v3 reads a zip through a mocked S3 client; with upstream 0.12.5 it fails with "Cannot find module '@aws-sdk/client-s3'".
- Upstream test suite (tap): 107 passing, 1 pending.

## Opting out

Remove the patch-cloud `registry=` override and reinstall from registry.npmjs.org to get the upstream tarball.

## GitHub Packages (scoped name)

The same patched build can be published to GitHub Packages as `@fitzyracing1/unzipper@0.12.5`
(GitHub Packages only accepts names scoped to the repo owner). The unscoped name `unzipper`
is what the patch-cloud registry serves; the scoped name is what GitHub Packages serves.

```ini
# .npmrc (project or ~/.npmrc). GitHub Packages npm needs a token even for public packages:
@fitzyracing1:registry=https://npm.pkg.github.com
//npm.pkg.github.com/:_authToken=${GITHUB_TOKEN}   # a token with read:packages
```

```bash
# keep require('unzipper') / import 'unzipper' working via an npm alias:
npm install unzipper@npm:@fitzyracing1/unzipper@0.12.5
# or install under the scoped name:
npm install @fitzyracing1/unzipper@0.12.5
```
