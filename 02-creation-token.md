## MODULE 2 : Création de Token sur Testnet avec CLI (45 min)

### 🎯 Objectifs
- Comprendre les concepts clés : wallet, testnet, mainnet
- Créer un wallet Solana
- Obtenir des SOL de test gratuits
- Créer votre premier token
- Mint (frapper) des tokens
- Effectuer des transferts

---

### Étape 2.1 : Comprendre les Concepts Clés (5 min)

#### Wallet vs Token Accounts - IMPORTANT à comprendre

Sur Solana, il existe **deux types de comptes différents** :

**1. Wallet Solana (Keypair)**
- Contient des **SOL** (la crypto native de Solana)
- **Adresse publique** : Comme un RIB, vous pouvez la partager (ex: `7xK3...9dB2`)
- **Clé privée** : Comme votre code PIN, NE JAMAIS LA PARTAGER
- Créé avec : `solana-keygen new`
- Utilisé pour : payer les frais de transaction, recevoir des SOL

**2. Token Accounts (pour les tokens SPL)**
- Contiennent des **tokens SPL** (USDC, votre token personnalisé, etc.)
- **CHAQUE type de token nécessite son propre Token Account**
- Exemple : Si vous voulez recevoir USDC + votre token, vous avez besoin de 2 Token Accounts
- Créé avec : `spl-token create-account`

**Analogie bancaire :**
- **Wallet Solana** = Votre compte en euros (monnaie par défaut)
- **Token Account** = Un compte en devises étrangères (dollars, yens, etc.)
- Vous avez UN compte principal (wallet), mais plusieurs comptes en devises (token accounts)

#### Testnet vs Mainnet

| Testnet (Devnet) | Mainnet |
|------------------|---------|
| Réseau de test | Réseau de production |
| SOL gratuits (sans valeur) | SOL réels (valeur $$$) |
| Pour apprendre et tester | Pour le vrai déploiement |
| Peut être réinitialisé | Permanent |

**Règle d'or : TOUJOURS tester sur Devnet d'abord !**

#### Les 3 Composants d'un Token SPL

1. **Mint Account** (Compte de Frappe)
   - L'identité unique de votre token
   - Stocke : supply totale, nombre de décimales, autorités
   - Adresse = identifiant du token
   - **C'est comme la "définition" du token**

2. **Token Account** (Compte de Token)
   - Un "porte-monnaie" pour UN type de token spécifique
   - Chaque personne a besoin d'un Token Account pour recevoir vos tokens
   - Stocke : le solde du propriétaire pour CE token précis
   - **Vous pouvez avoir plusieurs Token Accounts (un par type de token)**

3. **Associated Token Account (ATA)**
   - Un Token Account "par défaut" généré automatiquement
   - 1 ATA par personne par type de token
   - Adresse dérivée de : votre wallet + l'adresse du token
   - Simplifie l'envoi/réception (pas besoin de créer manuellement)

