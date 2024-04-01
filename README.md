# Install borgbackup
```sh
curl -OL https://github.com/borgbackup/borg/releases/download/1.2.8/borg-linuxnew64
sudo install borg-linuxnew64 /usr/local/bin/borg
rm borg-linuxnew64
```
# Install flatpaks
```sh
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
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
  libavcodec-free \
  libavfilter-free \
  libavformat-free \
  libavutil-free \
  libpostproc-free \
  libswresample-free \
  libswscale-free \
  --install ffmpeg
```
# Config git
```txt
# ~/.gitconfig
[user]
	email = hswongac@gmail.com
	name = whs
```
# Install firefox addons and user.js
```sh
(
profile=$(find ~/.mozilla/firefox/ -type d -name '*.default-release')
mkdir -p $profile/extentions
curl -o $profile/extensions/@testpilot-containers.xpi https://addons.mozilla.org/firefox/downloads/file/4186050/multi_account_containers-8.1.3.xpi
curl -o $profile/extensions/keepassxc-browser@keepassxc.org.xpi https://addons.mozilla.org/firefox/downloads/file/4250129/keepassxc_browser-1.9.0.2.xpi
curl -o $profile/extensions/uBlock0@raymondhill.net.xpi https://addons.mozilla.org/firefox/downloads/file/4237670/ublock_origin-1.56.0.xpi
curl -O --output-dir $profile https://raw.githubusercontent.com/arkenfox/user.js/master/user.js
)
```
