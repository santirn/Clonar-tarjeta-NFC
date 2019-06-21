# Clonar tarjeta NFC

##### En este PoC vamos a aprender a clonar tarjetas NFC mediante un ataque a sus claves. Este PoC es totalmente didáctico para aprender la tecnología NFC, Para ello utilizaremos de ejemplo tarjetas del Metro de Valencia, las cuales serán destruidas al final de este PoC debido a la ilegalidad de su uso una vez clonadas. 

##### ACTUALMENTE METRO VALENCIA NO TIENE NINGÚN SISTEMA DE SEGURIDAD PARA DETECTAR SI LA TARJETA HA SIDO CLONADA/MODIFICADA. ÚNICAMENTE NOS BLOQUEARA LA TARJETA EN UNA LISTA NEGRA EN CASO DE QUE DETECTE ANOMALÍAS EN ELLA. METRO VALENCIA ES CONSCIENTE DE ESTE FALLO DE SEGURIDAD, PERO NO TIENE MEDIOS PARA PALIARLO. 

###### SITUACIÓN: adquirimos una tarjeta de transporte y la recargamos con los viajes deseados. Para este PoC adquirimos la tarjeta del metro Valencia con 10 viajes en zona AB.  

###### Necesitamos un lector, escritor NFC como [este](https://www.amazon.es/dp/B07MVBHR29/ref=cm_sw_r_tw_dp_U_x_K8ndDbXRKSA18)
![lectorNFC](https://github.com/santirn/Clonar-tarjeta-NFC/blob/master/1.jpg)
