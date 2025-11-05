---
layout: default
title: "NSSM"
---

# Creació d'un servei amb NSSM
El primer serà descarregar el programa "nssm" aquest ens permet crear serveis, editar-los i esborrar-los. Tal com es mostra a continuació.

<img width="632" height="753" alt="0" src="https://github.com/user-attachments/assets/5954a8ad-820e-4998-bc26-2c1ae6b33071" />

El servei creat es l'anomenat marcsabateservei i segueixo amb aquesta configuració.

<img width="1118" height="508" alt="1" src="https://github.com/user-attachments/assets/e6ef0fdf-06e1-438e-8ba4-4a63ead70c48" />

A la configuració del servei com el que vull executar es un script amb powershell, selecciono la ruta on es el programa.

<img width="935" height="470" alt="2" src="https://github.com/user-attachments/assets/daca07fb-11fe-4a4d-a29c-01b2dbbc5d36" />

A la part de start up directory escollim la carpeta on tenim situat l'script.
En la part on tenim els arguments posem aquesta línia per executar-lo en segon pla:
-ExecutionPolicy Bypass -WindowStyle Hidden -File "C:\Windows\keyloger\keylog.ps1"

<img width="1191" height="481" alt="3" src="https://github.com/user-attachments/assets/0d6daee6-6e40-4a47-b4a0-485b204b2ee8" />

També podem posar més configuració al nostre servei buscant per les pestanyes del nssm. Jo he escollit les següents.
<img width="443" height="247" alt="4" src="https://github.com/user-attachments/assets/5c941fc4-0483-42f8-aa0f-f611f307d929" />

<img width="441" height="234" alt="5" src="https://github.com/user-attachments/assets/c4959596-1507-4f27-b93e-26140ec607f6" />

Ara ja tenim el servei montat, ara comprovarem que l'script sigui correcte. Tinc aquest Keylogger.
<img width="832" height="790" alt="key1" src="https://github.com/user-attachments/assets/ac204c7a-ffab-4d78-9cb8-f9a12823f611" />

<img width="926" height="783" alt="key2" src="https://github.com/user-attachments/assets/26a203b9-0d28-4e55-86f6-0cb5074a6005" />

<img width="1207" height="803" alt="key3" src="https://github.com/user-attachments/assets/761d6f6b-b29f-4667-a127-39020c94c15e" />

<img width="892" height="779" alt="key4" src="https://github.com/user-attachments/assets/ac82de10-6b95-483d-a268-f36b8dbf6e49" />

<img width="986" height="775" alt="key5" src="https://github.com/user-attachments/assets/4143e9d8-6b16-43d1-8851-fd3667379121" />

<img width="817" height="775" alt="key6" src="https://github.com/user-attachments/assets/0439fa7d-fdf0-4a13-ad07-54f780ad70b1" />

<img width="538" height="260" alt="key7" src="https://github.com/user-attachments/assets/41a7c88b-c109-4251-8b50-fc09a62cabf0" />

Per comprovar si tenim el servei ben configurat entrarem a "services", com podem veure aquest esta configurat.

<img width="1918" height="196" alt="serv1" src="https://github.com/user-attachments/assets/4494ee46-b697-4667-a4fa-5d9ce21c4325" />

Abans de reiniciar comprovaré que el keylogger funciona com s'espera:

<img width="1164" height="339" alt="serv2" src="https://github.com/user-attachments/assets/667890c7-5fc4-4b77-ad41-582e98ac6050" />

Després de veure que ja funciona, reiniciarem el sistema, i al fer login comprovarem que el servei està en execució i que el keylogger comença a registrar activitat.

<img width="977" height="637" alt="serv3" src="https://github.com/user-attachments/assets/57443409-0ba2-4b91-a9c1-cafeecf1f2a7" />

<img width="1126" height="178" alt="serv4" src="https://github.com/user-attachments/assets/0e41db04-62e0-4df8-8b1c-0ba04d77a1fb" />

<img width="1549" height="335" alt="serv5" src="https://github.com/user-attachments/assets/69b87df0-f88e-4a3b-a86e-0cb88fe30512" />

Per últim escriuré més amb el teclat per a que capturi més activitat.

<img width="897" height="89" alt="serv6" src="https://github.com/user-attachments/assets/70cec4ae-74f8-40d9-b2f7-6ba312f3aa09" />



[← Torna a l'índex](index.html)
