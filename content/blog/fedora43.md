+++
title = 'Fedora43'
date = 2026-02-10T03:17:11Z
draft = true
+++

### Install Dependencies
This whole chunk can be copy-pasted directly into the terminal
```
sudo dnf install -y \
gcc gcc-c++ clang llvm lld flex bison \
glibc-devel glibc-devel.i686 \
libX11-devel libX11-devel.i686 \
libXext-devel libXext-devel.i686 \
libXinerama-devel libXinerama-devel.i686 \
libXrender-devel libXrender-devel.i686 \
libXi-devel libXi-devel.i686 \
libXcursor-devel libXcursor-devel.i686 \
libXrandr-devel libXrandr-devel.i686 \
libXcomposite-devel libXcomposite-devel.i686 \
libXfixes-devel libXfixes-devel.i686 \
libXdamage-devel libXdamage-devel.i686 \
libxkbcommon-devel libxkbcommon-x11-devel \
freetype-devel freetype-devel.i686 \
fontconfig-devel fontconfig-devel.i686 \
libpng-devel libpng-devel.i686 \
libjpeg-turbo-devel libjpeg-turbo-devel.i686 \
libtiff-devel libtiff-devel.i686 \
libxml2-devel libxml2-devel.i686 \
libxslt-devel libxslt-devel.i686 \
libunwind-devel libunwind-devel.i686 \
wayland-devel wayland-devel.i686 \
alsa-lib-devel alsa-lib-devel.i686 \
pulseaudio-libs-devel pulseaudio-libs-devel.i686 \
cups-devel cups-devel.i686 \
sane-backends-devel sane-backends-devel.i686 \
libv4l-devel libv4l-devel.i686 \
libgphoto2-devel libgphoto2-devel.i686 \
gsm-devel gsm-devel.i686 \
openal-soft-devel openal-soft-devel.i686 \
vulkan-loader-devel vulkan-loader-devel.i686 \
mesa-libGL-devel mesa-libGL-devel.i686 \
mesa-libEGL-devel mesa-libEGL-devel.i686 \
systemd-devel systemd-devel.i686 \
libusb1-devel libusb1-devel.i686 \
pcsc-lite-devel pcsc-lite-devel.i686 \
krb5-devel krb5-devel.i686 \
SDL2-devel \
gstreamer1-devel \
gstreamer1-plugins-base-devel \
dbus-devel \
gnutls-devel gnutls-devel.i686 \
openldap-devel openldap-devel.i686 \
libpcap-devel libpcap-devel.i686 \
ocl-icd-devel ocl-icd-devel.i686 \
git wget curl pkgconf-pkg-config gettext

sudo dnf install SDL2-devel || true
sudo dnf install -y mesa-libGL-devel mesa-libEGL-devel || true
sudo dnf install -y \
mingw64-gcc mingw64-gcc-c++ mingw64-binutils \
mingw32-gcc mingw32-gcc-c++ mingw32-binutils
sudo ln -sf /usr/bin/x86_64-w64-mingw32-dlltool /usr/bin/dlltool
sudo dnf install -y \
cups cups-client cups-pdf system-config-printer \
msitools

sudo dnf install -y \
libX11-devel.x86_64 \
libXext-devel.x86_64 \
libXi-devel.x86_64 \
libXcursor-devel.x86_64 \
libXrandr-devel.x86_64 \
libXrender-devel.x86_64 \
libXfixes-devel.x86_64 \
libXinerama-devel.x86_64 \
libXcomposite-devel.x86_64

sudo dnf install -y freetype-devel.x86_64 pkgconf-pkg-config || true

sudo dnf install -y \
fontconfig-devel.x86_64 \
libXxf86vm-devel.x86_64 \
wayland-devel.x86_64 \
pipewire-jack-audio-connection-kit-devel.x86_64

sudo dnf install -y \
    bzip2-devel.i686 \
    harfbuzz-devel.i686 \
    Brotli-devel.i686 \
    glib2-devel.i686 \
    graphite2-devel.i686

sudo dnf install -y winetricks cabextract || true
sudo dnf install -y samba samba-winbind gnutls 

sudo dnf install -y mesa-libGL.i686 mesa-libEGL.i686 libglvnd-glx.i686 ncurses-libs.i686 wine || true

sudo dnf install -y \
libXcomposite.i686 \
libXcursor.i686 \
libXrandr.i686 \
libXinerama.i686 \
libXdamage.i686

sudo dnf install -y glib2-devel.i686 graphite2-devel.i686 || true

sudo dnf install -y \
pipewire-alsa \
pipewire-pulseaudio \
pulseaudio-libs.i686 \
sane-backends-libs.i686 \
libusb1.i686
```
### winecx
Download winecx for Fedora
{{< cards >}}
  {{< card link="https://www.mediafire.com/file/0i12r61ufq1cuk5/winecx.zip/file" title="Mediafire zip Download" icon="external-link" >}}
{{< /cards >}}

