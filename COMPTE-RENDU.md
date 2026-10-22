# Compte Rendu - TP DevSecOps

## 1. Gestion des Secrets (Gitleaks)
- Scan sur le projet propre : 0 fuite détectée.
- Test avec `demo.env` : 1 fuite détectée (`github-pat`), hook `pre-commit` fonctionnel et bloquant en local.

## 2. Analyse Statique (SAST - Semgrep)
- Projet propre : 75 règles exécutées, 0 alerte.
- `Dockerfile.naif` : 1 alerte détectée (utilisation de l'utilisateur root par défaut, autofix `USER non-root`).

## 3. Infrastructure as Code (Checkov)
- Vérification de la configuration des conteneurs et des bonnes pratiques Dockerfile. Échec sur le Dockerfile non durci lors des tests en CI.

## 4. Analyse des Images & Portes de Sécurité (Trivy & Pipeline CI)
- **Durée totale du pipeline :** ~4 minutes
- **Job le plus long :** `image`
- **Porte 1 (Image obsolète `node:16-alpine`) — Jobs en échec :** `image` (bloquage de la livraison suite aux vulnérabilités critiques).
- **Porte 2 (Dockerfile naïf) — Jobs en échec :** `iac` (bloquage de la fusion par Checkov).

## 5. Tri des Alertes (Triage)
- Distinction réussie entre les vrais positifs (ex: vulnérabilité critique OpenSSL `libcrypto3`) et les faux positifs (ex: URL publique d'exemple `API_URL` ou clé factice dans les tests).
