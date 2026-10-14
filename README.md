# email-forgery-check

Can email in your name be forged? A one-page SPF, DKIM and DMARC check that runs
entirely in the browser. No server, no sign-up, nothing stored.

**Use it:** https://stephen-psaradellis.github.io/demos/email-forgery-check.html
(or download `index.html` and open it - it is the whole tool).

## What it checks

Type a domain. The page asks public DNS-over-HTTPS resolvers (Cloudflare, falling
back to Google) for:

- **MX** - who handles the domain's mail, and which provider that is (Google
  Workspace, Microsoft 365, GoDaddy email, Zoho, Proton, Fastmail, Namecheap
  Private Email).
- **SPF** - the `v=spf1` TXT record at the domain.
- **DMARC** - the `v=DMARC1` TXT record at `_dmarc.<domain>`, and its policy
  (`none`, `quarantine`, `reject`).
- **DKIM** - a public key on 18 common selectors (`google`, `selector1`,
  `selector2`, `k1`, `s1`, `default`, `zoho`, `protonmail`, ...). A miss means
  "not on a common selector", not "no DKIM".

It then gives one plain-English verdict (anyone can send as you / receivers are
told to deliver forgeries anyway / protected), and the exact DNS records to add
for the detected mail provider: a `p=none` DMARC start with reports turned on
(or `p=reject` for a domain with no mail at all), and the step up to
`quarantine` once every real sender passes.

## Privacy

The only network requests are DNS lookups for the domain you type, sent to
`cloudflare-dns.com` and `dns.google`. Nothing is sent anywhere else and nothing
is logged by this page.

## Limits

- DKIM selectors are guessed; providers that use custom selector names show as
  "not found on the common selectors".
- It reads published records only. It does not send mail or read reports, so it
  cannot tell whether every real sender already passes - DMARC aggregate reports
  (`rua=`) are how you learn that before moving past `p=none`.

## Background

Built from a scan of 19,113 small-business mail domains: 57.2% publish no DMARC
record at all and 28.8% publish `p=none`
([the data story](https://stephen-psaradellis.github.io/demos/small-business-email-forgery-2026.html)).

Written with an AI system (Claude) under my direction; I review and maintain it.
Issues and pull requests welcome.

## Run IT for small businesses?

[MSP email-security prospects](https://stephen-psaradellis.github.io/demos/msp-email-prospects.html) lists, by metro,
the 13,096 of those businesses whose mail domain has no DMARC or only `p=none`, with the mail host each one uses.

## On WordPress?

[Mailwick Site Audit](https://github.com/Stephen-Psaradellis/mailwick-site-audit) runs the same email check
(plus SSL, updates, phone view and three more) inside Tools > Site Health.

## License

MIT
