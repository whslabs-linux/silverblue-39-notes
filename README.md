# Config git
```sh
git config --global core.symlinks false
```
# Create bin directory
```sh
mkdir -p ~/.local/bin
```
# Install borgbackup
https://github.com/borgbackup/borg
```sh
curl -OL https://github.com/borgbackup/borg/releases/download/1.4.1/borg-linux-glibc236
install borg-linux-glibc236 ~/.local/bin/borg
rm borg-linux-glibc236
```
# Install packer
https://github.com/hashicorp/packer
```sh
curl -O https://releases.hashicorp.com/packer/1.13.1/packer_1.13.1_linux_amd64.zip
unzip -d ~/.local/bin packer_1.13.1_linux_amd64.zip packer
rm packer_1.13.1_linux_amd64.zip
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
```sh
rpm-ostree kargs --delete=rootflags=subvol=root --append=rootflags=subvol=root,compress=zstd:1
```
```txt
# /etc/fstab
#UUID=[UUID] / btrfs subvol=root,compress=zstd:1,x-systemd.device-timeout=0,ro 0 0
```

