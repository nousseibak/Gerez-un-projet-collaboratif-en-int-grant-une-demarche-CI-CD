# Utilise l'image officielle de Node.js pour la phase de construction, basée sur la dernière version de Node
FROM node:latest as build

# Définit le répertoire de travail pour la phase de build
WORKDIR /usr/local/app

# Copie tous les fichiers de l'application dans le répertoire de travail de l'image
COPY ./ /usr/local/app/

# Installe les dépendances de l'application en utilisant Yarn (un gestionnaire de paquets pour Node.js)
RUN yarn

# Lance la commande de build pour produire une version optimisée de l'application dans le dossier `dist`
RUN npm run build

# Utilise l'image officielle de Nginx pour la phase de production (serveur de contenu statique)
FROM nginx:latest as production

# Copie le fichier de configuration personnalisé `nginx.conf` dans le dossier de configuration de Nginx
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Copie le contenu compilé (build) de l'application depuis l'image de build dans le répertoire Nginx pour le servir
COPY --from=build /usr/local/app/dist/bobapp /usr/share/nginx/html

# Expose le port 80 pour permettre les requêtes HTTP d’accès à l'application
EXPOSE 80
