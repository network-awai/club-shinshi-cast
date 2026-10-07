# club-shinshi-cast

The club-shinshi original cast, one character per directory. Every character
has a portable **did:webvh** identity and its profile. This repository is
the catalog ledger of the producer bot. It is not D1 and not a PDS: it is
content-addressed (git), anyone can verify it offline, and anything that
indexes it can be deleted and rebuilt from it (root ADR-2608039000,
ADR-2609132007, ADR-2608209300).

```
actors/<slug>/did.jsonl     did:webvh v1.0 log (genesis + any later rotations)
actors/<slug>/profile.json  the character's modelProfile, as the factory defined it
```

- **DID**: `did:webvh:<SCID>:shinshi.club:actors:<slug>`, which resolves to
  `https://shinshi.club/actors/<slug>/did.jsonl` once shinshi.club serves this
  tree. `portable: true` is set at genesis, so the DID survives a move to
  another host.
- **Who signs**: `updateKeys` holds the producer's webvh update key. Its seed
  is in the macOS Keychain (`shinshi.club/webvh-update/current`), separate
  from the aozora posting key. `nextKeyHashes` pre-commits the next key
  (`shinshi.club/webvh-update/next`), so a stolen current key cannot choose
  its successor.
- **Profile binding**: the DID document's `#profile` service carries
  `digestMultibase`. This is the base58btc sha256 multihash of the JCS form
  of `profile.json`. A profile edited without a new log entry no longer
  matches its DID.
- **Who is a character**: the deterministic factory (club-shinshi-app
  `tools/char-factory.cljk`) is the only authority. Indices `[0, 2000)` are
  the D1-era catalog. This repository holds the characters from index 2000
  onward.

Written by `scripts/shinshi-cast-bots/produce.cljk` in com-junkawasaki/root,
in one round per ISO week. Each round makes one commit that verifies before
it is pushed.
