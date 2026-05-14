# DNS Setup — Saturday Cheat Sheet

One-time setup. Execute in order. Total time: ~2 hours, mostly waiting on propagation.

DNS console: **Squarespace** → `account.squarespace.com` → Domains → `asperity.ai` → DNS Settings.

> **Order matters.** Some records depend on Workspace verification, some on Vercel SSL provisioning. Don't skip ahead.

---

## Phase A — Google Workspace setup (~45 min, mostly waiting)

### A1. Sign up

1. Go to `workspace.google.com` → Business Starter ($6/user/mo)
2. Use `satwant.dagar@gmail.com` as the recovery email
3. Enter domain: `asperity.ai`
4. Create primary admin user: `satwant@asperity.ai`

### A2. Verify domain ownership

Google Workspace gives you a **TXT verification record** at the end of signup. It looks like:

```
Host:  @
Type:  TXT
Value: google-site-verification=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
TTL:   3600 (1 hour)
```

Add this in Squarespace DNS. Wait 10–30 min, click "Verify" in Workspace.

### A3. Add MX records (mail routing)

Workspace gives you these. Standard Google MX records:

| Priority | Mail server                |
| -------- | -------------------------- |
| 1        | `smtp.google.com`          |

Wait — that's the v2 simplified set. Workspace now uses a single MX record. Confirm in the Workspace admin during setup. Old Google five-record MX set is deprecated.

Add in Squarespace:

```
Host:     @
Type:     MX
Priority: 1
Value:    smtp.google.com
TTL:      3600
```

Wait 10–30 min.

### A4. Create the hello@ alias

In Workspace admin (`admin.google.com`):

1. Users → Satwant Kumar → Add an alternate email address (alias) → `hello@asperity.ai`
2. Routing → enable receiving for the alias

Cost: $0 (aliases are free; only mailboxes cost a seat).

### A5. Set up DKIM

In Workspace admin:

1. Apps → Google Workspace → Gmail → **Authenticate email**
2. Click "Generate new record" — choose **2048-bit** key
3. Workspace gives you a record like:

```
Host:  google._domainkey
Type:  TXT
Value: v=DKIM1; k=rsa; p=MIIBIjANBgkqhki...VeryLongString...QIDAQAB
TTL:   3600
```

**Important** — Squarespace's UI sometimes balks at the long DKIM value. If it does:

- Try pasting as-is first.
- If rejected, paste the value in quotes: `"v=DKIM1; k=rsa; ..."`.
- If still rejected, split into two quoted strings on a single line.

Wait 30 min. Return to Workspace and click "Start authentication."

### A6. SPF record

Add a single SPF record (don't add multiple — that's invalid):

```
Host:  @
Type:  TXT
Value: v=spf1 include:_spf.google.com ~all
TTL:   3600
```

The `~all` (soft fail) is the right setting for week 1. After two weeks of clean delivery, you can tighten to `-all` (hard fail).

### A7. DMARC record

Start permissive. Strict policies break legitimate mail if anything is misconfigured.

```
Host:  _dmarc
Type:  TXT
Value: v=DMARC1; p=none; rua=mailto:satwant@asperity.ai; fo=1
TTL:   3600
```

**Sequence for tightening DMARC:**

