# Multi-Repo Codespaces Example

When working with a multi-repository application, you ofteen have a primary "bootstrap" repository that contains getting started information and local configuraiton files. You may also have a repository for your personal work with configuration set up in it that you use to create a local environment. In both cases, you may then clone multiple repositories onto your local machine that make up your application or multiple personal repositories.

GitHub Codespaces can conceptually work with this this model as well since you could create a Codespace from this boostrap repository and then clone other repositories into the resulting codespace. However, to improve security, Codespaces crrently limits access to other repositories through a repository scoped token by default. The good news is that you can use a personal access token instead to work around this limitation. This sample illustrates how to add a personal access token to a user-specific secret in GitHub, and assign it to a codespace for use instead of the default `GITHUB_TOKEN`.

> **Note:** Part of the reason Codespaces defaults to repo scoped tokens that it limits the impact of your codespace being unknowlingly compromised. For example, maybe a repository you cloned includes a malicious tool triggered on build. Using a personal access token as described here expands what malicious process or stoken token can access, so only do this with repositories you trust, and only add acces you need to the token.

The way this works is by using `postCreateCommand` in `.devcontainer/devcontainer.json` to execute `clone-repos.sh`. This script will change the git credential manager to use your personal access token, and automatically clone any repositories in `repos-to-clone.list` under the `/workspaces` folder in the Codespace. Anything this folder here will survive a rebuild, so this is safe to do.

Follow these steps to use this sample:

1. Fork this repository
1. Create a GitHub personal access token for use in these situations.
1. Create a Codespaces user secret called `GH_USER` with your GitHub user and `GH_TOKEN` with the personal access token you created in it. (To get to these secrets, click on your picture in upper-right > Settings > Codespaces tab > New secret button)
1. Give your forked repository access to the secrets when you set them up.
1. Update `repos-to-clone.list` in your fork with any repositories you want to automatically clone using this token.
1. Create a Codespace
1. After the "postCreateCommand" has finished, use **File > Open Folder...** in VS Code and go up one level (to `/workspaces`) to find the cloned repositories. 

## Optional: Multi-Root Workspace Setup
If you prefer, you can configure a multi-root workspace that you check into this bootstrap repository so that you can work with multiple repositoriies at the same time.

1. After following the steps above, go to **File > Add Folder to Workspace...** and select a folder.
1. After VS Code reloads, you'll see both the bootstrap repository and the cloned repository.
1. Save the workspace foor future use using **File > Save Workspace As...** and place it under `/workspaces/codespace-multi-repo` (or whatever you named your bootstrap repository).
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
