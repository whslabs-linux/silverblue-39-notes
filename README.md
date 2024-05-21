# Install borgbackup
```sh
curl -OL https://github.com/borgbackup/borg/releases/download/1.2.8/borg-linuxnew64
sudo install borg-linuxnew64 /usr/local/bin/borg
rm borg-linuxnew64
```
# Install flatpaks
```sh
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```
```sh
flatpak install -y flathub com.gitlab.davem.ClamTk
flatpak install -y flathub org.filezillaproject.Filezilla
flatpak install -y flathub org.keepassxc.KeePassXC
```
# Install rpmfusion
```sh
rpm-ostree install \
  https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm \
  https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm \
  ;
```
# Install nvidia driver and smplayer
```sh
rpm-ostree install \
  akmod-nvidia \
  smplayer \
  xorg-x11-drv-nvidia-cuda \
  ;
```
```sh
rpm-ostree kargs \
  --append=modprobe.blacklist=nouveau \
  --append=nvidia-drm.modeset=1 \
  --append=rd.driver.blacklist=nouveau \
  ;
```
# Install ffmpeg
```sh
rpm-ostree override remove \
  ffmpeg-free \
  libavcodec-free \
  libavdevice-free \
  libavfilter-free \
  libavformat-free \
  libavutil-free \
  libpostproc-free \
  libswresample-free \
  libswscale-free \
  --install ffmpeg
```
# Install virt-manager
```sh
rpm-ostree install \
  libguestfs-tools \
  libvirt-daemon-config-network \
  libvirt-daemon-kvm \
  qemu-kvm \
  virt-install \
  virt-manager \
  virt-viewer \
  ;
```
```sh
sudo systemctl enable --now libvirtd
```
```sh
grep -E '^libvirt:' /usr/lib/group | sudo tee -a /etc/group
sudo usermod -aG libvirt $USER
```
# Install packer
https://github.com/hashicorp/packer
```sh
curl -O https://releases.hashicorp.com/packer/1.10.2/packer_1.10.2_linux_amd64.zip
sudo unzip -d /usr/local/bin packer_*_linux_amd64.zip packer
rm packer_*_linux_amd64.zip
```
# Install brasero
```sh
rpm-ostree install brasero
```
# Install nix
```sh
curl --proto '=https' --tlsv1.2 -sSf -L https://install.determinate.systems/nix | sh -s -- install
. /nix/var/nix/profiles/default/etc/profile.d/nix-daemon.sh
mkdir -p ~/.config/nix
cat<<EOF>~/.config/nix/nix.conf
experimental-features = nix-command flakes
EOF
```
