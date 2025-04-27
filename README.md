# Run NixOS on a Lima VM
Heavily inspired from [patryk4815/ctftools](https://github.com/patryk4815/ctftools/tree/master/lima-vm)

## Generating the image
On a linux machine or ubuntu lima vm for example:

```bash
# install nix
sh <(curl -L https://nixos.org/nix/install) --daemon
# enable kvm feature
echo "system-features = nixos-test benchmark big-parallel kvm" >> /etc/nix/nix.conf
reboot

# build image
nix --extra-experimental-features nix-command --extra-experimental-features flakes build .#packages.x86_64-linux.img
cp $(readlink result)/nixos.img /tmp/lima/nixos-x86.img
```

On your mac:
* Move `nixos-x86_64.img` under `imgs`

## Running NixOS
```bash
limactl start --name=nix nixos.yaml

lima
# switch to this repo directory
nixos-rebuild switch --flake .#nixos --use-remote-sudo
```