#### Install winecx
```
cd Downloads
unzip winecx.zip
sudo mkdir -p /opt/winecx
sudo cp -r winecx /opt
```
#### Change permissions
```
sudo chown -R root:root /opt/winecx
sudo chmod -R a+rX /opt/winecx
```
### Create wineprefix for office programs
```
WINEPREFIX=$HOME/.office2016 wineboot
```
#### Configure the prefix for Windows 7
```
WINEPREFIX=~/.office2016 wine winecfg -v win7
```
#### Install dependencies
```
WINEPREFIX=~/.office2016 winetricks -q \
  corefonts msxml6 riched20 riched30 gdiplus vb6run vcrun2005 vcrun2008 \
  vcrun2010 vcrun2012 vcrun2013 dotnet48 vcrun2015

WINEPREFIX=~/.office2016 winetricks --force -q vcrun2019
```
#### Reconfigure the prefix for Windows 7
```
WINEPREFIX=~/.office2016 wine winecfg -v win7
```

### Download Office 2016 (32 Bit)
{{< cards >}}
  {{< card link="https://archive.org/details/microsoft-office-2016-professional-plus-32-and-64-bit" title="Internet Archive" icon="external-link" >}}
{{< /cards >}}

#### Install 7zip to extract the .iso
```
sudo dnf install 7zip -y
cd Downloads
file=~/Downloads/SW_DVD5_Office_Professional_Plus_2016_W32_English_MLF_X20-41353.ISO
outdir="${file%.iso}"
7z x "$file" -o"$outdir"
```

#### Define environment variables
```
export WINEPREFIX=$HOME/.office2016
export WINE=/opt/winecx/bin/wine
export WINEDLLPATH=/opt/winecx/lib/wine
export PATH=/opt/winecx/bin:$PATH
```
### Install Office
#### Run the office installer with the prefix
```
WINEPREFIX=$HOME/.office2016 /opt/winecx/bin/wine "$HOME/Downloads/SW_DVD5_Office_Professional_Plus_2016_W32_English_MLF_X20-41353/setup.exe" >/dev/null 2>&1 
```
### Apply Office Patches
#### Download requirements from Google Drive
{{< cards >}}
  {{< card link="https://drive.google.com/file/d/1_JBvLEarzUI4fEnnX983pP03b-oJsx5w/" title="Google Drive" icon="external-link" >}}
{{< /cards >}}

#### Unzip and open the folder
```
cd ~/Downloads
unzip "Requerimientos Office 2016.zip"
cd "Requerimientos Office 2016"
```
#### Install Gecko
```
msiextract -C gecko_extract wine-gecko-2.47.4-x86.msi
sudo mkdir -p /usr/share/wine/gecko
sudo cp "$HOME/Downloads/Requerimientos Office 2016/wine-gecko-2.47.4-x86.msi" /usr/share/wine/gecko/
sudo cp "$HOME/Downloads/Requerimientos Office 2016/wine-gecko-2.47.4-x86_64.msi" /usr/share/wine/gecko/
WINEPREFIX="$HOME/.office2016" wine reg add \
"HKLM\\Software\\Wine\\Gecko" \
/v Version /t REG_SZ /d "2.47.4" /f
```

#### Install Mono
```
msiextract -C mono_extract wine-mono-9.4.0-x86.msi
sudo mkdir -p /usr/share/wine/mono
sudo cp "$HOME/Downloads/Requerimientos Office 2016/wine-mono-9.4.0-x86.msi" /usr/share/wine/mono/
WINEPREFIX="$HOME/.office2016" wine reg add \
"HKLM\\Software\\Wine\\Mono" \
/v Version /t REG_SZ /d "9.4.0" /f
```

