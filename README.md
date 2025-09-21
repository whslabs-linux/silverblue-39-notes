# Config git
```sh
git config --global core.symlinks false
```
# Install borgbackup
https://github.com/borgbackup/borg
```sh
curl -OL https://github.com/borgbackup/borg/releases/download/1.4.1/borg-linux-glibc236
sudo install borg-linux-glibc236 /usr/local/bin/borg
rm borg-linux-glibc236
```
# Install packer
https://github.com/hashicorp/packer
```sh
curl -O https://releases.hashicorp.com/packer/1.14.1/packer_1.14.1_linux_amd64.zip
unzip packer_1.14.1_linux_amd64.zip packer
sudo install packer /usr/local/bin
rm packer_1.14.1_linux_amd64.zip packer
```
# Install flatpaks
```sh
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
```
```sh
flatpak install -y flathub com.github.tchx84.Flatseal
flatpak install -y flathub org.filezillaproject.Filezilla
flatpak install -y flathub org.keepassxc.KeePassXC
```
```sh
flatpak info --show-permissions org.keepassxc.KeePassXC
```
```sh
sudo flatpak override --unshare=network
sudo flatpak override --share=network org.filezillaproject.Filezilla
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
  --append=modprobe.blacklist=nouveau,nova-core \
  --append=nvidia-drm.modeset=1 \
  --append=rd.driver.blacklist=nouveau,nova-core \
  ;
```
Smplayer preference > advanced > mplayer/mpv > options
```txt
--vo=gpu-next --gpu-api=vulkan --hwdec=vulkan --gpu-context=waylandvk
```
# Install amd driver
```sh
rpm-ostree override remove mesa-va-drivers --install mesa-va-drivers-freeworld
rpm-ostree install mesa-vdpau-drivers-freeworld
rpm-ostree override remove mesa-vulkan-drivers --install mesa-vulkan-drivers-freeworld
```
# Install ffmpeg
```sh
rpm-ostree override remove \
    fdk-aac-free \
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
dnf group info Virtualization
```
```sh
rpm-ostree install \
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
rpm-ostree install libguestfs-tools
```
# Install brasero
```sh
rpm-ostree install brasero
```
# Install clamav
```sh
rpm-ostree install -y clamav clamd clamav-update
```
```txt
# /etc/clamd.d/scan.conf
LocalSocket /run/clamd.scan/clamd.sock
# /etc/systemd/system/clamav-clamonacc.service.d/override.conf
[Service]
ExecStart=
ExecStart=/usr/sbin/clamonacc -F --fdpass -v --config-file=/etc/clamd.d/scan.conf
```
```sh
sudo freshclam
sudo setsebool -P antivirus_can_scan_system 1
sudo systemctl enable --now clamav-clamonacc
sudo systemctl enable --now clamav-freshclam
sudo systemctl enable --now clamd@scan
```
# Install nix
```txt
# /etc/nix/nix.conf
trusted-users = whs
```
```sh
sudo systemctl restart nix-daemon
```
```txt
# /etc/ostree/prepare-root.conf
[composefs]
enabled = yes
[root]
transient = true
```
```sh
rpm-ostree initramfs-etc --track=/etc/ostree/prepare-root.conf
```
```sh
rpm-ostree update
flatpak update -y
nix flake update --flake ~/.config/home-manager
home-manager init --switch --impure
```
