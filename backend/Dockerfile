# --- STAGE 1 : BUILDER (Compilation) ---
# Utilise la version 1.23 de Go pour la compilation
FROM golang:1.23 as builder

# Définir le répertoire de travail dans le conteneur
WORKDIR /app

# Copier les fichiers de module (go.mod et go.sum)
COPY go.mod go.sum ./

# Télécharger les dépendances
RUN go mod download

# Copier le reste du code source
COPY . .

# Compiler l'application Go
# -o: spécifie le nom du binaire de sortie (server)
# cmd/server/main.go: spécifie le fichier main à compiler
# CGO_ENABLED=0: désactive CGo pour créer un binaire statique sans dépendance externe
RUN CGO_ENABLED=0 GOOS=linux go build -o /server cmd/server/main.go


# --- STAGE 2 : RUNNER (Environnement de Production) ---
# Utilise une image alpine légère pour exécuter le binaire
FROM alpine:latest

# Définir des variables d'environnement
ENV PORT=8080

# Installer le paquet ca-certificates (nécessaire pour les connexions HTTPS si votre Go en a besoin)
RUN apk --no-cache add ca-certificates

# Définir le répertoire de travail
WORKDIR /app

# Copier le binaire compilé depuis l'étape 'builder'
COPY --from=builder /server /server

# Expose le port par défaut du serveur Go (8080)
EXPOSE 8080

# Commande pour démarrer l'application
CMD ["/server"]