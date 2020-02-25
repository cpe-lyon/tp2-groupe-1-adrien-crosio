# TP2 Bash / Adrien Crosio

### Exercice 1. Variables d’environnement

https://github.com/cpe-lyon/tp2-groupe-1-adrien-crosio

1. Dans quels dossiers bash trouve-t-il les commandes tapées par l’utilisateur ?

Grace a la commande : 
```
printenv PATH:
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin
:/bin:/usr/games:/usr/local/games:/snap/bin
```

2. Quelle variable d’environnement permet à la commande cd tapée sans argument de vous ramener dans
votre répertoire personnel ?

C'est la variable d'environnement ```HOME``` qui contient le path vers notre répertoire personnel, ```HOME=/home/serveur```

3. Explicitez le rôle des variables LANG, PWD, OLDPWD, SHELL et _.

    * 'LANG' est le language d'encryptage des fichiers
    * 'PWD' est le chemin actuel
    * 'OLDPWD' est le chemin précedent
    * 'SHELL' est le chemin vers le fichier d'éxecution ```SHELL=/bin/bash```
    * '_.' est le chemin pour acceder a la liste des variables d'environnements

4. Créez une variable locale MY_VAR (le contenu n’a pas d’importance). Vérifiez que la variable existe.
```
MY_VAR=Bonjour //Déclaration variable local
echo $MY_VAR //Bonjour
```

5. Tapez ensuite la commande bash. Que fait-elle ? La variable MY_VAR existe-t-elle ? Expliquez. A la fin
de cette question, tapez la commande exit pour revenir dans votre session initiale.

La commande ```bash``` creer un nouveau niveau de bash, c'est un peu comme une fonction recursive. On peut voir le niveau de bash dans lequel on est avec la commande ```echo $SHLVL```.
La variable ```MY_VAR``` n'existe plus car c'est une variable local

6. Transformez MY_VAR en une variable d’environnement et recommencez la question précédente. Expliquez.

```
export MY_VAR=toto //Déclaration variable global
bash
echo $MY_VAR //toto
```
Puisque qu'on a déclaré une variable global a l'aide de ```export```, on peut y accéder même en ajoutant un niveau de bash.

7. Créer la variable d’environnement NOMS ayant pour contenu vos noms de binômes séparés par un espace.
Afficher la valeur de NOMS pour vérifier que l’affectation est correcte.
```
export NOM="Adrien Crosio"
echo $NOM // Adrien Crosio
```

8. Ecrivez une commande qui affiche ”Bonjour à vous deux, binôme1 binôme2 !” (où binôme1 et binôme2
sont vos deux noms) en utilisant la variable NOMS.
```
echo "Bonjour à vous, $NOM" // Bonjour à vous, Adrien Crosio
```

9. Quelle différence y a-t-il entre donner une valeur vide à une variable et l’utilisation de la commande
unset ?

La différence entre donner une valeur vide et utiliser la commande ```unset``` est que la commande delete la variable alors que si on met une valeur vide la variable existe toujours.

10. Utilisez la commande echo pour écrire exactement la phrase : $HOME = chemin (où chemin est votre
dossier personnel d’après bash)
```
echo '$HOME = '$HOME
```

## Programmation Bash

Vous enregistrerez vos scripts dans un dossier script que vous créerez dans votre répertoire personnel.
Tous les scripts sont bien entendu à tester.
Ajoutez le chemin vers script à votre PATH de manière permanente.
```
mkdir script //création du fichier
```
Mettre ```PATH=$PATH:~/script``` à la fin du fichier ```.bashrc``` à l'aide de la commande ```nano .bashrc```.

### Exercice 2. Contrôle de mot de passe

Écrivez un script ```testpwd.sh``` qui demande de saisir un mot de passe et vérifie s’il correspond ou non au
contenu d’une variable PASSWORD dont le contenu est codé en dur dans le script. Le mot de passe saisi par
l’utilisateur ne doit pas s’afficher.

