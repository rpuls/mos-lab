# mos-lab

The lab that tests changes to [My Own Suite](https://github.com/rpuls/my-own-suite). A throwaway MOS is installed on a GitHub runner from one commit, and the suite's own E2E runs against it. It has a public repository of its own because public repositories get the larger free runners, which the whole platform with every app needs.

Norn (`mos-norn[bot]`) starts it when the maintainer asks on a pull request, and posts the results there. To run it by hand, open **Actions → Lab → Run workflow** and give a commit or branch, plus either:

- E2E steps, such as `@full` or `@app-cycle immich`, which run on that commit, or on that commit merged with the branch given as `onto`, such as `staging`; or
- an app id, which installs the commit's base on `staging` and drills the update onto the commit (`@update`, then `@app-dr`).

The suite's `scripts/smoke/local-lab.cjs` does the work. The results stay in the run's `lab` artifact for 14 days.

## What it holds

The code under test runs here as root, so the job holds no GitHub token.

Its one secret is `ARES_HANDOUT_KEY`. That key fetches only what the lab hands MOS anyway: the disposable lab bucket's keys, and a DNS token for the lab zone, for apps that require HTTPS. Running the lab on a pull request hands both to that pull request's code. That makes a lab on an outside contributor's change the maintainer's call.

The key is made and set by the broker it belongs to, never by hand.
