# **PROJET INFORMATIQUE EMBARQUE 2024 :**
## **Capteur laser**

Réalisation du projet à l'aide de la [documentation-arduino](https://arduino-france.site/ir-arduino/)

Le schéma de montage de l'émetteur Arduino est disponible à la slide 27 du [PDF](https://drive.google.com/file/d/1Cq8dJHxjKIS5d8Cu2aH1feRFLzyRBjUo/view), de [sensorKit](https://sensorkit.joy-it.net/fr/sensors/ky-005). On retrouve sur le lien de sensorKit  des exemples de programmes applicables avec l'émetteur KY-005.
Le shéma de montage du récepteur Arduino est disponible sur le site de [sensorkit](https://sensorkit.joy-it.net/fr/sensors/ky-022) et [d'arduino-module](https://arduinomodules.info/ky-022-infrared-receiver-module/). On retrouve sur le lien de sensorKit  des exemples de programmes applicables avec l'émetteur KY-022.

### Algo émetteur
```
Tant que Vraie
    emission laser
Fin tant que
```

### Programme Arduino émission

Nous avons remarquer avec mon binôme, que l'envoi continu d'un signal infrarouge n'était pas détécté par le récepteur KY-022.
Nous avons donc décider d'opter pour une autre solution, l'envoi très rapide d'un signal intermitant, de l'ordre d'une dizaine de millisecondes.
En opérant ces changement dans le code nous constatons, que désormais, le signal émis par KY-005 est bien reçue et traité par KY-022.
```c++

int Led = 3;
 
void setup ()
{
  pinMode (Led, OUTPUT); // Déclaration de la broche de sortie LED

}

/*  La LED va envoyer un signal infrarouge quelconque,
*   pendant un laps de temps très courts (20 millisecondes).
*/
void loop () //Boucle de programme principale
{
  digitalWrite (Led, HIGH); 
  delay(20); // la LED est activée pendant 20 millisecondes.
  digitalWrite (Led, LOW);
  delay(20); // La LED se déstactive pendant 20 millisecondes.
}

```

### Algo récépeteur
```
activation <--- 0
temps_activation <--- null
Tant que Vraie
    Si Signal Faux Alors
        Si activation égale 1 Alors
            temps_chrono <--- (temps_actuelle - temps_activation)
            Retourner temps_chrono
        Sinon si activation égale 0 Alors
            Incrémente activation 1
            temps_activation <--- temps_actuelle
        Fin Si
    Fin si
Fin Tant que
```