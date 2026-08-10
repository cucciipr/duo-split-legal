# duosplit-legal

Public pages required by the App Store and Google Play. Served with GitHub Pages.

This README is for the maintainer. The four `.md` files next to it are what the public sees.

## Before the first commit

The repository is public and its history is public. Fill every placeholder **before**
committing — a value corrected later stays readable in the old commit forever.

    grep -rn '\[\[' *.md

| Token | What goes in it |
|---|---|
| `[[NAME]]` | Your full legal name. Already public: an individual Apple Developer account shows it as the seller on your App Store page. |
| `[[EMAIL]]` | Support email. **Not a personal Gmail** — this page gets crawled. Ideally on your own domain. |
| `[[DATE]]` | Effective date. Same value in both files. Use the day you publish, e.g. `12 August 2026`. |
| `[[HOST]]` | Hosting provider for the backend, e.g. `Railway`. |
| `[[REGION]]` | Where the servers are, e.g. `the United States`. Appears twice in `privacy.md`. |
| `[[JURISDICTION]]` | Governing law. See the note below before choosing. |
| `[[REPLY_TIME]]` | Honest support turnaround, e.g. `two business days`, or `as soon as we can`. |

## Publishing

1. Create a **public** repo named `duosplit-legal` (GitHub Pages needs public on the free plan).
2. Commit these files at the root.
3. Settings → Pages → Source: `main`, folder `/ (root)`.
4. Confirm all four URLs load:
   - `https://<user>.github.io/duosplit-legal/`
   - `.../privacy`
   - `.../terms`
   - `.../support`

Nothing else belongs in this repo. No `.env`, no notes, no screenshots.

## Where the URLs are used

- **App Store Connect** — Privacy Policy URL, Support URL (both required fields), and the Terms URL as your EULA.
- **Google Play Console** — Privacy Policy URL (required), plus the Data Safety form.
- **The app itself** — Settings → Help & Legal has four rows. Three open these URLs; About DuoSplit stays in-app.

Keep all three URLs in one constants file in the Flutter app, not scattered across widgets. Changing host later then means editing one place, not five.

## Decisions already made, so nobody re-opens them

**No postal address, no phone number.** EU DSA Articles 30–31 require Apple to publicly display a trader's address, phone, and email on the App Store product page for apps distributed in the EU, and there is no way to be a trader and hide the phone number. Because DuoSplit launches outside the EU, you declare **non-trader** in App Store Connect → Business, and none of that is published. The declaration itself is required globally even when you do not ship to the EU. If you ever add EU distribution, this decision reverses and you will need a publishable address and phone — a mail-forwarding service rather than your home.

**Terms of Service exists on purpose.** Apple does not require it; without one, Apple's standard EULA applies. DuoSplit supplies its own for a single reason: section 2, *DuoSplit is not a financial service*. For an app that shows balances between two people, saying plainly that it never moves money and that balances are not enforceable debts is worth more than the rest of the document combined. Do not soften section 2.

**Section 5 of both documents is deliberately blunt.** Most policies bury what a shared space exposes. Stating it before someone invites a partner is fairer to users and better protection for you: nobody can later claim they were not told that their partner sees the amounts, or that disconnecting keeps the history instead of deleting it. Both sections describe the same behaviour from different angles — if the app's sharing or deletion behaviour changes, change both.

## Still open

- **`[[JURISDICTION]]` and dispute resolution.** Section 12 is currently a plain governing-law clause with a consumer-rights carve-out. That is publishable and does not affect App Store review. What it does not include is arbitration or a class-action waiver, which are common in US consumer apps but easy to draft badly. Worth a lawyer once the app has users; not worth blocking launch.
- **Not reviewed by a lawyer.** These documents describe what the code actually does, because they were written from the codebase. The descriptive parts are accurate. The judgement parts — the liability cap in Terms section 9, governing law in section 12, the CCPA statement in Privacy section 11, and the international-transfer wording in Privacy section 7 — are not legal advice.
- **Both documents go stale if these ship:** push notifications (a device token becomes collected data) and receipt photos (image storage and deletion). The backend already has a dormant push-token field; nothing is collected while the feature is off, so the pages are accurate today.