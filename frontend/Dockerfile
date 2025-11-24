# --- STAGE 1 : BUILDER (Compilation) ---
# Utilise Node.js 18 pour compiler l'application React
FROM node:18 as builder

# Définir le répertoire de travail
WORKDIR /app

# Copier les fichiers de dépendances et installer
COPY package.json package-lock.json ./
RUN npm install

# Copier le reste du code source
COPY . .

# Exécuter la commande de build de production (npm run build)
# Le résultat est dans le répertoire 'dist'
RUN npm run build


# --- STAGE 2 : RUNNER (Serveur Web Statique) ---
# Utilise Nginx, une image très légère pour servir des fichiers statiques
FROM nginx:alpine

# Copier le build final depuis l'étape 'builder' vers le répertoire de service de Nginx
# /usr/share/nginx/html est le répertoire par défaut de Nginx
COPY --from=builder /app/dist /usr/share/nginx/html

# Copier la configuration Nginx customisée depuis votre répertoire 'frontend/'
# Cette configuration inclut la redirection /api vers le backend:8080
COPY frontend/nginx.conf /etc/nginx/conf.d/default.conf

# Expose le port standard de Nginx (80)
EXPOSE 80

# La commande CMD par défaut de Nginx est utilisée pour démarrer le serveur (nginx -g 'daemon off;')