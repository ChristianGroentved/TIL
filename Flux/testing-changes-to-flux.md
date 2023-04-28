# How to test changes to flux

1. Create a branch on the flux repo
   1. if you want to test changes on eg. `external/dev` clusters then go into `external/dev/flux-system` and edit `gotk-sync.yaml`
   2. Change branch from `master` to your desired branch on the `gitrepository` resource. Otherwise flux will switch back to master
2. Apply changes to the branch
3. In terminal switch to the cluster where you want to test changes
4. run `k edit gitrepositories.source.toolkit.fluxcd.io -n flux-system flux-system` to edit the flux configuration
5. In the file find `branch` and change it from `master` to your desired branch
6. In the cluster run `flux reconcile source git flux-system` to get flux to reconcile with the new branch
7. Remeber to revert the changes to the flux configuration when you are done testing.