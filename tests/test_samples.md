# Test Results
Python version used: 3.13.3 | Operating system: Windows | Date: 07-11-2026

| # | Input | Expected result | Actual result | Pass/Fail |
|---|-------|-----------------|---------------|-----------|
| 1 | https://www.example.com/login | No listed warning signs | No listed warning signs found. This does NOT prove safety. | PASS |
| 2 | http://192.0.2.10/account | IP address + HTTP | Uses an IP address instead of a familiar domain; Uses HTTP | PASS |
| 3 | http://example.com@sample.invalid/login | @ symbol + HTTP | Contains an @ symbol; Uses HTTP | PASS |
| 4 | https://a.b.c.d.e.example.invalid/path | Many hostname segments | Contains many hostname segments/subdomains | PASS |
| 5 | https://xn--example.invalid/ | Punycode-style hostname | Contains a punycode-style hostname | PASS |

## Bonus Checks

| Input | Result |
|-------|--------|
| (empty, just Enter) | No URL entered. |
| www.example.com | Uses HTTP. HTTPS alone does not prove trust. |
| https://secure-login.example.invalid | No listed warning signs found. This does NOT prove safety. |
| http://[bad (original program) | Crashed with ValueError: Invalid IPv6 URL |
| http://[bad (after fix) | This text is not a valid URL format. |

## Notes

**What I learned**
- The program only reads the text of a URL and never opens it. It checks six warning signs: an @ symbol, a long URL, an IP-address hostname, many hostname segments, punycode (xn--) and the HTTP scheme.
- A URL typed without http:// or https:// is treated as HTTP, so it gets the HTTP warning.

**What surprised me**
- https://www.example.com/login showed no warning signs, but that does not prove the link is safe. The tool only gives clues, so a dangerous link can still pass.
- https://secure-login.example.invalid also showed no warning signs even though the name looks suspicious. No warnings does not mean safe.

**Problem found**
- http://[bad crashed the original program with "ValueError: Invalid IPv6 URL" at the urlparse() line. This was a real gap in my code.

**Fixes and re-testing**
- Improvement C: I wrapped urlparse() in try/except ValueError. Now http://[bad prints "This text is not a valid URL format." instead of crashing.
- Improvement A: I added a loop so I can test many URLs in one run and quit by typing q, quit or exit.
- After the changes I re-ran all five tests and http://[bad. All five still passed, the crash was gone, and typing q closed the program with "Goodbye! Stay alert online."


