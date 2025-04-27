# NixOS on a Lima VM

## Generate the image
On a linux machine or ubuntu lima vm for example:

```bash
# Install nix
sh <(curl -L https://nixos.org/nix/install) --daemon
# Enable the kvm feature
echo "system-features = nixos-test benchmark big-parallel kvm" >> /etc/nix/nix.conf
reboot

# Build the image
bash -c "nix --extra-experimental-features nix-command --extra-experimental-features flakes build .#packages.x86_64-linux.img"
mkdir imgs && cp $(readlink result)/nixos.img imgs/nixos-x86.img && rm -rf result
```

## Running the VM
```bash
limactl start --name=nix nixos.yaml
export LIMA_INSTANCE=nix
lima

# Switch to this repo directory
nixos-rebuild switch --flake .#nixos --use-remote-sudo
```
