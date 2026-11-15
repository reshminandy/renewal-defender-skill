# Renewal Defender

**Tired of reading blogs? Watch this one instead.**

Everyone is writing a blog these days, and reading them all is tiring. So start with the picture. The concept animation runs for about a minute and shows the whole idea: one bar of deployed hours, explained piece by piece, until you can see where the effort leaked and who owns it.

1. **Watch it.** Open [`visual-concept/renewal-defender-concept.html`](visual-concept/renewal-defender-concept.html), download it with the button at the top right of the file, open it in your browser and press Play. It is one file, with the animated JavaScript inside it.
2. **Liked the concept? Read the blog.** [The blog](blog/renewal-defender-blog.md) covers the method, the data form and what to do when data is missing.
3. **Convinced? Run it.** Load [`SKILL.md`](renewal-defender/SKILL.md) into your AI chat window, fill in the data form with whatever you have, and ask for the reconciliation of the last three months. The same text is in [`SKILL.txt`](renewal-defender/SKILL.txt) if a markdown file is awkward to download.

## What is in this repository

| Folder | Contents |
| --- | --- |
| [`visual-concept/`](visual-concept) | The animated concept model: six steps, from the data form to the attack sheet |
| [`blog/`](blog) | The full blog: the method, the data form, the risks and the funding case |
| [`renewal-defender/`](renewal-defender) | The skill itself, as `SKILL.md` and as `SKILL.txt` |

## What it does

For an account coming up for renewal, the skill reconciles the hours deployed against the work produced over the last 12, 6 and 3 months. The hours that do not reconcile are leaked effort. The skill attributes them to a cause and an owner: the vendor, the customer or a third party. It then writes the attack sheet a challenger would build from the same facts, with one honest defence for each line.

## Two things it will not do

- Assume a productivity norm. You supply the norm in force today, or the skill reports actual hours per unit and stops.
- Invent a counter-proof. A defence is marked Prove only when your own data contains the evidence.

## Make it better

If your account has a leak cause that is not on the list, or a data check the skill misses, open an issue or send a pull request. Say what you changed and what it caught. Keep client names and client data out of anything you post here.

Version 2.0, November 2026. Reshmi Nandy.
