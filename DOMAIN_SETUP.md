# encrypteverything.tech — GitHub Pages DNS handoff

Prepared 2026-08-26 from GitHub's current Pages documentation. The live site remains at
<https://xalisher.github.io/encrypt-everything-promo/> until this handoff is applied and verified.

## Remove current parking records

- Remove the apex `A` record pointing to Namecheap parking (`192.64.119.209`).
- Remove the `www` CNAME pointing to `parkingpage.namecheap.com`.
- Do not add a wildcard record.

Keep unrelated mail/SPF records unless you intentionally want to change email forwarding.

## Add these records

Use host `@` for the apex if Namecheap asks for a host. A TTL of 5 minutes during migration is
helpful; it can be raised after HTTPS is stable.

| Type | Host | Value |
|---|---|---|
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| AAAA | `@` | `2606:50c0:8000::153` |
| AAAA | `@` | `2606:50c0:8001::153` |
| AAAA | `@` | `2606:50c0:8002::153` |
| AAAA | `@` | `2606:50c0:8003::153` |
| CNAME | `www` | `xalisher.github.io` |

Source: [GitHub Docs — Managing a custom domain for your GitHub Pages site](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site).

## Coordinated activation

Reply after applying the records. The release agent will then:

1. Run `scripts/check-custom-domain`; it exits `2` while DNS is pending and exits `0` only after
   the exact A/AAAA/CNAME records resolve and HTTPS serves the expected site.
2. Set the Pages custom domain to `encrypteverything.tech`, adding the root `CNAME` file.
3. Wait for GitHub's TLS certificate to be issued.
4. Enable HTTPS enforcement.
5. Verify apex and `www`, HTTP-to-HTTPS and canonical redirects, certificate names, assets, and the
   public download CTA.

GitHub recommends configuring the custom domain in Pages before DNS to prevent takeover. For this
coordinated handoff, domain verification and the Pages setting should be completed adjacent to the
DNS update so the currently working `github.io` site has minimal disruption.
