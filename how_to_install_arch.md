# Instalação

## Preparação

- Carregar perfil de teclado  
**loadkeys br-abnt2**

- Editar o mirrorlist (movendo a linha para cima ou comentando os outros com '#')  
**reflector --country Brazil --sort rate --save /etc/pacman.d/mirrorlist**

- Testar conexão  
**ping google.com**

- Em caso de wifi  
**wifi-menu**

## Particionamento

- Exibir todas os dispositivos disponíveis  
**fdisk -l**

- Para exibir as partições do disco sda  
**fdisk -l /dev/sda**

- Para criar uma tabela de partição  
**cfdisk /dev/sda**  
	- Escolher a label gpt
	- Criar 4 partições
    	- **sda1** 500MB efi
    	- **sda2** 50GB /
    	- **sda3** Todo o resto para /home/ 
    	- **sda4** 2GB swap

- Formatar partições  
**mkfs.fat -F32 /dev/sda1**  
**mkfs.ext4 /dev/sda2**  
**mkfs.ext4 /dev/sda3**  
**mkswap /dev/sda4**

## Montagem

- Montar o / no /mnt  
**mount /dev/sda2 /mnt**

- Criar pasta /home no /mnt  
**mkdir /mnt/home**

- Criar pasta /boot no /mnt  
**mkdir /mnt/boot**

- Criar pasta /mnt/boot/efi (se for usar EUFI)  
**mkdir /mnt/boot/efi**

- Montar o /home no /mnt/home  
**mount /dev/sda3 /mnt/home**

- Montar a efi em /mnt/boot/efi  
**mount /dev/sda1 /mnt/boot/efi**

- Ativar o swap  
**swapon /dev/sda4**

## Instação do sistema base

- Instalar o sistema  
**pactrap /mnt base linux linux-firmware**

- Se quiser instalar a versão de desenvolvedor, rodar:  
**pactrap /mnt base base-devel**

## Configuração do sistema

- Gerar um FSTAB para informar onde estão as partições do sistema  
**genfstab -U -p /mnt >> /mnt/etc/fstab**
	- Para verificar se foi gerado:  
	**cat /mnt/etc/fstab**

- Atualizar o mirrorlist do novo sistema  
**reflector --country Brazil --sort rate --save /mnt/etc/pacman.d/mirrorlist**

- Mudar para o novo sistema criado  
**arch-chroot /mnt**

- Definir relógio  
**ln -sf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime**

- Definir o idioma do sistema, descomentando a linha pt_BR.UTF-8 UTF-8  
**nano /etc/locale.gen**

- Gere as configurações de linguagem  
**locale-gen**

- Mais algumas configurações de lingua  
**echo LANG=pt_BR.UTF-8 >> /etc/locale.conf**  
**echo KEYMAP=br-abnt2 >> /etc/vconsole.conf**

- Definir o hostname para arch  
**echo arch >> /etc/hostname**

- Definir o arquivo de host  
**echo 127.0.01 localhost.localdoamin localhost >> /etc/hosts**  
**echo ::1 localhost.localdomain localhost >> /etc/hosts**    
**echo 127.0.1.1 arch.localdomain arch >> /etc/hosts**

- Definir a nova senha de root com:  
**passwd**

- Adicionar um novo usuário  
**useradd -m -g users -G wheel eduardo**

- Definir a senha do usuário eduardo  
**passwd eduardo**

- Adicionar pacotes essênciais  
**pacman -S dosfstools os-prober mtools network-manager-applet networkmanager wpa_supplicant wireless_tools dialog sudo alacritty firefox git pulseaudio pulseaudio-alsa jack**

- Adicionar usuário a lista de sudo  
**nano /etc/sudoers**  
	- Adicione o seguinte código ao final do arquivo:  
	**eduardo ALL=(ALL) ALL**  
	- Ou descomentar linha:  
	**# %wheel All=(ALL) NOPASSWD: All**

## Instação do GRUB

- Seguir as instruções baseadas no tipo de instalação utilizada    
	- **BIOS**  
		- Instalar o grub  
		**pacman -S grub**

		- Rodar grub-install  
		**grub-install --target=i386-pc --recheck /dev/sda**

		- Copiar um arquivo para a pasta do grub  
		**cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo**

	- **UEFI**  
		- Instalar o grub  
		**pacman -S grub-efi-x86_64 efibootmgr**

		- Rodar grub-install  
		**grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=arch_grub --recheck**

		- Copiar um arquivo para a pasta do grub  
		**cp /usr/share/locale/en\@quot/LC_MESSAGES/grub.mo /boot/grub/locale/en.mo**
	
	- Rodar  
	**grub-mkconfig -o /boot/grub/grub.cfg**

# Pós-instalação 

- Para conectar ao wifi  
**wifi-menu**

- Para conectar a rede cabeada (ativar o serviço)  
**systemctl status NetworkManager**  
**systemctl enable NetworkManager**  
**systemctl start NetworkManager**

- Atualizar sistemas  
**pacman -Sy**

- Instalar o xorg  
**pacman -S xorg-server**

# Instalar drivers de video

## Intel

- Instalar driver  
**pacman -S xf86-video-intel libgl mesa**

## NVidia

- Instalar driver  
**pacman -S nvidia nvidia-libgl mesa**

## AMD

- Instalar driver  
**pacman -S mesa xf86-video-amdgpu**

## VirtualBox

- Instalar driver de convidado para virtualbox em maquina virtual  
**pacman -S virtualbox-guest-utils virtualbox-host-modules-arch mesa mesa-libgl**

# Instalação do ambiente gráfico

- Escolher o ambiente gráfico que mais lhe agrade, algumas opções são, xfce, dde, gnome, qtile, i3, cinnamon, etc.  
- Para o i3 usar:  
**pacman -S i3-gaps i3status i3lock dmenu**  
- Para xfce  usar:  
**pacman -S xfce4 xfce4-goodies**  
- Para gnome usar:  
**pacman -S gnome**  
- Para plasma usar:  
**pacman -S plasma kde-applications-meta**  
- Para tratar o appstream-data usar:  
  **sudo downgrade archlinux-appstream-data**  
  **escolher opção 20200103**  
- Instalar o YAY(AUR):  
  **git clone https://aur.archlinux.org/yay.git**    
  **cd yay**  
  **makepkg -si**
# Info

Para mais informações, acessar https://diolinux.com.br/2019/07/como-instalar-arch-linux-tutorial-iniciantes.html