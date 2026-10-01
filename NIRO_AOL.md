# StarPilot 6.7.8-Niro-AOL

Copie personnelle de StarPilot 6.7.8 pour essai d'une correction AOL sur Kia
e-Niro de première génération. Base officielle :
`firestar5683/StarPilot`, branche `StarPilot`, commit
`2a12dbd0ad94b46f8a7b6099d32d7216b7676f79`.

## Comportement visé

| Position sélectionnée avec MODE | AOL |
| --- | --- |
| CRUISE | Activé lorsque les conditions existantes l'autorisent |
| LIMIT | Arrêté |
| Aucun mode | Arrêté |
| Retour en CRUISE | Réactivé lorsque les conditions existantes l'autorisent |

Le correctif fait suivre AOL à l'état CRUISE confirmé par les messages du
régulateur Kia. Il concerne uniquement `KIA_NIRO_EV`, avec
`openpilotLongitudinalControl == False` et le bouton principal affecté à AOL.
Il attend la confirmation de l'état du régulateur si elle arrive après l'appui.

Le régulateur constructeur reste utilisé pour la vitesse. Le contrôle
longitudinal logiciel doit rester désactivé pour cette configuration. Les
conditions existantes de calibration, frein, vitesse, rapports de boîte et
d'arrêt de l'assistance sont conservées. Aucun fichier de firmware Panda ou
de commandes longitudinales Hyundai/Kia n'est modifié.

## Validation et limites

Le prototype a réussi 32 cas logiciels avec états de voiture simulés. Les 64
tests du module AOL complet ont également réussi sous Linux, avec les imports
réels du projet, ses schémas Cap'n Proto et les modules de messagerie et de
coordonnées compilés pour l'environnement de test. Ces tests utilisent les
fixtures du projet, notamment une mémoire de paramètres simulée ; ils ne
valident pas toute la pile de conduite. Le patch s'applique et se retire à
l'identique sur la base officielle. Cinq tests de régression sont ajoutés au
module de tests AOL du projet.

Les essais sur cet exemplaire de Kia et sur Comma Four restent à effectuer.
La valeur de `SCC11.MainMode_ACC` doit effectivement distinguer CRUISE de LIMIT
et d'aucun mode. La disparition des voyants et le fonctionnement matériel du
freinage d'urgence ne sont pas établis par les tests logiciels.

Cette branche conserve les binaires précompilés de la base officielle. La
modification de fonctionnement est un fichier Python chargé par le logiciel ;
le suffixe `-Niro-AOL` permet d'identifier la copie personnalisée à l'écran.

## Installation pour essai

Branche dédiée : `niro-aol-stock-scc`, dépôt `jcstinger-git/openpilot`.

Adresse complète pour Custom Software :

`https://installer.comma.ai/jcstinger-git/niro-aol-stock-scc`

Avant une réinstallation, conserver les informations et réglages de la version
actuellement installée. Voiture garée, Wi-Fi disponible et alimentation stable,
suivre le [guide officiel d'installation StarPilot](https://wiki.firestar.link/software/starpilot/).
Utiliser l'adresse de cette branche à l'étape Custom Software. Ne pas débrancher
le Comma pendant l'installation ou une mise à jour d'AGNOS.

Après installation, vérifier la version `6.7.8-Niro-AOL`, l'identification du
véhicule et le maintien du contrôle longitudinal constructeur. Vérifier ensuite
le cycle MODE et les états AOL dans un environnement de test contrôlé. Si les
alertes ADAS persistent, arrêter l'essai de cette assistance et revenir à la
version stable avant une utilisation normale.

## Retour et mises à jour

Retour à la version officielle : suivre le même guide de réinstallation et
utiliser `firestar5683/StarPilot`.

Les mises à jour de cette copie doivent suivre `niro-aol-stock-scc` sur le dépôt
personnel. Les nouvelles versions officielles devront être intégrées à cette
branche avec le correctif et ses tests. Le bouton GitHub Sync fork concerne la
branche sélectionnée : remplacer la branche corrigée par l'amont peut retirer
le correctif.

Les crédits et notices de licence de StarPilot et de ses dépendances sont
conservés dans le dépôt.
