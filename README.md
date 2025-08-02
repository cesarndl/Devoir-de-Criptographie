
# TP de Cryptographie et Réseaux 2e Semestre
Cesar Ndala

## Partie 1: Pré-requis et Calculs

### 1. Calculs initiaux pour un Sous-Réseau /28

**a. Masque de sous-réseau en notation décimale pointée pour un /28:**
- Un masque /28 signifie que les 28 premiers bits sont à 1
- En binaire: 11111111.11111111.11111111.11110000
- Conversion en décimal: 255.255.255.240

**b. Bits alloués:**
- Partie réseau: 28 bits (24 bits du réseau principal + 4 bits de sous-réseau)
- Partie hôte: 4 bits (32 bits total - 28 bits réseau)

**c. Nombre total d'adresses IP par sous-réseau:**
- 2^4 = 16 adresses par sous-réseau

**d. Nombre maximal d'hôtes utilisables par sous-réseau:**
- 2^4 - 2 = 14 hôtes (on soustrait l'adresse réseau et l'adresse de broadcast)

### 2. Détermination des Plages d'Adresses IP

Tableau des quatre premiers sous-réseaux:

| Sous-réseau | Adresse réseau | Première IP utilisable | Dernière IP utilisable | Adresse broadcast |
|-------------|----------------|------------------------|------------------------|-------------------|
| 1           | 192.178.12.0   | 192.178.12.1           | 192.178.12.14          | 192.178.12.15     |
| 2           | 192.178.12.16  | 192.178.12.17          | 192.178.12.30          | 192.178.12.31     |
| 3           | 192.178.12.32  | 192.178.12.33          | 192.178.12.46          | 192.178.12.47     |
| 4           | 192.178.12.48  | 192.178.12.49          | 192.178.12.62          | 192.178.12.63     |

## Partie 2: Conception et Implémentation du Réseau

### 1. Plan d'adressage IP corrigé

Conversion des adresses binaires en décimales:

| N° | Type de machine | Adresse IP (binaire) | Adresse IP (décimal) | Valide dans 192.178.12.0/28? |
|----|------------------|-----------------------|----------------------|------------------------------|
| 01 | Laptop           | 11000000.1010010.0000100.00000000 | 192.178.12.0 | Non (adresse réseau) |
| 02 | Desktop          | 11000000.10110010.00001100.00000001 | 192.178.12.1 | Oui |
| 03 | Desktop          | 11000000.10110010.00001100.00000011 | 192.178.12.3 | Oui |
| 04 | Desktop          | 11000000.10110010.00001100.00000110 | 192.178.12.6 | Oui |
| 05 | Laptop           | 11000000.10110010.00001100.00001011 | 192.178.12.11 | Oui |
| 06 | Imprimante       | 11000000.10110010.00001100.00010001 | 192.178.12.17 | Non (hors plage 0-15) |
| 07 | Serveur          | 11000000.10110010.00001100.00001101 | 192.178.12.13 | Oui |
| 08 | Serveur          | 11000000.10110010.00001100.00001011 | 192.178.12.11 | Oui (identique au N°05) |
| 09 | Laptop           | 11000000.10110010.00001100.00001110 | 192.178.12.14 | Oui |
| 10 | Imprimante       | 11000000.10110010.00001100.00001111 | 192.178.12.15 | Non (adresse broadcast) |

### 2. Erreurs détectées:

1. **Machine N°01 (192.178.12.0)**: Adresse réseau, non utilisable pour un hôte
2. **Machine N°06 (192.178.12.17)**: Hors plage du sous-réseau 192.178.12.0/28 (doit être entre 0-15)
3. **Machine N°10 (192.178.12.15)**: Adresse broadcast, non utilisable pour un hôte
4. **Conflit d'adresses**: N°05 et N°08 ont la même adresse IP (192.178.12.11)

### 3. Machine infiltrée:

La machine N°06 avec l'adresse 192.178.12.17 n'appartient pas au réseau 192.178.12.0/28.

**Réseau correct pour cette machine:**
- L'adresse 192.178.12.17 appartient au sous-réseau 192.178.12.16/28
- Adresse réseau: 192.178.12.16
- Adresse broadcast: 192.178.12.31

### 4. Cryptage du fichier:

Pour décrypter le fichier texte, nous avons besoin de:
1. **Dernier octet en binaire de l'adresse broadcast de la machine infiltrée**:
   - Adresse broadcast: 192.178.12.31 → dernier octet: 31 → en binaire: 00011111

2. **Deux premières lettres de l'adresse MAC de l'ordinateur ayant l'IP 182.216.58.64**:
   - Cette information n'est pas fournie dans l'énoncé (nécessite le fichier Wireshark mentionné)

La clé de décryptage serait donc la concaténation de "00011111" avec les deux premiers caractères de l'adresse MAC (qui ne sont pas disponibles ici).

### Recommandations pour le réseau:
1. Réattribuer des adresses valides aux machines N°01, N°06 et N°10
2. Changer l'adresse du N°05 ou N°08 pour résoudre le conflit
3. Utiliser des adresses entre 192.178.12.1 et 192.178.12.14 pour les hôtes
4. Vérifier que toutes les adresses sont uniques avant la configuration