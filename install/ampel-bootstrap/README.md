# ampel-bootstrap

This action is the trust anchor for all Carabiner tool installers. It downloads
a **pinned** version of the ampel binary and verifies its integrity against
hardcoded SHA-256 hashes before making it available for use.

Other installer actions (ampel, bnd) use the bootstrap binary to verify their
downloads against SLSA provenance attestations. The trust chain is:

```
caller pins action @SHA  ─>  hardcoded ampel SHA-256  ─>  bootstrap ampel verifies target via SLSA provenance
```

## Why hardcoded hashes?

ampel is the tool that verifies binary integrity, but it can't verify itself
before it exists on the runner. The hardcoded SHA-256 hashes break this
chicken-and-egg problem. The hashes are trusted because the action is pinned by
commit SHA — anyone referencing `step-security/carabiner-dev-actions/install/ampel-bootstrap@<sha>`
is explicitly trusting the hashes at that commit.