#### Apply DirectX fixes
```
WINEPREFIX="$HOME/.office2016" wine reg add \
"HKCU\\Software\\Wine\\Direct2D" \
/v max_version_factory /t REG_DWORD /d 0 /f

WINEPREFIX="$HOME/.office2016" wine reg add \
"HKCU\\Software\\Wine\\Direct3D" \
/v MaxVersionGL /t REG_DWORD /d 0x30002 /f
```

#### Test Excel
```
WINEPREFIX=$HOME/.office2016 wine "C:\\Program Files (x86)\\Microsoft Office\\Office16\\EXCEL.EXE"
```

#### Icons and dlls
```
unzip "Office 2016 icons.zip"
unzip "OfficeSoftwareProtectionPlatform.zip"
```
```
cd "OfficeSoftwareProtectionPlatform"

cp OSPPC.DLL OSPPCEXT.DLL sppcs.dll "$HOME/.office2016/drive_c/Program Files (x86)/Common Files/Microsoft Shared/OfficeSoftwareProtectionPlatform/"
cp sppcs.dll "$HOME/.office2016/drive_c/Program Files (x86)/Microsoft Office/Office16/"
```
```
sudo mkdir -p /usr/share/icons/hicolor/256x256/apps
sudo cp "$HOME/Downloads/Requerimientos Office 2016/Office 2016 icons/"*.png /usr/share/icons/hicolor/256x256/apps/
sudo update-icon-cache /usr/share/icons/hicolor/
```

### Install Microsoft fonts
#### Download
{{< cards >}}
  {{< card link="https://drive.google.com/file/d/1Wf9qP9tPScQ45QjaUdBH5URPkkvFijhG/" title="Google Drive" icon="external-link" >}}
{{< /cards >}}


#### Unzip and install
```
cd ~/Downloads
unzip ~/Downloads/FuentesOffice365.zip -d ~/Downloads/
mkdir -p $HOME/.office2016/drive_c/windows/Fonts
sudo mkdir -p /usr/share/fonts/Windows 
sudo chown -R "$USER:$USER" "$HOME/Downloads/FuentesOffice365"
chmod -R u+rwX "$HOME/Downloads/FuentesOffice365"
cd "$HOME/Downloads/FuentesOffice365"
cp *.ttf *.TTF *.ttc $HOME/.office2016/drive_c/windows/Fonts
sudo cp *.ttf *.TTF *.ttc /usr/share/fonts/Windows
```
#### Register fonts
```
WINEPREFIX=$HOME/.office2016 bash -c '
FONTDIR="$WINEPREFIX/drive_c/windows/Fonts"
REGFILE="$WINEPREFIX/allfonts.reg"

echo "REGEDIT4" > "$REGFILE"
echo "" >> "$REGFILE"
echo "[HKEY_LOCAL_MACHINE\\Software\\Microsoft\\Windows NT\\CurrentVersion\\Fonts]" >> "$REGFILE"

for f in "$FONTDIR"/*.ttf "$FONTDIR"/*.TTF "$FONTDIR"/*.otf "$FONTDIR"/*.OTF; do
    [ -e "$f" ] || continue
    base=$(basename "$f")
    name="${base%.*}"

    # Convertir nombre a formato amigable
    label=$(echo "$name" | sed "s/_/ /g" | sed "s/Regular//g" )

    echo "\"$label (TrueType)\"=\"${base}\"" >> "$REGFILE"
done

wine regedit "$REGFILE"
'
```
### Configure filetypes
#### Create a folder for wrappers
```
sudo mkdir -p /opt/wine/launchers
sudo chmod 755 /opt/wine/launchers
```

