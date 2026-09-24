Pràctica: Target, servei i script amb systemd 

**1 - Crear un target propi, fer-lo default target i comprovar que accedim amb el nostre target.**

**2 - Crear un servei dintre del nostre target i comprovar que s'inicia correctament al reiniciar.**

**3 - Modificar el servei perquè executi un script amb permisos root.**

**4 - Programar un script amb el que vulguem i executar-lo manualment per veure si funciona.**


**PAS 1 — Comprovar el target actual**

<img width="489" height="36" alt="2" src="https://github.com/user-attachments/assets/d3219e9d-edde-42ec-8e55-5a6283fe03ca" />

Abans de realitzar cap modificació, he comprovat quin és el target configurat per defecte al sistema mitjançant l'ordre systemctl get-default. El sistema utilitza inicialment graphical.target, que permet iniciar Ubuntu amb l'entorn gràfic.


**PAS 2 — Crear el nostre target**

<img width="489" height="36" alt="2" src="https://github.com/user-attachments/assets/72efb35e-fbc8-4a2e-a209-5bdef6fbfa0c" />

He creat un target propi anomenat nick.target dins del directori /etc/systemd/system/. Aquest target manté una relació amb graphical.target per conservar l'entorn gràfic d'Ubuntu Desktop. 

<img width="364" height="187" alt="4" src="https://github.com/user-attachments/assets/40cdedae-5c95-4709-bc21-056a8e5f7331" />

Després de crear-lo, he recarregat la configuració de systemd amb systemctl daemon-reload.


**PAS 3 — Posar-lo com a target per defecte**

<img width="464" height="26" alt="image" src="https://github.com/user-attachments/assets/30304e3e-3a61-4bc3-a54a-b5be5a50dde8" />

He configurat nick.target com a target per defecte mitjançant systemctl set-default. 

<img width="334" height="42" alt="5" src="https://github.com/user-attachments/assets/194e807c-230a-4beb-a9c2-0d5299b5a041" />

Posteriorment, he comprovat el canvi amb systemctl get-default, obtenint nick.target com a resultat.


**PAS 4 — Crear el nostre script**

<img width="703" height="732" alt="6" src="https://github.com/user-attachments/assets/0a094f0c-4bc8-4471-9a3d-88b72aef9b24" />

Creare l'script amb la seguent informació.

<img width="761" height="93" alt="image" src="https://github.com/user-attachments/assets/2e30a9be-24eb-42d1-9057-6d68c45b4788" />

Una vegada creat l'script, li he assignat permisos d'execució mitjançant l'ordre chmod +x. Posteriorment, he comprovat els permisos del fitxer amb ls -l, verificant que l'script disposa de permisos d'execució.


**PAS 5 — Executar manualment l'script**

<img width="542" height="25" alt="8" src="https://github.com/user-attachments/assets/ba2e6d9d-2894-4fc2-b9b4-802f2c68c9b1" />

Abans d'integrar l'script en un servei de systemd, l'he executat manualment per comprovar-ne el funcionament.

<img width="813" height="991" alt="7" src="https://github.com/user-attachments/assets/259a3d46-f0fc-4fe5-9031-b41b82c1a06d" />

L'script genera un informe del sistema al fitxer /var/log/nick-system.log, on registra informació com la data, l'usuari d'execució, el nom de l'equip, el temps d'activitat, la memòria RAM, l'espai en disc i les adreces IP. La prova mostra que l'script s'ha executat correctament amb l'usuari root (UID 0).


**PAS 6 — Crear el servei**

Ara farem que systemd executi aquest script creant **nick.service**.

<img width="616" height="25" alt="image" src="https://github.com/user-attachments/assets/7ad1a174-0804-423c-91a8-796546e00b29" />

<img width="566" height="329" alt="image" src="https://github.com/user-attachments/assets/6f0d4cbf-60c7-413a-ab26-721b7960c08d" />

