# Sinus-Milieu
Workflow-Dokumentation  zur Geodatenverarbeitung von Sinus-Milieu-Daten in Kombination mit ALKIS-Adressen in Python, Zitation unter:

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.14886797.svg)](https://doi.org/10.5281/zenodo.14886797)

Die erste Codezelle umfasst die Trennung der ALKIS Adressen, die in einer Eintragszeile zusammengefasst sind. Liegen die Adressen wie folgt vor: 

• Beispielstraße 10, 12, 14; Teststraße 7, 9, 11

werden diese am ; wie folgt getrennt:

• Beispielstraße 10, 12, 14

• Teststraße 7, 9, 11

Für jede neu geschriebene und getrennte Adresse werden die anhängenden Informationen in die neu geschriebene Zeile übernommen. Die ursprüngliche Adresszeile wird zum Schluss entfernt.

Die zweite Codezelle beinhaltet ein Skript, welches die Adressen bei der Hausnummer trennt und in einer neuen Adresszeile zusammenführt. Dabei werden Straßennamen und Hausnummern in unterschiedlichen Dataframes zwischengespeichert und anschließend wieder neu mit allen gebäudespezifischen Informationen zusammengeführt. Die Trennung erfolgt für folgendes Beispiel auf folgendem Weg: 

• Adresse: Beispielstraße 10, 12, 24

Nach dem isdigit() befehl wird wie folgt getrennt:

• street: Beispielstraße

• house_numbers: 10, 12, 14

Anschließend werden die Hausnummern am , getrennt und in eine Liste house_numbers_list geschrieben. Jedes Objekt dieser Liste wird darauffolgend mit dem Straßennamen (street) wieder in einer neuen Adresszeile zusammengeführt. Die alte Zeile wird gelöscht. Die neuen Adresszeilen sehen wie folgt aus:

• Beispielstraße 10

• Beispielstraße 12

• Beispielstraße 14

Die dritte und vierte Codezeile dient der räumlichen Aggregation von Haushalts- und Milieudaten, indem er die Anzahl der Haushalte pro SINUS-Milieu innerhalb bestimmter Zielpolygone, wie Baublöcke oder dem Lambert-Raster, berechnet. Die Daten werden dabei aus einer Shape-Datei mit Haushaltsinformationen entnommen, anhand der geografischen Lage den jeweiligen Zielpolygonen zugeordnet und anschließend aggregiert. Das Ergebnis wird als neue Shape-Datei gespeichert, um eine strukturierte Darstellung der Haushaltsverteilung pro Milieu zu ermöglichen. Die Codezellen unterscheiden sich für das jeweilige Input-Feature, zum einen kann dies ein Polygondatensatz sein, bei dem Centroide betsimmt werden zur Aggregation, zum anderen können es auch einfache Punktdaten sein.


Die letzte Codezell ist zur Ermittlung des dominanten Milieus je Baublock oder Rasterzelle, wobei für jede Baublock-ID (BB_Nr)/Rasterzellen-ID (id) das Milieu mit der höchsten Haushaltszahl (Total_Hous) bestimmt wird. Der entsprechende Eintrag wird in einem separaten DataFrame (df_dominant) gespeichert, der nur die Baublock- oder Rasterzellen-ID und das dominante Milieu (Dominant_Milieu) enthält. Nach dieser Selektion erfolgt eine weitere Aggregation der Daten. Dabei werden für jeden Baublock/jede Rasterzelle:

• Alle vorhandenen SINUS-Milieus als kommagetrennte Zeichenfolge zusammengeführt.

• Die Haushaltszahlen der verschiedenen Milieus ebenfalls als kommagetrennte Werte gespeichert.

• Eine repräsentative Geometrie für den Baublock/die Rasterzelle beibehalten.

Diese Aggregation erfolgt über die groupby()-Methode, die für jeden Baublock eine einzige Zeile erstellt, in der alle
relevanten Informationen zusammengefasst werden. Das zuvor bestimmte dominante Milieu (df_dominant) wird
anschließend durch eine Verknüpfung (merge) mit den aggregierten Daten verbunden, sodass jeder Baublock oder jede Rasterzelle
eindeutig mit einem Hauptmilieu versehen wird.
