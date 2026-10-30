# Mise en ligne de alkubuntu.com — pas à pas

> Coût total : 9,59 € TTC/an (le domaine). Tout le reste est gratuit.

## Étape 0 — Vérifier le domaine chez OVH ✅ (fait)
- `alkubuntu.com` est **Enregistré / Actif**.
- 👉 Dans l'espace client OVH, ouvre le domaine et active le **renouvellement automatique**
  (la colonne « Fréquence de renouvellement » indique « Aucune » : sans ça, le domaine expire au bout d'un an).

## Étape 1 — Préparer le dossier
Ce dossier contient déjà :
- `index.html` : le site
- `CLAUDE.md` : le contexte du projet pour Claude Code
- `GUIDE-MISE-EN-LIGNE.md` : ce guide
- `img/` : **à remplir avec les 13 images** (liste dans `CLAUDE.md`)

## Étape 2 — GitHub
1. Crée un compte sur https://github.com
2. **New repository** → nom `alk-urithi` → **Public** → ne rien cocher d'autre → *Create*.
3. Dans le dossier du projet, ouvre Claude Code et demande :
   « Initialise git, fais un premier commit et pousse vers https://github.com/TON-PSEUDO/alk-urithi »
   (ou à la main : `git init && git add . && git commit -m "Premier commit" && git branch -M main && git remote add origin https://github.com/TON-PSEUDO/alk-urithi.git && git push -u origin main`)

## Étape 3 — Cloudflare Pages (l'hébergement)
1. Crée un compte gratuit sur https://dash.cloudflare.com
2. Menu **Workers & Pages** → **Create** → onglet **Pages** → **Connect to Git** → autorise GitHub → choisis `alk-urithi`.
3. Réglages de build :
   - Framework preset : **None**
   - Build command : **(laisser vide)**
   - Build output directory : **/**
4. **Save and Deploy**. Le site est en ligne sur une adresse du type `alk-urithi.pages.dev` → vérifie qu'il s'affiche bien (images, panier, bouton WhatsApp).

## Étape 4 — Confier le DNS du domaine à Cloudflare
1. Chez **OVH** : Noms de domaine → `alkubuntu.com` → **DNSSEC** → **désactiver** (à faire en premier !).
2. Chez **OVH**, onglet **Zone DNS** : note (ou fais une capture de) les lignes **MX**, **TXT** (SPF, `v=spf1 ...`) et **CNAME** (`autoconfig`, `autodiscover`…) — ce sont celles du mail Zimbra.
3. Chez **Cloudflare** : **Add a domain** → `alkubuntu.com` → plan **Free**.
   Cloudflare importe la zone : vérifie que les MX et le TXT SPF d'OVH sont bien présents (sinon, ajoute-les à la main).
   Les MX et l'autoconfig doivent rester en **DNS only** (nuage gris), pas en « Proxied ».
4. Cloudflare affiche **2 serveurs DNS** (ex. `xxx.ns.cloudflare.com`).
5. Chez **OVH** : `alkubuntu.com` → onglet **Serveurs DNS** → **Modifier les serveurs DNS** → remplace ceux d'OVH par les 2 de Cloudflare → valider.
6. Attendre la propagation (de quelques minutes à 24-48 h). Cloudflare envoie un mail quand le domaine est **Active**.

## Étape 5 — Brancher le domaine sur le site
1. Cloudflare → **Workers & Pages** → projet `alk-urithi` → **Custom domains** → **Set up a custom domain** → `alkubuntu.com` → valider.
2. Recommencer avec `www.alkubuntu.com`.
3. Cloudflare crée les enregistrements et le certificat HTTPS tout seul.
4. Test : https://alkubuntu.com et https://www.alkubuntu.com 🎉

## Étape 6 — Réactiver le DNSSEC
Cloudflare → `alkubuntu.com` → **DNS** → **Settings** → **DNSSEC** → **Enable**.
Cloudflare donne un enregistrement **DS** à recopier chez OVH (Noms de domaine → `alkubuntu.com` → DNSSEC / « Enregistrements DS »).

## Étape 7 — Vérifier le mail
Webmail Zimbra OVH : crée `contact@alkubuntu.com`, envoie-toi un mail depuis Gmail et réponds-y pour vérifier que tout passe.

## Étape 8 — Avant d'ouvrir les précommandes
- [ ] Page mentions légales + CGV + confidentialité (demande à Claude Code de la créer, voir `CLAUDE.md`)
- [ ] Remplacer `alk-urithi@proton.me` par `contact@alkubuntu.com` si vous voulez
- [ ] Compte PayPal **professionnel** (ou Stripe Payment Links)
- [ ] Optionnel : Google Form pour garder une trace des commandes (`FORM_URL` dans `index.html`)

## Au quotidien
Pour modifier le site (stock, prix, textes) : ouvre Claude Code dans ce dossier, demande la modification,
puis « commit et push ». Cloudflare met le site à jour en ~1 minute.
