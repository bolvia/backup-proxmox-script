# backup-proxmox-script
Backup automatizado proxmox para WinSCP

Criar um diretório
- mkdir /backups

  
+++++++++++

Jogar o script no caminho backup_script.sh


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

++++++++++++++++++++

Script sem as legendas de instrução

#!/bin/bash


backup_dir="/backups


win_server="seu_servidor_windows"


win_user="seu_usuario_windows"

win_password="sua_senha_windows"


win_destination="/path/to/destination/on/windows/server"


for vmid in $(qm list | awk '!/VMID/{print $1}')
do

  vzdump $vmid --compress lzo 
done


+++++++++++++++++++++++++

# Comando para transferir os backups para o servidor Windows usando o WinSCP
# Certifique-se de ter o WinSCP instalado e o comando 'winscp' disponível no seu Proxmox
winscp /command "option batch abort" "option confirm off" "open scp://$win_user:$win_password@$win_server/" "put $backup_dir/*.vma.lzo $win_destination/" "exit"

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

# O script de backup  realizará o backup de todas as VMs ativas no Proxmox. O comando vzdump é usado para fazer o backup das máquinas virtuais no Proxmox
# Realiza o backup de todas as VMs ativas
# for vmid in $(qm list | awk '!/VMID/{print $1}')

# vzdump $vmid --compress lzo
# done
# loop for irá percorrer todas as VMs ativas no Proxmox usando o comando qm list. A parte awk '!/VMID/{print $1}' é usada para filtrar a lista de VMs e obter somente os IDs das VMs (VMID).

# Em seguida, para cada VM ativa, o script executará o comando vzdump com o ID da VM ($vmid). Isso fará o backup da VM usando a opção --compress lzo, que comprime o backup no formato LZO.

# Portanto, o script realizará o backup de todas as VMs ativas no Proxmox e, em seguida, transferirá esses backups para o servidor Windows usando o WinSCP, conforme configurado na parte do script relacionada ao WinSCP.

# Caso você queira fazer backup de VMs específicas ou excluir algumas VMs do processo de backup, é possível modificar o script para atender às suas necessidades. Você pode ajustar o comando qm list para listar apenas as VMs desejadas ou adicionar condições ao loop for para fazer o backup somente de VMs específicas.

# Lembre-se sempre de testar cuidadosamente o script e verificar se as VMs são salvas corretamente antes de colocá-lo em produção.


  
