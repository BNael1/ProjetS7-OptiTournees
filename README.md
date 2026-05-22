# OptiTournées — Planificateur de tournées (TSP)

> Projet S7 — Master 1 Informatique, Université de Lorraine (2025-2026)
> UE *Analyse et Conception Logicielle* + UE *Structures de Données et Algorithmes*

Application client-serveur de **planification de tournées** sur les régions de France.
L'utilisateur choisit une région, l'application calcule un parcours optimisé reliant ses villes
en s'appuyant sur plusieurs stratégies de résolution du problème du voyageur de commerce (TSP).

## Rapports

- [Rapport_ACL.pdf](Rapport_ACL.pdf) — rapport principal (partie ACL + survol de la partie algorithmique).
- [Rapport_Algo.pdf](Rapport_Algo.pdf) — rapport détaillé de la partie algorithmique (UE SDA).

## Aperçu

- **Client Java (Swing)** — sélection de la région, visualisation de la tournée sur fond cartographique.
- **Serveur C++** — calcule la tournée optimale, communique avec le client via socket TCP (port 8080).
- **Bibliothèque de graphes générique** — conçue en C++ moderne avec templates (`PElement<T>`, `AElement`, `Graphe`).
- **Stratégies TSP interchangeables** — patron *Strategy* : distance, temps, et heuristiques.
- **Données réelles** — coordonnées GPS des principales villes de chaque région française au format JSON.

## Stack technique

| Couche       | Technologies                                          |
|--------------|-------------------------------------------------------|
| Serveur      | C++17, sockets POSIX, `nlohmann/json`                 |
| Algorithmes  | C (programmation dynamique, FPTAS), liés au serveur via `extern "C"` |
| Client       | Java 17, Swing                                        |
| Build        | Make (par module), CMake (build global)               |
| Doc          | Doxygen                                               |

## Architecture

```
Projet_ACL/
├── Partie_ACL/                  # Conception logicielle (UE ACL)
│   ├── cpp/                     # Serveur C++ + bibliothèque graphes
│   │   ├── Graphes/             # Graphe générique (templates)
│   │   ├── Modeles/             # Ville, modèles métier
│   │   ├── Outils/              # JSON, CSV, calculs géo
│   │   ├── lib/                 # Stratégies TSP, sélecteurs, parseurs
│   │   └── Serveur.cpp          # Point d'entrée serveur
│   ├── java/                    # Client Swing
│   │   └── app/src/
│   │       ├── modeles/         # Modèle métier côté client
│   │       └── ui/              # PanneauGraphe, PanneauControle, ...
│   └── Assets/Region/           # GeoJSON des 13 régions FR
├── Partie_Algo/                 # Algorithmes (UE SDA)
│   └── code/                    # FPTAS, algo dynamique, scheduling
├── Rapport_ACL.pdf              # Rapport principal
└── Rapport_Algo.pdf             # Rapport partie algorithmique
```

## Compilation & exécution

### Serveur C++

```bash
cd Partie_ACL/cpp
make                 # compile tous les binaires dans bin/
./bin/Serveur        # lance le serveur (port 8080)
```

Commandes utiles :

| Commande                       | Description                            |
|--------------------------------|----------------------------------------|
| `make`                         | Compile tous les tests et le serveur   |
| `make bin/<nom>`               | Compile un test précis                 |
| `make debug`                   | Recompile en mode debug (`-g -O0`)     |
| `make run prog=<nom>`          | Exécute un test                        |
| `make valgrind prog=<nom>`     | Valgrind complet                       |
| `make valgrind-lite prog=<nom>`| Valgrind allégé                        |
| `make clean`                   | Nettoie les binaires                   |

### Client Java

```bash
cd Partie_ACL/java
java -cp out/production/Projet_ACL src.Main
```

> Si lancé via le script, ajuster `REGION_DIR = "Partie_ACL/Assets/Region/"` dans `RegionUtils`.

## Auteurs

Projet réalisé en équipe — M1 Informatique, Université de Lorraine.

- MORLET Hugo
- CASTRO Alexis
- SABATIER Guillaume
- RAHALI Linda
- BENSAADI Naël
