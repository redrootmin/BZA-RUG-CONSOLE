# BZA-RUG [Bezymnye Zapiski Altika - Russian Utility for Gamers]
# Файл с полезными командами для настройки ОС Альт Рабочая станция/Альт Образование/Simply Linux из коносоли ( BZA-RUG - Безумные Записки Альтовода - Российская Утилита для Геймеров [on Linux] )
# https://www.basealt.ru/#mm-5
# дата 0109252227

# ПЕРВИЧНАЯ НАСТРОЙКА СИСТЕМЫ И УСТАНОВКА УТИЛИТ/ПРОГРАММ

# Включаем sudo для пользователя
su -
# если sudo не стоит: sudo apt-get install sudo
control sudowheel enabled

# Сделать из Альт системы переносную для загрузки из USB диска/флешки
nano /etc/initrd.mk
# В /etc/initrd.mk добавить:
MODULES_TRY_ADD += kernel/drivers/scsi/sd_mod.ko
MODULES_TRY_ADD += kernel/drivers/usb
# выполнить
make-initrd
# если нужно ядро которое не загружено
ls /lib/modules/
sudo make-initrd --kernel=папка с модулями полученными из выхлопа 'ls /lib/modules/'

# полное обновление системы
apt-get update
apt-get dist-upgrade
# обновление ядра
#update-kernel -t std-def
# установка тестового/в разработке ядра
#update-kernel -t un-def

# удалить/закоментировать если отвалиться wi-fi rtl8821ce
sudo nano /etc/modprobe.d/blacklist-rtl8821ce.conf

# [Отключить в ядре заплатки  MELTDOWN\SPECTRE+]
sudo nano /etc/sysconfig/grub2
#  и добавлям
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash ...." + mitigations=off
# пример
# GRUB_CMDLINE_LINUX_DEFAULT="quiet splash amdgpu.ppfeaturemask=0xffffffff mitigations=off"
# save
sudo grub-mkconfig -o /boot/grub/grub.cfg

#[TEARFREE  IN AMD]
(for rx400\500\vega\rx5000\6000)
sudo nano /etc/X11/xorg.conf.d/10-amdgpu.conf

Section "Device"
Identifier "AMDgpu"
Option  "DRI" "3"
Option  "TearFree" "true"
EndSection

# install flathub repo
flatpak remote-add flathub https://flathub.org/repo/flathub.flatpakrepo
# flatpak install flathub org.mozilla.vpn
# flatpak install flathub org.virt_manager.virt-manager
# flatpak install flathub io.github.nokse22.inspector
# flatpak install flathub net.nokyan.Resources
# flatpak install flathub com.mattjakeman.ExtensionManager
# flatpak install flathub org.onlyoffice.desktopeditors
sudo flatpak override --filesystem=/usr/share/fonts org.onlyoffice.desktopeditors
# flatpak install flathub org.libreoffice.LibreOffice
# flatpak install flathub com.wps.Office
# flatpak install flathub de.allotropia.ZetaOffice
# flatpak install flathub net.code_industry.MasterPDFEditor
# flatpak install flathub org.upscayl.Upscayl
# flatpak install flathub com.icons8.Lunacy
# flatpak install flathub org.kde.krita
# flatpak install flathub org.inkscape.Inkscape
# flatpak install flathub no.mifi.losslesscut
# flatpak install flathub org.shotcut.Shotcut
# flatpak install flathub org.kde.kdenlive
# flatpak install flathub ru.linux_gaming.PortProton
# flatpak install flathub io.github.radiolamp.mangojuice
# flatpak install flathub net.retrodeck.retrodeck
# flatpak install flathub net.davidotek.pupgui2
# flatpak install flathub net.rpcs3.RPCS3
# flatpak install flathub net.shadps4.shadPS4
# flatpak install flathub io.github.ryubing.Ryujinx
# flatpak install flathub org.DolphinEmu.dolphin-emu
# flatpak install flathub net.pcsx2.PCSX2 
# flatpak install flathub org.duckstation.DuckStation
# flatpak install flathub io.github.antimicrox.antimicrox
# flatpak install flathub com.moonlight_stream.Moonlight
# flatpak install flathub net.veloren.veloren
# flatpak install flathub com.mojang.Minecraft
# flatpak install flathub com.modrinth.ModrinthApp
# flatpak install flathub com.atlauncher.ATLauncher
# flatpak install flathub io.mrarm.mcpelauncher
# flatpak install flathub page.codeberg.JakobDev.jdMinecraftLauncher