I amb la seguent comanda informarem a systemd que hem creat un servei.


**PAS 7 — Habilitar el servei dintre de nick.target**

<img width="640" height="71" alt="image" src="https://github.com/user-attachments/assets/c2a21d4a-8a6c-4f24-814e-20ace61a2a6e" />

He creat el servei nick.service dins de systemd i l'he configurat perquè executi l'script /usr/local/bin/nick-script.sh amb l'usuari root. El servei és de tipus oneshot, ja que l'script s'executa una vegada i finalitza. 

Aquesta línia és important perquè indica que:

nick.target
    ↓
nick.service

estan vinculats.

<img width="596" height="263" alt="image" src="https://github.com/user-attachments/assets/1740b246-5e48-42dd-9c81-fb281340fae5" />

Podem comprovar-ho amb la seguent comanda.
Finalment, he habilitat el servei perquè quedi vinculat a nick.target.


**PAS 8 — Provar manualment el servei**

<img width="638" height="73" alt="image" src="https://github.com/user-attachments/assets/1c949fe3-42ec-4a44-acc6-988633a157d8" />

<img width="638" height="376" alt="image" src="https://github.com/user-attachments/assets/8503745c-88a6-4d04-a0fe-d4358282617d" />

<img width="642" height="63" alt="image" src="https://github.com/user-attachments/assets/0d70b479-de19-4f95-9dde-c5b098d8ebcc" />

Això demostra:

nick.service
      ↓
executa nick-script.sh
      ↓
crea un nou informe

Abans de reiniciar el sistema, he iniciat manualment nick.service per comprovar que la seva configuració funciona correctament. El servei apareix com active (exited), ja que és de tipus oneshot: executa l'script una vegada i finalitza. També he comprovat el fitxer de registre i s'ha generat un nou informe, demostrant que el servei executa correctament l'script.



**PAS 9 — Reiniciar Ubuntu**

<img width="413" height="49" alt="image" src="https://github.com/user-attachments/assets/013cb787-ec88-458b-ab91-8d903b335f35" />

Una vegada comprovat manualment el funcionament del servei, he verificat que nick.target continua configurat com a target per defecte. A continuació, he reiniciat Ubuntu per comprovar si el target carrega automàticament el servei durant l'arrencada.


**PAS 10 — Comprovar-ho després del reinici**

<img width="335" height="45" alt="image" src="https://github.com/user-attachments/assets/ec6b5bd6-4ad7-443d-af76-892fb9573420" />

Això demostra que el meu target continua sent el predeterminat.


<img width="652" height="257" alt="image" src="https://github.com/user-attachments/assets/bf751db1-5781-4b89-809d-dac6081e626f" />

Veiem que l'hora que apareix al costat de Active:, correspon aproximadament amb l'hora de l'arrencada.


<img width="619" height="54" alt="image" src="https://github.com/user-attachments/assets/61b9e185-9f11-482f-a1bd-79d411767020" />

Aquesta és una prova molt clara: el nou informe s'ha creat sense que executessim manualment l'script.


<img width="650" height="453" alt="image" src="https://github.com/user-attachments/assets/93ceb81e-f004-4321-9a77-90975b6781d8" />

La data correspon amb el reinici que acabem de fer.



<img width="274" height="157" alt="image" src="https://github.com/user-attachments/assets/db7246fa-916b-40c5-af9d-b7f9221b8fea" />

<img width="638" height="554" alt="image" src="https://github.com/user-attachments/assets/702bbcbe-f8a6-45ec-ba14-e9cd77240e95" />

Finalment, he comprovat el fitxer /var/log/nick-system.log. El nombre d'informes ha augmentat després del reinici i l'últim registre correspon a la nova arrencada. En aquest informe també es pot observar que l'script s'ha executat com a root (UID 0). D'aquesta manera es confirma que nick.target carrega el servei i que aquest executa automàticament l'script durant l'arrencada.
