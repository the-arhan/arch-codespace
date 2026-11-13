# Arch Linux Codespaces

A lightweight Arch Linux dev container for GitHub Codespaces. The base includes zsh, Git, Git LFS, SSH, sudo, and shell utilities. Node.js, Python, and Go toolchains are opt-in build arguments.

## Use this environment

For a Codespace from this repository, choose **Code → Codespaces → Create codespace on main**. To apply the same environment to a project repository, copy `.devcontainer/` into that repository or use this repository as the starting template. Each project repository needs its own dev container configuration; GitHub's account-wide dotfiles setting applies shell preferences, not the operating-system image.

After changing the Dockerfile or build arguments, rebuild the container in VS Code with **Codespaces: Rebuild Container**.

## Optional language toolchains

Edit `.devcontainer/devcontainer.json` and set one or more build arguments to the string `"true"`:

- `INSTALL_NODE`: Node.js and npm
- `INSTALL_PYTHON`: Python and pip
- `INSTALL_GO`: Go

They default to `"false"` to keep the base image small. Rebuild after changing them. Keep project dependencies in that project's own configuration.

## Publish to GitHub Container Registry

The workflow at `.github/workflows/publish-image.yml` builds the Dockerfile and publishes `ghcr.io/the-arhan/arch-codespace` on pushes to `main` or version tags (`v*`). It also publishes an immutable commit-SHA tag. It uses the built-in `GITHUB_TOKEN` with `packages: write`; no repository secret is required.

The first successful publish creates the package. To let other Codespaces pull the image without credentials, open the package's **Package settings** and change visibility to **Public**. If you use a private package, grant the consuming repository access and ensure Codespaces can authenticate to it.

The example devcontainer builds from the Dockerfile in this repository. To reuse the published image instead, replace its `build` property with `"image": "ghcr.io/the-arhan/arch-codespace:latest"`. Pin a SHA tag for repeatable builds when needed.

## Use the dotfiles repository

Select `the-arhan/dotfiles` in your personal GitHub **Settings → Codespaces → Dotfiles** page and enable automatic installation. This account-level setting applies to new Codespaces only. See [the dotfiles setup guide](https://github.com/the-arhan/dotfiles#enable-for-every-new-codespace).

## Design notes

- VS Code is the editor client; Codespaces provides its remote VS Code environment. The dev container selects zsh as the integrated terminal profile.
- Arch Linux is rolling release. Rebuild the image regularly to receive package updates.
- Keep language toolchains and project dependencies opt-in to reduce build and storage costs.