# установка установка различных полезных утилит, библиотек, программ (выбор redroot'та)
sudo apt-get install -y -f sudo anilibria-winmaclinux hplip eepm flatpak inxi paprefs pavucontrol helvum p7zip meld cpu-x psensor xsensors kdiskmark system-monitoring-center encfs cpupower yad zenity libgtksourceview3 neofetch git meson gcc gcc-c++ cmake ninja-build terminator gnome-disk-utility gparted corectrl qbittorrent timeshift nano python3-module-pip cameractrls 
#  пакеты и утилиты необходимые для различных программ
sudo apt-get install -f -y libwebkit2gtk-gir libwebkit2gtk-devel libwebkit2gtk libwebkit2gtk4.1 libwebkit2gtk4.1-devel libwebkit2gtk4.1-gir libwebkit2gtk-gir-devel libwebkit2gtk4.1-gir-devel libwxGTK3.0-webview libwxGTK3.2-webview gambas-gb-gtk3-webview
# для сборки пакетов и компиляции из исходников
sudo apt-get install -y rpm-build rpmlint python gear hasher patch rpmdevtools

# pywebview - запуск вебстраниц как python срипт/программа
pip3 install pywebview glob2

# GDM Настройки (gnome 47+)
sudo apt-get install -y -f gdm-settings

# иконки numix+papirus
# иконки numix не подходят для КДЕ6!
cd "/home/$USER"
rm -f alt-numix-icons-all.tar.xz
wget "https://github.com/redrootmin/bzu-gmb-modules/releases/download/v4/alt-numix-icons-all.tar.xz" || wget "https://github.com/redrootmin/bzu-gmb-modules/releases/download/v4/alt-numix-icons-all.tar.xz"
cd "/usr/share/icons"
sudo tar -xpJf "/home/$USER/alt-numix-icons-all.tar.xz"
rm -f "/home/$USER/alt-numix-icons-all.tar.xz"
# иконки papirus
sudo apt-get install -y papirus-icon-theme

# redroot wallpapers
cd "/home/$USER"
rm -f "alt-gnome-wallpapers-v1.tar.xz"
wget "https://github.com/redrootmin/bzu-gmb-modules/releases/download/v4/alt-gnome-wallpapers-v1.tar.xz" || wget "https://github.com/redrootmin/bzu-gmb-modules/releases/download/v4/alt-gnome-wallpapers-v1.tar.xz"
cd "/usr/share"
sudo tar -xpJf "/home/$USER/alt-gnome-wallpapers-v1.tar.xz"
rm -f "/home/$USER/alt-gnome-wallpapers-v1.tar.xz"


# NVIDIA GPU
epm play switch-to-nvidia
epm play i586-fix
sudo apt-get install -y gwe nvidia-cuda-devel
# проверить что установлена nvidia cuda и версия:
nvcc --version
# NVIDIA-ZINK
env __GLX_VENDOR_LIBRARY_NAME=mesa __EGL_VENDOR_LIBRARY_FILENAMES=/usr/share/glvnd/egl_vendor.d/50_mesa.json MESA_LOADER_DRIVER_OVERRIDE=zink GALLIUM_DRIVER=zink LIBGL_KOPPER_DRI2=1 mangohud <app>


