
# Fraud Detection Project

## Présentation

Projet de détection de fraude bancaire utilisant le Machine Learning.

L'objectif est de détecter automatiquement les transactions suspectes à partir d'un historique de transactions de cartes bancaires.

---

## Dataset

Source : Kaggle - Credit Card Fraud Detection

Caractéristiques :

- 284 807 transactions
- 30 variables explicatives
- 492 fraudes
- Dataset fortement déséquilibré

---

## Approche

1. Chargement et contrôle qualité des données
2. Séparation Train / Test avec stratification
3. Régression Logistique
4. Random Forest
5. Optimisation du seuil de décision
6. Déploiement sous Streamlit

---

## Modèle retenu

Random Forest Classifier

Paramètres :

- n_estimators = 100
- class_weight = "balanced"
- random_state = 42

Seuil de décision :

- 0.30

---

## Résultats

Random Forest (seuil = 0.30)

- Precision fraude : 92.22 %
- Recall fraude : 84.69 %
- F1-score fraude : 88.30 %
- ROC-AUC : 95.29 %

Matrice de confusion :

[[56857, 7],
 [15, 83]]

---

## Structure du projet

fraud_detection_project/

├── app.py
├── fraud_detection_model.joblib
├── requirements.txt
└── README.md

---

## Technologies utilisées

- Python
- Pandas
- NumPy
- Scikit-Learn
- Joblib
- Streamlit

---

## Utilisation

1. Lancer l'application Streamlit
2. Charger un fichier CSV
3. Calculer la probabilité de fraude
4. Télécharger les résultats

---

## Auteur

SAKANDE Abdoul Aziz

Ingénieur Statisticien Économiste

Spécialisation :
- Data Science
- Machine Learning
- Trade Finance
- Risk Management
