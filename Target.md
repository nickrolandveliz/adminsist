Pràctica: Target, servei i script amb systemd 

1 - Crear un target propi, fer-lo default target i comprovar que accedim amb el nostre target.
2 - Crear un servei dintre del nostre target i comprovar que s'inicia correctament al reiniciar.
3 - Modificar el servei perquè executi un script amb permisos root.
4 - Programar un script amb el que vulguem i executar-lo manualment per veure si funciona.

(1)
Abans de realitzar cap modificació, he comprovat quin és el target configurat per defecte al sistema mitjançant l'ordre systemctl get-default. El sistema utilitza inicialment graphical.target, que permet iniciar Ubuntu amb l'entorn gràfic.

(2)-()
He creat un target propi anomenat nick.target dins del directori /etc/systemd/system/. Aquest target manté una relació amb graphical.target per conservar l'entorn gràfic d'Ubuntu Desktop. Després de crear-lo, he recarregat la configuració de systemd amb systemctl daemon-reload.

()

