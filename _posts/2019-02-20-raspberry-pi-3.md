---
title: Raspberry PI 3 + RaspPlex + Google Drive (Rclone) + NAS (samba)
updated: 2019-02-20 00:04
---

**NOTE:**  Estou atualizando esse post com a evolução do projeto

## Raspberry Pi 3

O seguinte guia abaixo é para mostrar os links que utilizei para montar o meu Raspberry PI (RPI) 3 B+ , utilizando o PLEX como media center, RClone para backup de dados no Google Drive, Samba para compartilhar dados do storage, VNC para acesso remoto, Transmission para client torrent web e como lidei com o HD que estava em HFS+ no linux

Como não quero reinventar a roda, seguem os links

# Links & Hints

1. Media Center - Plex : [link](https://www.raspberrypi.org/forums/viewtopic.php?t=214655)
2. Backup Google Drive - RClone : [link](https://github.com/pageauc/rclone4pi/wiki#how-to-configure-a-remote-storage-service)
3. Automatizar Tarefas - Crontab : [link](https://www.raspberrypi.org/documentation/linux/usage/cron.md)
   1. No link ele mostra como utilizar a interface gráfica gnome-schedule. Eu preferi fazer por ela, por conta de ser mais fácil.
   2. Você pode schedular tarefas para o root ou para o usuário PI
    ```bash
    # como usuário 
    gnome-schedule -e
    # como root
    sudo gnome-schedule -e
    ```
   3. No meu caso sempre executei a UI como sudo por conta dos scripts que eram executados
   4. Adicionei ao final de cada JOB o comando abaixo gerar logs de cada sincronização do rclone
    ```bash
    >> /home/pi/logs/sync-mybook1.log 2>&1
    ```
   5. Criei uma tarefa para realizar a limpeza no dia 7 de cada mês dos logs
    ```bash
    # crontab
    0 0 */7 * * su pi -c "rm -f /home/pi/logs/*.log"
    # interface gráfica (gnome-schedule)
    su pi -c "rm -f /home/pi/logs/*.log"
    ```
4. Acesso Remoto - VNC : [link](https://www.raspberrypi.org/forums/viewtopic.php?t=214655)
   1. Procure por _Set UP VNC_ 
   2. Não esqueça de criar sua conta : [link](https://www.realvnc.com/en/raspberrypi/)
5. NAS - Samba : [link](https://www.arduinoecia.com.br/2016/05/como-instalar-samba-raspberry-pi.html)
   1. Eu compartilhei as pastas diretamente do HD
6. Client Torrent Web - Transmission [link](http://www.techpi.com.br/2018/04/01/cliente-torrent-na-raspberry-pi/)
7. HFS e HFS+ no Linux : hfsplus
   1. Precisei instalar o hfsplus hfsutils e hfsprogs [link](https://www.raspberrypi.org/forums/viewtopic.php?t=60437)
    ```bash
    sudo apt-get install hfsplus hfsutils hfsprogs
    ```
   2. Desabilitei o Journaling das partições com HFS [link](https://fosswire.com/post/2007/09/dealing-with-mac-formatted-drives-on-linux/)
    ```bash
    # executei esse comando no mac
    sudo diskutil disableJournal "/Volumes/sdaXsX"
    ```
