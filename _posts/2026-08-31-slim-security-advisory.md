---
title: "Security Advisory: Route constraint bypass >= 4.4.0, <= 4.15.2"
description: A route parameter constraint can be bypassed with doubly percent-encoded input inSlim 4.4.0 to 4.15.2 inclusive. Please update to Slim 4.15.3 to resolve this issue.
layout: post
---

A security issue has recently been reported in Slim's routing component that allows a route parameter constraint to be bypassed via double percent-encoded input.

### Impact

A route parameter constraint can be bypassed with doubly percent-encoded input, so a handler receives characters that the router was configured to reject.

Slim (4.0.0-4.15.2) decodes the request path before matching a route, then decodes each captured argument a second time before passing it to the handler. This means that validation will run against a different value than the one the application receives.

Given the route `/test/{value:[^/]+}`, which forbids a path separator:

| Stage | Value |
| --- | --- |
| Incoming request | `/test/a%252Fb` |
| Path decoded, before matching | `a%2Fb` |
| Checked against `[^/]+` | passes, no separator present |
| Argument decoded again, at the handler | `a/b` |

Singly-encoded input is not affected. `/test/a%2Fb` is a 404, since the decode happens before matching and the resulting separator breaks the capture. Only doubly-encoded input reaches a handler in a different form than the router validated.

You are affected if you run Slim 4.0.0 through 4.15.2 *and* use route placeholder values without revalidating them.

Severity: Medium — CVSS 6.5 (`CVSS:3.1/AV:N/AC:L/PR:N/UI:N/S:U/C:L/I:L/A:N`)


## Affected versions

All versions from 4.0.0 to 4.15.2 inclusive are affected.

### Patches

The issue is fixed in 4.15.3.

### Workarounds

Without upgrading, applications can revalidate route placeholder parameters in the handler and not rely on the route pattern. Reject any argument containing a path separator before using it.

### Acknowledgments

We are grateful to and thank [NormanProdham](https://github.com/NomanProdhan) and [Ilia Alshanetsky](https://github.com/iliaal) for independently reporting this issue.


## Further information

* [GHSA-h377-p8x2-prf9 advisory on GitHub](https://github.com/slimphp/Slim/security/advisories/GHSA-h377-p8x2-prf9)

