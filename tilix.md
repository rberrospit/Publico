# Install tilix

Update kali
```
apt-get update && apt dist-upgrade -y
```

Download and install Tilix
```
wget http://ftp.de.debian.org/debian/pool/main/t/tilix/tilix_1.8.9-1_amd64.deb
wget http://ftp.de.debian.org/debian/pool/main/l/ldc/libphobos2-ldc-shared82_1.12.0-1_amd64.deb
wget http://ftp.de.debian.org/debian/pool/main/g/gtk-d/libgtkd-3-0_3.8.5-1_amd64.deb
wget http://ftp.de.debian.org/debian/pool/main/t/tilix/tilix-common_1.8.9-1_all.deb
wget http://ftp.de.debian.org/debian/pool/main/g/gtk-d/libvted-3-0_3.8.5-1_amd64.deb

dpkg -i tilix-common_1.8.9-1_all.deb
dpkg -i libphobos2-ldc-shared82_1.12.0-1_amd64.deb
dpkg -i libgtkd-3-0_3.8.5-1_amd64.deb
dpkg -i libvted-3-0_3.8.5-1_amd64.deb
dpkg -i tilix_1.8.9-1_amd64.deb
```

Launch  Tilix
```
tilix
```