# AMD HIP RENDER
sudo apt-get install -y blender-cycles-hip-kernels hip-runtime-amd
# errors/freez/artifacts blender + amd hip + vega/rdna
# Memory access fault by GPU node-1 (Agent handle: 0x7f828183fe00) on address 0x7f8203aa7000. Reason: Page not present or supervisor privilege.
sudo usermod -a -G video $USER
sudo usermod -a -G render $USER
# start blender in terminal
blender --factory-startup --debug-all

#overclock+undervolting в картах АМД rx400/500+
sudo apt-get install -y -f  corectrl 
# запуск corectrl без пароля и включение в автозагрузку
sudo usermod -a -G corectrl $USER
reboot
cp /usr/share/applications/org.corectrl.corectrl.desktop ~/.config/autostart/

sudo nano /etc/sysconfig/grub2
# => GRUB_CMDLINE_LINUX_DEFAULT="quiet splash" + amdgpu.ppfeaturemask=0xffffffff + mitigations=off(TORN OFF MELTDOWN\SPECTRE+)]
 GRUB_CMDLINE_LINUX_DEFAULT="quiet splash amdgpu.ppfeaturemask=0xffffffff mitigations=off"
# включить возможность использовать Vulkan 1.1 в видеокартах HD7970 
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash amdgpu.ppfeaturemask=0xffffffff modprobe.blacklist=radeon radeon.si_support=0 radeon.cik_support=0 amdgpu.si_support=1 amdgpu.cik_support=1 radeon.audio=1 mitigations=off"

sudo grub-mkconfig -o /boot/grub/grub.cfg

#3 GAMING
epm play -y discord
sudo apt-get install -y -f ds4drv corectrl gamescope mangohud goverlay portproton steam vkBasalt obs-vkcapture 

# for RPCS3 appimage
sudo mkdir -p /etc/ssl/certs
sudo ln -s /etc/pki/tls/certs/ca-bundle.crt  /etc/ssl/certs/ca-certificates.crt

# открытые и бесплатные  игры в Альт
sudo apt-get install -y minetest supertux2 supertuxkart 0ad warzone2100 openttd

# OpenRA - Red Alert
flatpak install --from https://flathub.org/repo/appstream/net.openra.OpenRA.flatpakref
flatpak install --from https://flathub.org/beta-repo/appstream/net.openra.OpenRA.flatpakref

#OpenGothic 
epmi libvulkan-devel vulkan-tools vulkan-validation-layers vulkan-examples libVulkanUtilityLibraries-devel glslang glslang-devel libglslang15 libXcursor libXcursor-devel libalsa libalsa-devel
# 1st time build:
git clone --recurse-submodules https://github.com/Try/OpenGothic.git
cd OpenGothic
cmake -B build -DBUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE:STRING=RelWithDebInfo
make -C build -j $(nproc)

# following builds:
git pull --recurse-submodules
make -C build -j $(nproc)
simple: 
mangohud MANGOHUD_CONFIGFILE='/mnt/ssd-date-home/.config/MangoHud/MangoHud-60fps.conf' ./Gothic2Notr.sh -g Gothic2Gold -g2 -aa 2  -gi 1 -rt 1

# gamescope-session-steam gamescope-session-plus


# ds4drv  - драйвер для геймпадов sony DS4
systemctl enable --now ds4drv
#kernel 6.6 + 6.12+
sudo apt-get install -y --reinstall dkms git kernel-headers-common kernel-headers-6.12 kernel-headers-modules-6.12 kernel-headers-modules-rt kernel-headers-rt kernel-source-6.12


#ИСПРАВЛЕННЫЙ HID_SONY ДЛЯ ПОДДЕЛЬНЫХ ГЕЙМПАДОВ DUALSHOCK 4
#Некоторые поддельные DualShock 4 не могут получить feature report 0x81. Данный драйвер пропатчен так, чтобы получать feature report 0x12 как резервный вариант.
#sony 0003:054C:09CC.001F: failed to retrieve feature report 0x81 with the DualShock 4 MAC address
#Другая проблема: иногда переменная calib->sens_denom возвращает ноль, и ядро зависает. В этом драйвере calib->sens_denom никогда не возвращает ноль.

