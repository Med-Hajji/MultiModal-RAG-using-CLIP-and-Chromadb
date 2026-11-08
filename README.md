# MultiModal RAG using CLIP and ChromaDB

RAG multimodal (images) avec **CLIP**, **ChromaDB** et **Gemini**.

Le projet montre comment construire un système de recherche et de question-réponse sur une collection d'images de voitures (différentes marques et différentes couleurs). On interroge la base en langage naturel, ou avec une image, et un LLM multimodal répond à partir des images les plus proches retrouvées.

## Principe

Un RAG classique indexe du texte. Ici, on indexe des **images** :

1. **Embeddings** : CLIP encode les images et le texte dans le même espace vectoriel. Une requête comme « une voiture rouge » se retrouve donc proche des images de voitures rouges.
2. **Stockage** : les embeddings des images sont enregistrés dans une base vectorielle **ChromaDB** (dossier `vehicules-store-vdb`).
3. **Retrieval** : pour une question, on calcule son embedding CLIP et on récupère les _k_ images les plus similaires.
4. **Génération** : les images retrouvées et la question sont envoyées à **Gemini** (`gemini-3.8-flash`), qui génère la réponse.

```
Question ──► CLIP ──► ChromaDB ──► images similaires ──► Gemini ──► Réponse
                         ▲
Dossier d'images ──► CLIP (indexation)
```

## Structure du projet

```
MultiModal-RAG-using-CLIP-and-Chromadb/
├── Vehicules/            # images de voitures (marques et couleurs variées)
├── vehicules-store-vdb/  # base ChromaDB (générée)
├── rag-mm.ipynb          # notebook principal
├── requirements.txt
├── .env                  # clé API (non versionné)
└── README.md
```

## Installation

```powershell
git clone https://github.com/Med-Hajji/MultiModal-RAG-using-CLIP-and-Chromadb.git
cd MultiModal-RAG-using-CLIP-and-Chromadb

python -m venv venv
venv\Scripts\activate        # Windows
# source venv/bin/activate   # Linux / macOS

pip install -r requirements.txt
```

## Configuration

Créez un fichier `.env` à la racine du projet, avec votre clé Gemini (obtenue sur [Google AI Studio](https://aistudio.google.com/)) :

```
GEMINI_API_KEY=votre_cle_ici
```

Le fichier `.env` est dans le `.gitignore` : ne le publiez jamais.

## Utilisation

1. Placez vos images dans le dossier `Vehicules/`.
2. Ouvrez `rag-mm.ipynb` et exécutez les cellules dans l'ordre :
   - chargement des images et calcul des embeddings CLIP ;
   - création de la collection ChromaDB et ajout des images ;
   - recherche par texte ou par image ;
   - génération de la réponse avec Gemini.

### Exemples de requêtes

- « Montre-moi les voitures rouges »
- « Quelles voitures de la marque X as-tu ? »
- « Trouve des voitures similaires à cette image »

## Technologies

- [CLIP](https://openai.com/research/clip) : embeddings image et texte
- [ChromaDB](https://www.trychroma.com/) : base de données vectorielle
- [Gemini](https://ai.google.dev/) : LLM multimodal
- [LangChain](https://www.langchain.com/) : orchestration
- Python, Jupyter Notebook

## Limites et pistes d'amélioration

- CLIP reconnaît bien les couleurs et les types d'objets, mais il est moins précis pour distinguer des modèles très proches d'une même marque.
- Ajouter des métadonnées (marque, couleur, année) dans ChromaDB pour filtrer les résultats.
- Construire une interface avec Streamlit ou Gradio.
- Évaluer la qualité du retrieval (precision@k) sur un jeu de requêtes de test.

## Licence

Projet réalisé à des fins pédagogiques. Ajoutez la licence de votre choix (par exemple MIT).
