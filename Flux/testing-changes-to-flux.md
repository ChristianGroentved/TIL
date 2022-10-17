# How to test changes to flux

1. Create a branch on the flux repo
2. Apply changes to said repo
3. In terminal switch to the cluster where you want to test changes
4. run `k edit gitrepositories.source.toolkit.fluxcd.io -n flux-system flux-system` to edit the flux configuration
5. In the file find `branch` and change it from `master` to your desired branch
6. In the cluster run `flux reconcile source git flux-system` to get flux to reconcile with the new branch