#### Create launchers
```
#WORD
sudo tee /opt/wine/launchers/word2016.sh > /dev/null <<'EOF'
#!/bin/bash
set -e
export WINEPREFIX="$HOME/.office2016"
export LANG=C.UTF-8
export WINEDEBUG=-all

app="C:\\Program Files (x86)\\Microsoft Office\\Office16\\WINWORD.EXE"
wineserver -p >/dev/null 2>&1 || true

if [ $# -eq 0 ]; then
    exec wine "$app"
else
    for file in "$@"; do
        fullpath=$(realpath "$file")
        winpath="Z:${fullpath//\//\\}"
        wine "$app" "$winpath"
    done
fi
EOF
sudo chmod +x /opt/wine/launchers/word2016.sh

sudo tee /usr/share/applications/word2016.desktop > /dev/null <<'EOF'
[Desktop Entry]
Name=Microsoft Word 2016
Exec=/opt/wine/launchers/word2016.sh %F
Type=Application
StartupNotify=true
Terminal=false
Icon=word2016
Categories=Office;WordProcessor;
MimeType=application/msword;application/vnd.openxmlformats-officedocument.wordprocessingml.document;application/vnd.ms-word.document.macroEnabled.12;application/rtf;text/plain;
EOF

#
#EXCEL
sudo tee /opt/wine/launchers/excel2016.sh > /dev/null <<'EOF'
#!/bin/bash
set -e
export WINEPREFIX="$HOME/.office2016"
export LANG=C.UTF-8
export WINEDEBUG=-all

app="C:\\Program Files (x86)\\Microsoft Office\\Office16\\EXCEL.EXE"

wineserver -p >/dev/null 2>&1 || true

if [ $# -eq 0 ]; then
    exec wine "$app"
else
    for file in "$@"; do
        fullpath=$(realpath "$file")
        winpath="Z:${fullpath//\//\\}"
        wine "$app" "$winpath"
    done
fi
EOF

sudo chmod +x /opt/wine/launchers/excel2016.sh

# Desktop
sudo tee /usr/share/applications/excel2016.desktop > /dev/null <<'EOF'
[Desktop Entry]
Name=Microsoft Excel 2016
Exec=/opt/wine/launchers/excel2016.sh %F
Type=Application
StartupNotify=true
Terminal=false
Icon=excel2016
Categories=Office;Spreadsheet;
MimeType=application/vnd.ms-excel;application/vnd.openxmlformats-officedocument.spreadsheetml.sheet;application/vnd.ms-excel.sheet.macroEnabled.12;text/csv;
EOF

#
#POWERPOINT

sudo tee /opt/wine/launchers/powerpoint2016.sh > /dev/null <<'EOF'
#!/bin/bash
set -e
export WINEPREFIX="$HOME/.office2016"
export LANG=C.UTF-8
export WINEDEBUG=-all

app="C:\\Program Files (x86)\\Microsoft Office\\Office16\\POWERPNT.EXE"
wineserver -p >/dev/null 2>&1 || true

if [ $# -eq 0 ]; then
    exec wine "$app"
else
    for file in "$@"; do
        fullpath=$(realpath "$file")
        winpath="Z:${fullpath//\//\\}"
        wine "$app" "$winpath"
    done
fi
EOF
sudo chmod +x /opt/wine/launchers/powerpoint2016.sh

sudo tee /usr/share/applications/powerpoint2016.desktop > /dev/null <<'EOF'
[Desktop Entry]
Name=Microsoft PowerPoint 2016
Exec=/opt/wine/launchers/powerpoint2016.sh %F
Type=Application
StartupNotify=true
Terminal=false
Icon=powerpoint2016
Categories=Office;Presentation;
MimeType=application/vnd.ms-powerpoint;application/vnd.openxmlformats-officedocument.presentationml.presentation;application/vnd.ms-powerpoint.presentation.macroEnabled.12;
EOF

#
#OUTLOOK

sudo tee /opt/wine/launchers/outlook2016.sh > /dev/null <<'EOF'
#!/bin/bash
set -e
export WINEPREFIX="$HOME/.office2016"
export LANG=C.UTF-8
export WINEDEBUG=-all

app="C:\\Program Files (x86)\\Microsoft Office\\Office16\\OUTLOOK.EXE"
wineserver -p >/dev/null 2>&1 || true

if [ $# -eq 0 ]; then
    exec wine "$app"
else
    for file in "$@"; do
        fullpath=$(realpath "$file")
        winpath="Z:${fullpath//\//\\}"
        wine "$app" "$winpath"
    done
fi
EOF
sudo chmod +x /opt/wine/launchers/outlook2016.sh

sudo tee /usr/share/applications/outlook2016.desktop > /dev/null <<'EOF'
[Desktop Entry]
Name=Microsoft Outlook 2016
Exec=/opt/wine/launchers/outlook2016.sh %F
Type=Application
StartupNotify=true
Terminal=false
Icon=outlook2016
Categories=Office;Email;
MimeType=application/vnd.ms-outlook;application/mbox;message/rfc822;
EOF

#
#ACCESS

sudo tee /opt/wine/launchers/access2016.sh > /dev/null <<'EOF'
#!/bin/bash
set -e
export WINEPREFIX="$HOME/.office2016"
export LANG=C.UTF-8
export WINEDEBUG=-all

app="C:\\Program Files (x86)\\Microsoft Office\\Office16\\MSACCESS.EXE"
wineserver -p >/dev/null 2>&1 || true

if [ $# -eq 0 ]; then
    exec wine "$app"
else
    for file in "$@"; do
        fullpath=$(realpath "$file")
        winpath="Z:${fullpath//\//\\}"
        wine "$app" "$winpath"
    done
fi
EOF
sudo chmod +x /opt/wine/launchers/access2016.sh

sudo tee /usr/share/applications/access2016.desktop > /dev/null <<'EOF'
[Desktop Entry]
Name=Microsoft Access 2016
Exec=/opt/wine/launchers/access2016.sh %F
Type=Application
StartupNotify=true
Terminal=false
Icon=access2016
Categories=Office;Database;
MimeType=application/vnd.ms-access;application/x-msaccess;
EOF

#
#PUBLISHER

sudo tee /opt/wine/launchers/publisher2016.sh > /dev/null <<'EOF'
#!/bin/bash
set -e
export WINEPREFIX="$HOME/.office2016"
export LANG=C.UTF-8
export WINEDEBUG=-all

app="C:\\Program Files (x86)\\Microsoft Office\\Office16\\MSPUB.EXE"
wineserver -p >/dev/null 2>&1 || true

if [ $# -eq 0 ]; then
    exec wine "$app"
else
    for file in "$@"; do
        fullpath=$(realpath "$file")
        winpath="Z:${fullpath//\//\\}"
        wine "$app" "$winpath"
    done
fi
EOF
sudo chmod +x /opt/wine/launchers/publisher2016.sh

sudo tee /usr/share/applications/publisher2016.desktop > /dev/null <<'EOF'
[Desktop Entry]
Name=Microsoft Publisher 2016
Exec=/opt/wine/launchers/publisher2016.sh %F
Type=Application
StartupNotify=true
Terminal=false
Icon=publisher2016
Categories=Office;Publishing;
MimeType=application/x-mspublisher;
EOF

#

-Corregir asociaciones EXCEL

cat > /tmp/fix_excel_associations.reg <<'EOF'
Windows Registry Editor Version 5.00

[HKEY_CLASSES_ROOT\.xls]
@="Excel.Sheet.8"

[HKEY_CLASSES_ROOT\.xlsx]
@="Excel.Sheet.12"

[HKEY_CLASSES_ROOT\Excel.Sheet.8\shell\Open\command]
@="\"C:\\Program Files (x86)\\Microsoft Office\\Office16\\EXCEL.EXE\" \"%1\""

[HKEY_CLASSES_ROOT\Excel.Sheet.12\shell\Open\command]
@="\"C:\\Program Files (x86)\\Microsoft Office\\Office16\\EXCEL.EXE\" \"%1\""

[HKEY_CLASSES_ROOT\Excel.Sheet.8\shell\Open\ddeexec]
@=""

[HKEY_CLASSES_ROOT\Excel.Sheet.12\shell\Open\ddeexec]
@=""
EOF

WINEPREFIX=$HOME/.office2016 wine regedit /S /tmp/fix_excel_associations.reg
```

#### Update application base
```
sudo update-desktop-database /usr/share/applications
```


### Sources
{{< cards >}}
  {{< card link="https://www.youtube.com/watch?v=9iSB53aC_qE" title="Fedora 43 + Office 2016" icon="youtube" >}}
  {{< card link="https://docs.google.com/document/d/1rCm4BGY2UjKV5szCb0DzogYxCJZG2A2d" title="Installation Instructions" icon="document-text" >}}
  {{< card link="https://docs.google.com/document/d/19gaewqDBz3tmKTjzgvwWkplrh9siXhg68yYwMLvlXus" title="Script" icon="document-text" >}}
{{< /cards >}}