# Verifying the dogfood receipt

```bash
export RUNX_RECEIPT_VERIFY_KID=epistemedeus-prospect-seq
export RUNX_RECEIPT_VERIFY_ED25519_PUBLIC_KEY_BASE64=cn60NwEwk2fj07Se3C+TaQV34vZ2rFVPz1PZliT2yt8=
runx verify --receipt receipt.json --json
```

Expected: `"valid": true` (digest valid, signature valid). Public key only — no secret.
