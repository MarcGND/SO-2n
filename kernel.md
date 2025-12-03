---
layout: default
title: "Kernel"
---

# Compilació i personalització del Kernel Linux

El primer pas és comprovar els fitxers descarregats i descomprimir el codi font del kernel. Tal com es mostra a continuació:

<img width="1187" height="193" alt="1" src="https://github.com/user-attachments/assets/a618616b-bb92-4cc9-8592-7abdce7b7a58" />

Un cop descomprimit, accedim al directori del kernel i carreguem la configuració per defecte basada en el kernel actual amb la comanda:

<img width="853" height="557" alt="2" src="https://github.com/user-attachments/assets/bdcba735-46eb-48ae-a43e-2727ddc90b04" />

<img width="1054" height="560" alt="3" src="https://github.com/user-attachments/assets/734ea784-4bba-483d-8b14-7c32f8f74b9c" />

Ara editem el fitxer .config per habilitar opcions importants com BPF, CPU Frequency Governors i paràmetres de seguretat (SELinux, SMACK). Aquest pas és clau per personalitzar el comportament del kernel.

<img width="1256" height="757" alt="4" src="https://github.com/user-attachments/assets/f943d492-85e4-4555-98eb-2aa259466764" />

També configurem les opcions de xarxa, com els planificadors de cua i els algoritmes de congestió TCP (CUBIC, BBR, etc.), així com el suport per IPv6.

Afegim un missatge personalitzat al procés d’arrencada modificant el fitxer init/main.c i aplicant el patch amb:

<img width="1065" height="544" alt="5" src="https://github.com/user-attachments/assets/b9b2b736-8914-448b-8b75-0b9043379ae1" />

Després, utilitzem make menuconfig per ajustar paràmetres addicionals i afegir una versió local al kernel.

<img width="439" height="187" alt="6" src="https://github.com/user-attachments/assets/f22ccf8d-4e94-4621-a6d8-56b611969cda" />

<img width="555" height="436" alt="7" src="https://github.com/user-attachments/assets/6d12b45b-47a5-49a6-aa0b-ed63a78a9173" />

<img width="553" height="601" alt="8" src="https://github.com/user-attachments/assets/34b4a957-767c-48b9-a46e-b1808b5dbb2a" />

<img width="542" height="596" alt="9" src="https://github.com/user-attachments/assets/c85a3d49-238e-4558-a811-cfe1cfd5e57e" />

<img width="541" height="751" alt="10" src="https://github.com/user-attachments/assets/9932d368-39b6-4aff-82ce-c90d10103697" />

<img width="567" height="586" alt="11" src="https://github.com/user-attachments/assets/da07970b-6f6c-4497-b06c-1651358d3f6a" />

<img width="637" height="668" alt="12" src="https://github.com/user-attachments/assets/b7599e38-f1b0-4322-9829-2e2879c8c26e" />

Un cop modificat amb l'old menu config creem un patch personalitzat per a l'arrencada del kernel.

<img width="1641" height="756" alt="13" src="https://github.com/user-attachments/assets/b9ff1edd-de2d-46ac-8d64-57f3d5d7db0f" />

Apliquem la diferencia del patch al main.c

<img width="1276" height="416" alt="14" src="https://github.com/user-attachments/assets/0e54cc23-6206-40e0-a7f4-6f9d55b656ba" />

<img width="981" height="246" alt="15" src="https://github.com/user-attachments/assets/bd723df5-f701-4c28-a18d-20bde4f6762e" />

Un cop fet el patch i l'old menu config, canviarem alguns parametres.

<img width="813" height="40" alt="16" src="https://github.com/user-attachments/assets/df9774bc-9a52-455a-9055-0c6d2f835da8" />

Al obrir el menú principal de make menuconfig, on podem personalitzar la configuració del kernel abans de compilar-lo. Aquí es mostren les categories principals com General setup, Processor type and features, Networking support, Security options i altres, que permeten habilitar o deshabilitar funcionalitats segons les necessitats.

<img width="1833" height="786" alt="17" src="https://github.com/user-attachments/assets/aaf3415e-3a54-4935-a3f9-1b8f19c8594b" />

En aquest apartat del menú General setup podem afegir una versió local al kernel amb l’opció Local version – append to kernel release. Això permet identificar fàcilment el kernel personalitzat afegint un sufix al nom de la versió.

<img width="1015" height="393" alt="18" src="https://github.com/user-attachments/assets/0c0917ef-6382-4619-949d-e7bd9174c705" />

En aquesta finestra s’introdueix el sufix que s’afegirà a la versió del kernel. En aquest cas s’ha escrit -custom-perf, que permet identificar fàcilment el kernel personalitzat quan es mostri a GRUB o en comandes com uname -r.

<img width="802" height="274" alt="19" src="https://github.com/user-attachments/assets/9170218b-4d28-47c4-80b4-3bde23a332d5" />

En aquest apartat de Networking options es configuren les opcions de xarxa del kernel. Aquí s’han habilitat funcionalitats com TCP/IP networking i IP multicasting, juntament amb altres protocols i interfícies relacionades amb sockets, seguretat i transformacions.

<img width="1082" height="734" alt="20" src="https://github.com/user-attachments/assets/58cda6c2-59ca-4ea3-b5d4-2d6e64c678ab" />

En aquest apartat es configuren els algoritmes de control de congestió TCP. S’han habilitat diverses opcions com CUBIC (per defecte) i BBR TCP com a mòdul, que millora el rendiment en xarxes amb alta latència i ample de banda.

<img width="804" height="468" alt="21" src="https://github.com/user-attachments/assets/83a17adf-5413-4945-99d2-d285526b4f5d" />

En aquesta finestra es guarda la configuració del kernel en un fitxer. S’ha indicat .config com a nom, que és el fitxer per defecte on es desa tota la configuració seleccionada amb menuconfig.

<img width="704" height="361" alt="22" src="https://github.com/user-attachments/assets/c4969294-c842-4f86-808d-65f539e106a0" />

En aquesta imatge s’està editant el fitxer /etc/default/grub per modificar la configuració del carregador GRUB. Aquí es poden ajustar paràmetres com el temps d’espera i l’estil del menú abans d’actualitzar la configuració amb update-grub.

<img width="1176" height="290" alt="23" src="https://github.com/user-attachments/assets/c9ac0e1b-8625-48a7-a093-44037431aea3" />

En aquesta imatge s’ha executat la comanda update-grub després de modificar el fitxer de configuració. El sistema genera el fitxer de GRUB i detecta les imatges del kernel disponibles. Finalment, es mostra la comanda make-kpkg --initrd kernel_image kernel_headers, que s’utilitza per compilar el kernel i crear els paquets .deb per a la seva instal·lació.

<img width="1045" height="359" alt="24" src="https://github.com/user-attachments/assets/9acf5b19-fd2a-405c-a164-1bd6a960b556" />






[← Torna a l'índex](index.html)
