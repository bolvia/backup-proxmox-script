# backup-proxmox-script
Backup automatizado proxmox para Wincp

Criar um diretório
- mkdir /backups
+++++++++++

Jogar o script no caminho backup_script.sh

#!/bin/bash

#!/bin/bash

# Diretório onde os backups serão armazenados localmente no Proxmox
backup_dir="/backups

# Nome do servidor Windows, endereço IP ou nome DNS
win_server="seu_servidor_windows"

# Nome de usuário e senha do servidor Windows
win_user="seu_usuario_windows"
win_password="sua_senha_windows"

# Diretório de destino no servidor Windows (utilize /caminho/para/diretorio/destino/no/servidor/windows)
win_destination="/path/to/destination/on/windows/server"

# Comando para realizar o backup das VMs ativas
for vmid in $(qm list | awk '!/VMID/{print $1}')
do
  vzdump $vmid --compress lzo 
done

# Comando para transferir os backups para o servidor Windows usando o WinSCP
# Certifique-se de ter o WinSCP instalado e o comando 'winscp' disponível no seu Proxmox
winscp /command "option batch abort" "option confirm off" "open scp://$win_user:$win_password@$win_server/" "put $backup_dir/*.vma.lzo $win_destination/" "exit"

++++++++++++

Dar permissao para o diretorio

- chmod +x backup_script.sh

+++++++++++++++++


Testar o script manualmente
  
Rodar:

  ./backup_script.sh

  ++++++++++++

  Definir o intervalo de backup via crontab

Acesse o diretorio crontab -e

Meu caso esta para rodar de 15 e 15 dias

0 2 */15 * * /root/backup_script.sh

++++++

Permitindo

chmod +x /root/backup_script.sh



  
