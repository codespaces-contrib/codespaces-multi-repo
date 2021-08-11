# Multi-Repo Codespaces Example

Generally when you are dealing with a multi-repository application, you end up with a primary "bootstrap" repository that contaains getting started information and local configuraiton files.

GitHub Codespaces conceptually fits well into this model since you could create a Codespace from this boostrap and then clone other repositories into it. However, to improve security, Codespaces currelty provides a repository scoped token by default. However, you can use SSH keys or a personal access token instead. This sample illustrates how to configure a user secret and assign it to a Codespace so that it is used instead of the default `GITHUB_TOKEN`.

The script here will clone multiple repisotires and place them under the `/workspaces` folder in the Codespace. Anything this folder here will survive a rebuild, so this is safe to do. 

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
1. Save the workspace foor future use using **File > Save Workspace As...** and place it under `/workspaces/codespace-multi-reepo` (or whatever you named your bootstrap repository).
1. Commit and push the change to the bootstrap repository.
