---
layout: post
title: Questions/Réponses
---
# Question 1 - Acquisition de données

## Comment télécharger les fichiers?
Afin de pouvoir télécharger les données de play-by-play, il est important d'importer la classe StatsApiProxy.py.
Une fois que vous avez accès aux méthodes de la classe, il suffit de faire un appel à la méthode fetch_live_data_for_years(years_to_fetch, path_to_directoy, override).
La méthode se chargera de générer les fichiers selon les saisons demandées.

## fetch_live_data_for_years(years_to_fetch, path_to_directoy, override)
Le paramètre years_to_fetch prend une liste de nombres qui représentent toutes les saisons qui doivent être télécharger.
Le paramètre path_to_directoy prend une String qui représente le chemin ou les fichiers seront enregistrer par le programme.
Le paramètre override prend une valeur de type boolean qui permet d'indiquer si les fichiers devrait être retélécharger si ceux-ci existe déjà dans le chemin indiquer.

## Structure de fichier
Prenons l'exemple suivant:
Nous voulons télécharger les play-by-play de la saison 2017 vers le chemain : C:\data\

Suite à l'exécution du programme, nous aurions la structure de fichiers suivante :
C:\data\Season20172018\Regular20172018
C:\data\Season20172018\Regular20172018\2017020001.json
...

C:\data\Season20172018\Playoff20172018
C:\data\Season20172018\Regular20172018\2017030111.json
...

C:\data\Season20172018\season20172018.json

Le répertoire Regular20172018 contient toutes les données de play-by-play dans la saison régulière 2017. Le nom d'un fichier est son propre gameid.
Le répertoire Playoff20172018 contient toutes les données de play-by-play dans la saison playoff 2017. Le nom d'un fichier est son propre gameid.
Le fichier season20172018.json contient toutes les données play-by-play de la saison régulière 2017 ainsi que la saison playoff 2017. La structure du fichier utilise le gameid comme clé de dictionnaire et la valeur est le contenu de l'appelle vers l'api de hockey.

# Question 4 - Rangez les données

## 1 - dataframe_head(10)
![Alt text](./../pictures/question_4_1.png "dataframe_head(10)")
Veuiller observer ici que le nom des colonnes semble étrange mais elles sont en fait basé sur le fait qu'elle représente le chemin dans la structure de données ce qui permet d'ajouter de nouvelles colonnes dans le dataframe sans nécessairement devoir modifier le code.

## 2 - Avantage numérique?
Discutez de la façon dont vous pourriez ajouter les informations sur la force réelle (c'est-à-dire 5 contre 4, etc.) aux tirs et aux buts, compte tenu des autres types d'événements (au-delà des tirs et des buts) et des fonctionnalités disponibles.

Il existe le type d’événement ‘PENALTY’ qui indique lorsqu’une pénalité a lieu ainsi que la durée de celle-ci et le joueur recevant la pénalité.
Nous pouvons utiliser l’information dans le jeu ‘GOAL’ et lorsque celui-ci indique qu’un but a été fait avec un avantage ou désavantage, nous pouvons prendre le ‘timestamp’ de lorsque le but a été fait voir dans tous les évènements de type ‘PENALTY’.
Observer voir si la pénalité date d’avant le but et si oui, est ce qu’elle est encore active lors du but.
Avec cela nous pourrions déterminer la situation sur la glace de l’équipe marquante.

## 3 - Nouvelles fonctionnalités
discutez d’au moins 3 fonctionnalités supplémentaires que vous pourriez envisager de créer à partir des données disponibles dans cet ensemble de données.

Une fonctionnalité supplémentaire que nous pourrions envisager de créer est de déterminer le moment dans la période auquel chaque but a été marqué.
Ainsi, supposons que nous divisions toute période de hockey en trois sous-périodes distinctes (premières 5 minutes, dernières 5 minutes et les 10 minutes intermédiaires).
Grâce à l’attribut “periodTime” dans “about”, nous pourrions assigner le numéro de la sous-période à chaque but et cette nouvelle fonctionnalité nous permettrait d’évaluer, par exemple, si une équipe X est plus performante ou moins performante lors des débuts de période que l’ensemble des autres équipes de la LNH.

Une fonctionnalité supplémentaire que nous pourrions envisager de créer est de déterminer, pour chaque action de type “Goal”, le nombre de passes qui ont mené au but entre 0 et 2.
Pour extraire cette information, nous pourrions simplement additionner le nombre de “players” ayant le “playerType” “Assist”.
Nous pourrions alors déterminer le niveau d’individualité propre à chaque équipe (les buts se marquent-ils souvent en solo, ou non)?

Une fonctionnalité supplémentaire que nous pourrions envisager de créer est de déterminer la position du joueur (exemple ‘LEFT WING’) effectuant un événement de type ‘SHOT’ et ‘GOAL’.
Il serait possible de retrouver cette information en utilisant le ‘link’ fourni dans chacun des événements.
Cette information permettrait de voir si des équipes ont potentiellement plus de chances de faire un but étant donné que chaque joueur aurait une meilleure chance de faire un but lors d'une partie.

# Question 5 - Visualisations simples

## 1 - Les types de tirs
![Alt text](./../pictures/question_5_1.png "Nombre de tirs total contre le nombre de but par type")
Il est possible d'observer que parmis les différents type de tir, le wrist shot semble être le type de tir le plus utilisé lors d'une saison.
Nous avons utilisé ce graphique, car il permet de voir rapidement lequel des types de tir est le plus utilisé rapidement et avec un peu d'observation, il est possible de faire une estimation sur celui qui semble avoir le meilleur pourcentage de réussite aussi.
En regardant le graphique, il est possible d'estimer que le Tip-In ait un meilleur taux de réussit à faire un but simplement du au fait que la section verte couvre une plus grande surface que que les autres. Il serait possible que le Backhand ait un meilleur taux de but à la fin mais visuellement nous aurions tendance a vouloir dire que le Tip-In est meilleur.

## 3 - Probabilité de faire un but selon la distance et le type de tir
![Alt text](./../pictures/question_5_3.png "probabilité de faire un but")
En observant le graphique, nous sommes capable de déterminer le type de tir qui semble être le plus dangereux dans le sense que celui-ci a une probabilité de faire un but plus élever que le reste.
Le Tip-In serait le tir le plus dangereux avec la plus haute probabilité.
Il est possible d'observer aussi qu'il semblerait y avoir une relation entre la probabilité de faire un but et la distance du tir.
Plus un tir se fait de loins, moins celui-ci à de chance de faire un but.