#УСТАНОВКА

sudo git clone https://github.com/ozz-is-here/hid-sony-fix-dkms.git /usr/src/hid-sony-fix-dkms-0.1
sudo dkms install -m hid-sony-fix-dkms -v 0.1

Добавьте blacklist hid_sony в /etc/modprobe.d/blacklist.conf.
#УДАЛЕНИЕ
sudo dkms remove -m hid-sony-fix-dkms -v 0.1
sudo rm -rf /usr/src/hid-sony-fix-dkms-0.1


# ОФИСНЫЕ ПАКЕТЫ
# onlyoffice №1
#wget https://download.onlyoffice.com/install/desktop/editors/linux/onlyoffice-desktopeditors.x86_64.rpm
#epm install --repack --scripts onlyoffice-desktopeditors.x86_64.rpm
# onlyoffice №2
epm play onlyoffice
# epm play --update all

# wps_office
epm play -y wpsoffice

# r7-office
epm play -y r7-office
# myoffice
epm play -y myoffice
# scribus
sudo apt-get install -y scribus
# pdf редактирование
epm play -y master-pdf-editor
# для интернета
sudo apt-get install -y chromium-gost yandex-browser-stable thunderbird firefox nextcloud-client chromium 

#epm play -y telegram
sudo apt-get install -f telegram-desktop 
# принтеры HP
sudo apt-get install -y system-config-printer  hplip
hp-plugin -i
# Следуйте инструкциям. Будьте готовы ввести пароль суперпользователя.

# Yandex-disk
wget https://repo.yandex.ru/yandex-disk/yandex-disk-latest.x86_64.rpm
sudo apt-get install -y yandex-disk-latest.x86_64.rpm;sudo apt-get install -y yandex-disk-indicator 

# Программы для создания и обработки мультимедиа
sudo apt-get install -y audacity audacious krita inkscape scribus blender obs-studio codium kdenlive droidcam mpv vlc simplescreenrecorder shotcut darktable rawtherapee

#v1
epm play -y lunacy
#v2
flatpak install flathub com.icons8.Lunacy


# Декстопная виртуализация
sudo apt-get install -y libvirt libvirt-kvm libvirt-qemu virt-manager swtpm-tools libswtpm
sudo gpasswd -a $USER vmusers
sudo nano /etc/libvirt/qemu.conf
#add
user = "root"
group = "root"
swtpm_user = "root"
swtpm_group = "root"
#save
sudo rm /lib/tmpfiles.d/libvirtd.conf
sudo systemctl restart libvirtd
# ADD support GL virtualization
sudo nano /etc/sysconfig/grub2
# add
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash ...." + For Intel, intel_iommu=on iommu=pt. for AMD, amd_iommu=on
# save
sudo grub-mkconfig -o /boot/grub/grub.cfg


# virtualbox
sudo apt-get install -y virtualbox kernel-modules-virtualbox-$(uname -r|cut -f2,3 -d-)
sudo gpasswd -a $USER vboxusers
sudo reboot
# [VIRTUALBOX 1920x1080]
xrandr --newmode "1920x1080"  173.00  1920 2048 2248 2576  1080 1083 1088 1120 -hsync +vsync
xrandr --addmode Virtual1 1920x1080
xrandr --output Virtual1 --mode 1920x1080

# v2
epm play virtualbox

