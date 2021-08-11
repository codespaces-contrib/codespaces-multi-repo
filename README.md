# Multi-Repo Codespaces Example

Generally when you are dealing with a multi-repository application, you end up with a primary "bootstrap" repository that contaains getting started information and local configuraiton files.

GitHub Codespaces conceptually fits well into this model since you could create a Codespace from this boostrap and then clone other repositories into it. However, to improve security, Codespaces currelty provides a repository scoped token by default. However, you can use SSH keys or a personal access token instead. This sample illustrates how to configure a user secret and assign it to a Codespace so that it is used instead of the default `GITHUB_TOKEN`.

Follow these steps to use it:

1. Fork this repository
1. Create a GitHub personal access token for use in these situations.
1. Create a Codespaces user secret called `GH_USER` with the appropriate GitHub user and `GH_TOKEN` with the personal access token in it. (Your picture in upper-right > Settings, Codespaces, New secret)
1. Give your forked repository access to the secrets
1. Update `repos-to-clone.list` with any repositories you want to clone using this token and push it to the repository.
1. Create a Codespace
1. After the "postCreateCommand" has finished, use **File > Open...** in VS Code and go up one level (to `/workspaces`) to find the cloned repositories.  You can also use a multi-root workspace that you check into this bootstrap repository.

Anything under the `/workspaces` folder will survive a rebuild.