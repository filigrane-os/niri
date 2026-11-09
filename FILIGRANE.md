# niri for Filigrane

This fork carries one patch on top of the niri release that Void Linux ships. It is built as the `fili-niri` package
of [Filigrane](https://github.com/filigrane-os/filigrane) (`distro/niri/`), which replaces Void's `niri` package.
It is not a general-purpose fork: it exists only until niri fixes the bug below, then Filigrane goes back to niri.

## The bug

On GPUs with few CRTCs (three on Intel Ice Lake, e.g. the T2 MacBooks), a laptop panel turned off by niri
(`output "eDP-1" { off }` or `niri msg output eDP-1 off`) keeps its CRTC reserved, because the kernel still reports
it as connected. The third monitor of a DP-MST dock then gets no CRTC and is silently ignored: it shows a copy of
another monitor until niri is restarted. Upstream issue: [niri-wm/niri#2691](https://github.com/niri-wm/niri/issues/2691).

## The patch

Branch `filigrane`, one commit on top of the release tag (see its message). The CRTC of a connector that niri keeps
off is lent to a connected connector that has none; it also includes the fix of Smithay/smithay#2076 for niri
releases that predate it.

The patch was written with the help of an AI assistant and tested on a T2 MacBook with a three-monitor dock. It is
not proposed upstream: niri's [CONTRIBUTING.md](CONTRIBUTING.md) does not accept LLM-written code.

## Updates

Filigrane's CI rebuilds `fili-niri` when Void updates niri: the commit is rebased on the new release tag. If it no
longer applies or no longer builds, an issue is opened in the Filigrane repository and the previous `fili-niri` stays
in place. When #2691 is closed, another issue says to try plain niri again.
