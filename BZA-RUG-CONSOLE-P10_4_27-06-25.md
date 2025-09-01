# BZA-RUG [Bezymnye Zapiski Altika - Russian Utility for Gamers]

# ПЕРВИЧНАЯ НАСТРОЙКА СИСТЕМЫ И УСТАНОВКА УТИЛИТ/ПРОГРАММ

# Включаем sudo для пользователя
su -
# если sudo не стоит: sudo apt-get install sudo
control sudowheel enabled

# полное обновление системы
apt-get update
apt-get dist-upgrade
# обновление ядра
update-kernel -t std-def
# установка тестового/в разработке ядра
update-kernel -t un-def

# установка ядра 6.12 в р10 (нестабильная версия для тестирования)
sudo apt-repo test 361653
sudo apt-get install kernel-image-6.12 kernel-modules-drm-6.12

# install flathub repo
flatpak remote-add flathub https://flathub.org/repo/flathub.flatpakrepo
# flatpak install flathub org.virt_manager.virt-manager
# flatpak install flathub no.mifi.losslesscut
# flatpak install flathub com.mattjakeman.ExtensionManager
# flatpak install flathub org.onlyoffice.desktopeditors
# flatpak install flathub net.retrodeck.retrodeck
# flatpak install flathub net.davidotek.pupgui2
# flatpak install flathub net.rpcs3.RPCS3
# flatpak install flathub net.shadps4.shadPS4
# flatpak install flathub io.github.ryubing.Ryujinx
# flatpak install flathub org.DolphinEmu.dolphin-emu
# flatpak install flathub net.pcsx2.PCSX2 
# flatpak install flathub org.duckstation.DuckStation
# flatpak install flathub ru.linux_gaming.PortProton
# flatpak install flathub io.github.antimicrox.antimicrox
# flatpak install flathub com.moonlight_stream.Moonlight
# flatpak install flathub net.veloren.veloren
# flatpak install flathub com.mojang.Minecraft

# Сделать из Альт системы переносную для загрузки из USB диска/флешки
nano /etc/initrd.mk
# В /etc/initrd.mk добавить:
MODULES_TRY_ADD += kernel/drivers/scsi/sd_mod.ko
MODULES_TRY_ADD += kernel/drivers/usb
# выполнить
sudo make-initrd 
# если нужно ядро которое не загружено
ls /lib/modules/
sudo make-initrd --kernel=папка с модулями полученными из выхлопа 'ls /lib/modules/'

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

# установка установка различных полезных утилит, библиотек, программ (выбор redroot'та)
sudo apt-get install -f -y sudo anilibria-winmaclinux hplip eepm flatpak inxi paprefs pavucontrol cvt cpu-x psensor xsensors kdiskmark system-monitoring-center encfs cpupower yad zenity libgtksourceview3 neofetch git  terminator gnome-disk-utility gparted corectrl cameractrls qbittorrent timeshift nano python3-module-pip gnome-screenshot gnome-system-monitor  p7zip meld
# для сборки пакетов и компиляции из исходников
sudo apt-get install -y rpm-build rpmlint python gear hasher patch rpmdevtools meson gcc gcc-c++ cmake ninja-build

# pywebview - запуск вебстраниц как python срипт/программа
sudo apt-get install -f -y  libwebkit2gtk-gir libwebkit2gtk-devel libwebkit2gtk libwxGTK3.0-webview libwxGTK3.2-webview gambas-gb-gtk3-webview python3-module-pip;pip3 install pywebview glob2 Signal8 PyGTK


# ИКОНКИ NUMIX+PAPIRUS
cd "/home/$USER"
rm -f alt-numix-icons-all.tar.xz
wget "https://github.com/redrootmin/bzu-gmb-modules/releases/download/v4/alt-numix-icons-all.tar.xz" || wget "https://github.com/redrootmin/bzu-gmb-modules/releases/download/v4/alt-numix-icons-all.tar.xz"
cd "/usr/share/icons"
sudo tar -xpJf "/home/$USER/alt-numix-icons-all.tar.xz"
rm -f "/home/$USER/alt-numix-icons-all.tar.xz"
sudo apt-get install -y papirus-icon-theme

# REDROOT WALLPAPERS
cd "/home/$USER"
rm -f "alt-gnome-wallpapers-v1.tar.xz"
wget "https://github.com/redrootmin/bzu-gmb-modules/releases/download/v4/alt-gnome-wallpapers-v1.tar.xz" || wget "https://github.com/redrootmin/bzu-gmb-modules/releases/download/v4/alt-gnome-wallpapers-v1.tar.xz"
cd "/usr/share"
sudo tar -xpJf "/home/$USER/alt-gnome-wallpapers-v1.tar.xz"
rm -f "/home/$USER/alt-gnome-wallpapers-v1.tar.xz"
sudo apt-get install -y papirus-icon-theme

# NVIDIA GPU
epm play switch-to-nvidia
sudo apt-get install -y gwe nvidia-cuda-devel
# NVIDIA-ZINK
env __GLX_VENDOR_LIBRARY_NAME=mesa __EGL_VENDOR_LIBRARY_FILENAMES=/usr/share/glvnd/egl_vendor.d/50_mesa.json MESA_LOADER_DRIVER_OVERRIDE=zink GALLIUM_DRIVER=zink LIBGL_KOPPER_DRI2=1 mangohud <app>