#OLLAMA — ЭТО ПРОГРАММНОЕ ОБЕСПЕЧЕНИЕ, ПОЗВОЛЯЮЩЕЕ ПОЛЬЗОВАТЕЛЯМ ЗАПУСКАТЬ БОЛЬШИЕ ЯЗЫКОВЫЕ МОДЕЛИ ЛОКАЛЬНО В КОМАНДНОЙ СТРОКЕ ИЛИ ЧЕРЕЗ WEB-ИНТЕРФЕЙС (OPENWEBUI). ОНО ДОСТУПНО ДЛЯ MACOS, WINDOWS (ЧЕРЕЗ WSL2) И LINUX, А ТАКЖЕ МОЖЕТ УСТАНАВЛИВАТЬСЯ ЧЕРЕЗ DOCKER. 

# скачивания скрипта установки онлайн и установка, открываем терминал и вводим:
curl -fsSL https://ollama.com/install.sh | sh
#проверяем что утилита установилась
ollama --version
# запускаем и разрешаем работу службы
systemctl is-active ollama.service
sudo systemctl start ollama.service
 sudo systemctl enable ollama.service
 # запускаем утилиту и даем команду на запуск локальной нейросети, если она ранее не скачивалась, утилита ее скачет (для примера взял deepseek примерно 4,7Гб)
ollama run deepseek-r1:7b
# после скачивания или если нейросеть уже на диске появиться приглашение на работу с сетью: >>> Send a message (/? for help)
# можно просто вводить вопросы и т.д. нейросети или через / вводить системные команды утилите ollama, например для выключения: /bye
# для примера: ты знаешь русский язык? 
# ВНИМАНИЕ: утилита ollama автоматически выбирает ускоритель обработки данных в вашем ПК/Ноутбуке, если не будет подходящей видеокарты, то начнет использовать процессор, тогда ответы могут будет долгими и большая нагрузка на систему.
# Аддон для доступа из браузера: https://addons.mozilla.org/en-US/firefox/addon/page-assist/
#Список поддержки видеокарт АМД (пополняется):  https://ollama.com/blog/amd-preview

# ПОЛНЫЙ КЛОН СИСТЕМЫ НА НОВЫЙ  DISK/SSD (ЭТО МОЖНО СДЕЛАТЬ ПРЯМ В ЗАГРУЖЕННОЙ СИСТЕМЫ, НО ЛУЧШЕ В ПРОЦЕССЕ НЕ МЕНЯТЬ ДАННЫЕ!)
# команда для копирования диска равного размера
sudo dd if=/dev/sda of=/dev/sdb bs=64K conv=noerror,sync status=progress
# если диски не равны по емкости, но разделы помещаются. (результат 70% что получиться, например с ссд 512 на 500гб клон получается перенести если на 512 занято 450гб.)
sudo dd if=/dev/nvme0n1 of=/dev/sda bs=1M conv=sparse,sync status=progress

# Здесь:
#    sdb- это пункт назначения
#    sda- исходный жесткий диск
#    bs- это команда размера блока, соответствующая 64 КБ
#    conv = нет ошибки, синхронизациясинхронизирует ввод-вывод и в случае ошибки не останавливается.
# Значение по умолчанию для настроек 64 КБ составляет 512 байт, что относительно мало. В качестве условия лучше включить 64К или 128К. С другой стороны, передача небольшого блока более надежна.

# ПЕРЕУСТАНОВИТЬ GRUB-EFI, ЕСЛИ ДРУГАЯ ОС ЗАТЕРЛА ИЛИ МАТ.ПЛАТА ПЕРЕСТАЛА ВИДИТЬ
sudo apt-get install -f --reinstall grub-efi

# [CHROOT - для аварийного восстановления/доработки системы из лайф системы]
# sdXX - это имя диска и номер раздела где установлена система а иммено корень диска, например - sdb2
mount /dev/sdXX /mnt
mount -o bind /dev /mnt/dev
mount -o bind /proc /mnt/proc
mount -o bind /run /mnt/run
mount -o bind /sys /mnt/sys
chroot /mnt/ /bin/bash

