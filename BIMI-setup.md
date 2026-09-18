# BIMI — EmpathIQ Advisors

The logo file is done. The rest of BIMI is DNS and certificates, and there is a
sequencing problem worth understanding before spending anything.

## Files

| File | Use |
|---|---|
| `assets/bimi-logo.svg` | The BIMI logo. Navy ground, mark reversed out. 885 bytes. |
| `assets/bimi-logo-light.svg` | Light-ground alternate. Not recommended — see below. |

Both are SVG Tiny Portable/Secure (`baseProfile="tiny-ps"`), 1:1 square, with a
`<title>`, no scripts, no external references, no raster images, no gradients,
and a solid background. Well under the 32 KB ceiling.

Use the **navy** version. Tested at 32, 48, 64 and 128 px with a circular crop
(how inboxes actually render it): on the light ground the mint outer ring
disappears against white at small sizes. On navy the mark stays legible at 32 px.

## The four requirements

1. **DMARC at enforcement** — `p=quarantine` or `p=reject`, `pct=100`. `p=none`
   does not qualify with any provider. Most providers also want the domain to
   have held enforcement for ~30 consecutive days before they will pull the logo.
2. **SPF and DKIM passing and aligned** with the visible From domain.
3. **A certificate** — VMC or CMC. This is the blocker. See below.
4. **A BIMI DNS record** pointing at the logo over HTTPS.

## The certificate problem

| | VMC | CMC |
|---|---|---|
| Requires | Registered trademark | 12 months of public logo use |
| Gmail logo | Yes | Yes |
| Gmail blue check | Yes | No |
| Apple Mail | Yes | No |
| Typical cost | $1,500–3,000/yr | $500–1,500/yr |

EmpathIQ currently qualifies for **neither**. There is no registered trademark,
and the brand is new, so there is no 12-month history of public logo use for a
CA to verify against web archives.

**What that means practically:** Gmail and Apple Mail will not show the logo yet.
Yahoo and AOL have historically displayed BIMI logos without requiring a
certificate, so publishing now may get display there.

**Why publish now anyway:** the CMC clock is based on public use of the logo on a
domain you control. Getting the site live with this mark starts that clock. In
roughly a year a CMC becomes available at the lower price point, with no
trademark needed.

## DNS record

Once DMARC is at enforcement and the logo is live over HTTPS:

```
Name:  default._bimi.empathiqadvisors.com
Type:  TXT
Value: v=BIMI1; l=https://empathiqadvisors.com/assets/bimi-logo.svg;
```

Add the certificate later with the `a=` tag:

```
v=BIMI1; l=https://empathiqadvisors.com/assets/bimi-logo.svg; a=https://empathiqadvisors.com/assets/bimi.pem;
```

The logo **must** be served over HTTPS. A redirect from http will fail.

## Suggested order

1. Get the site live on the real domain.
2. Publish SPF and DKIM for whatever actually sends her mail.
3. DMARC at `p=none` first, read the aggregate reports for a few weeks, confirm
   every legitimate sender passes.
4. Move to `p=quarantine`, then `p=reject`. Do not skip step 3 — going straight
   to enforcement is how legitimate mail gets silently dropped.
5. Publish the BIMI record above.
6. Revisit a CMC once the logo has ~12 months of public use.

Steps 1–4 are worth doing regardless of BIMI. DMARC enforcement is the single
best protection against someone spoofing her domain, which matters more for a
firm corresponding with pharmaceutical clients than a logo in an inbox does.
