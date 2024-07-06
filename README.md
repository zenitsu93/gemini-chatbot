# Agent conversationnel Gemini Pro

Une application de chat en Streamlit adossée à Gemini Pro, avec historique de session.

Le dépôt est petit mais structuré : l'appel au modèle est isolé dans une couche de services, au lieu d'être noyé dans le fichier de l'interface.

## Organisation

```
app.py                           interface Streamlit et gestion de l'historique
config/globals.py                rôles de conversation et amorce initiale
services/google/generative_ai.py encapsulation de l'appel à Gemini
```

Cette séparation a un intérêt concret : changer de fournisseur de modèle ne touche qu'un seul fichier. L'interface ne connaît qu'une méthode d'envoi de message, pas le SDK qu'il y a derrière.

L'historique est conservé dans l'état de session Streamlit, ce qui permet au modèle de tenir compte des échanges précédents plutôt que de traiter chaque message isolément.

## Mise en route

```bash
pip install -r requirements.txt
```

La clé se place dans un fichier `.env` :

```
GOOGLE_API_KEY=votre_clé
```

Puis :

```bash
streamlit run app.py
```

## Limites

L'historique vit dans la session du navigateur : il disparaît au rechargement de la page. Pour le conserver, il faudrait le persister.

Et comme tout historique transmis en entier à chaque tour, il finit par saturer la fenêtre de contexte sur une longue conversation. Une troncature ou un résumé glissant serait la suite logique.