**Analogie complète :**
- **Mint Account** = La Banque Centrale de l'Euro (définit ce qu'est l'euro)
- **Token Account** = Un compte bancaire qui contient des euros
- **ATA** = Votre compte bancaire principal par défaut
- **Wallet Solana** = Votre identité (carte d'identité bancaire)

---

### Étape 2.2 : Créer un Wallet Testnet (10 min)

#### Créer un nouveau wallet

```bash
solana-keygen new --outfile ~/solana-wallet.json
```

**Ce qui se passe :**
- Génère une paire de clés (publique + privée)
- Sauvegarde dans le fichier `solana-wallet.json`
- Vous demande une passphrase (optionnel, appuyez sur Entrée pour sauter)

**Sortie attendue :**
```
Generating a new keypair

For added security, enter a BIP39 passphrase

NOTE! This passphrase improves security of the recovery seed phrase NOT the
keypair file itself, which is stored as insecure plain text

BIP39 Passphrase (empty for none): [Appuyez sur Entrée]

Wrote new keypair to /home/solanadev/solana-wallet.json
================================================================================
pubkey: 7xK3DiKPqYj9VoW2tN8HrPjZfYqE9dB2nL4mX1pQ5rK8
================================================================================
Save this seed phrase and your BIP39 passphrase to recover your new keypair:
[12 mots affichés ici - NOTEZ-LES DANS UN ENDROIT SÛR]
================================================================================
```

#### 🔒 IMPORTANT - Sauvegarder votre seed phrase

Les **12 mots** affichés permettent de récupérer votre wallet. Notez-les :
- Sur papier (pas en numérique pour la sécurité)
- Dans un endroit sûr
- **Pour le testnet, ce n'est pas critique, mais prenez l'habitude !**

#### Configurer le wallet par défaut

```bash
solana config set --keypair ~/solana-wallet.json
```

#### Configurer le réseau Testnet (Devnet)

```bash
solana config set --url https://api.devnet.solana.com
```

#### Vérifier la configuration

```bash
solana config get
```

**Sortie attendue :**
```
Config File: /home/solanadev/.config/solana/cli/config.yml
RPC URL: https://api.devnet.solana.com
WebSocket URL: wss://api.devnet.solana.com/ (computed)
Keypair Path: /home/solanadev/solana-wallet.json
Commitment: confirmed
```

#### Voir votre adresse publique

```bash
solana address
```

**Sortie :** Votre adresse publique (ex: `7xK3DiKPqYj9VoW2tN8HrPjZfYqE9dB2nL4mX1pQ5rK8`)

#### Vérifier votre balance

```bash
solana balance
```

**Sortie :** `0 SOL` (normal, le wallet est vide)

---

### Étape 2.3 : Obtenir des SOL de Test (Airdrop) (5 min)

#### Qu'est-ce qu'un Airdrop ?

Un **airdrop** est un envoi gratuit de tokens. Sur le testnet, on peut demander des SOL gratuits pour tester.

#### Demander un airdrop

```bash
solana airdrop 2
```

**Explication :** Demande 2 SOL de test

**Sortie attendue :**
```
Requesting airdrop of 2 SOL

Signature: 5J7Z...k3Qm

2 SOL
```

#### Vérifier la balance

```bash
solana balance
```

**Sortie :** `2 SOL`

#### 🚨 Problème courant : Airdrop échoue

Si vous voyez :
```
Error: airdrop request failed. This can happen when the rate limit is reached.
```

**Solutions :**
1. Attendez 1-2 minutes et réessayez
2. Demandez moins : `solana airdrop 1`
3. Utilisez le faucet web : https://faucet.solana.com/
   - Collez votre adresse
   - Cliquez sur "Confirm Airdrop"

---

### Étape 2.4 : Créer Votre Premier Token (15 min)

#### Étape A : Créer le Mint Account

Le **Mint Account** est l'identité de votre token. C'est comme créer le concept de "l'Euro" avant que les euros existent.

```bash
spl-token create-token --program-id TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb
```

**Explication de la commande :**
- `spl-token` : Outil pour gérer les tokens SPL
- `create-token` : Crée un nouveau type de token
- `--program-id TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb` : **IMPORTANT !** Utilise Token-2022 au lieu du Token Program classique

**💡 Pourquoi on utilise `--program-id` ?**

Sur Solana, il existe 2 programmes pour créer des tokens :

1. **Token Program** (programme original)
   - Commande : `spl-token create-token` (sans --program-id)
   - Fonctionnalités de base uniquement
   - C'est l'ancien standard

2. **Token-2022 Program** (programme moderne) ⭐
   - Commande : `spl-token create-token --program-id TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb`
   - 16 extensions avancées (fees, metadata, transfer hooks, etc.)
   - **C'est le standard professionnel 2025**

**Dans cette formation, nous utilisons Token-2022** parce que :
- ✅ Plus moderne et puissant
- ✅ Extensions avancées (Module 3.5)
- ✅ Recommandé pour les nouveaux projets
- ✅ Toujours compatible avec les outils standards

**Le `--program-id TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb` est l'adresse du Token-2022 Program sur Solana.**

**Sortie attendue :**
```
Creating token 3mKPqEHGPf7H8h9JCxZ4NLrQqJ5n2Yv8KpR7BnT1kL9m under program TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb

Address:  3mKPqEHGPf7H8h9JCxZ4NLrQqJ5n2Yv8KpR7BnT1kL9m
Decimals:  9

Signature: 2kN9...pL7x
```

Notez bien la ligne **"under program TokenzQdBNbLqP5VEhdkAS6EPFLC1PHnBqCXEpPxuEb"** qui confirme que vous utilisez Token-2022 !

**📝 NOTEZ CETTE ADRESSE !** C'est l'identifiant unique de votre token. Exemple : `3mKPqEHGPf7H8h9JCxZ4NLrQqJ5n2Yv8KpR7BnT1kL9m`

#### Comprendre les Decimals

**Decimals: 9** signifie que votre token peut être divisé en 9 décimales.

**Exemples :**
- Bitcoin : 8 décimales (0.00000001 BTC = 1 satoshi)
- USDC : 6 décimales (0.000001 USDC)
- Votre token : 9 décimales (comme SOL)

**Concrètement :**
- Si vous créez 1,000,000 tokens
- Vous pouvez envoyer 0.123456789 tokens à quelqu'un

#### Voir les informations de votre token

Remplacez `YOUR_TOKEN_ADDRESS` par l'adresse de votre token :

```bash
spl-token display YOUR_TOKEN_ADDRESS
```

**Exemple :**
```bash
spl-token display 3mKPqEHGPf7H8h9JCxZ4NLrQqJ5n2Yv8KpR7BnT1kL9m
```

**Sortie :**
```
Address: 3mKPqEHGPf7H8h9JCxZ4NLrQqJ5n2Yv8KpR7BnT1kL9m
Decimals: 9
Supply: 0
```

**Supply: 0** → Aucun token n'a encore été créé (comme une banque sans argent)

---

#### Étape B : Créer un Token Account

Avant de pouvoir recevoir des tokens, vous devez créer un "porte-monnaie" pour ce type de token.

```bash
spl-token create-account YOUR_TOKEN_ADDRESS
```

**Exemple :**
```bash
spl-token create-account 3mKPqEHGPf7H8h9JCxZ4NLrQqJ5n2Yv8KpR7BnT1kL9m
```

**Sortie :**
```
Creating account 9kX2pQv8H3nL1mR4TfB7KcE5YjW6ZnA8DqM3NhP2VxS1

Signature: 7hM4...nQ2k
```

**📝 Cette nouvelle adresse** (`9kX2p...`) est votre Token Account pour ce token spécifique.

#### Vérifier votre balance de tokens

```bash
spl-token balance YOUR_TOKEN_ADDRESS
```

**Sortie :**
```
0
```

Normal, vous n'avez pas encore mint (créé) de tokens.

---

#### Étape C : Mint (Frapper) des Tokens

**Mint** = Créer des tokens. Comme une banque centrale qui imprime de la monnaie.

Créons **1,000,000 tokens** :

```bash
spl-token mint YOUR_TOKEN_ADDRESS 1000000
```

**Exemple :**
```bash
spl-token mint 3mKPqEHGPf7H8h9JCxZ4NLrQqJ5n2Yv8KpR7BnT1kL9m 1000000
```

**Sortie :**
```
Minting 1000000 tokens
  Token: 3mKPqEHGPf7H8h9JCxZ4NLrQqJ5n2Yv8KpR7BnT1kL9m
  Recipient: 9kX2pQv8H3nL1mR4TfB7KcE5YjW6ZnA8DqM3NhP2VxS1

Signature: 3pL9...mK1x
```

**⚠️ IMPORTANT - Comprendre les Décimales**

Quand vous mintez `1000000`, vous créez **1 million de tokens** (unités complètes).

**Les 9 décimales NE multiplient PAS ce nombre !**

Les décimales permettent de **diviser** vos tokens en fractions :
- Vous avez **1,000,000 tokens** au total
- Vous pouvez envoyer **0.5 tokens** (une fraction)
- Vous pouvez envoyer **0.123456789 tokens** (jusqu'à 9 décimales)

**Analogie avec SOL :**
- 1 SOL = 1,000,000,000 lamports (représentation interne)
- Mais vous avez **1 SOL**, pas 1 milliard de SOL
- Les lamports permettent juste de diviser : 0.000000001 SOL = 1 lamport

**Pour vos tokens :**
- Supply affichée = **1,000,000 tokens**
- Représentation interne = 1,000,000 × 10^9 = 1 trillion d'unités de base
- **Mais pour l'utilisateur : vous avez 1 million de tokens !**

#### Vérifier la balance

```bash
spl-token balance YOUR_TOKEN_ADDRESS
```

**Sortie :**
```
1000000
```

**🎉 Félicitations ! Vous avez créé 1 million de tokens !**

#### Vérifier la supply totale

```bash
spl-token supply YOUR_TOKEN_ADDRESS
```

**Sortie :**
```
1000000
```

---

### Étape 2.5 : Transférer des Tokens (10 min)

#### Créer un deuxième wallet (pour simuler un destinataire)

```bash
solana-keygen new --outfile ~/recipient-wallet.json
```

**Notez l'adresse publique affichée**, exemple : `4nR8...kL3m`

Ou affichez-la avec :
```bash
solana-keygen pubkey ~/recipient-wallet.json
```

#### Transférer des tokens

Envoyons **50,000 tokens** au destinataire :

**⚠️ IMPORTANT : Frais de Token Account**

Si le destinataire n'a **jamais reçu ce token**, un Token Account doit être créé (~0.002 SOL de frais).

**Problème :** Le wallet `recipient-wallet` que vous venez de créer est vide (0 SOL) !

**Solution : Utilisez DEUX FLAGS ensemble** pour que VOUS (l'envoyeur) payiez les frais :

```bash
spl-token transfer YOUR_TOKEN_ADDRESS 50000 RECIPIENT_ADDRESS --allow-unfunded-recipient --fund-recipient
```

**Exemple :**
```bash
spl-token transfer 3mKPqEHGPf7H8h9JCxZ4NLrQqJ5n2Yv8KpR7BnT1kL9m 50000 4nR8...kL3m --allow-unfunded-recipient --fund-recipient
```

**Explication des flags :**
- `--allow-unfunded-recipient` : Autorise l'envoi à un wallet qui n'a pas de SOL
- `--fund-recipient` : VOUS payez les frais de création du Token Account (~0.002 SOL)

**Ce qui se passe :**
1. Vous payez ~0.002 SOL pour créer le Token Account du destinataire
2. Le destinataire reçoit ses tokens même si son wallet est vide (0 SOL)
3. Tout le monde est content ! 🎉

**Sortie attendue :**
```
Transfer 50000 tokens
  Sender: 9kX2pQv8H3nL1mR4TfB7KcE5YjW6ZnA8DqM3NhP2VxS1
  Recipient: 4nR8...kL3m
  Recipient associated token account: 2xM9...vP4k
  Funding recipient: 2xM9...vP4k

Signature: 8qN3...rM2p
```

Notez bien la ligne **"Funding recipient"** qui confirme que vous avez payé les frais de création !

**Alternative (si vous ne voulez pas payer les frais) :**

Envoyez d'abord des SOL au destinataire :
```bash
solana transfer RECIPIENT_ADDRESS 0.01
# Puis faites le transfer sans --fund-recipient
spl-token transfer YOUR_TOKEN_ADDRESS 50000 RECIPIENT_ADDRESS
```

#### Vérifier votre nouvelle balance

```bash
spl-token balance YOUR_TOKEN_ADDRESS
```

**Sortie :**
```
950000
```

**Logique :** 1,000,000 - 50,000 = 950,000 ✅

---

### ✅ Checkpoint Module 2

Vous devriez pouvoir :
- ✅ Créer un wallet Solana
- ✅ Obtenir des SOL de test
- ✅ Créer un token avec Token-2022
- ✅ Mint 1 million de tokens
- ✅ Transférer des tokens à quelqu'un

**Concepts maîtrisés :**
- Mint Account = identité du token
- Token Account = portefeuille pour un token spécifique
- Mint = créer de nouveaux tokens
- Transfer = envoyer des tokens

---