# Fairytale Factory Chatbot
## _Agent conversationnel LLM_

Cet chatbot LLM est conçu avec le framework **RASA** et **Python3**

# Version des différentes technologies
| Technologies| Version |
| ------ | ------ |
| **Rasa Version** | 3.6.13|
| **Minimum Compatible Version** | 3.5.0 |
| **Rasa SDK Version** | 3.6.2 |
|  **Python Version**| 3.10.9 |


# Prérequis
Vous devez avoir python installer en local afin de pouvoir utiliser cette application

## installer python 
Pour installer python sur windows, macos ou linux veillez, [cliquer ici](https://kinsta.com/knowledgebase/install-python/) pour les instructions à suivre

# Installation du chatbot en local

## 1- Creation d'un environnement virtuel python 
Dans votre terminal, allez dans le reportoire du chatbot puis faites :
```sh
$ python -m venv env
```
>Notez que _\<environment name\>_  est un nom que vous choississez, mais par convention, nous utilisons env
### Activation de l'environnement virtuel:
Après la création de l'environnement virtuel vous devez l'activer
#### sur windows :
```sh
env\Scripts\activate.bat
```
#### sur macos ou linux :
```sh
source env/bin/activate
```
après l'activation de l'environnement virtuel dans votre projet, il s'affichera comme ça :
```sh
(env) ~/HESTIA_CHATV2.0/
```
finalisez cette étape par la mise à jour de pip :
```sh
pip install -–upgrade pip
```

## 2- Installation du requirements.txt
Tous les paquets pythons nécessaires au bon fonctionnement de ce projet ont été répertoriés dans un fichier texte,
afin que vous n'ayez pas à les installer individuellement.
Vous pouvez les installer en tapant dans votre terminal, la commande suivante:
```sh
pip install -r requirements.txt
```
Bravo 👏, votre chatbot est maintenant près à l'emploi.

# Structure du projet
Le projet est structuré comme suit :

+ |--actions/
    +|-- __init__.py
    + |-- actions.py
+ |--data/
    + |-- nlu.yml
    + |--rules.yml
    + |--stories.yml
+ |--models/
    + |--_modele entrainé_
+ |--tests/
    + |--test_stories.yml
+ |--config.yml
+ |--credentials.yml
+ |--domain.yml
+ |--endpoints.yml
+ |--liste_apart.txt
+ |--requirements.txt

Pour en apprendre sur les différents fichiers, veuillez lire la documentation *Rasa*  [Doc Rasa](https://rasa.com/docs/rasa/chitchat-faqs)

# Comment ça marche 
voici le **Workflow de développement de chatbot avec Rasa**:
## 1- preparation des données:
La première étape consiste à collecter des données d'entraînement : des conversations, des exemples d'intentions, et des entités.
Ces données sont utilisées pour entraîner le modèle NLU et le modèle Core.

exemples:

**nlu.yml**:
```sh
version: "3.1"

nlu:
- intent: saluer
  examples: |
    - Hé
    - Bonjour
    - Salut
    - bonjour
    - salut
    - ça va
    - tu vas bien
    - vous allez bien
    - salut à vous
    - Salutations !
    - salutations
    - Bonsoir !
    - Salut, comment ça va ?
    - Salut, comment allez-vous
    - Bonjour, comment vas-tu ?
    - Bonjour, comment allez-vous


- intent: aurevoir
  examples: |
    - au revoir
    - à plus tard
    - bonne nuit
    - au revoir
    - Super Merci!
    - passe une bonne journée
    - passez une bonne journée
    - À un de ces quatre
    - Bye Bye
    - à plus tard
    - Merci beaucoup!
    - Merci!
    - bon après-midi
    - bonne soirée

- intent: numero_appartement
  examples: |
    - [1](appartement)
    - [2](appartement)
    - [3](appartement)
    - [4](appartement)
    - [5](appartement)
    - [6](appartement)
    - [7](appartement)
    - [8](appartement)
    - [9](appartement)

- intent: adresse
  examples: |
    - où se trouve votre appartement ?
    - où se trouve l'appartement ?
    - où se situe l'appartement ?
    - Pouvez-vous me donner votre adresse?
    - Pourriez-vous me donner l'adresse de l'appartement ?
    - quelle est l'adresse de l'appartement ?
    - c'est quoi votre adresse ?
    - quelle est votre adresse ?
    - dites-moi où vous êtes basé.
```
l'expression *intent* désigne l'intention de l'utilisateur, pour chaque intention on définit un certain nombre d'exemples pour permettre au modèle de reconnaitre l'intention dans l'input de 
l'utilisateur lors de la conversation.

**domain.yml***:
```sh
version: "3.1"

intents:
  - saluer
  - aurevoir
  - numero_appartement
  - adresse
  - etage
  - ascenseur
  - presence_parking
  - emplacement_parking
  - local_poubelle
  - check_in
  - cles_acces
  - inclus_reservation
  - taxi
  - check_out
  - plainte
  - internet_wifi
  - television
  - chauffage
  - stationnement
  - parking_sous_sol
  - eau_chaude
  - caution
  - retour_caution
  - surveillance
  - contact
  - transport
  - activites
  - nombre_chambre
  - composition_couchage
  - capacite_accueil
  - draps_serviettes
  - eau_robinet
  - geolocalisation
  - billet_disney
  - animaux_domestique
  - autre_appartement


entities:
  - appartement


slots:
  appartement:
    type: categorical
    values:
      - 1
      - 2
      - 3
      - 4
      - 5
      - 6
      - 7
      - 8
      - 9
      - 10
    mappings:
      - type: from_entity
        entity: appartement

actions:
  - action_demander_appart
  - action_fallback
  - action_confirm_appartement
  - action_adresse_appartement
  - action_etage_appartement
  - action_ascenseur_appartement
  - action_presence_parking
  - action_parking_appartement
  - action_local_poubelle
  - action_nombre_chambre
  - action_composition_couchage
  - action_nombre_personne
  - action_changer_appart



responses:
  utter_saluer:
  - text: "Salut! , je suis *HESTIA* votre assistant conversationnel, afin de pouvoir mieux vous aider, je vous prie de m'indiquer l'appartement sur lequel vous aimeriez avoir des réponses, en tapant simplement le chiffre qui vient avant le nom de l'appartement"

  utter_aurevoir:
  - text: "Merci! à très bientôt"
  
  ## Reponses à l'intent adresse par appartement
  utter_120449_adresse:
  - text: "C’est au 21 rue d'Amsterdam , 77144, Montevrain."

  utter_206036_adresse:
  - text: "19 Avenue Marie Curie, 77600, Bussy-Saint-Georges"

  utter_106576_adresse:
  - text: "4 Boulevard Camille Saint Saens , 77185, Lognes"

  utter_181119_adresse:
  - text: "13 Rue Konrad Adenauer, 77600, Bussy-Saint-Georges."

  utter_162702_adresse:
  - text: "4 Rue de Galmy, 77700, Chessy."

  utter_873398_adresse:
  - text: "8 Avenue Haroun Tazieff, 77600, Bussy Saint Georges."

  utter_112637_adresse:
  - text: "22 Avenue Jacques Cartier, 77600, Bussy-Saint-Georges."

  utter_169603_adresse:
  - text: "2 Rue Konrad Adenauer, 77600, Bussy-Saint-Georges."

  utter_87270_adresse:
  - text: "Bâtiment C, 9 Rue Konrad Adenauer , 77600, Bussy Saint Georges."

  utter_117958_adresse:
  - text: "2 Rue Buissonnière, 77600, Bussy Saint George"

  utter_168306_adresse:
  - text: "2 Rue de la Fontaine Rouge, 77700, Chessy"

  utter_157403_adresse:
  - text: "40 cours de la Garonne, 77700, Serris"

  utter_118056_adresse:
  - text: "12 Rue du Vieux Colombier, 95650 , Montgeroult"

  utter_141337_adresse:
  - text: "4 Rue de l'Aviateur Martel, 77600, Bussy-Saint-Georges"

  utter_192118_adresse:
  - text: "7 rue de l'aviateur martel, 77600, Bussy-Saint-Georges"

  utter_104231_adresse:
  - text: "2 rue Konrad Adenauer, 77600, Bussy Saint Georges"

  utter_106354_adresse:
  - text: "10 Avenue Haroun Tazieff, 77600, Bussy Saint Georges"

  utter_89830_adresse:
  - text: "37 Boulevard du Segrais,, 77185, Lognes"

  utter_113076_adresse:
  - text: "33 Cours du tage , 77700, Serris"

  utter_119645_adresse:
  - text: "8 Rue de la marne, 77700, Chessy"

  utter_177757_adresse:
  - text: "1 Place de la Libération, 77600, Bussy-Saint-Georges"

  utter_192119_adresse:
  - text: "41 Avenue Marie Curie, 77600, Bussy-Saint-Georges"

  utter_188203_adresse:
  - text: "10 Avenue de la Société des Nations, 77144, Montévrain"

  utter_123977_adresse:
  - text: "36 Rue de la Charbonnière, 77144, Montévrain"

  utter_121504_adresse:
  - text: "7 place d'Amilly, 77700, Serris"

  utter_102523_adresse:
  - text: "1 Rue du pere Brottier, 77600, Bussy Saint Georges"

  utter_172401_adresse:
  - text: "2 Rue Robert Surcouf, 77600, Bussy-Saint-Georges"

  utter_191533_adresse:
  - text: "2 Rue Eric Tabarly, 77600, Bussy-Saint-Georges"

  utter_96610_adresse:
  - text: "44 rue des berges, 77700, Bailly romainvilliers"

  utter_146402_adresse:
  - text: "1 Avenue Jacques Cartier, 77600, Bussy-Saint-Georges"

```

Dans le fichier domain.yml, on liste les différentes intentions, on a definit une entité pour stocker le numéro de l'appartement de l'utilisateur,
les différentes actions et enfin toutes les réponses que le chatbot devra fournir aux différentes intentions.

**stories.yml** :

```sh
version: "3.1"

stories:


- story: information sur appartement
  steps:
  - intent: saluer
  - action: action_demander_appart
  - intent: numero_appartement
  - action: action_confirm_appartement
  - intent: adresse
  - action: action_adresse_appartement
  - intent: etage
  - action: action_etage_appartement
  - intent: ascenseur
  - action: action_ascenseur_appartement
  - intent: presence_parking
  - action: action_presence_parking
  - intent: emplacement_parking
  - action: action_parking_appartement
  - intent: stationnement
  - action: utter_stationnement
  - intent: parking_sous_sol
  - action: utter_parking_sous_sol
  - intent: local_poubelle
  - action: action_local_poubelle
  - intent: check_in
  - action: utter_check_in
  - intent: cles_acces
  - action: utter_cles_acces
  - intent: inclus_reservation
  - action: utter_inclus_reservation
  - intent: taxi
  - action: utter_taxi
  - intent: check_out
  - action: utter_check_out
  - intent: plainte
  - action: utter_plainte
  - intent: internet_wifi
  - action: utter_internet_wifi
  - intent: television
  - action: utter_television
  - intent: chauffage
  - action: utter_chauffage
  - intent: eau_chaude
  - action: utter_eau_chaude
  - intent: caution
  - action: utter_caution
  - intent: retour_caution
  - action: utter_retour_caution
  - intent: surveillance
  - action: utter_surveillance
  - intent: contact
  - action: utter_contact
  - intent: transport
  - action: utter_transport
  - intent: activites
  - action: utter_activites
  - intent: nombre_chambre
  - action: action_nombre_chambre
  - intent: composition_couchage
  - action: action_composition_couchage
  - intent: capacite_accueil
  - action: action_nombre_personne
  - intent: draps_serviettes
  - action: utter_draps_serviettes
  - intent: eau_robinet
  - action: utter_eau_robinet
  - intent: geolocalisation
  - action: utter_geolocalisation
  - intent: billet_disney
  - action: utter_billet_disney
  - intent: animaux_domestique
  - action: utter_animaux_domestique
  - intent: aurevoir
  - action: utter_aurevoir
```
Dans le fichier stories.yml on procède à la mise en place des étapes de machine learning afin de permettre au chatbot de contextualiser ses échanges avec l'utilisateur.

## 2- Entrainement du chatbot:
Une fois les données de l'entrainement fournies, il faut dabord procéder à leur validation, puis à l’entraînement pour la création d'un model .

### Validation de données:
```sh
   rasa data validate
```
### entrainement du chatbot:
```sh
   rasa train
```
Après l'entrainement du chatbot vous pouvez tester le chatbot en interagissant avec lui depuis le terminal 
```sh
   rasa shell
```
