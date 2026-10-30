# ALK Urithi — site de précommande (alkubuntu.com)

## Le projet
Site vitrine + précommande pour **ALK Urithi**, marque de vêtements créée entre amis.
Collection n°1 « Ubuntu » : un sweat brodé (coton bio 350 g/m², 49,99 €), 3 masques africains
(Mukuji M01, Kpelie M02, Bwa M03), plusieurs coloris, tailles S à XL, série limitée à 25 pièces.

## Fonctionnement technique
- **Site 100 % statique** : `index.html` (HTML + CSS + JS dans un seul fichier) et le dossier `img/` (images `.webp`).
- Aucune base de données, aucun serveur, aucun framework, aucune étape de build.
- Panier enregistré dans le `localStorage` du visiteur.
- La précommande est envoyée sur **WhatsApp Business** (message pré-rédigé via `wa.me`).
- Le paiement se fait ensuite par **lien PayPal** envoyé dans la discussion (Stripe Payment Links envisagé plus tard).
- Réglages modifiables en haut du `<script>` : `PRICE`, `WA_LINK` (lien court WhatsApp, prioritaire), `WA_NUMBER`, `EMAIL`, `PAYPAL`, `FORM_URL`/`FORM_FIELDS` (Google Form, vide pour l'instant), `STOCK`.
- ⚠️ Le `STOCK` est écrit en dur et n'est pas partagé entre visiteurs : il faut le mettre à jour à la main après chaque vente confirmée, puis redéployer.

## Images attendues dans `img/` (13 fichiers)
`ALK_noir.webp`, `ALK_blanc.webp`, `mask_M01.webp`, `mask_M02.webp`, `mask_M03.webp`,
`M01_NAV.webp`, `M01_NAT.webp`, `M01_CHI.webp`, `M02_NAV.webp`, `M02_GLG.webp`,
`M03_NAV.webp`, `M03_NAT.webp`, `M03_GLG.webp`

## Décisions prises (ne pas remettre en cause sans raison)
- **Domaine** : `alkubuntu.com`, acheté chez **OVHcloud** (compte « Particulier »), avec DNSSEC et 1 compte mail **Zimbra Starter** inclus. Aucun hébergement OVH.
- **Code** : dépôt **GitHub public** `alk-urithi`.
- **Hébergement** : **Cloudflare Pages** (gratuit, usage commercial autorisé), relié au dépôt GitHub → déploiement automatique à chaque push sur `main`.
  GitHub Pages a été écarté : ses conditions interdisent les sites dont le but principal est la vente.
- **DNS** : les serveurs DNS du domaine passent d'OVH à Cloudflare. Il faut recopier chez Cloudflare les enregistrements **MX / SPF (TXT) / autoconfig** d'OVH pour que le mail Zimbra continue de marcher.
  Désactiver le **DNSSEC chez OVH avant** le changement de serveurs DNS, puis le réactiver côté Cloudflare.

## Plan de travail
- **Phase 1 (en cours)** : mise en ligne guidée depuis l'application Claude (Projet « ALK Urithi »).
  L'utilisateur fait les actions lui-même dans le navigateur : upload des fichiers sur GitHub (Add file → Upload files),
  tableau de bord Cloudflare, espace client OVH. Pour toute modification, fournir le fichier `index.html` complet à re-téléverser.
- **Phase 2 (plus tard)** : Claude Code en ligne de commande, dépôt cloné depuis GitHub, authentification via `gh auth login`.
  Claude Code modifie, commit et push ; Cloudflare redéploie automatiquement.

## Reste à faire
Voir `GUIDE-MISE-EN-LIGNE.md` pour le pas-à-pas. En résumé :
1. Ajouter les 13 images dans `img/`.
2. Créer le dépôt GitHub et pousser le code.
3. Créer le projet Cloudflare Pages relié au dépôt (pas de commande de build, dossier de sortie `/`).
4. Ajouter `alkubuntu.com` à Cloudflare, changer les serveurs DNS chez OVH, recopier les enregistrements mail.
5. Ajouter le domaine personnalisé `alkubuntu.com` + `www` au projet Pages.
6. Créer une page **mentions légales + CGV + confidentialité** (`mentions-legales.html`) et la lier dans le footer.
   Infos à demander : nom du responsable de publication, statut (particulier / micro-entreprise + SIRET), adresse de contact, hébergeur (Cloudflare, Inc., 101 Townsend St, San Francisco, CA 94107, USA).
   Prévoir le droit de rétractation de 14 jours et la politique de retours.
7. Plus tard : Google Form pour journaliser les commandes, Stripe Payment Links, passage PayPal en compte professionnel.

## Conventions
- Tout le contenu du site est en **français**.
- Garder le site en un seul `index.html` tant que c'est possible ; respecter le style existant (variables CSS sur `:root`, thème clair/sombre, polices Josefin Sans + Karla).
- Tester l'affichage mobile (largeur 375 px) avant chaque mise en ligne.
