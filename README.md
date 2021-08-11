# Multi-Repo Codespaces Example

Generally when you are dealing with a multi-repository application, you end up with a primary "bootstrap" repository that contaains getting started information and local configuraiton files.

GitHub Codespaces conceptually fits well into this model since you could create a Codespace from this boostrap and then clone other repositories into it. However, to improve security, Codespaces currelty provides a repository scoped token by default. However, you can use SSH keys or a personal access token instead. This sample illustrates how to configure a user secret and assign it to a Codespace so that it is used instead of the default `GITHUB_TOKEN`.



