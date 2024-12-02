
# Clear-Linux-config
Memo de configuration pour mon Clear Linux sur ZENBOOK 13

ATTENTION : mémo en cours de rédaction, certains réglages ne sont pas adaptés à Clear Linux mais proviennent de Fedora!!
  
Sommaire :

1. [Installation](https://github.com/Shogu/Configuration-Fedora-ASUS-Zenbook-13/blob/main/README.md#1---installation)

2. [Réglages de base](https://github.com/Shogu/Configuration-Fedora-ASUS-Zenbook-13/blob/main/README.md#2---réglages-de-base)

3. [Remplacement et installation de logiciels et codecs](https://github.com/Shogu/Configuration-Fedora-ASUS-Zenbook-13/blob/main/README.md#3---remplacement-et-installation-de-logiciels-et-codecs)

4. [Réglages des navigateurs Opera & Firefox](https://github.com/Shogu/Configuration-Fedora-ASUS-Zenbook-13/blob/main/README.md#4---réglages-des-navigateurs-opera--firefox)

5. [Réglages de l'UI Gnome Shell](https://github.com/Shogu/Configuration-Fedora-ASUS-Zenbook-13/blob/main/README.md#5---réglages-de-lui-gnome-shell)
   
6. [Allégement du système](https://github.com/Shogu/Configuration-Fedora-ASUS-Zenbook-13/blob/main/README.md#6---allégement-du-système)

7. [Optimisation du système](https://github.com/Shogu/Configuration-Fedora-ASUS-Zenbook-13/blob/main/README.md#7---optimisation-du-système)

8. [Maintenance de la distribution](https://github.com/Shogu/Configuration-Fedora-ASUS-Zenbook-13?tab=readme-ov-file#7---optimisation-du-système)

   
   

## **1 - Installation**

* a - Désactiver `Secure Boot` dans le Bios (F2)

* b - Désactiver la caméra et le lecteur de carte dans le bios

* c - Passer les arguments suivants dans le kernel pour ne pas brider le processeur & pour un démarrage silent boot :
  
  ```
  mitigations=off quiet loglevel=3 vt.global_cursor_default=0 systemd.show_status=false
  ```
  
* d - ATTENTION : le mot de passe user doit pouvoir être saisi sur un clavier US ET un clavier FR : ne pas metre de a 
      ou de z ou de m...Voir si ce réglage permet de régler ce problème :
  ```
  sudo localectl set-keymap fr
  ```




## **2 - Réglages de base**

* a - dès le reboot, penser à installer les applications Flatpak, sans quoi le bug avec curl empêche de le faire plus 
      tard (voir cette page : https://community.clearlinux.org/t/flatpak-issues-after-42630-update/10166) :

   ```
  flatpak install flathub com.mattjakeman.ExtensionManager io.github.giantpinkrobots.flatsweep net.nokyan.Resources 
  org.jdownloader.JDownloader  org.onlyoffice.desktopeditors de.haeckerfelix.Fragments
  ```

     Y ajouter `Opera, Papers, Loupe, Nicotine+, Web, Celluloid, Gestionnaire d'extensions, Opera, Showtime ???, Menu Principal`.

* - b - Installer Maestral pour Dropbox :
    ```
    python3 -m venv maestral-venv
    mv maestral-venv .maestral-venv
    source .maestral-venv/bin/activate
    sudo python3 -m pip install --upgrade 'maestral[gui]'
    ```
    Puis lancer Maestral avec `maestral gui` et mettre une icone au dossier Dropbox

* b - Régler le système avec `Paramètres` puis `Ajustements` (lancement au boot de Dropbox + rendu de la police + 
      stretch pour le fond d'écran, sans quoi le wallpaper par défaut est          cropé...)
  

* c - Régler Nautilus (ouvrir d'un seul clic, taille des icones, cacher les dossiers Bureau Public et Modèles...) & 
      créer un marque-page pour `Dropbox` & pour l'accès `ftp` au disque         SSD sur la TV Android :
  
  ```
  ftp://192.168.31.68:2121
  ```
  

* d - Supprimer le mot de passe au démarrage avec le logiciel Mots de passe puis penser à reconnecter le compte 
      Google dans Gnome :

  ```
  rm -v ~/.local/share/keyrings/*.keyring && reboot
  ```

* e - Configurer le compte utilisateur : télécharger la vignette user `user-astronaut`, la rendre invisible avec . et 
      la mettre dans /home. Puis permettre le login automatique en contournant comme suit le bug de Clear Linux :

  ```
  sudo gnome-text-editor  /etc/gdm/custom.conf
  ```
p
     Et saisir :

  ```
  [daemon]
  AutomaticLoginEnable=True
  AutomaticLogin=ogu
  ```


## **3 - Suppression de logiciels**


* a - Supprimer les bundles inutiles :

  ```
  sudo swupd bundle-remove firefox x11-tools xterm xscreensaver x11vnc vim totem snapshot gnome-weather gnome-todo gnome-system-monitor gnome-photos gnome-music gnome-font-viewer gnome-characters  geary gimp evince eog emacs-x11 baobab aspell-es aspell-de Endeavour gvim gnome-terminal hardware-printing
  ```

* b - Supprimer les entrées des logiciels inutiles mais impossibles à supprimer : lancer Menu Principal et masquer les entrées. En profiter pour mettre l'icone de Gnome-terminal pour Console, et pour renommer les applis.

  Nota : il est possible de supprimer les binaires des logiciels suivants, swupd-repair les remettra en place si besoin :
  ```
  cd /usr/bin/ && sudo rm system-config-printer yelp nm-connection-editor gnome-connections gnome-terminal emoji-picker gnome-extensions pavucontrol idle3 idle3.13 simple-scan
  ```
  

## **4 - Réglages du navigateur Opera**

* a - Passer Opera en navigateur par défaut dans Gnome : 
  ```
  xdg-settings set default-web-browser com.opera.Opera.desktop
  ```

* b - Passer Opera en français : éditer le raccourci avec l'application :
  
  ```
  flatpak install flathub page.codeberg.libre_menu_editor.LibreMenuEditor
  ```
  puis remplacer le chemin du raccourci par  `/usr/bin/flatpak run --branch=stable --arch=x86_64 --command=opera --file-forwarding com.opera.Opera @@u --lang=fr %U @@`
  

* c - Editer les raccourcis Opera : pour rouvrir un onglet fermé avec `ctrl-q`, & `ctrl-s` pour `Sélectionner l'onglet actif précédent`

* d - Passer Opera sur `wayland` avec le flag `chrome://flags/#ozone-platform-hint` & `opera://flags/#wayland-per-window-scaling`, puis activer l'`autoclose` de xwayland (voir plus bas).
  
* e - Créer les `tuiles` dans la page d'accueil

* f - Extensions Opera :

   1 - [Decentraleyes](https://addons.opera.com/fr/extensions/details/decentraleyes/)

   2 - [Raindrop](https://raindrop.io/r/extension/chrome)

   3 - [uBlock Origin](https://addons.opera.com/fr/extensions/details/ublock/) Penser à désactiver le bloqueur de pub natif d'Opera qui est bien moins performant, puis régler l'extension avec les filtres.
  
   4 - [Enhancer for Youtube](https://chromewebstore.google.com/detail/enhancer-for-youtube/ponfpcnoihfmfllpaingbgckeeldkhle?hl=fr) qui intègre leurs fonctions. Le configurer d'un coup en rentrant le code du fichier `youtube_enhancer_conf`

   5 - [Tiny Suspender](https://chromewebstore.google.com/detail/tiny-suspender/bbomjaikkcabgmfaomdichgcodnaeecf)
  

   
* g - Désactiver les options inutiles et `Faire defiler les onglets dans l'ordre d'utilisation`
  
* h - Dans `about:flags`, désactiver les options suivantes :
  

  1 - disable Pinboard : `chrome://flags/#pinboard` 
  
  2 - disable Emoji tab : `chrome://flags/#tab-art`
      
  3 - disable Sync banner : `chrome://flags/#startpage-sync-banner`
    
  4 - disable Wallet : `chrome://flags/#native-crypto-wallet`
    
  5 - disable Caption : `chrome://flags/#enable-live-caption-multilang` & `chrome://flags/#enable- 
  accessibility-live-caption`
      
  6 - disable Reading mode Screen2x : `chrome://flags/#read-anything-with-screen2x`

  7 - disable Drag des groupes & onglets : `chrome://flags/#drag-multiple-tabs`
  

  et *activer* celles-ci :
  

  8 - enable Sidebar : `opera://flags/#sidebar-site-panel`
  
  9 - enable GPU Rasterization : `chrome://flags/#enable-gpu-rasterization` & `chrome://flags/#canvas-oop-rasterization`
  
  10 - enable Memory Saver : `opera://flags/#memory-saver` & `chrome://flags/#memory-saver-multi-state-mode` puis activer les options.
    
  11 - enable Auto-Disable accessibility : `chrome://flags/#enable-auto-disable-accessibility`
     
  12 - enable Dark thème pour les sites : `opera://flags/#enable-force-dark-from-settings` & `chrome://flags/#enable-force-dark`
    
  13 - enable Parallel Downloading : `opera://flags/#enable-parallel-downloading`

  14 - enable Oop video decoding : `opera://flags/#use-out-of-process-video-decoding`

 

* i - Créer dans la barre latérale d'Opera les sites internet suivants APRES s'être connecté à 
      Google :
  
     [Gmail](https://mail.google.com/mail/u/0/?pli=1#inbox)
    
     [Raindrop](https://app.raindrop.io/my/0)
  




## **5 - Réglages de l'UI Gnome Shell**

* a - Adwaita Darker :

  https://github.com/varunbpatil/Adwaita-darker
  Nota : il faut se déconnecter de Github pour avoir accès au téléchargement du dossier (??)

* b - Curseurs Bibata :
      https://github.com/ful1e5/Bibata_Cursor?tab=readme-ov-file

* a - Installer diverses extensions :
  
    1 - [Appindicator](https://extensions.gnome.org/extension/615/appindicator-support/)

    2 - [Alphabetical Grid](https://extensions.gnome.org/extension/4269/alphabetical-app-grid/) puis la supprimer : l'ordre alphabetique persistera.

    3 - [AutoActivities](https://extensions.gnome.org/extension/5500/auto-activities/)
  
    4 - [Battery Time](https://extensions.gnome.org/extension/5425/battery-time/)
    
    5 - [Caffeine](https://extensions.gnome.org/extension/517/caffeine/)
  
    6 - [Clipboard History](https://extensions.gnome.org/extension/4839/clipboard-history/)
    
    7 - [Hot Edge](https://extensions.gnome.org/extension/4222/hot-edge/)
    
    8 - [Custom Command Toggle](https://extensions.gnome.org/extension/7012/custom-command-toggle/)
    
    9 - [Privacy Quick Settings](https://extensions.gnome.org/extension/4491/privacy-settings-menu/) puis la 
         supprimer une fois les réglages réalisés.
  
   10 - [Grand Theft Focus](https://extensions.gnome.org/extension/5410/grand-theft-focus/)



* e - Raccourcis à éditer dans Gnome : mettre ```kgx``` à la place de la touche Exposant, et la 
      commande ```flatpak run net.nokyan.Resources``` pour la combinaison ```ctrl-alt-supp```.

* f - Améliorer  Celluloid :
    - inscrire `hwdec=auto-safe` ou `vo=gpu-next`dans Paramètres --> Divers --> Options supplémentaires
    - installer les deux scripts lua suivants pour la musique :
      [Visualizer](https://www.dropbox.com/scl/fi/bbwlvfhtjnu8sgr4yoai9/visualizer.lua?rlkey=gr3bmjnrlexj7onqrxzjqxafl&dl=0)
      [Delete File avec traduction française](https://www.dropbox.com/scl/fi/c2cacmw2a815husriuvc1/delete_file.lua?rlkey=6b9d352xtvybu685ujx5mpv7v&dl=0)
      - activer l'option `focus` et `toujours afficher les boutons de titre`
  
* j - Améliorer l'autocomplétion du terminal en téléchargeant le fichier`.inputrc` et le placer dans `~/`, puis 
      changer les polices au profit de `Liberation Mono 12`.
      Fichier inputrc :
      ```
      set completion-ignore-case on       # Ignore la casse lors de la complétion
      set show-all-if-ambiguous on        # Affiche toutes les options si ambiguïté
      set show-all-if-unmodified on       # Affiche toutes les options si la ligne n'a pas changé
      set visible-stats on                # Montre des infos comme les permissions (ls-like)
      TAB: menu-complete                  # Permet de parcourir les suggestions avec TAB
      ```
  


* l - Télécharger le script de `transfert des vidéos` intitulé `.transfert_videos` pour déplacer automatiquement les 
      vidéos vers Vidéos en supprimant le sous-dossier d'origine : en faire un raccourci avec l'éditeur de menu et 
      lui mettre l'icone `/usr/share/icons/Adwaita/scalable/devices/drive-multidisk.svg`






******** Rajouter des toggles au menu de Gnome-Shell ********
     
    
* p - Créer un toggle `Powertop` qui va : lancer powertop en `auto-tune` après avoir lancé pendant une heure la commande `sudo powertop --calibrate` pour économiser encore plus de batterie,  baisser la luminosité sur 5%, enfin désactiver le Turbo Boost du processeur: rentrer cette commande pour le toggle activé :
  
```
pkexec powertop --auto-tune && pkexec sh -c 'echo 0 > /sys/devices/system/cpu/intel_pstate/no_turbo' && gdbus call --session --dest org.gnome.SettingsDaemon.Power --object-path /org/gnome/SettingsDaemon/Power --method org.freedesktop.DBus.Properties.Set org.gnome.SettingsDaemon.Power.Screen Brightness "<int32 5>" && notify-send "Mode Powertop activé"
```
  
Et cette commande pour le toggle désactivé :
      
```
pkexec sh -c 'echo 1 > /sys/devices/system/cpu/intel_pstate/no_turbo' && gdbus call --session --dest org.gnome.SettingsDaemon.Power --object-path /org/gnome/SettingsDaemon/Power --method org.freedesktop.DBus.Properties.Set org.gnome.SettingsDaemon.Power.Screen Brightness "<int32 12>" && notify-send "Turbo Boost réactivé"
```
     
  Enfin rentrer le nom de l'icone : `power-profile-power-saver-symbolic` 

 
* q - Créer un toggle `Performance` pour switcher de Powersave à Performance :

  ```
  echo performance | pkexec tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor && notify-send “Mode Performance activé”
  ```

  ```
  echo powersave | pkexec tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor && notify-send “Mode Powersave activé”
  ```
  
  Icone :  `power-profile-performance-symbolic`


## **6 - Allégement du système**

  
* c - Supprimer les autostarts inutiles :
  
  ```
  sudo rm -rf /usr/share/gdm/greeter/autostart/orca.desktop
  sudo rm -rf /usr/share/xdg/autostart/gnome-flackback*
  sudo rm -rf /usr/share/xdg/autostart/org.clearlinux.initFlathubRepo.desktop

  ```

  
* f - Supprimer et masquer les services inutiles :
  
  ```
  sudo systemctl mask NetworkManager-wait-online.service auditd.service ModemManager.service avahi-daemon.service 
  plymouth-quit-wait.service switcheroo-control.service sys-kernel-tracing.mount sys-kernel-debug.mount httpd.service 
  mdmonitor.service mdmonitor.service raid-check.timer sssd-kcm.service pcscd raid-check.timer fwupd
  avahi-daemon.socket
  ```
  
     et désactiver le Bluetooth pour l' activer à la volée (voir script dans la rubrique UI Gnome) + cups :
  
  ```
  sudo systemctl disable bluetooth.service cups
  ```
  

     Enfin, reboot puis controle de l'état des services avec :
  
  ```
  systemd-analyze blame | grep -v '\.device$'
  ```

     et :

```
systemctl list-unit-files --type=service --state=enabled
```
  
* g - Supprimer l'autoboot d'Orca si besoin :
  
  ```
  rm -rf /usr/share/gdm/greeter/autostart/orca.desktop
  ```
  

* h - Alléger les journaux système et les mettre en RAM :
  
  ```
  sudo gnome-text-editor /usr/lib/systemd/journald.conf.d/*.conf
  ```
  et saisir `Storage=volatile`
  
     ou remplacer le contenu du fichier par celui du fichier `journald.conf.txt` & relancer le service :
  
  ```
  sudo systemctl restart systemd-journald
  ```
  
* j - Supprimer les `coredump`:
  
  ``` 
  sudo ln -sf /dev/null /usr/lib/sysctl.d/50-coredump.conf
  ```

  Ou bien checker s'il est préférable ou pas d'éditer le fichier comme suit :
  
  ```
  [Coredump]
  Storage=none
  ProcessSizeMax=0
  ```
     


* k - Supprimer le `watchdog` et blacklister les pilotes inutiles `Nouveau` & `ELAN:Fingerprint` : éditer le fichier 
      suivant :
  
  ```
  sudo gnome-text-editor /etc/sysctl.conf
  ```
     et ajouter :
  
  ```
  kernel.nmi_watchdog=0
  ```
     Puis créer un fichier `blacklist` ```sudo gnome-text-editor /etc/modprobe.d/blacklist.conf``` et l'éditer :
  
  ```
  blacklist iTCO_vendor_support
  blacklist wdat_wdt
  blacklist intel_pmc_bxt
  blacklist nouveau
  blacklist ELAN:Fingerprint
  ```

  
* m - Faire le tri dans `~/.local/share/`, `/home/ogu/.config/`, `/usr/share/` et `/etc/`




## **7 - Optimisation du système**


* a - Passer `xwayland` en autoclose : sur dconf-editor, modifier la clé suivante.
  
  ```
  org.gnome.mutter experimental-features
  ```


  
     Au reboot, contrôler le fichier de boot de `systemd-boot` avec la commande :
  
  ```
  cat /proc/cmdline
  ```

* b - Activer fstrim sur le SSD :

  ```
  systemctl enable fstrim.timer
  ```

* c - Se donner plus de marge de swap : passer le swap à 512 Mo

  ```
  sudo swapoff /var/swapfile 
  sudo rm /var/swapfile 
  sudo fallocate -l 512MiB /var/swapfile
  sudo mkswap /var/swapfile
  sudo chmod 0600 /var/swapfile
  sudo swapon /var/swapfile
  ```
  Controler avec `free -h`
  
* d - Supprimer le délai d'attente du chargeur de démarrage

  ```
  sudo clr-boot-manager set-timeout && sudo clr-boot-manager update
  ```


* i - Accélérer `swupd` & faire les maj à partir de tmpfs :
  
  ```
  echo 'max_parallel_downloads=20' | sudo tee -a /etc/dnf/dnf.conf

  ```

  ```
sudo cp -r /var/lib/swupd /tmp/swupd
swupd update -S /tmp/swupd && swupd 3rd-party update -S /tmp/swupd
rm -rf /var/lib/swupd
mv /tmp/swupd /var/lib/swupd
```
  
* j - Diviser le nombre de `ttys` au boot par deux :
  
  ```
  sudo gnome-text-editor /etc/systemd/logind.conf
  ```
     puis editer `NautoVTS=3`

* k - Vérifier que le système utilise bien les DNS du routeur Xiaomi (192.168.31.1) :

  ```
  nmcli dev show |grep DNS
  ```






## **8 - Maintenance de la distribution**

sudo swupd verify --fix --picky



 * a -  Télécharger le script complet de mise à jour & nettoyage `.update_fedora.sh` dans `~/`
        NOTA : pensez à lancer Bleachbit en gui une première fois pour sélectionner les options.
   

