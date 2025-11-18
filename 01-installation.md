## MODULE 1 : Installation de l'Environnement (30 min)

### 🎯 Objectifs
- Préparer votre système d'exploitation
- Installer Rust et Cargo
- Installer les outils Solana
- Vérifier que tout fonctionne

---

### 📋 Choisissez votre système d'exploitation

Cette formation est compatible avec **Windows**, **Linux** et **macOS**.

- **Windows** → Suivez [Étape 1.1 : Installation de WSL](#étape-11--installation-de-wsl-windows-uniquement)
- **Linux** → Passez directement à [Étape 1.2 : Installation de Rust](#étape-12--installation-de-rust-et-cargo)
- **macOS** → Suivez [Étape 1.1 : Prérequis macOS](#étape-11--prérequis-macos-uniquement)

---

### Étape 1.1 : Installation de WSL (Windows uniquement)

#### Qu'est-ce que WSL ?

**WSL** (Windows Subsystem for Linux) permet d'exécuter Linux directement sur Windows, sans machine virtuelle. C'est essentiel car les outils Solana sont optimisés pour Linux.

#### Installation

1. **Ouvrir PowerShell en tant qu'administrateur**
   - Cliquez sur le menu Démarrer
   - Tapez "PowerShell"
   - Clic droit → "Exécuter en tant qu'administrateur"

2. **Installer WSL**
   ```powershell
   wsl --install
   ```

3. **Redémarrer votre ordinateur** (obligatoire)

4. **Configurer Ubuntu**
   - Au redémarrage, Ubuntu s'ouvrira automatiquement
   - Créez un nom d'utilisateur (exemple : `solanadev`)
   - Créez un mot de passe (il ne s'affichera pas quand vous tapez, c'est normal)

5. **Vérification**
   ```bash
   lsb_release -a
   ```

   Vous devriez voir quelque chose comme :
   ```
   Distributor ID: Ubuntu
   Description:    Ubuntu 22.04.x LTS
   ```

#### 🚨 Problèmes courants

**Erreur "WSL 2 requires an update to its kernel component"**
- Téléchargez et installez : https://aka.ms/wsl2kernel

**Ubuntu ne démarre pas**
- Ouvrez PowerShell et tapez : `wsl --set-default-version 2`

---

### Étape 1.1 : Prérequis macOS (macOS uniquement)

#### Installation de Homebrew

**Homebrew** est le gestionnaire de paquets pour macOS, il facilite l'installation des outils de développement.

1. **Ouvrir le Terminal**
   - Appuyez sur `Cmd + Espace`
   - Tapez "Terminal" et appuyez sur Entrée

2. **Installer Homebrew**
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

3. **Suivre les instructions post-installation**
   - Homebrew affichera des commandes à exécuter pour l'ajouter au PATH
   - Copiez et exécutez ces commandes (généralement quelque chose comme) :
   ```bash
   echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
   eval "$(/opt/homebrew/bin/brew shellenv)"
   ```

4. **Vérification**
   ```bash
   brew --version
   ```

#### 🚨 Problèmes courants

**Erreur "xcrun: error: invalid active developer path"**
- Installez les outils de développement Xcode :
  ```bash
  xcode-select --install
  ```

**Permission denied**
- Ne jamais utiliser `sudo` avec Homebrew

---

### Étape 1.2 : Installation de Rust et Cargo

#### Qu'est-ce que Rust et Cargo ?

- **Rust** : Langage de programmation ultra-performant utilisé par Solana
- **Cargo** : Gestionnaire de paquets pour Rust (comme npm pour JavaScript)

Nous n'écrirons PAS de Rust dans cette formation, mais les outils Solana en ont besoin.

#### Installation

> **Note** : Ces instructions sont identiques pour **Windows (WSL)**, **Linux** et **macOS**.

1. **Dans votre terminal** (Ubuntu WSL pour Windows, Terminal pour Linux/macOS), exécutez :
   ```bash
   curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
   ```

2. **Choisir l'installation par défaut**
   - Tapez `1` puis Entrée

3. **Recharger la configuration**
   ```bash
   source $HOME/.cargo/env
   ```

4. **Vérification**
   ```bash
   rustc --version
   cargo --version
   ```

   Vous devriez voir des versions (exemple: `rustc 1.83.0`, `cargo 1.83.0`)

#### Temps d'installation
L'installation peut prendre 5-10 minutes selon votre connexion.

---

### Étape 1.3 : Installation de Solana CLI

#### Qu'est-ce que Solana CLI ?

**Solana CLI** (Command Line Interface) est l'outil officiel pour interagir avec la blockchain Solana : créer des wallets, envoyer des transactions, créer des tokens, etc.

#### Installation

> **Note** : Ces instructions sont identiques pour tous les systèmes d'exploitation.

1. **Installer Solana CLI**
   ```bash
   sh -c "$(curl -sSfL https://release.anza.xyz/stable/install)"
   ```

2. **Ajouter Solana au PATH**

   **Pour Linux et Windows (WSL)** :
   ```bash
   echo 'export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"' >> ~/.bashrc
   source ~/.bashrc
   ```

   **Pour macOS** :
   ```bash
   echo 'export PATH="$HOME/.local/share/solana/install/active_release/bin:$PATH"' >> ~/.zshrc
   source ~/.zshrc
   ```

3. **Vérification**
   ```bash
   solana --version
   ```

   Vous devriez voir : `solana-cli 1.18.x` (ou version supérieure)

4. **Installer SPL Token CLI**
   ```bash
   cargo install spl-token-cli
   ```

   ⏱️ Cette étape peut prendre 10-15 minutes.

5. **Vérification SPL Token**
   ```bash
   spl-token --version
   ```

#### 🚨 Problèmes courants

**macOS : "command not found" après installation**
- Assurez-vous d'avoir bien rechargé votre configuration shell :
  ```bash
  source ~/.zshrc
  ```
- Ou fermez et rouvrez votre terminal

**Linux : Permission denied lors de l'installation de Cargo**
- N'utilisez jamais `sudo` pour installer des paquets Cargo
- Vérifiez que vous avez les droits sur le dossier `~/.cargo`

---

### ✅ Checkpoint Module 1

> **Note** : Ce checkpoint est valable pour **tous les systèmes d'exploitation**.

Vérifiez que toutes ces commandes fonctionnent :

```bash
# Affiche la version de Rust
rustc --version

# Affiche la version de Cargo
cargo --version

# Affiche la version de Solana
solana --version

# Affiche la version de SPL Token
spl-token --version
```

**Si tout fonctionne, bravo ! Vous avez un environnement de développement Solana opérationnel. 🎉**

---