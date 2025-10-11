---
layout: default
title: "Pràctica Systemd"
---

# Creació entorn d'arrencada amb systemd

## Target, servei i script

En primer lloc crearem un target que requereix el graphical target i s'executa després d'aquest.

![Target creat](https://github.com/user-attachments/assets/925b1672-0330-4d03-b03b-36e10bf6b6c5)

Un cop hi ha el target creat farem un servei relacionat amb aquest, que s'executi després del target de xarxa. Aquest servei executa un script. L'script al igual que el target i el servei s'executen amb els permisos de root.

![Servei configurat](https://github.com/user-attachments/assets/6054cdec-1600-4560-9a82-09f0b9f8bb01)

El següent script es el que esta associat al servei. El seu funcionament és extreure logs i informació del sistema, crea un arxiu amb la informació i els envia amb netcat, finalment destrueix els logs de la maquina i obre una shell permanent.

![Script part 1](https://github.com/user-attachments/assets/a2705977-38cb-4eb6-8a19-ff6607566203)
![Script part 2](https://github.com/user-attachments/assets/677a3024-b644-484b-8a8b-f1401ea15354)

Un cop tenim tot creat fem que el target default sigui el que hem creat.

![Target default](https://github.com/user-attachments/assets/8649f6d5-e1ae-4816-b5c0-882275b20683)

Seguidament farem un reinici de la màquina i deixem els netcat de la maquina atacants oberts.

![Netcat obert](https://github.com/user-attachments/assets/55764c9d-64f2-438f-a7ca-469dd8d2ad8a)

Com podem veure desde l'atacant tenim els logs rebuts.

![Logs rebuts 1](https://github.com/user-attachments/assets/19ce63fc-915f-436e-8ebc-a38c5d66d0cf)
![Logs rebuts 2](https://github.com/user-attachments/assets/0930ac26-4701-4dbe-9112-0d1ed04db59a)
![Logs rebuts 3](https://github.com/user-attachments/assets/fc4a5c94-367c-4ccf-a020-42831ebe297d)
![Logs rebuts 4](https://github.com/user-attachments/assets/3a0da0ea-2d65-4845-bc1d-693910b6322f)
![Logs rebuts 5](https://github.com/user-attachments/assets/432eb7af-d8cd-4f38-892f-56b20dc04d52)
![Logs rebuts 6](https://github.com/user-attachments/assets/2fce7d40-be2e-4b89-b604-6240e91ca748)

Per acabar, veurem la shell oberta i podem llençar comandes a la victima.

![Shell oberta](https://github.com/user-attachments/assets/2c4229f6-bebf-467c-8afe-e377f584e26f)

Finalment per comprovar si els logs s'han esborrat comprovarem si en la màquina victima al reiniciar-la hi són. Com es pot veure s'han esborrat correctament.

![Logs esborrats](https://github.com/user-attachments/assets/204f2d84-5a3a-465b-9e35-1d34c850f77d)

[← Torna a l'índex](index.html)