#### Exemple de script simple
```
touch exemple.sh // creation du fichier
chmod +x exemple.sh // ajout des droit d'execution du fichier
nano exemple.sh // pour editer le fichier
```
Script:
```
#!/bin/bash

echo "HELLO, TOI!!"
```
Execution:
```
./exemple.sh
```

#### Realisation de l'exercice 2
Script:
```
#!/bin/bash

PASSWORD='15'
read -s -p 'Saisissez votre mot de passe:' a
printf "\n"
if [ $PASSWORD = $a ]; then
    echo 'Votre mot de passe est correct'
else
    echo 'Votre mot de passe est incorrect'
fi
```

### Exercice 3. Expressions rationnelles

Ecrivez un script qui prend un paramètre et utilise la fonction suivante pour vérifier que ce paramètre
est un nombre réel :
```
function is_number()
{
    re='^[+-]?[0-9]+([.][0-9]+)?$'
    if ! [[ $1 =~ $re ]] ; then
        return 1   
    else
        return 0
    fi
}
```
Il affichera un message d’erreur dans le cas contraire.
```
#!/bin/bash

function is_number()
{
    re='^[+-]?[0-9]+([.][0-9]+)?$'
    if ! [[ $1 =~ $re ]] ; then
        return 1
    else
        return 0
    fi
}

is_number
if [ $? = 1 ]; then
    echo 'Votre parametre est un nombre '
else
    echo 'Votre parametre n est pas un nombre'
fi
```

### Exercice 4. Contrôle d’utilisateur

Écrivez un script qui vérifie l’existence d’un utilisateur dont le nom est donné en paramètre du script. Si le script est appelé sans nom d’utilisateur, il aﬀiche le message: "utilisation: nom_du_script nom_utilisateur”, où nom_du_script est le nom de votre script récupéré automatiquement (si vous changez le nom de votre script, le message doit changer automatiquement)

```
#!/bin/bash

if [ -n "$1" ]; then
    if [ "$1" = "$USER" ]; then
        echo "C'est le bon user"
    else
        echo "C'est pas le bon user"
    fi
else
    echo "Utilisateur: $0 $USER"
fi
```

### Exercice 5. Factorielle 

Écrivez un programme qui calcule la factorielle d’un entier naturel passé en paramètre (on supposera que l’utilisateur saisit toujours un entier naturel).

```
#!/bin/bash
var1=1
for i in $(seq 1 "$1"); do
    var1=$((var1 * i))
done
echo "$var1"
```

### Exercice 6. Le juste prix 

Écrivez un script qui génère un nombre aléatoire entre 1 et 1000 et demande à l’utilisateur de le deviner. Le programme écrira ”C’est plus!”, ”C’est moins!” ou ”Gagné!” selon les cas (vous utiliserez $RANDOM).

```
#!/bin/bash

rand="$RANDOM"
if [ "$1" -eq "$rand" ]; then
    echo "Gagné!"
else
    if [ "$1" -lt "$rand" ]; then
        echo "C’est plus!"
    else
        echo "C’est moins!"
    fi
fi
echo "$rand"
```

### Exercice 7. Statistiques 

1. Écrivez un script qui prend en paramètres trois entiers (entre -100 et +100) et aﬀiche le min, le max et la moyenne. Vous pouvez réutiliser la fonction de l’exercice 3 pour vous assurer que les paramètres sont bien des entiers. 
2. Généralisez le programme à un nombre quelconque de paramètres (pensez à SHIFT) 
3. Modifiez votre programme pour que les notes ne soient plus données en paramètres, mais saisies et stockées au fur et à mesure dans un tableau.

```
#!/bin/bash

somme=0
min=100
max=-100
for param in $*; do
    somme=$((somme + param))
    if [ "$param" -lt "$min" ]; then
        min="$param"
    fi
    if [ "$param" -gt "$min" ]; then
        max="$param"
    fi
done
moy=$((somme / $#))
echo "Min = $min, Max = $max, Somme = $somme, Moyenne = $moy"
```