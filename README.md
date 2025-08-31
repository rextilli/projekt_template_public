<h1 align="center">Beispielhaftes README: Simulation & Klassifikation kosmischer Teilchen</h1>

---

## Inhaltsverzeichnis
1. [Datensatzübersicht](#-datensatzübersicht)  
   - [Erklärung](#erklärung)  
   - [Struktur](#struktur)  
2. [Experimentelle Setups](#-experimentelle-setups)  
3. [Klassifikator-Architektur](#-klassifikator-architektur)  
4. [Ergebnisse](#-ergebnisse)  
   - [Baseline](#baseline)  
   - [Jitter-Augmentation](#jitter-augmentation)  
   - [Vortraining: Methode A vs. Methode B](#vortraining-vergleich-methode-a-vs-methode-b)  
   - [GAN-Augmentation](#gan-gestützte-augmentation)  
   - [Einfaches Vortraining](#einfaches-vortraining)  
   - [Kombinierte Augmentationsmethoden](#kombinierte-augmentationsmethoden)  

---

## Datensatzübersicht

In diesem Projekt werden fiktive Datensätze nach folgender Struktur benannt:

{teilchenart}{umgebungsbedingung}{intensität}

markdown
Kopieren
Bearbeiten

Beispiel: `PRGA50`

### Erklärung

- **teilchenart**:  
  Dieses Repository fokussiert sich auf simulierte kosmische Teilchenstrahlung (`PR` = Protonenstrahlung).

- **umgebungsbedingung**:  
  Zwei Szenarien werden verwendet:
  - `GA`: Galaktische Umgebung (für **Vortraining** der Modelle).
  - `SO`: Sonnennähe (für **Feintuning** und **Evaluation**).

- **intensität**:  
  Verwendete Werte sind:  
  `{10, 20, 30, 40, 50}`

### Struktur

Jeder Datensatz (z. B. `PRGA50`) enthält ca. **5000 simulierte Ereignisse**.  
Jedes Ereignis ist:
- Eine Zeitreihe mit **2000 Datenpunkten**.
- Gespeichert als `.pkl` Datei mit einem einzelnen Label.
- Benannt nach dem Muster `PX_YYY.pkl`, wobei:  
  - `PX` (z. B. `P05`) das Label ist.  
  - `YYY` (z. B. `T300`) ein Identifier (nicht für Labels genutzt).

Die verwendeten Labels sind:
{P05, P10, P15, P20, P25, P30, P35, P40, P45, P50}

php-template
Kopieren
Bearbeiten

Diese Datensätze werden in unterschiedlichen Kombinationen für **Vortraining**, **Datenaugmentation** und **Evaluation** eingesetzt.

---

## Experimentelle Setups

Um zu untersuchen, wie sich die Menge an verfügbaren Labeldaten auf die Klassifikationsleistung auswirkt, werden die Datensätze in acht Trainingssplits aufgeteilt:

- `train_20`, `train_50`, `train_100`, `train_200`, `train_300`,  
  `train_400`, `train_500`, `train_600`

Jeder `train_k` Split ist **kumulativ** (enthält also alle Samples des vorherigen Splits).  
So lässt sich die **Skalierung der Modellleistung** bei wachsendem Datenvolumen untersuchen.

Ein spezieller Validierungssatz `val_1000` wird zur Performancebewertung verwendet.  
Dabei wird über mehrere Datensatz-Varianten (PRGA10–PRGA50) getestet, um Generalisierungsfähigkeit unter unterschiedlichen Intensitäten zu prüfen.

---

## Klassifikator-Architektur

Das Modell in allen Experimenten ist ein **1D-CNN (Convolutional Neural Network)** mit folgender Struktur:

- **Input**: Eindimensionales Signal mit Länge 2000.
- **Drei Convolution-Blöcke**:
  - Jeder Block: `Conv1D → BatchNorm → ReLU → MaxPool`.
  - Kanalprogression: 1 → 16 → 32 → 64.
- **Fully Connected Head**:
  - Flatten → Dense(128) → ReLU → Dropout(0.5) → Output Layer (10 Klassen).

Implementierung: **PyTorch**, Training mit:  
- Optimizer: Adam (`lr=1e-3`, `weight_decay=1e-5`)  
- Scheduler: StepLR (Decay alle 15 Epochen)  
- Loss: CrossEntropyLoss  
- Training: 50 Epochen (Basiswert, je nach Methode variiert)  
- Gradient Clipping: max norm = 1.0  
- Evaluation: Durchschnittliche Accuracy über 10 Seeds  

---

## Ergebnisse
<details>
  <summary id="baseline">Baseline</summary>
  <div align="center">
    <img src="https://i.imgur.com/gZGy95c.jpeg" alt="Baseline Ergebnis" style="max-width: 100%; height: auto;">
  </div>
</details>
<details>
  <summary id="jitter-augmentation">Jitter-Augmentation</summary>
  <div align="center">
    <img src="https://i.imgur.com/gqMoy77.jpeg" alt="Jitter Augmentation" style="max-width: 100%; height: auto;">
  </div>
</details>
<details>
  <summary id="vortraining-vergleich-methode-a-vs-methode-b">Vortraining: Vergleich Methode A vs. Methode B</summary>
  <div align="center">
    <img src="https://i.imgur.com/6oVkmdQ.jpeg" alt="Pretraining Methodenvergleich" style="max-width: 100%; height: auto;">
    <img src="https://i.imgur.com/ioQ8WmT.jpeg" alt="Diagramm 2" style="max-width: 100%; height: auto;">
  </div>
</details>
<details>
  <summary id="gan-gestützte-augmentation">GAN-gestützte Augmentation</summary>
  <div align="center">
    <img src="https://i.imgur.com/XZPKUgc.jpeg" alt="GAN Augmentation" style="max-width: 100%; height: auto;">
  </div>
</details>
<details>
  <summary id="einfaches-vortraining">Einfaches Vortraining</summary>
  <div align="center">
    <img src="https://i.imgur.com/CvEMc5p.jpeg" alt="Einfaches Vortraining" style="max-width: 100%; height: auto;">
  </div>
</details>
<details>
  <summary id="kombinierte-augmentationsmethoden">Kombinierte Augmentationsmethoden</summary>
  <div align="center">
    <img src="https://i.imgur.com/JsZxvxw.jpeg" alt="Kombinierte Methoden" style="max-width: 100%; height: auto;">
  </div>
</details>

