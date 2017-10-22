1) Création de deux dossiers pour chaque image
a) Tomcat
commande : mkdir mytomcat
b) Mypostgres
commande : mkdir mypostgres

2) Récupération du dossier warrepo depuis le github
commande : git clone https://github.com/lauryao/warrepo
Cela permet de récupérer les fichiers nécessaires pour la base de donnée et l'application web
Mettre ensuite le fichier dbproject.war dans le dossier mytomcat et init-db.sql dans mypostgres

3) Création des deux Dockerfile
Pour cela il vous faudra créer un Dockerfile dans chaque dossier (mytomcat et mypostgres)
grâce à la commande : vi Dockerfile
a) Mytomcat
Ecrivez ensuite les instructions suivantes :

FROM tomcat:8-jre8
COPY ./dbproject.war /usr/local/tomcat/webapps

puis sauvegarder.

b) Mypostgres
Ecrivez les instructions suivantes :

FROM postgress:9.5
COPY ./init-db.sql docker-entrypoint-initdb.d/
VOLUME ["/var/lib/postgressql/data"]

Sauvegardez de même.

4) Exécution des conteneurs
Pour cette étapes il faudra commencer par construire les images avec la commande : docker build -t mytomcat (pour le docker du tomcat) et docker build -t mypostgres (pour le docker de postgres)

Après avoir construit les images il ne reste plus qu'a créer les conteneurs avec la commande : docker run -d --name db mypostgres (pour le postgres) et docker run -d --name mytomcat -p 8888:80 --link db mytomcat afin de lier les deux conteneurs

5) Vérification
Pour vérifier si notre application fonctionne il faudra ouvrir un navigateur et aller sur le lien : http://192.168.99.100:8888/dbproject/accueil.jsp
Entrer ensuite un article et valider celui-ci.

Pour savoir si la base de donnée persiste vous devez l'éteindre et le rallumer.
commande : docker stop mytomcat
commande : docker stop db
puis
commande : docker start mytomcat
commande : docker start db
si tout ce passe bien, les données de l'ancien formulaire devrait être présent.

