# pdn-fbb

FBB BBS forwarding for .NET, published on nuget.org as **Packet.Fbb** (namespace `Packet.Fbb`).

It covers compressed forwarding (B1F, and B2F objects) on both the calling and the answering side: the SID, proposals and FS lines, block framing, LZHUF, R: lines, B2 messages and the header text codec. The session is a sans-IO state machine (`FbbSession`): you feed it lines and bytes from whatever transport you have, and it tells you what to send and what it received. It has no dependencies beyond .NET 10.

## Who uses it

- [pdn-bbs](https://github.com/packet-net/pdn-bbs), the packet BBS app for the Packet.NET node, to forward mail with other BBSes.
- [pdn-mailcast](https://github.com/packet-net/pdn-mailcast), whose receiver hands bulletins to the local BBS and whose head end takes them from one.

## Where it came from

This code was `src/Bbs.Fbb` (with `PacketText` from `src/Bbs.Core`) and `tests/Bbs.Fbb.Tests` in pdn-bbs, moved here at pdn-bbs commit `8f8b000b054adedb6efdfb4c0cce57ad7234bc40`; see that repo for its history. On the way the namespace changed from `Bbs.Fbb` to `Packet.Fbb` and dashes in comments became hyphens. Nothing else changed.

## Build and test

```
dotnet build
dotnet test
```

## Releasing

Tag a commit on main with its full SHA and push the tag:

```
git tag v0.2.0 <full-commit-sha>
git push origin v0.2.0
```

`release.yml` runs the tests, packs `Packet.Fbb` at the tag's version, and attaches the `.nupkg` and `SHA256SUMS` to a GitHub Release. It then starts `nuget.yml`, which pushes that `.nupkg` to nuget.org with NuGet trusted publishing, so there is no API key anywhere. nuget.org's trusted publishing policy names `nuget.yml`, so keep its name. To push an existing release's package by hand: `gh workflow run nuget.yml -f tag=v0.1.0`.

## Licence

AGPL-3.0-or-later; see [LICENSE](LICENSE).
