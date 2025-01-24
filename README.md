# fluxcd-demo-apps
A repository of example apps deployed and managed using Flux CD

## Creating Sealed Secrets

We assume the use of sealed secrets.

TODO: add more instructions!

## How to install

The host cluster must have the [Flux CD](https://fluxcd.io/) controllers installed.

Configuring Flux to manage the apps defined in the repository is a one-time operation:

```sh
flux create source git myapps --url=<giturl> --branch=main
flux create kustomization myapps --source=GitRepository/myapps --prune=true
```
