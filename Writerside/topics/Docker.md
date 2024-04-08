# Jour-1 -- Docker


## Prérequis sous windows
* utilisable uniquement avec Win pro ou entreprise X64 >= 10
* s'assurer qu'Hyper-V, Sous-système pour linux et plateforme de machine virtuelle soient activées  
  ![Activation fonctionnalités windows](Activation-Virtualisation-BIOS.png)
* installer docker desktop
  cocher 'use WSL 2'  
  ![config docker desktop](Config-Docker-WSL2.png)

## Création d'un container

Nécessite la création d'au minimum 2 fichier dans le projet :
* `Dockerfile`
* `compose.yaml`

### Dockerfile
C'est dans ce fichier que l'on va indiquer quels packages nous allons installer dans le container.
Ne comporte pas d'extension de fichier.  
On peu spécifier des installations personnalisées, lancer des commandes...  
ce fichier doit être pensé comme une procédure de mise en place d'un environnement :
1. installation du server
2. MAJ des paquets et installation des dépendances nécessaires
3. installation des extensions PHP nécessaires
4. installation d'extensions supplémentaires
5. installation de composer
6. installation de Node.js
7. installation de yarn
8. activation du module rewrite d'apache
9. définition du répertoire de travail
10. copie du fichier de configuration Apache
11. ajout le chemin des binaires de composer au path

cette liste est un exemple lié au Dockerfile contenu dans le dossier code du topic

### compose.yalm
C'est le fichier de configuration des différents services.  
Syntaxe stricte, indentation importante.  
Chaques services définis dans ce fichier sera installer dans un ?'sous-container'?.  
Exemple de service web :
```yaml
# Définit les services de l'application
services:
  # Service web
  web:
    # Construit le service à partir du Dockerfile spécifié
    build:
      context: .  # Contexte de construction (répertoire actuel)
      dockerfile: docker/php/Dockerfile  # Chemin vers le Dockerfile
    container_name: docker_ssb_web  # Nom du conteneur
    ports:
      - '8080:80'  # Redirige le port 8080 de l'hôte vers le port 80 du conteneur
    volumes:
      - ./:/var/www/html  # Montez le répertoire actuel sur /var/www/html dans le conteneur
```  
Pour un exemple complet, regarder dans le dossier fichier_code.
> Quand on doit spécifier un nouveau port, il est conventionnel de rajouter
> un chiffre identique au premier du port par défaut.  
> *exemple : 3306 devient 33306*

### Placement dans le projet
Il est possible de ranger le fichier Dockerfile en le plaçant dans '/projet/docker/php'.
Le compose.yalm doit se trouver à la racine du projet.

### Attention
Apache nécessite un fichier de configuration supplémentaire.  
Le nom importe peu, mais doit avoir pour extension '.conf'.  
Il est placé à côté du Dockerfile.
Dans Dockerfile rajouter une commande pour copier ce fichier au bon endroit dans le container (cf : Dockerfile.l44).

## Build de l'image
Dans un terminal, se placer à la racine du projet, et taper :
```bash
docker compose build ## Build l'image
```

## Lancement du conteneur
Dans un terminal, se placer à la racine du projet, et taper :
```bash
docker compose up -d ## Lance le conteneur
```

Une fois ces étapes terminées, le conteneur est actif.  
On accède à son site web par l'URL suivante : http://localhost:8080  