## Abstract

In general, if encountering PSBT fields that are unrecognized, to avoid costly
mistakes it is best to fail and not assume that those fields are backwards
compatible.

However, some information, for example BIP 353 `PSBT_OUT_DNSSEC_PROOF`, and may
safely be ignored by devices that in this case might not able verify the BIP
353 DNSSEC signature chain.

## Specification

Top level `PSBT_GLOBAL_RULESETS`. keydata contains identifier e.g. `BIP <n>`
for well known protocols or arbitrary domain separators (encoded like
proprietary keys or or as hashes). rationale: this allows definition of machine
readable schema of rules that are supposed to be enforced, something which may
not be self evident from just the fields depending on the state of the PSBT.

Top level `PSBT_GLOBAL_OPTIONAL_FIELDS` - keydata identifying the fields that
are safe to ignore. these are presumed to correspond to the active rulesets.

---

active BIPs or domain separators could refer script related ones, which
suggests the possibility of adding global policy scripts (as keydata to a
single global field) that need to validate before signing, with the value
optionally being used to specify evaluation rules or provide witness data. For
example if simplicity is used to specify a zk proof validator, then signers
should only sign if this evaluates to true. signaling such policy rules in a
consistent manner would allow relatively safe interoperability even for
conceivably quite complicated scripts, and facilitates experimentation by
providing a generic procedure for multisig covenant emulation by all input
signers sharing a PSBT.