# AMD GPU
epm play i586-fix
sudo apt-get install -y i586-xorg-drv-radeon i586-xorg-dri-radeon i586-xorg-dri-swrast i586-libGL i586-libEGL i586-libxatracker i586-libgbm i586-libGLES i586-libGLX i586-libglvnd i586-libnsl1

# AMD HIP RENDER (тестировал только в р11)
sudo apt-get install -y blender-cycles-hip-kernels hip-runtime-amd
# errors/freez/artifacts blender + amd hip + vega/rdna
# Memory access fault by GPU node-1 (Agent handle: 0x7f828183fe00) on address 0x7f8203aa7000. Reason: Page not present or supervisor privilege.
sudo usermod -a -G video $USER
sudo usermod -a -G render $USER
# start blender in terminal
blender --factory-startup --debug-all

#overclock+undervolting в картах АМД rx400/500+
sudo nano /etc/sysconfig/grub2
# => GRUB_CMDLINE_LINUX_DEFAULT="quiet splash" + amdgpu.ppfeaturemask=0xffffffff + mitigations=off(TORN OFF MELTDOWN\SPECTRE+)]
 GRUB_CMDLINE_LINUX_DEFAULT="quiet splash amdgpu.ppfeaturemask=0xffffffff mitigations=off"
# включить возможность использовать Vulkan 1.1 в видеокартах HD7970 
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash amdgpu.ppfeaturemask=0xffffffff modprobe.blacklist=radeon radeon.si_support=0 radeon.cik_support=0 amdgpu.si_support=1 amdgpu.cik_support=1 radeon.audio=1 mitigations=off"

sudo grub-mkconfig -o /boot/grub/grub.cfg


# INTEL GPU
sudo apt-get install -y i586-libGL i586-libGLU i586-xorg-dri-intel


#3 GAMING
epm play -y discord i586-fix steam
sudo apt-get install -y ds4drv mangohud goverlay portproton antimicro
#  как альтернатива corectrl
sudo apt-get install -f -y radeon-profile radeon-profile-daemon

# открытые и бесплатные  игры в Альт
sudo apt-get install -y minetest supertux2 supertuxkart 0ad warzone2100 openttd

# for RPCS3 appimage
sudo mkdir -p /etc/ssl/certs
sudo ln -s /etc/pki/tls/certs/ca-bundle.crt  /etc/ssl/certs/ca-certificates.crt

# запуск corectrl без пароля и включение в автозагрузку
sudo usermod -a -G corectrl $USER
reboot
cp /usr/share/applications/org.corectrl.corectrl.desktop ~/.config/autostart/

# gamescope-session-steam gamescope-session-plus


# ds4drv  - драйвер для геймпадов sony DS4
systemctl enable --now ds4drv
#std-def
sudo apt-get install -y --reinstall dkms kernel-headers-std-def kernel-headers-modules-std-def git
# un-def
sudo apt-get install -y --reinstall dkms kernel-headers-un-def kernel-headers-modules-un-def git

sudo git clone https://github.com/ozz-is-here/hid-sony-fix-dkms.git /usr/src/hid-sony-fix-dkms-0.1
sudo dkms install --force -m hid-sony-fix-dkms -v 0.1


# Офисные пакеты
# onlyoffice №1
wget https://download.onlyoffice.com/install/desktop/editors/linux/onlyoffice-desktopeditors.x86_64.rpm
epm install --repack --scripts onlyoffice-desktopeditors.x86_64.rpm
# onlyoffice №2
epm play onlyoffice
# epm play --update all
# wps_office
epm play -y wpsoffice

# r7-office
epm play r7-office
# myoffice
epm play myoffice
# scribus
sudo apt-get install -y scribus
# pdf редактирование
epm play -y master-pdf-editor
# для интернета
sudo apt-get install -y chromium-gost yandex-browser-stable thunderbird firefox nextcloud-client chromium 
epm play -y telegram

# принтеры HP
sudo apt-get install system-config-printer  hplip
hp-plugin -i
# Следуйте инструкциям. Будьте готовы ввести пароль суперпользователя.

# Yandex-disk
wget -Nc https://repo.yandex.ru/yandex-disk/yandex-disk-latest.x86_64.rpm
sudo apt-get install -y yandex-disk-latest.x86_64.rpm;sudo apt-get install -y yandex-disk-indicator 

# Программы для создания и обработки мультимедиа, программирование, аудио монтаж/обработка
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
sudo apt-get install -y cinnamon-minimal galculator gedit gnome-screenshot nemo nemo-fileroller nemo-python nemo-share nemo-extensions-translations nemo-translations  nemo-share-common nemo-terminal pix xsensors gnome-system-monitor blueberry gnome-disk-utility bluez bluez-tools rfkill blueman libgtop libgtop-devel libgtop-gir

#v2 полная
sudo apt-get install -y cinnamon-default galculator gedit gnome-screenshot nemo nemo-fileroller nemo-python nemo-share nemo-extensions-translations nemo-translations  nemo-share-common nemo-terminal pix xsensors gnome-system-monitor blueberry gnome-disk-utility bluez bluez-tools rfkill blueman libgtop libgtop-devel libgtop-gir
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

