# Pro Audio in Manjaro

1) Install Manjaro KDE

2) realtimeconfigquickscan
    ```shell
    git clone https://github.com/raboof/realtimeconfigquickscan.git
    cd realtimeconfigquickscan
    ./realtimeconfigquickscan
    ```

3) install realtime-privileges
    ```shell
    yay -S realtime-privileges
    ```
    Add user to "realtime" group and "audio" group (to satisfy realtimeconfigquickscan)
    ```shell
    sudo usermod -a -G realtime,audio $USER
    ```
    Log out/in or reboot...

4) add "threadirqs" as kernel parameter
    ```shell
    sudo nano /etc/default/grub
    ```
    change 
    `GRUB_CMDLINE_LINUX=""` to `GRUB_CMDLINE_LINUX="threadirqs"`
    
    ```shell
    sudo update-grub
    ```

5) Set governor to "performance"

    i. Temporary:
    ```shell
    sudo cpupower frequency-set -g performance
    ```
    ii. Permanent:
    
    Add `cpufreq.default_governor=performance` as a kernel parameter:
   
    ```shell
    sudo nano /etc/default/grub
    ```
    LIne should now read: 
     `GRUB_CMDLINE_LINUX="cpufreq.default_governor=performance threadirqs"`
     ```shell
    sudo update-grub
    ```
    or, for kernels < 5.9:
    ```shell
    sudo nano etc/default/cpupower` (uncomment governor and change to performance)
    systemctl enable --now cpupower.service
    systemctl start cpupower.service
    ```


6) swappiness
    ```shell
    sudo nano /etc/sysctl.d/99-sysctl.conf
    ```
    add "vm.swappiness=10"

7) base-devel (as necessary)
    ```shell
    sudo pacman -S base-devel
    ```

8) install udev-rtirq
    ```shell
    git clone https://github.com/jhernberg/udev-rtirq.git
    cd udev-rtirq
    make install
    reboot
    ```

9) Jack2 + Jack D-Bus
    ```shell
    yay -S qjackctl jack2-dbus
    ```
    Enable Jack D-Bus interface:  
    ![image](https://user-images.githubusercontent.com/79659262/124497122-51218300-ddb2-11eb-8cb8-4bf873e026cd.png)


10) DAW & Plugins

    REAPER: 
    http://reaper.fm/download.php or,  
    ```shell
    yay -S reaper-bin
    ```
    change RT priority to 40 on audio device page?  

* Native plugins  
  * x42-plugins  
  * airwindows  
  * LSP-plugins  
  * zam-plugins  
  * DISTRHO  
  * DPF-plugins  
  * Dragonfly
  * Bertom Denoiser (https://www.bertomaudio.com/denoiser.html)

11) Wine-tkg + yabridge

    i. Install wine-tkg from https://github.com/Frogging-Family/wine-tkg-git/releases/tag/6.4.r0.g7ec998e1  
    ii. Install yabridge
    ```shell
    yay -S yabridge-bin
    ```
    iii. Configure yabridge according to https://github.com/robbert-vdh/yabridge#readme  
    iv. append "export WINEFSYNC=1" to ~/.bash_profile  
    ```shell
    nano ¬/.bash_profile
    . ~/.bash_profile
    ```
    v. Install Windows VST2 and VST3 (preferred) plugins
    
    
   ### Thanks
   Robbert van der Helm (of yabridge fame) for pointing out typos/omissions and suggesting alternative methods. JamesPeters (REAPER forums) for suggesting expansion of steps 3, 4 & 5.




