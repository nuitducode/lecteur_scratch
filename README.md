# LECTEUR SCRATCH POUR LA NDC

2. Créer un fork (optionnel mais recommandé)
Pour personnaliser Scratch-GUI, il est préférable de forker le dépôt officiel sur votre propre compte GitHub :

Allez sur le dépôt Scratch-GUI : https://github.com/LLK/scratch-gui.
Cliquez sur Fork pour créer une copie dans votre compte GitHub.
Clonez ensuite votre fork :

bash
Copy code
git clone https://github.com/VotreNomUtilisateur/scratch-gui.git
cd scratch-gui
3. Installer les dépendances
Installez les dépendances nécessaires :

bash
Copy code
npm install
4. Modifier Scratch-GUI pour accepter les URLs des fichiers .sb3
Dans le fichier src/lib/project-fetcher-hoc.jsx, modifiez le code pour permettre de charger un fichier .sb3 via une URL. Ajoutez cette logique pour récupérer un paramètre projectUrl dans l’URL :

javascript
Copy code
import queryString from 'query-string';

// Récupérer le paramètre 'projectUrl' depuis l'URL
const parsed = queryString.parse(window.location.search);
const projectUrl = parsed.projectUrl;

if (projectUrl) {
    fetch(projectUrl)
        .then(response => response.arrayBuffer())
        .then(arrayBuffer => {
            // Charger le projet dans Scratch
            onFetchedProject(arrayBuffer);
        })
        .catch(error => {
            console.error('Erreur lors du chargement du projet:', error);
        });
}
5. Compiler pour la production
Compilez une version optimisée pour le déploiement sur GitHub Pages :

bash
Copy code
npm run build
Les fichiers générés se trouvent dans le dossier build/.

6. Pousser vers GitHub
Initialisez un dépôt Git local, ajoutez les fichiers et poussez-les vers GitHub :

bash
Copy code
git init
git add .
git commit -m "Initial commit"
git branch -M main
git remote add origin https://github.com/VotreNomUtilisateur/scratch-gui.git
git push -u origin main
7. Activer GitHub Pages
Accédez à votre dépôt sur GitHub.
Allez dans Settings > Pages.
Dans la section "Build and Deployment", choisissez la branche main et le dossier /root ou /docs.
Une URL sera générée, par exemple : https://VotreNomUtilisateur.github.io/scratch-gui/.
Étape 2 : Intégrer le lecteur Scratch dans un autre site web
Vous pouvez intégrer votre lecteur Scratch-GUI hébergé sur GitHub Pages dans une iframe sur une autre page web.

Exemple de page HTML avec iframe
Voici un exemple simple pour intégrer Scratch-GUI dans un autre site :

html
Copy code
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Lecteur Scratch</title>
</head>
<body>
    <h1>Lecteur Scratch</h1>

    <!-- Formulaire pour charger un fichier .sb3 -->
    <form id="scratchForm">
        <label for="projectUrl">URL du projet Scratch (.sb3) :</label>
        <input type="text" id="projectUrl" placeholder="https://mon-serveur.com/projets/mon-projet.sb3" required>
        <button type="submit">Charger le projet</button>
    </form>

    <!-- Conteneur iframe pour Scratch-GUI -->
    <div id="scratchPlayer" style="width: 100%; height: 600px; margin-top: 20px; display: none;">
        <iframe id="scratchIframe" src="" style="width: 100%; height: 100%; border: none;"></iframe>
    </div>

    <script>
        const form = document.getElementById('scratchForm');
        const iframe = document.getElementById('scratchIframe');
        const scratchPlayer = document.getElementById('scratchPlayer');

        form.addEventListener('submit', function (e) {
            e.preventDefault();

            const projectUrl = document.getElementById('projectUrl').value;

            if (projectUrl) {
                // URL de Scratch-GUI avec le paramètre projectUrl
                const scratchGuiUrl = `https://VotreNomUtilisateur.github.io/scratch-gui/?projectUrl=${encodeURIComponent(projectUrl)}`;

                // Charger Scratch-GUI dans l'iframe
                iframe.src = scratchGuiUrl;
                scratchPlayer.style.display = 'block';
            } else {
                alert('Veuillez entrer une URL valide.');
            }
        });
    </script>
</body>
</html>
Étape 3 : Tester et finaliser
Hébergez la page d’intégration : Mettez la page HTML sur un autre serveur ou service d’hébergement comme GitHub Pages, Netlify, ou un serveur Apache/Nginx.

Tester le lecteur :

Accédez à la page d’intégration.
Entrez l’URL d’un fichier .sb3 dans le champ texte.
Le fichier .sb3 devrait se charger et s’exécuter dans l’iframe Scratch-GUI.
Personnalisation : Vous pouvez personnaliser l’apparence et ajouter des fonctionnalités comme :

Une liste déroulante de fichiers .sb3.
Des options pour modifier les paramètres Scratch (mode plein écran, zoom, etc.).
