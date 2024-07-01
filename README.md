A docker image supporting [GitHub Codespaces](https://github.com/features/codespaces) and [local devcontainers](https://containers.dev) for UCEAP Drupal projects, based on [Microsoft's PHP image](https://github.com/devcontainers/images/tree/main/src/php).

See [Local development for UCEAP Drupal projects](https://github.com/UCEAP/.github-private/wiki/Local-development-for-UCEAP-Drupal-projects) in the UCEAP Software Engineering wiki for more information on getting started.

## Personalization

Devcontainers support dotfiles!

See the [GitHub documentation](https://docs.github.com/en/codespaces/setting-your-user-preferences/personalizing-github-codespaces-for-your-account#dotfiles) for more info, and check out [Brandt's personal dotfiles](https://github.com/kurowski/dotfiles) for an example.

## Quality of life

This image includes several scripts that integrate with the devcontainer lifecycle, but these can also be used independently:

* `/usr/local/bin/uceap-drupal-dev-on-create`
* `/usr/local/bin/uceap-drupal-dev-post-create`
* `/usr/local/bin/uceap-drupal-dev-post-start`
* `/usr/local/bin/uceap-drupal-dev-update-content`

I frequently invoke `uceap-drupal-dev-update-content` to reset my local environment after switching branches. It runs `composer install` and invokes `db-rebuild.sh` with a fresh copy of the latest snapshot of the dev environment database and files. With zsh completions installed, it's as easy as `dev-up<TAB>`.

> 👉 When working on a PR that adds update hooks or makes config changes, it's generally a good idea to make sure it applies cleanly to a database matching the QA environment. To do this, switch to the `qa` branch, run update-content, switch back to your branch, and run the deploy command (e.g. `drush md` for the portal):
> ``` zsh
> git checkout qa
> uceap-drupal-dev-update-content
> git checkout -
> composer install
> drush $DRUSH_TASK
> ```

Sometimes a process can die or port forwarding can fail. `uceap-drupal-dev-post-start` runs a few commands that should get things working again. (Again, zsh shell completion makes this `post-s<TAB>`).

Using devcontainers faciliates treating local environments as ephemeral: they're quick and easy to setup. Treat them as safe to destroy because you can always create a new one (or multiple new ones, to suit your needs). One thing you might miss is your shell history. Check out [Atuin](https://atuin.sh/) to sync your shell history across environments. `Control-R` has never looked so good 😎