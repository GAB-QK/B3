---
id: Compte
created_date: 03/10/2023
updated_date: 03/10/2023
type: note
---

#  Compte
- **🏷️Tags** :  #10-2023 

## 📝 Notes

### Création du Docker file 

```Dockerfile
FROM node:14-alpine
WORKDIR /app
COPY . ./
RUN npm install
EXPOSE 3000
CMD ["npm", "run", "watch"]
```

>[!attention] 
>placer le fichier dans le répertoire app

Procéder au build de l'image docker avec la commande 

```bash
docker image build ./../app  
```

Une fois le Dockerfile crée utiliser la commande pour retrouver l'id de cell-ci


```bash
input : 
docker images 
-----------------------------------------------------------
output : 
REPOSITORY   TAG       IMAGE ID       CREATED              SIZE
<none>       <none>    b3078068249c   About a minute ago   284MB

```

### CMD et entrypoint 

```Dockerfile
FROM ubuntu
RUN apt-get update && apt-get install -y apt-transport-https
RUN apt-get -y install mtr
WORKDIR /app 
COPY . ./ 
EXPOSE 3000 
CMD ["mtr", "8.8.8.8"] 
```

```dockerfile
FROM node:ubuntu
RUN apt-get update && apt-get install -y apt-transport-https
RUN apt-get -y install mtr
WORKDIR /app 
COPY . ./ .
EXPOSE 3000
ENTRYPOINT ["mtr"]
```

Pour monter les deux docker file il suffit comme precedent de les placer dans le dossier racine et de les build avec la commande suivante


```bash
sudo docker image build -t entrypoint -f ./Dockerfile_Entrypoint ./../app

&&

sudo docker image build -t CMD -f ./Dockerfile_CMD ./../app

```

pour lancer le docker run on utilise les deux commandes suivantes:

```dockerfile
sudo docker run -it <id_cmd>

sudo docker run -it <id_entrypoint>
```

la difference entre les deux montage 

### TP_2
#### création du *docker-compose.yml* 

Dans un nouveau repertoire crée le fichier suivant 
```yml

version: '3'
services:

# Service MySQL
	mysql:
	image: mysql:8.0
	environment: # Définition des variables d'environnement
		MYSQL_ROOT_PASSWORD: VotreMotDePasseRoot
		MYSQL_DATABASE: wordpress_db
		MYSQL_USER: wordpress_user
		MYSQL_PASSWORD: MotDePasseUtilisateurWordPress
	volumes:
		- mysql_data:/var/lib/mysql
	
	# Service WordPress
	wordpress: # Création du service
		image: wordpress:latest
		environment:
			WORDPRESS_DB_HOST: mysql
			WORDPRESS_DB_NAME: wordpress_db
			WORDPRESS_DB_USER: wordpress_user
			WORDPRESS_DB_PASSWORD: MotDePasseUtilisateurWordPress
		ports: # Ouverture du port 80
			- "80:80"
		depends_on:
			- mysql

volumes: # Création du volume
mysql_data:

```

Pour lancer le fichier utiliser la commande 

```bash
sudo docker compose up
```

une fois terminé se rendre sur le localhost avec le port 80 et on arrive sur le wordpress.

![[Pasted image 20231003150352.png]]

>[!tip] Note
>3 types d'images,  **Officiel**, **gérer par une organisation**, **auto-héberger** (privé)
### TP_3

pousser un projet avec un Docker file avec une application sur un dépôts Gitlab, dans un second temps crée un fichier ".yml" et associer la template Docker.

une fois cela fait commit le fichier et vérifier dans les jobs le déploiement avec gitlab-runner 
si cela fonctionne correctement vous devrez avoir l'output suivant : 

![[Pasted image 20231004141851.png]]

Pour bien verifier la création des images se rendre dans Deploy/packageRegistry

![[Pasted image 20231004141950.png]]
![[Pasted image 20231004142012.png]]





## Questions/Thoughts


## 🔗 Links
- 