# [Создание SWAP-файла, если раздела нет или его не хватает ]
sudo fallocate -l 4g /home/4GiB.swap
sudo chmod 600 /home/4GiB.swap # даем спец права на файл
sudo mkswap /home/4GiB.swap # форматируем файл для свопа
sudo swapon /home/4GiB.swap # задействуем файл как своп
echo '/home/4GiB.swap swap swap defaults 0 0' | sudo tee -a /etc/fstab
free -h # for test


# [RAM + HDD\SSD оптимизация, когда памяти 8гб+]
echo -e "vm.swappiness=10" | sudo tee -a /etc/sysctl.conf
echo -e "vm.vfs_cache_pressure=1000" | sudo tee -a /etc/sysctl.conf
echo -e "vm.dirty_background_ratio = 50" | sudo tee -a /etc/sysctl.conf
echo -e "vm.dirty_ratio = 80" | sudo tee -a /etc/sysctl.conf


# [РАЗГОН ФАЙЛОВОЙ СИСТЕМЫ EXT4, ДЛЯ ДОМАШНЕГО ПК]
su -
nano /etc/fstab
# добавляем эти флаги к корневой точки монтирования "noatime,nodiratime,nobarrier"
# Пример: UUID=5a18c97e-bd40-4b55-a2c8-ab7da4cf20f0 / ext4 errors=remount-ro,noatime,nodiratime,nobarrier 0 1
#INFO https://qna.habr.com/q/427095

# [КОМАНДУ КОТОРАЯ УМЕНЬШАЕТ РЕЗЕРВИРОВАНИЕ МЕСТА EXT4]
#по умолчанию EXT4 резервирует 5% места на диске#
#резервирование можно уменьшить или вообще отключить# 
sudo tune2fs -m 1 /dev/sda1
###sda1 - это имя диска, на котором отключем резервирование, что бы проверить какие диске есть в системе, их имена, точки монтирования введи в консоли это: df -h | grep sd ###
#цифра 1 это процент резервирования (1%), можно поставить 0 если диск логический без системы, пример:#
sudo tune2fs -m 0 /dev/sda1 
#но не рекомендую делать на системном диске меньше: 1%#
#можно применять на дисках где уже есть инфо, нечего плохого не случиться, проверено!#


# НАБОР ПРОШИВОК ДЛЯ АУДИОЧИПОВ INTEL (ЕСЛИ НЕ РАБОТАЕТ INTEL AUDIO)
sudo  apt-get install -y  firmware-alsa-sof

# ПРОБЛЕМЫ С HDMI + PIPEWIRE!!!
# если pipeware не позволяет выводить обычное стерео через HDMI, то это решение для Вас!
sudo nano /etc/modprobe.d/alsa-base.conf
# добовляем в него следующую команду:
options snd_hda_intel index=1,0

# sudo echo "options snd_hda_intel index=1,0" > /etc/modprobe.d/alsa-base.conf
# ИНФОРМАЦИЯ КОТОРАЯ ТРЕБУЕТСЯ ТЕХ. ПОДДЕРЖКЕ ДЛЯ АГНОСТИКИ ПРОБЛЕМЫ
#Версия ОС:
 cat /etc/os-release >> os.txt
#Ядро:
 uname -a >> uname.txt
#Репозитории:
 apt-repo >> repo.txt
#Дополнительно выполните:
 su -  #(минус обязателен)
# system-report (возможно потребуется установка пакета system-report)
# Файлы *.txt и архив sysreport-00000000.tar.xz, полученные по итогам выполнения команд, прикрепите к вашему сообщению.



# НЕСТАБИЛЬНЫЕ/ТЕСТОВЫЕ, НО НОВЫЕ ВЕРСИИ MESA + KERNEL  (ПОКА РАБОТАЕТ С Р11, но с 01.11.24 будет только Сизиф!)
# блокируем/ставим на паузу удаление steam и portproton
sudo echo "/* held due to problems with this package in sisyphus as>
RPM::Hold {
	"^steam*";"^portproton*"
};" > /etc/apt/apt.conf.d/hold-gamingapp.conf

