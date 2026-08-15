# annaeus-site — working notes

Static one-page author site for the **Annaeus** pen name, served on GitHub Pages at
`annaeus.de` (see `CNAME`).

## Pushing — use `git pushx`, not `git push`

This machine's git auth routes through the GitHub CLI, and `gh` only ever serves the
**active** account. The active account is normally `PinkameanaDianePy` (personal). This
repo belongs to the **AnnaeusBooks** account, kept deliberately separate from the personal
identity. A plain `git push` therefore either 403s or pushes under the wrong account.

Push with:

```bash
git pushx
```

It's a repo-local alias that switches the active `gh` account to AnnaeusBooks, pushes,
then switches back to PinkameanaDianePy (even if the push fails). Everything else on the
machine stays on the personal account.

If `git pushx` is ever missing (fresh clone), recreate it:

```bash
git config --local alias.pushx '!f() { gh auth switch --user AnnaeusBooks; git push "$@"; rc=$?; gh auth switch --user PinkameanaDianePy; return $rc; }; f'
```

## Identity settings for this repo (already set, local scope)

- `user.name`  = `AnnaeusBooks`
- `user.email` = `317307045+AnnaeusBooks@users.noreply.github.com` (GitHub no-reply — keeps
  the personal email off pen-name commits)
- `remote.origin.url` = `https://AnnaeusBooks@github.com/AnnaeusBooks/annaeus-site.git`

Do **not** cross-grant collaborator access between the personal and AnnaeusBooks accounts —
that re-mingles the two identities, which is the thing this setup exists to avoid.

## Mailing list

Signup posts to MailerLite. The form carries a hidden `fields[language]` value so
subscribers are tagged by language at signup (`en` today; a future `/de` page's form would
use `de`). This can't be backfilled later, so the field must stay on every signup form.
Confirmation emails send from `hello@news.annaeus.de` (subdomain, own SPF/DKIM — never
touches the daily-use IONOS mailbox).
