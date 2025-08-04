# Villa Agency - Application Web Django avec CI/CD AWS

Une application web moderne d'agence immobilière développée avec Django, incluant un pipeline CI/CD complet pour le déploiement automatisé sur AWS.

> 📺 **Projet basé sur la vidéo YouTube de [donaldtedom devops](https://www.youtube.com/@donaldtedom)**  
> Ce projet suit le tutoriel DevOps pour apprendre le déploiement d'applications Django sur AWS avec CI/CD.

## 📋 Table des matières

- [Aperçu du projet](#aperçu-du-projet)
- [Fonctionnalités](#fonctionnalités)
- [Architecture](#architecture)
- [Technologies utilisées](#technologies-utilisées)
- [Installation locale](#installation-locale)
- [Déploiement AWS](#déploiement-aws)
- [Structure du projet](#structure-du-projet)
- [Configuration](#configuration)
- [Scripts de déploiement](#scripts-de-déploiement)
- [Contribution](#contribution)
- [Licence](#licence)

## 🏠 Aperçu du projet

Villa Agency est une application web d'agence immobilière moderne qui permet de présenter des propriétés (appartements, villas, penthouses) avec une interface utilisateur élégante et responsive. L'application est conçue pour être déployée automatiquement sur AWS via un pipeline CI/CD.

Ce projet est un excellent exemple pratique pour apprendre :
- Le développement d'applications web avec Django
- La mise en place d'un pipeline CI/CD avec AWS CodeBuild et CodeDeploy
- La configuration de serveurs web avec Nginx et Gunicorn
- Les bonnes pratiques DevOps pour le déploiement automatisé

## ✨ Fonctionnalités

- **Interface moderne** : Design responsive avec template TemplateMo Villa Agency
- **Catalogue de propriétés** : Présentation d'appartements, villas et penthouses
- **Pages détaillées** : Informations complètes sur chaque propriété
- **Formulaire de contact** : Interface pour contacter l'agence
- **Planification de visites** : Système de prise de rendez-vous
- **Interface d'administration Django** : Gestion du contenu via l'admin Django
- **Déploiement automatisé** : Pipeline CI/CD avec AWS CodeDeploy

## 🏗️ Architecture

```
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   GitHub Repo   │───▶│  AWS CodeBuild  │───▶│ AWS CodeDeploy  │
└─────────────────┘    └─────────────────┘    └─────────────────┘
                                                        │
                                                        ▼
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│     Nginx       │◀───│    Gunicorn     │◀───│   EC2 Instance  │
│  (Reverse Proxy)│    │  (WSGI Server)  │    │  (Ubuntu Server)│
└─────────────────┘    └─────────────────┘    └─────────────────┘
```

## 🛠️ Technologies utilisées

### Backend
- **Django 5.1.5** - Framework web Python
- **Gunicorn 23.0.0** - Serveur WSGI
- **WhiteNoise 6.8.2** - Gestion des fichiers statiques
- **SQLite** - Base de données (par défaut)

### Frontend
- **HTML5/CSS3** - Structure et style
- **Bootstrap** - Framework CSS responsive
- **JavaScript** - Interactivité côté client
- **Font Awesome** - Icônes
- **Owl Carousel** - Carrousel d'images

### Infrastructure & Déploiement
- **AWS CodeBuild** - Service de build
- **AWS CodeDeploy** - Service de déploiement
- **Nginx** - Serveur web et reverse proxy
- **Ubuntu Server** - Système d'exploitation
- **Systemd** - Gestion des services

## 🚀 Installation locale

### Prérequis
- Python 3.8+
- pip
- virtualenv (recommandé)

### Étapes d'installation

1. **Cloner le repository**
```bash
git clone https://github.com/BriacLeGuilloux/USDA-CORN_prediction.git
cd cicdaws-main
```

2. **Créer un environnement virtuel**
```bash
python3 -m venv venv
source venv/bin/activate  # Linux/Mac
# ou
venv\Scripts\activate     # Windows
```

3. **Installer les dépendances**
```bash
pip install -r requirements.txt
```

4. **Configurer la base de données**
```bash
python manage.py makemigrations
python manage.py migrate
```

5. **Créer un superutilisateur (optionnel)**
```bash
python manage.py createsuperuser
```

6. **Collecter les fichiers statiques**
```bash
python manage.py collectstatic
```

7. **Lancer le serveur de développement**
```bash
python manage.py runserver
```

L'application sera accessible sur `http://localhost:8000`

## ☁️ Déploiement AWS

### Configuration requise

1. **Instance EC2** avec Ubuntu Server
2. **CodeBuild Project** configuré
3. **CodeDeploy Application** et Deployment Group
4. **IAM Roles** appropriés pour CodeBuild et CodeDeploy

### Pipeline CI/CD

Le pipeline est automatiquement déclenché lors des commits sur la branche principale :

1. **Build Phase** (CodeBuild) :
   - Installation des dépendances Python
   - Exécution des tests (si configurés)
   - Création des artefacts

2. **Deploy Phase** (CodeDeploy) :
   - Arrêt de l'application existante
   - Installation des dépendances système
   - Configuration de Gunicorn et Nginx
   - Démarrage de la nouvelle version

### Variables d'environnement

Les variables suivantes sont configurées dans `buildspec.yml` :
- `DJANGO_SETTINGS_MODULE`
- `SECRET_KEY`
- `DATABASE_DEFAULT_URL`
- Autres clés API selon les besoins

## 📁 Structure du projet

```
cicdaws-main/
├── agency/                 # Configuration principale Django
│   ├── __init__.py
│   ├── settings.py        # Paramètres Django
│   ├── urls.py           # URLs principales
│   ├── wsgi.py           # Configuration WSGI
│   └── asgi.py           # Configuration ASGI
├── home/                  # Application Django principale
│   ├── views.py          # Vues de l'application
│   ├── urls.py           # URLs de l'app home
│   ├── models.py         # Modèles de données
│   └── admin.py          # Configuration admin
├── templates/             # Templates HTML
│   ├── index.html        # Page d'accueil
│   ├── properties.html   # Liste des propriétés
│   ├── property-details.html
│   └── contact.html      # Page de contact
├── static/               # Fichiers statiques
│   └── assets/
│       ├── css/         # Feuilles de style
│       ├── js/          # Scripts JavaScript
│       ├── images/      # Images du site
│       └── webfonts/    # Polices web
├── scripts/              # Scripts de déploiement
│   ├── start_app.sh     # Démarrage de l'application
│   ├── stop_app.sh      # Arrêt de l'application
│   ├── instance_os_dependencies.sh
│   ├── python_dependencies.sh
│   ├── gunicorn.sh      # Configuration Gunicorn
│   └── nginx.sh         # Configuration Nginx
├── nginx/
│   └── nginx.conf       # Configuration Nginx
├── gunicorn/
│   ├── gunicorn.service # Service systemd
│   └── gunicorn.socket  # Socket systemd
├── appspec.yml          # Configuration CodeDeploy
├── buildspec.yml        # Configuration CodeBuild
├── requirements.txt     # Dépendances Python
└── manage.py           # Script de gestion Django
```

## ⚙️ Configuration

### Settings Django

Les paramètres principaux dans `agency/settings.py` :
- `DEBUG = True` (à modifier pour la production)
- `ALLOWED_HOSTS = []` (à configurer pour la production)
- Configuration WhiteNoise pour les fichiers statiques
- Base de données SQLite par défaut

### Configuration Nginx

Le fichier `nginx/nginx.conf` configure :
- Écoute sur le port 80
- Proxy vers le socket Gunicorn
- Gestion des fichiers statiques

### Configuration Gunicorn

Le service `gunicorn/gunicorn.service` configure :
- 3 workers par défaut
- Socket Unix pour la communication avec Nginx
- Utilisateur `ubuntu` et groupe `www-data`

## 📜 Scripts de déploiement

### Scripts principaux

- **`clean_instance.sh`** : Nettoyage de l'instance avant installation
- **`instance_os_dependencies.sh`** : Installation des dépendances système
- **`python_dependencies.sh`** : Installation des dépendances Python
- **`gunicorn.sh`** : Configuration du service Gunicorn
- **`nginx.sh`** : Configuration du service Nginx
- **`start_app.sh`** : Démarrage de l'application
- **`stop_app.sh`** : Arrêt de l'application

### Hooks CodeDeploy

L'ordre d'exécution défini dans `appspec.yml` :
1. `BeforeInstall` : Nettoyage
2. `AfterInstall` : Installation et configuration
3. `ApplicationStop` : Arrêt de l'ancienne version
4. `ApplicationStart` : Démarrage de la nouvelle version

## 🎓 Ressources d'apprentissage

Ce projet est parfait pour apprendre :
- **Django** : Framework web Python moderne
- **DevOps** : Pratiques de déploiement automatisé
- **AWS** : Services cloud pour l'hébergement et CI/CD
- **Linux** : Administration système Ubuntu
- **Nginx/Gunicorn** : Configuration de serveurs web

## 🤝 Contribution

1. Fork le projet
2. Créer une branche pour votre fonctionnalité (`git checkout -b feature/AmazingFeature`)
3. Commit vos changements (`git commit -m 'Add some AmazingFeature'`)
4. Push vers la branche (`git push origin feature/AmazingFeature`)
5. Ouvrir une Pull Request

## 📄 Licence

Ce projet est sous licence MIT. Voir le fichier [LICENSE](LICENSE) pour plus de détails.

---

## 📞 Contact

**Créateur original :** [donaldtedom devops](https://www.youtube.com/@donaldtedom)


## 🙏 Remerciements

- **donaldtedom devops** pour le tutoriel YouTube original
- Template design par [TemplateMo](https://templatemo.com/tm-591-villa-agency)
- Framework Django par la Django Software Foundation
- Hébergement et CI/CD par Amazon Web Services
