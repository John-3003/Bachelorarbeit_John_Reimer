# Modellreduktion für dynamische Systeme mittels Autoencodern

Dieses Projekt enthält den Python-Code zu meiner Bachelorarbeit. Untersucht wird, wie die Modellkonfiguration, die Trainingsstrategie und der Vorhersagehorizont die Rekonstruktions- und Vorhersageleistung eines Autoencoders beeinflussen.

Als Datengrundlage dienen numerisch berechnete Trajektorien des dreidimensionalen Lorenz-Systems. Aus jedem Lorenz-Zustand wird mithilfe von sechs Legendre-Polynomen eine künstliche 128-dimensionale Beobachtung erzeugt. Der Encoder bildet diese Beobachtung auf einen dreidimensionalen latenten Zustand ab. Der Decoder rekonstruiert daraus die ursprüngliche Beobachtung. Für die Vorhersage wird der Autoencoder um ein neuronales Netz erweitert, das die Änderung des latenten Zustands für den nächsten Zeitschritt schätzt.

## Übersicht der Notebooks

| Notebook | Inhalt |
| --- | --- |
| `Rekonstruktion_Lorenz-Daten.ipynb` | **Modell A – Rekonstruktion.** Trainiert einen Autoencoder zur Rekonstruktion der 128-dimensionalen Beobachtungen. Das Notebook dient dem Vergleich verschiedener Netzarchitekturen und Aktivierungsfunktionen. |
| `Lorenz_Ein-Schritt-Vorhersage-Gem_Train.ipynb` | **Modell B – gemeinsames Training.** Encoder, Decoder und Dynamikmodell werden gemeinsam für die Rekonstruktion und die Ein-Schritt-Vorhersage trainiert. |
| `Lorenz_Ein-Schritt-Vorhersage_Getr_Trai.ipynb` | **Modell C – getrenntes Training.** Zunächst werden Encoder und Decoder für die Rekonstruktion trainiert. Anschließend bleiben ihre Gewichte fest, während das Dynamikmodell die Ein-Schritt-Vorhersage lernt. |
| `Lorenz_Mehr-Schritt-Vorhersage.ipynb` | Trainiert das Modell auf einem Zeitschritt und wertet es über mehrere Vorhersageschritte aus. Beim Teacher Forcing beginnt jeder Folgeschritt mit der codierten tatsächlichen Beobachtung. Bei der autoregressiven Vorhersage verwendet das Modell seinen zuvor vorhergesagten latenten Zustand. |
| `Graph_Mehr-Schritt-Vorhersage.ipynb` | Stellt die im Notebook hinterlegten MSE-Werte über dem Vorhersagehorizont dar. Eine weitere Abbildung zeigt die sechs Legendre-Polynome, die zur Erzeugung der Beobachtungen verwendet werden. |

## Ausführung

Benötigt werden Python, Jupyter Notebook sowie die Pakete NumPy, SciPy, PyTorch und Matplotlib. Sie lassen sich mit folgendem Befehl installieren:

    python -m pip install notebook numpy scipy torch matplotlib

Jupyter lässt sich im Projektordner mit `jupyter notebook` starten. Anschließend das gewünschte Notebook öffnen und die Zellen von oben nach unten ausführen. Die Modell-Notebooks erzeugen ihre Trainings- und Validierungsdaten selbst und können unabhängig voneinander ausgeführt werden.

## Einstellungen und Auswertung

- `SEED` legt den Startwert für die verwendeten Zufallszahlengeneratoren fest.
- `NOISE_STRENGTH` steuert in den Vorhersage-Notebooks die Standardabweichung des addierten gaußschen Rauschens auf den Trainingseingaben. Mit `0.0` wird ohne zusätzliches Rauschen trainiert.
- `HORIZON` legt im Notebook zur Mehr-Schritt-Vorhersage fest, über wie viele Schritte das Modell ausgewertet wird. In der vorliegenden Konfiguration sind es 30 Schritte.

Als Fehlermaß dient der mittlere quadratische Fehler (MSE). Bei der Rekonstruktion werden die rekonstruierte und die tatsächliche Beobachtung desselben Zeitpunkts verglichen. Bei der Vorhersage wird der vorhergesagte Zustand mit dem zugehörigen Zielzustand am jeweiligen Vorhersagezeitpunkt verglichen.
