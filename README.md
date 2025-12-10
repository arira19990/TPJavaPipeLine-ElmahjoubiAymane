# TP Java Pipeline -- Elmahjoubi Aymane

Ce dépôt contient la réalisation du TP *JavaPipeLine*, dont l'objectif
est de mettre en place : - un projet Java Maven simple, - une image
Docker personnalisée, - un pipeline CI/CD avec Jenkins utilisant un
agent Docker, - des captures d'exécution confirmant le bon
fonctionnement.

------------------------------------------------------------------------

## 🚀 Objectifs du TP

-   Créer un dépôt GitHub nommé **TPJavaPipeLine-ElmahjoubiAymane**.
-   S'assurer que le projet Java compile sans erreur.
-   Ajouter un **Jenkinsfile** fonctionnel pour automatiser :
    -   la récupération du code,
    -   la compilation Maven,
    -   l'exécution de l'application.
-   Utiliser Docker pour créer un environnement reproductible.
-   Fournir des captures d'écran du pipeline.
-   Rédiger un petit rapport.

------------------------------------------------------------------------

## 🐳 Dockerfile

L'image Docker `my-maven-git:latest` contient : - Maven 3.9.6 - JDK 17 -
Git

Elle sert d'environnement d'exécution pour Jenkins.

Commande de construction de l'image :

``` bash
docker build -t my-maven-git:latest .
```

------------------------------------------------------------------------

## 🤖 Jenkins Pipeline

Le fichier **Jenkinsfile** permet d'automatiser : - le checkout du
dépôt, - la compilation du projet Maven, - les tests, - l'exécution du
`.jar`.

Le pipeline utilise l'agent Docker :

``` groovy
pipeline {
    agent {
        docker {
            image 'my-maven-git:latest'
            args '-v $HOME/.m2:/root/.m2'
        }
    }

    stages {
        stage('Build') {
            steps {
                sh "mvn clean package"
            }
        }

        stage('Run') {
            steps {
                sh "java -jar target/*.jar"
            }
        }
    }
}
```

------------------------------------------------------------------------

## 🖥️ Configuration Jenkins sous Windows

Jenkins est exécuté dans un conteneur Docker et utilise Docker Desktop
via l'API TCP :

1.  Activer depuis Docker Desktop :\
    **Expose daemon on tcp://localhost:2375 without TLS**

2.  Lancer Jenkins avec :

``` powershell
docker run -d --name jenkins ^
  -p 8080:8080 -p 50000:50000 ^
  -v jenkins_home:/var/jenkins_home ^
  -e DOCKER_HOST=tcp://host.docker.internal:2375 ^
  jenkins/jenkins:lts
```

3.  Installer Docker CLI dans Jenkins :

``` bash
apt-get update
apt-get install -y docker.io
```

------------------------------------------------------------------------

## 📸 Captures d'écran à insérer ici

-   ✔ Interface Jenkins\
-   ✔ Job `java-maven`\
-   ✔ Console Output du build `SUCCESS`\
-   ✔ Preuve que le conteneur Docker se lance\
-   ✔ Structure du projet GitHub

<img width="2560" height="1600" alt="image" src="https://github.com/user-attachments/assets/2d2b1a22-74a8-428e-a0ae-4feece77a8e2" />

------------------------------------------------------------------------

## 🏁 Résultat final

Le pipeline a été exécuté avec succès (statut **SUCCESS**) et le
conteneur Docker utilisé par Jenkins a été : - créé automatiquement, -
utilisé pour compiler et exécuter le projet, - supprimé en fin de tâche.

Le TP respecte l'ensemble des consignes demandées.

------------------------------------------------------------------------

## 👨‍💻 Auteur

**Elmahjoubi Aymane**\
TP -- Java Pipeline\
2025

