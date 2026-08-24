
import streamlit as st
import pandas as pd
import joblib

# Configuration de la page
st.set_page_config(
    page_title="Détection de fraude",
    page_icon="🔍",
    layout="wide"
)

# Titre de l'application
st.title("Détection de fraude bancaire")

st.write(
    "Cette application estime le risque de fraude "
    "à partir des caractéristiques d'une transaction."
)

st.warning(
    "Outil d'aide à la décision. "
    "Toute alerte doit faire l'objet d'une validation humaine."
)

# Chargement du modèle
@st.cache_resource
def charger_modele():
    artefact = joblib.load("fraud_detection_model.joblib")

    return (
        artefact["model"],
        artefact["threshold"],
        artefact["features"]
    )


modele, seuil, variables = charger_modele()

st.success("Modèle chargé avec succès.")

# Informations sur le modèle
col1, col2, col3 = st.columns(3)

col1.metric(
    label="Modèle",
    value="Random Forest"
)

col2.metric(
    label="Seuil d'alerte",
    value=f"{seuil:.0%}"
)

col3.metric(
    label="Nombre de variables",
    value=len(variables)
)

st.subheader("Analyser un fichier de transactions")

fichier = st.file_uploader(
    "Sélectionnez un fichier CSV",
    type=["csv"]
)

if fichier is not None:

    donnees = pd.read_csv(fichier)

    st.write("Aperçu des données")
    st.dataframe(donnees.head())

    colonnes_manquantes = [
        colonne
        for colonne in variables
        if colonne not in donnees.columns
    ]

    if colonnes_manquantes:

        st.error(
            "Colonnes manquantes : "
            + ", ".join(colonnes_manquantes)
        )

    else:

        # Conserver les variables dans l'ordre attendu
        X_nouvelles = donnees[variables].copy()

        # Calcul des probabilités de fraude
        probabilites = modele.predict_proba(
            X_nouvelles
        )[:, 1]

        # Application du seuil métier
        predictions = (
            probabilites >= seuil
        ).astype(int)

        # Ajouter les résultats
        resultats = donnees.copy()

        resultats["Probabilite_fraude"] = probabilites
        resultats["Prediction"] = predictions

        resultats["Niveau_risque"] = pd.cut(
            resultats["Probabilite_fraude"],
            bins=[-0.01, 0.10, seuil, 0.70, 1.00],
            labels=[
                "Faible",
                "Modéré",
                "Élevé",
                "Très élevé"
            ]
        )

        nombre_alertes = int(
            resultats["Prediction"].sum()
        )

        st.subheader("Résultats")

        col1, col2, col3 = st.columns(3)

        col1.metric(
            "Transactions analysées",
            len(resultats)
        )

        col2.metric(
            "Alertes déclenchées",
            nombre_alertes
        )

        col3.metric(
            "Taux d'alerte",
            f"{nombre_alertes / len(resultats):.2%}"
        )

        st.dataframe(resultats)

        fichier_resultat = resultats.to_csv(
            index=False
        ).encode("utf-8")

        st.download_button(
            label="Télécharger les résultats",
            data=fichier_resultat,
            file_name="resultats_fraude.csv",
            mime="text/csv"
        )
