# Palvelinten Hallinta H4
	http://terokarvinen.com/2018/aikataulu-%E2%80%93-palvelinten-hallinta-ict4tn022-3004-ti-ja-3002-to-%E2%80%93-loppukevat-2018-5p

Tehtävä tehty xubuntu 18.04 käyttöjärjestelmällä 8gb ram.

Yritys nro. 3 Koneen RAM Muistia ei riitä vagrantin käyttöön joten kone kaatui kesken tehtävän. 
c tehtävä on kirjoitettu muistista joten siinä saattaa olla muutama virhe.


Alkutoimet

	setxkbmap fi && sudo apt-get update && sudo apt-get install -y leafpad chromium-browser


## a) Tee skripti, joka tekee koneestasi salt-orjan.

Aloitin ensin asentamalla salt mestarin.

	sudo apt-get install -y salt-master

Sitten aloitin skriptin rakentamisen.

Ensin kokeilin miten bash skriptit toimivatkaan.

Sivulta https://www.linux.fi/wiki/Bash-skriptaus löysin asian mitä en muistanut

	#!/bin/bash

Sitten tein lyhyen skriptin.

	nano terveiset.sh

	#!/bin/bash
	echo "Hello"
	hostname
	echo -e "\nStatus"
	w

Skripti toimi oikein mainiosti.

** bash1H4 KUVA **

Kun testi oli valmis aloitin salt skriptin rakentamisen.

	nano saltslaveinst.sh

	#!/bin/bash
	echo "Updating packets"
	sudo apt-get update
	echo -e "\nInstalling salt-minion"
	sudo apt-get -y install salt-minion
	echo -e "\nConfiguring salt-minion"
	echo -e "master: 192.168.10.47\nid: SaltSlave1"|sudo tee /etc/salt/minion
	echo -e "\nRestarting minion"
	sudo systemctl restart salt-minion

Tämä skripti toimii vain jos master koneen lokaali osoite on 192.168.10.47

Skriptin teko oli helpompaa kuin oletin. Kun se oli ajettu hyväksyin orjan masterilla.

	sudo salt-key -A

Sitten vielä kokeilin toimiiko salt.

saltminionWH4 **KUVA**


## c) Vagrant. Asenna Vagrant. Kokeile jotain uutta kuvaa Atlaksesta. Huomaa, että kuvat ovat vieraita binäärejä, ja virtuaalikoneista on mahdollista murtautua ulos. Jokohan Ubuntun virallinen  Suodatin: VirtualBox, järjestys: Most downloads. https://app.vagrantup.com/boxes/search?provider=virtualbox

Asensin vagrantin komennolla.

	sudo apt-get install vagrant virtualbox

Asennuksessa kesti jonkin aikaa.

Sitten tein ensimmäisen vagrantin.

	mkdir virtual1 && cd virtual1 && vagrant init debian/jessie64 && vagrant up

Ensimmäinen asennus kestää koska levy kuva ladataan samalla.

Kun se oli valmis virtuaali koneelle pääsi komennolla 

	ssh

Päivitin vagrantin ja tein siitä salt orjan.

	sudo apt-get update
	sudo apt-get install salt-minion
	echo -e "master: 10.0.2.2\nid: vagrantslave1"|sudo tee /etc/salt/minion
	sudo systemctl restart salt-minion

Sitten hyväksyin orjan masterilla

	sudo salt-key -A

ja kokeilin viellä vastaako orja.

	sudo salt '*' cmd.run w

Orja vastasi.

** Tähän tulisi kuva mutta se meni koneen kaatumisen mukana **

