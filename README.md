# Débits journaliers estimés par bassin versant

Ce projet fournit une estimation simplifiée des débits journaliers d’un cours d’eau à partir de données pluviométriques et de la délimitation du bassin versant. L’interface fonctionne entièrement dans un navigateur web.

---

## 1. Mode d’emploi

1. **Sélection du point d’étude :**
   - Cliquez sur la carte pour choisir l’exutoire (près d’un cours d’eau), ou saisissez directement les coordonnées (latitude et longitude).
   - Un marqueur indique le point choisi.

2. **Délimitation du bassin versant :**
   - Cliquez sur **“Délimiter le bassin versant”**.
   - L’API MERIT-Hydro (via Global Watersheds) calcule le polygone du bassin et son emprise.
   - La surface du bassin (km²) et la bounding box sont affichées dans la barre latérale.

3. **Paramètres pluie-débit :**
   - Choisissez la période de calcul (date de début et date de fin).
   - Définissez le coefficient de ruissellement \(C\) (entre 0 et 1).
   - Indiquez le débit de base \(Q_b\) en m³/s.
   - Sélectionnez le nombre de points d’échantillonnage pluie à l’intérieur du bassin.

4. **Calcul des débits :**
   - Cliquez sur **“Calculer les débits journaliers”**.
   - L’application récupère les précipitations journalières sur les points choisis via l’API Open-Meteo.
   - Le débit journalier moyen est calculé pour chaque jour de la période.

5. **Export CSV :**
   - Après calcul, un bouton permet de télécharger un fichier CSV avec :
     - Date
     - Pluie quotidienne moyenne (mm)
     - Débit journalier estimé (m³/s)

---

## 2. Formules utilisées pour les calculs

Pour chaque jour \(i\) :

1. **Moyenne spatiale de la pluie sur \(n\) points :**

\[
P_i = \frac{1}{n} \sum_{j=1}^{n} p_{i,j}
\]

où \(p_{i,j}\) est la pluie (mm) mesurée au point \(j\) le jour \(i\).

2. **Conversion pluie → volume ruisselé :**

\[
V_i = C \times P_i \times \frac{A}{1000}
\]

- \(V_i\) : volume ruisselé journalier (m³)  
- \(C\) : coefficient de ruissellement (0–1)  
- \(P_i\) : pluie moyenne journalière (mm)  
- \(A\) : surface du bassin (m²)  
- Division par 1000 pour convertir mm → m.

3. **Débit moyen journalier :**

\[
Q_i = \frac{V_i}{86400} + Q_b
\]

- \(Q_i\) : débit moyen journalier (m³/s)  
- \(V_i\) : volume ruisselé journalier (m³)  
- \(86400\) : nombre de secondes dans une journée  
- \(Q_b\) : débit de base (m³/s)

## 2.1 Schéma simplifié du calcul

      Pluie quotidienne P_i (mm)
                 │
                 ▼
     Moyenne spatiale sur n points
                 │
                 ▼
  Volume ruisselé V_i = C * P_i * A / 1000 (m³)
                 │
                 ▼
  Débit moyen journalier Q_i = V_i / 86400 + Qb (m³/s)

--------------------------------------

## 3. Sources des données

Délimitation du bassin versant :

Global Watersheds / MERIT-Hydro

API REST pour récupérer les polygones et les rivières en amont.

Pluviométrie :

Open-Meteo Archive API

Précipitations journalières historiques pour un point donné (latitude, longitude).

## 4. Notes importantes

Les estimations sont simplifiées, basées sur le produit 

C×pluie×surface, sans routage ni modélisation hydraulique détaillée.

Vérifiez vos résultats avec des données de terrain ou des stations hydrométriques locales si disponibles.

Les calculs ne prennent pas en compte les pertes d’infiltration, stockage temporaire ou retenues naturelles.

## 5. Licence

Code sous licence MIT.

