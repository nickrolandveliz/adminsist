Pràctica: Target, servei i script amb systemd 

1 - Crear un target propi, fer-lo default target i comprovar que accedim amb el nostre target.
2 - Crear un servei dintre del nostre target i comprovar que s'inicia correctament al reiniciar.
3 - Modificar el servei perquè executi un script amb permisos root.
4 - Programar un script amb el que vulguem i executar-lo manualment per veure si funciona.

1
<img width="489" height="36" alt="2" src="https://github.com/user-attachments/assets/d3219e9d-edde-42ec-8e55-5a6283fe03ca" />

Abans de realitzar cap modificació, he comprovat quin és el target configurat per defecte al sistema mitjançant l'ordre systemctl get-default. El sistema utilitza inicialment graphical.target, que permet iniciar Ubuntu amb l'entorn gràfic.


2
<img width="489" height="36" alt="2" src="https://github.com/user-attachments/assets/72efb35e-fbc8-4a2e-a209-5bdef6fbfa0c" />

<img width="364" height="187" alt="4" src="https://github.com/user-attachments/assets/40cdedae-5c95-4709-bc21-056a8e5f7331" />


He creat un target propi anomenat nick.target dins del directori /etc/systemd/system/. Aquest target manté una relació amb graphical.target per conservar l'entorn gràfic d'Ubuntu Desktop. Després de crear-lo, he recarregat la configuració de systemd amb systemctl daemon-reload.


3
<img width="464" height="26" alt="image" src="https://github.com/user-attachments/assets/30304e3e-3a61-4bc3-a54a-b5be5a50dde8" />

<img width="334" height="42" alt="5" src="https://github.com/user-attachments/assets/194e807c-230a-4beb-a9c2-0d5299b5a041" />

He configurat nick.target com a target per defecte mitjançant systemctl set-default. Posteriorment, he comprovat el canvi amb systemctl get-default, obtenint nick.target com a resultat.


4
<img width="703" height="732" alt="6" src="https://github.com/user-attachments/assets/0a094f0c-4bc8-4471-9a3d-88b72aef9b24" />
<img width="761" height="93" alt="image" src="https://github.com/user-attachments/assets/2e30a9be-24eb-42d1-9057-6d68c45b4788" />
Una vegada creat l'script, li he assignat permisos d'execució mitjançant l'ordre chmod +x. Posteriorment, he comprovat els permisos del fitxer amb ls -l, verificant que l'script disposa de permisos d'execució.


5
<img width="542" height="25" alt="8" src="https://github.com/user-attachments/assets/ba2e6d9d-2894-4fc2-b9b4-802f2c68c9b1" />