- Week 1: `p=none` (monitor only — what we're setting now)
- Week 3 (if reports look clean): `p=quarantine; pct=25` then `pct=100`
- Week 6+ (if still clean): `p=reject`

Do **not** start at `p=reject`. You will silently lose mail.

### A8. Verify email works

1. Send a test email from `satwant@asperity.ai` to your `satwant.dagar@gmail.com`. Check it arrives and isn't in spam.
2. Reply from `satwant.dagar@gmail.com` to `satwant@asperity.ai`. Check it arrives.
3. Run `mail-tester.com` — paste their address into "To:" in Gmail, send from `satwant@asperity.ai`, then check the score. **Target: 9/10 or 10/10.** If lower, recheck SPF/DKIM/DMARC alignment.

---

## Phase B — Vercel deployment (~30 min, mostly waiting on SSL)

### B1. Create Vercel project

1. `vercel.com` → log in with the satwant.dagar@gmail.com account
2. Add New → Project → Import the `asperity-site` GitHub repo (from `github.com/Asperity-ai`)
3. Framework preset: **Other**
4. Build command: `npm run build` (already set in `vercel.json`)
5. Output directory: (leave empty — root)
6. Install command: `npm install` (auto)
7. Deploy

The first deploy lands at something like `asperity-site-xxx.vercel.app`. Verify it renders.

### B2. Add asperity.ai as a custom domain

In Vercel project → Settings → Domains:

1. Add `asperity.ai`
2. Vercel shows you the DNS records to add. For apex domains (no `www`), you'll need an **A record**:

```
Host:  @
Type:  A
Value: 76.76.21.21       (Vercel's anycast IP — confirm in Vercel UI)
TTL:   3600
```

Add it in Squarespace.

3. Wait for SSL to provision (5–30 min). Vercel auto-provisions a Let's Encrypt cert.
4. Verify `https://asperity.ai` loads.

### B3. Add sister domains as redirects

For each of `asperityrobotics.com`, `asperityrobotics.ai`, `asperityai.com`:

1. In Vercel project → Settings → Domains → Add domain
2. When prompted, choose **Redirect to** `asperity.ai` (permanent / 301)
3. Add the DNS records Vercel shows for each domain (Squarespace).
4. Wait for SSL.
5. Test: `curl -I https://asperityrobotics.com` → should return `301 Location: https://asperity.ai`.

---

## Phase C — Google Business Profile (~20 min + verification wait)

Submit Saturday morning. Verification takes 5–14 days by postcard, faster (often instant) by video call if offered.

1. `business.google.com` → Manage now → Add business
2. Business name: **Asperity Industries**
3. Business category: **Robotics Equipment Supplier** or **Industrial Automation Service** (pick whichever matches Google's options closest)
4. Add location: Yes — `7568 Princedale, Tyler, TX 75703`
5. After adding: Settings → Service area business → mark address as **not displayed publicly**
6. Service areas: Smith County, Tyler, East Texas
7. Phone: `(903) 202-0758`
8. Website: `https://asperity.ai`
9. Verification: choose video call if offered (much faster than postcard). If only postcard is offered, accept it.

---

## Phase D — Cloudflare DNS migration (optional, ~30 min)

Skip if not pursuing. The site works fine on Squarespace DNS.

If pursuing:

1. Sign up at `cloudflare.com` (free plan)
2. Add site → `asperity.ai`
3. Cloudflare scans existing DNS records (from Squarespace). Confirm all the records from Phases A and B are imported.
4. Cloudflare gives you two nameservers (e.g., `ada.ns.cloudflare.com`, `dan.ns.cloudflare.com`)
5. At Squarespace: Domains → asperity.ai → Advanced Settings → Use custom nameservers → paste Cloudflare's pair
6. Propagation takes 1–24 hours.
7. Once active, all DNS changes happen at Cloudflare, not Squarespace.

Benefits: faster global DNS lookups, free DDoS protection, free analytics, free WAF if you want it later.

---

## Phase E — Search Console + SEO submission (~15 min)

After site is live on asperity.ai:

### E1. Google Search Console

1. `search.google.com/search-console`
2. Add property → Domain property → `asperity.ai`
3. Verify via DNS TXT record (preferred — works for all subdomains):

```
Host:  @
Type:  TXT
Value: google-site-verification=YYYY...
TTL:   3600
```

4. Submit sitemap: `https://asperity.ai/sitemap.xml`

### E2. Bing Webmaster Tools

1. `bing.com/webmasters`
2. Add site
3. Import from Google Search Console (saves time)
4. Submit sitemap

### E3. Validate structured data

1. `search.google.com/test/rich-results` → paste `https://asperity.ai/`
2. Confirm Organization, Person, LocalBusiness all parse without errors

---

## Phase F — final verification checklist

After everything is live:

- [ ] `https://asperity.ai/` loads in <1.5s on mobile 4G (use Chrome DevTools throttling)
- [ ] All four domains 301 to asperity.ai
- [ ] Email from `satwant@asperity.ai` reaches a Gmail inbox without spam folder
- [ ] `mail-tester.com` score ≥ 9/10
- [ ] Google Search Console verified, sitemap submitted
- [ ] Bing Webmaster Tools verified, sitemap submitted
- [ ] Schema.org JSON-LD parses cleanly in Rich Results test
- [ ] Google Business Profile submitted (verification pending is acceptable)
- [ ] Lighthouse score: Performance ≥ 95, Accessibility ≥ 95, Best Practices ≥ 95, SEO = 100

---

## Common pitfalls

| Symptom | Likely cause | Fix |
| --- | --- | --- |
| MX records won't save in Squarespace | Existing default MX from Squarespace is there | Delete the default Squarespace MX records first |
| DKIM TXT rejected for length | Squarespace UI string-length limit | Split value into two quoted strings on one line |
| `mail-tester.com` score is 6/10 | DMARC misalignment | Confirm DKIM is active in Workspace admin (not just DNS-added — Workspace must "Start authentication") |
| Site loads but no styles | `dist/style.css` 404 | Verify Vercel build succeeded; check Vercel build logs |
| `asperity.ai` shows Squarespace parking | Old Squarespace web hosting still attached | At Squarespace: Disconnect domain from any Squarespace site |
| SSL cert pending forever | DNS not propagated yet | `dig asperity.ai` to confirm A record points to Vercel; wait |

If anything breaks during execution, the fastest debugging command is:

```sh
dig +short asperity.ai          # should return Vercel's IP
dig +short asperity.ai MX       # should return smtp.google.com
dig +short google._domainkey.asperity.ai TXT
dig +short _dmarc.asperity.ai TXT
dig +short asperity.ai TXT      # SPF lives here
```
