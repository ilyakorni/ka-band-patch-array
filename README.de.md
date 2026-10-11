<p align="center">
  <a href="README.md">Русский</a> | <a href="README.en.md">English</a> | <b>Deutsch</b>
</p>

# Breitbandige aperturgekoppelte gestapelte Mikrostreifen-Patchantenne mit zirkularer Polarisation für das Ka-Band (28–30 GHz)

Dieses Repository dokumentiert die Auslegung, die analytische Berechnung, CAD-Modelle sowie die dreidimensionale elektromagnetische Simulation einer breitbandigen zirkular polarisierten Mikrostreifen-Patchantenne für das Ka-Band im Frequenzbereich 28.0–30.0 GHz. Die Antenne basiert auf einer schlitzgekoppelten Aperturstruktur (Cross-Slot) in Verbindung mit einem gestapelten parasitären Resonator (Stacked Patch) zur Breitbandanpassung und Vergrößerung der Zirkularpolarisationsbandbreite.

## Inhaltsverzeichnis

[Projektübersicht](#projektübersicht)<br>
[Projektstruktur](#projektstruktur)<br>
[Analytische Berechnung](#analytische-berechnung)<br>
[3D-Modellierung und Optimierung](#3d-modellierung-und-optimierung)<br>
[Ergebnisse der elektromagnetischen Simulation](#ergebnisse-der-elektromagnetischen-simulation)<br>
[Lizenz](#lizenz)

## Projektübersicht

Die Antenne ist für breitbandige Anwendungen im Ka-Band (28.0–30.0 GHz) konzipiert, darunter Millimeterwellen-Satellitenverbindungen, Radarsysteme und drahtlose Hochfrequenz-Frontends.

Wesentliche Architekturmerkmale:

Topologie: Mehrlagige gestapelte Mikrostreifen-Patchantenne (Stacked Patch).<br>
Anregungsmechanismus: Berührungslose elektromagnetische Aperturkopplung über einen kreuzförmigen Schlitz (Cross-Slot) in der Massefläche, gespeist durch eine Mikrostreifenleitung.<br>
Anpassung: Zentrierte leerlaufende Lambda/Viertel-Anpassungsleitung ($\lambda_g/4$ Tuning Stub), platziert unmittelbar über dem Aperturkreuz zur Kompensation der Schlitzreaktanz.<br>
Zirkulare Polarisation: Diagonale Eckenabschrägungen (Corner Truncation, $45^\circ$) auf den Resonatorelementen zur Anregung zweier orthogonaler Moden ($TM_{10}$ und $TM_{01}$) in Quadratur ($90^\circ$ Phasenverschiebung).<br>
Bandbreitenvergrößerung: Ein über dem Hauptstrahler angeordneter parasitärer Resonator zur Erzeugung eines doppelresonanten Frequenzverhaltens im Bereich 28.0–30.0 GHz.

## Projektstruktur

Die Struktur des Repositories gliedert sich wie folgt:

```
├── .gitignore
├── README.md
├── README.en.md
├── README.de.md
├── cad/
│   ├── antenna_enclosure_base.step            # Antennengehäuse: Gehäuseunterteil
│   ├── antenna_enclosure_cover.step           # Antennengehäuse: Radom-Abdeckung
│   ├── antenna_pcb_layout.pcbdoc              # Leiterplatten-Entwurfsprojekt (Altium Designer)
│   ├── antenna_pcb_odbpp.zip                  # Fertigungsdaten im ODB++-Format
│   ├── pcb_driven_patch.step                  # 3D-STEP-Modell: Untere Leiterplatte (Speiseleitung und Hauptpatch)
│   ├── pcb_parasitic_patch.step               # 3D-STEP-Modell: Obere Leiterplatte (parasitärer Patch)
│   └── sma_edgemount_connector.step           # 3D-STEP-Modell: SMA End-Launch HF-Steckverbinder
├── calculations/
│   └── patch_antenna_analytical_calc.xmcd     # Mathematische Auslegung und Leitungsberechnung (Mathcad 14)
├── docs/
│   └── images/
│       ├── axial_ratio_3d_pattern_29ghz.png   # 3D-Diagramm des Achsenverhältnisses (Axial Ratio)
│       ├── axial_ratio_polar_29ghz.png        # Polardiagramm des Achsenverhältnisses (Schnitt Phi=90°)
│       ├── parasitic_patch_geometry.png       # Geometrische Abmessungen des oberen parasitären Resonators
│       ├── patch_truncation_geometry.png      # Geometrische Abmessungen des unteren Resonators und Abschrägungen
│       ├── simulation_s11_band_edge_lower.png # Untere Grenzfrequenz für S11 = -10 dB (27.94 GHz)
│       ├── simulation_s11_band_edge_upper.png # Obere Grenzfrequenz für S11 = -10 dB (29.58 GHz)
│       ├── simulation_s_parameters_s11.png    # Frequenzverlauf des Reflexionsfaktors S11
│       ├── simulation_vswr.png                # Stehwellenverhältnis (VSWR)
│       └── simulation_z_parameters.png        # Eingangsimpedanz Z11 (Real- und Imaginärteil)
└── simulation/
    └── cp_stacked_patch_antenna_29ghz.cst     # Elektromagnetisches 3D-Simulationsmodell (CST Studio Suite)
```

## Analytische Berechnung

Die mathematische Vordimensionierung erfolgte in Mathcad ([patch_antenna_analytical_calc.xmcd](calculations/patch_antenna_analytical_calc.xmcd)):

### 1. Substratparameter
Relative Permittivität: $\varepsilon_r = 2.33$<br>
Substratdicke: $h = 0.254$ mm (10 mil)<br>
Dicke der Kupferkaschierung: $t = 35\ \mu\text{m}$<br>
Entwurfsfrequenzbereich: $f_1 = 28.0$ GHz, $f_2 = 30.0$ GHz, Mittenfrequenz $f_0 = 29.0$ GHz

### 2. Abmessungen des rechteckigen Grundresonators
Berechnete Breite:
$$W = \frac{c}{2 f_0} \sqrt{\frac{2}{\varepsilon_r + 1}} \approx 4.01\text{ mm}$$
Streufeld-Längenkorrektur: $\Delta L \approx 0.129$ mm.<br>
Effektive Resonanzlänge: $L_{\text{eff}} \approx 3.13$ mm.

### 3. Geometrie der Kreuzschlitzapertur (Cross-Slot)
Schlitzbreite: $W_{\text{cross}} = 1.0$ mm.<br>
Resonante Schlitzlänge: $L_{1\text{cross}} = L_{2\text{cross}} \approx 1.69$ mm ($\approx \lambda_g / 4$).

### 4. Mikrostreifen-Speiseleitung und Anpassungsstichleitung
Quasistatische effektive Permittivität (Modell nach Wheeler & Schneider): $\varepsilon_{\text{eff},0} = 1.961$.<br>
Frequenzdispersionskorrektur im Hochfrequenzbereich (Modell nach Kobayashi): $\varepsilon_{\text{eff}}(29\text{ GHz}) = 1.997$.<br>
Leiterdickenkorrektur (Wheeler): $\Delta W_2 \approx 0.215$ mm.<br>
Lambda/Viertel-Anpassungsstichleitung: Länge $L_t = \lambda_g / 4 \approx 1.83$ mm, Leitungswellenlänge $\lambda_g \approx 7.32$ mm, Leiterbahnbreite $W_t \approx 0.54$ mm für $Z_0 = 50\ \Omega$.

<p align="center">
  <img src="docs/images/patch_truncation_geometry.png" width="500" alt="Resonator- und Abschrägungsgeometrie">
  <br>
  <em>Abb. 1. Layout des quadratischen unteren Resonators: Kantenlänge 2.65 mm, Eckenabschrägung 0.35 mm.</em>
</p>

## 3D-Modellierung und Optimierung

Die numerische elektromagnetische Simulation und Optimierung wurden in CST Studio Suite durchgeführt ([cp_stacked_patch_antenna_29ghz.cst](simulation/cp_stacked_patch_antenna_29ghz.cst)). Der iterative Optimierungsprozess umfasste folgende Schritte:

### 1. Grundkonfiguration des Einzelpatch-Strahlers
Aufbau der Mikrostreifenleitung mit zentrierter Lambda/Viertel-Stichleitung über dem Aperturkreuz zur Kompensation der Schlitzreaktanz.

### 2. VSWR-Anpassung
Parametrische Feinabstimmung der Resonatorabmessungen zur Optimierung der Anpassung im Betriebsband.

### 3. Erzeugung der zirkularen Polarisation
Einbringen diagonaler Eckenabschrägungen mit einer Kantenlänge von $\Delta c = 0.35$ mm auf dem quadratischen Resonator (2.65 mm $\times$ 2.65 mm). Dies bewirkt eine Modenaufspaltung in zwei zueinander orthogonale Feldkomponenten mit $90^\circ$ Phasenverschiebung.

### 4. Bandbreitenerweiterung mittels parasitärem Resonator (Stacked Patch)
Integration einer zweiten Substratschicht mit einer Dicke von $h_2 = 0.203$ mm mit einem parasitären Patch über dem Hauptstrahler:

Die Abmessungen des quadratischen parasitären Patches betragen 2.01 mm $\times$ 2.01 mm.<br>
Diagonale Eckenabschrägungen des oberen Elements betragen $\Delta c_{\text{top}} = 0.29$ mm unter $45^\circ$, was die Erhaltung der Zirkularpolarisation auf beiden Resonanzmoden gewährleistet.<br>
Die Resonatorkopplung erweitert die Anpassungsbandbreite ($S_{11} < -10$ dB) sowie die Bandbreite des Achsenverhältnisses ($\text{AR} < 3$ dB) für das gesamte Frequenzband 28.0–30.0 GHz.

<p align="center">
  <img src="docs/images/parasitic_patch_geometry.png" width="500" alt="Geometrie des oberen parasitären Resonators">
  <br>
  <em>Abb. 2. Layout des oberen parasitären Strahlers: Kantenlänge 2.01 mm, Abschrägung 0.29 mm, Substratdicke 0.203 mm.</em>
</p>

<p align="center">
  <img src="docs/images/axial_ratio_3d_pattern_29ghz.png" width="650" alt="3D-Modell und räumliches Achsenverhältnis">
  <br>
  <em>Abb. 3. 3D-Simulationsmodell und räumliche Verteilung des Achsenverhältnisses.</em>
</p>

## Ergebnisse der elektromagnetischen Simulation

### 1. Reflexionsfaktor ($S_{11}$) und Anpassungsbandbreite
Im Breitbandbereich von 28.0–30.0 GHz weist die Antenne eine zuverlässige Anpassung auf.<br>
Bei 29.0 GHz beträgt der Reflexionsfaktor $S_{11} = -13.24$ dB.<br>
Das Resonanzminimum erreicht $-21.5$ bis $-28.0$ dB im Durchlassbereich.

Grenzfrequenzen für die -10 dB-Anpassung:

Untere Grenzfrequenz: $f_{\text{low}} = 27.938$ GHz bei $S_{11} = -10.28$ dB.<br>
Obere Grenzfrequenz: $f_{\text{high}} = 29.576$ GHz bei $S_{11} = -10.58$ dB.<br>
Betriebsbandbreite bei -10 dB: $\Delta f = 1.638$ GHz (Bereich 27.94–29.58 GHz).

<p align="center">
  <img src="docs/images/simulation_s_parameters_s11.png" width="700" alt="S-Parameter S11 Breitband">
  <br>
  <em>Abb. 4. Frequenzverlauf des Reflexionsfaktors S11 im weiten Frequenzbereich.</em>
</p>

<p align="center">
  <img src="docs/images/simulation_s11_band_edge_lower.png" width="700" alt="Untere Bandgrenze S11">
  <br>
  <em>Abb. 5. Untere Grenzfrequenz bei S11 = -10 dB (Marker 27.938 GHz, -10.28 dB).</em>
</p>

<p align="center">
  <img src="docs/images/simulation_s11_band_edge_upper.png" width="700" alt="Obere Bandgrenze S11">
  <br>
  <em>Abb. 6. Obere Grenzfrequenz bei S11 = -10 dB (Marker 29.576 GHz, -10.58 dB).</em>
</p>

### 2. Stehwellenverhältnis (VSWR)
Bei 29.0 GHz liegt das Stehwellenverhältnis bei 1.56. Im gesamten Betriebsband unterschreitet das VSWR den Grenzwert von 2.0 und erreicht ein Minimum von 1.08 im optimalen Anpassungsbereich.

<p align="center">
  <img src="docs/images/simulation_vswr.png" width="700" alt="Stehwellenverhältnis VSWR">
  <br>
  <em>Abb. 7. Frequenzverlauf des Stehwellenverhältnisses (VSWR).</em>
</p>

### 3. Eingangsimpedanz ($Z_{11}$)
Bei 29.0 GHz beträgt die Eingangsimpedanz $\mathrm{Re}(Z_{11}) = 32.66\ \Omega$ und $\mathrm{Im}(Z_{11}) = 4.92\ \Omega$. Der vernachlässigbare Blindwiderstand bestätigt den Parallelresonanzbetrieb des gekoppelten Netzwerks.

<p align="center">
  <img src="docs/images/simulation_z_parameters.png" width="700" alt="Eingangsimpedanz Z11">
  <br>
  <em>Abb. 8. Real- und Imaginärteil der Eingangsimpedanz Z11 über der Frequenz.</em>
</p>

### 4. Achsenverhältnis (Axial Ratio / Polarisation)
In Hauptstrahlrichtung ($\theta = 0^\circ$) beträgt das Achsenverhältnis bei 29.0 GHz etwa 1.0 dB, was den Grenzwert für zirkulare Polarisation von 3.0 dB erfüllt. Das Achsenverhältnis verbleibt unter 3.0 dB über einen weiten Winkelbereich von $|\theta| \le 30^\circ$.

<p align="center">
  <img src="docs/images/axial_ratio_polar_29ghz.png" width="650" alt="Polardiagramm des Achsenverhältnisses">
  <br>
  <em>Abb. 9. Polardiagramm des Achsenverhältnisses (Schnitt Phi=90°) bei 29 GHz.</em>
</p>

## Lizenz

Copyright (c) 2026 Ilya Kornilov

Dieses Dokument beschreibt Open Hardware und ist unter der CERN-OHL-P v2 lizenziert. 
Sie dürfen diesen Quellcode unter den Bedingungen der CERN-OHL-P v2 (https://cern.ch/cern-ohl) 
weitergeben, modifizieren und Produkte auf dessen Grundlage herstellen.

Dieser Quellcode wird OHNE JEGLICHE AUSDRÜCKLICHE ODER STILLSCHWEIGENDE GEWÄHRLEISTUNG 
vertrieben, EINSCHLIESSLICH DER STILLSCHWEIGENDEN GEWÄHRLEISTUNG DER MARKTGÄNGIGKEIT, 
ZUFRIEDENSTELLENDEN QUALITÄT UND EIGNUNG FÜR EINEN BESTIMMTEN ZWECK. 
Genaue Bestimmungen entnehmen Sie bitte der CERN-OHL-P v2.
