# Multi-Repo Codespaces Example

When working with a multi-repository application, you often have a primary "bootstrap" repository that contains getting started information and local configuration files. You may also have a repository for your personal work that includes configuration or scripts that you use to create set up a local environment. In both cases, you may then clone multiple repositories onto your local machine that make up your application or multiple personal repositories.

GitHub Codespaces can conceptually work with this this model as well since you can create a codespace from this bootstrap repository and then clone other repositories you need into it. However, to improve security, Codespaces currently limits access to other repositories through a repository scoped token by default. While you can expect improvements here in time, the good news is that you can use a personal access token instead to work around this limitation today. This sample illustrates how to add a personal access token to a user-specific secret in GitHub, and assign it to a codespace for use instead of the default `GITHUB_TOKEN`.

> **Note:** Part of the reason Codespaces defaults to repo scoped tokens is to limit the impact of your codespace being unknowingly compromised. For example, a repository you cloned could include a malicious tool that triggers on build. Using a personal access token as described here expands what a malicious process or stolen token can access, so only do this with repositories you trust, and consider minimizing privileges you give the token to only what you actually need.

The way this sample works is by using `postCreateCommand` in `.devcontainer/devcontainer.json` to execute `clone-repos.sh`. This script will change the git credential manager to use your personal access token, and automatically clone any repositories in `repos-to-clone.list` under the `/workspaces` folder in the codespace.

Follow these steps to try it out:

1. Fork this repository
1. Create a GitHub personal access token for use in these situations.
1. Create a Codespaces user secret called `GH_USER` with your GitHub user and `GH_TOKEN` with the personal access token you created in it. (To set to these secrets, from github.com, click on your picture in upper-right > Settings > Codespaces tab > New secret button)
1. Give your forked repository access to the secrets when you create the secrets.
1. Update `repos-to-clone.list` in your fork with any repositories you want to automatically clone using this token. You can always clone manually later if you prefer.
1. Create a codespace from your fork.
1. After the "postCreateCommand" has finished, use **File > Open Folder...** in VS Code and go up one level (to `/workspaces`) to find the cloned repositories. 

## Optional: Multi-Root Workspace Setup
If you prefer, you can configure a multi-root workspace that you check into this bootstrap repository. This makes it easier to work with multiple repositories at the same time.

1. After following the steps above, go to **File > Add Folder to Workspace...** and select a folder.
1. After VS Code reloads, you'll see both the bootstrap repository and the cloned repository.
1. Save the workspace for future by going to **File > Save Workspace As...** and selecting a location under `/workspaces/codespace-multi-repo` (or whatever you named your bootstrap repository).
1. Commit and push the change to the bootstrap repository.

## How it works

The main trick is two lines in `clone-repos.sh` that removes the default credential manager and adds a different one:

```bash
sudo sed -i -E 's/helper =.*//' /etc/gitconfig
git config --global credential.helper '!f() { sleep 1; echo "username=${GH_USER}"; echo "password=${GH_TOKEN}"; }; f'
```

The `.devcontainer/devcontainer.json` file then includes a `postCreateCommand` to execute the script.

```json
{
    "postCreateCommand": "bash clone-repos.sh"
}
```

You can add this to any other `devcontainer.json` - this just uses the default Codespaces image rather than a custom one.
