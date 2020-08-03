- Carregar perfil de teclado  
**loadkeys br-abnt2**

- Testar conexão  
**ping google.com**

- Em caso de wifi  
**wifi-menu**

- Exibir todas os dispositivos disponíveis  
**fdisk -l**

- Para exibir as partições do disco sda  
**fdisk -l /dev/sda**

- Para criar uma tabela de partição  
**cfdisk /dev/sda**  
	- Escolher a label gpt
	- Criar 4 partições
    	- **sda1** 500MB efi (type = BIOS boot)
    	- **sda2** 50GB /
    	- **sda3** Todo o resto para /home/ 
    	- **sda4** 2GB swap

- Formatar partições  
**mkfs.fat -F32 /dev/sda1**  
**mkfs.ext4 /dev/sda2**  
**mkfs.ext4 /dev/sda3**  
**mkswap /dev/sda4**

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

- Editar o mirrorlist (movendo a linha para cima ou comentando os outros com '#')  
**nano /etc/pacman.d/mirrorlist**

- Instalar o sistema  
**pactrap /mnt base linux linux-firmware**

- Se quiser instalar a versão de desenvolvedor, rodar:  
**pactrap /mnt base base-devel**

- Gerar um FSTAB para informar onde estão as partições do sistema  
**genfstab -U -p /mnt >> /mnt/etc/fstab**
	- Para verificar se foi gerar:  
	**cat /mnt/etc/fstab**

- Mudar para o novo sistema criado  
**arch-chroot /mnt**

- Definir relógio  
**ln -sf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime**