# если больше не нужно удаляем
# sudo rm -f  /etc/apt/apt.conf.d/hold-gamingapp.conf
# репозиторий с mesa 
# https://packages.altlinux.org/ru/sisyphus/srpms/apt-repo-lakostis/rpms/
sudo apt-get install apt-repo-lakostis
sudo apt-get install -f --reinstall mesa-gallium-drivers i586-mesa-gallium-drivers pocl-devices-vulkan vkroots-devel libvulkan-devel libvulkan1 vulkan-tools vulkan-validation-layers vulkan-amdgpu vulkan-examples libVulkanUtilityLibraries-devel i586-libVulkanUtilityLibraries-devel i586-libvulkan-devel i586-libvulkan1 i586-mangohud i586-vkBasalt i586-vkmark i586-vkroots-devel i586-vulkan-amdgpu i586-vulkan-validation-layers libvulkan-memory-allocator-devel vulkan-filesystem vulkan-headers vulkan-registry libVkLayer_MESA vulkan-dzn vulkan-intel vulkan-intel_hasvk vulkan-lvp vulkan-nvk vulkan-radeon vulkan-terascale vulkan-virtio i586-libVkLayer_MESA i586-vulkan-intel i586-vulkan-radeon
# репозиторий с kernel
# https://packages.altlinux.org/ru/sisyphus/srpms/apt-repo-wks-kernel/rpms/
sudo apt-get install apt-repo-wks-kernel
sudo update-kernel -t lks-wks
sudo apt-get install -f kernel-headers-modules-lks-wks kernel-headers-lks-wks


# УСТАНОВКА ДРУГИХ  DE


# Установка MATE
#v1
sudo apt-get install mate-minimal
#v2
sudo apt-get install mate-default

# Установка GNOME
#v1
sudo apt-get install gnome3-minimal
#v2
sudo apt-get install gnome3-default

# Проблема с отображением пользователя на экране приветствия GDM
# https://alt-gnome.wiki/hidden-user-in-userlist-workaround.html
su -
cat /etc/shadow | grep "имя пользователя"
# если вы там себя не нашли, то добавляем
echo "имя пользователя:*:19709::::::" >> /etc/shadow
# пример: echo "gamer:*:19709::::::" >> /etc/shadow
#  проверяем
cat /etc/shadow | grep "имя пользователя"

# GNOME ONLY
sudo apt-get install -y gnome-screenshot gnome-disk-utility gnome-system-monitor 
#v1
sudo apt-get install -y gdm-settings
#2
flatpak install flathub io.github.realmazharhussain.GdmSettings

#dbus-run-session -- gnome-shell --nested --wayland


# УСТАНОВКА И ЗАПУСК CINNAMON
#v1 минимальная
sudo apt-get install -y cinnamon-minimal galculator gedit gnome-screenshot nemo nemo-fileroller nemo-python nemo-share nemo-extensions-translations nemo-translations  nemo-share-common nemo-terminal pix xsensors gnome-system-monitor blueberry gnome-disk-utility rfkill blueman bluez bluez-alsa bluez-tools gkrellm-bluez libgtop libgtop-devel libgtop-gir

#v2 полная
sudo apt-get install -y cinnamon-default galculator gedit gnome-screenshot nemo nemo-fileroller nemo-python nemo-share nemo-extensions-translations nemo-translations  nemo-share-common nemo-terminal pix xsensors gnome-system-monitor blueberry gnome-disk-utility rfkill blueman bluez bluez-alsa bluez-tools gkrellm-bluez libgtop libgtop-devel libgtop-gir
# default nemo
sudo apt-get install -y nemo nemo-fileroller nemo-python nemo-share nemo-extensions-translations nemo-translations  nemo-share-common nemo-terminal 
xdg-mime default nemo.desktop inode/directory application/x-gnome-saved-search

# Error font Symbola
cd "/home/$USER"
rm -f "Symbola_hint_cinnamon.tar.xz"
wget "https://github.com/redrootmin/bzu-gmb-modules/releases/download/v4/Symbola_hint_cinnamon.tar.xz" || wget "https://github.com/redrootmin/bzu-gmb-modules/releases/download/v4/Symbola_hint_cinnamon.tar.xz"
cd "/usr/share/fonts"
sudo tar -xpJf "/home/$USER/Symbola_hint_cinnamon.tar.xz"
rm -f "/home/$USER/Symbola_hint_cinnamon.tar.xz"


# Установка KDE
# https://www.altlinux.org/KDE
#v1
sudo apt-get install kde5 kde5-profile lightdm-kde-greeter
#v2
sudo apt-get install kde5-big  kde5-profile lightdm-kde-greeter

# включить полный функционал КДЕ (спасибо Соня!)
sudo nano /etc/kf5/xdg/kdeglobals
# сделать ghns=true  и перезагрузка
[KDE Action Restrictions][$i]
ghns=true

#виджиты
https://github.com/chf2000/smartER-video-wallpaper-color-fix

# Simple Desktop Display Manager (SDDM) (не рекомендую)
sudo apt-get install kde5-display-manager-5-sddm
sudo systemctl status display-manager
sudo systemctl disable display-manager
sudo systemctl enable sddm
sudo systemctl reboot

# LightDM — это экранный менеджер для X Window System X11, Wayland
sudo apt-get install kde5-display-manager-lightdm
sudo systemctl disable display-manager
systemctl enable lightdm
sudo systemctl reboot

# CONVERT ALL VIDEO FILE *.AVI IN FOLDER TO YOUTUBE CONFORT COMPRESSION AND MP4 FORMAT, RUN IN TERMINAL IN FOLDER
for i in *.avi; do ffmpeg -i "$i" -c:a aac -b:a 128k -c:v libx264 -crf 20 "${i%.avi}.mp4"; done
#stabilize video
for i in *.mp4; do ffmpeg -i "$i" -vf deshake "stabilized-${i}.mp4"; done














#В РАЗРАБОТКЕ
MESA_VK_DEVICE_SELECT=list vulkaninfo
MESA_VK_DEVICE_SELECT='002:6863' %command%

nmcli con mod wg0 connection.autoconnect no



#[OPTIMIZATION SOUND CARD]
Открываем
sudo gedit /etc/pulse/daemon.conf
#Находим строки и заменяем
resample-method = speex-float-1 на resample-method = src-sinc-medium-quality
default-sample-format = s16le на default-sample-format = s32le
default-sample-rate = 44100 на default-sample-rate = 192000
alternate-sample-rate = 48000 на alternate-sample-rate = 192000
#Если перед строками стоят ; или # - удаляем эти знаки.
#Нажимаем в редакторе сохранить и выходим.
#Открываем
sudo gedit /usr/share/alsa/alsa.conf
#Находим строку и заменяем
defaults.pcm.dmix.rate 192000
#Нажимаем в редакторе сохранить и выходим.
Перезагружаемся!
ЕСЛИ ПОЯВИЛСЯ ПРОТИВНЫЙ СРИП\СКРЕЖЕТ В ЗВУКЕ
# Открываем sudo gedit /etc/pulse/daemon.conf
#Находим строку и заменяем
realtime-scheduling = yes и меняем на realtime-scheduling = no
# Если перед строками стоят ; или # - удаляем эти знаки.


# подключение к сети wi-fi без запроса пароля
echo  $USER
# запомнить имя пользователя
su -
echo"polkit.addRule(function(action, subject) {
	if (action.id == "org.freedesktop.NetworkManager.settings.modify.system" && subject.isInGroup("имя пользователя")) {
		return polkit.Result.YES;
	};
});" > /etc/polkit-1/rules.d/99-networkmanager.rules
# перелогиниться
# если возникнут проблемы удалить файл
sudo rm -f  /etc/polkit-1/rules.d/99-networkmanager.rules

