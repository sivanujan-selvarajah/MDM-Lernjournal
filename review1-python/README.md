# Review 1 Python

## Beurteiltes Projekt

|       | Bitte ausfüllen |
|-------|-----------------|
| Review von   (ZHAW-Kürzel) | (alvarchr)  |
| Review durch  (ZHAW-Kürzel) | (selvasiv) |
| Datum Review, von/bis |  25.03.2025    |

## Review

| Thema                                                                      | Skala | Mängel* | Verbesserungsmöglichkeiten* |
|----------------------------------------------------------------------------|-------|--------|----------------------------|
| Datenquelle klar definiert (Projekt 2: zusätzlich Abgrenzung zu Projekt 1) | 3  | Keine ersichtlichen Mängel | Datenquelle (yfinance)  ist klar und korrekt benannt. Eine visuelle Darstellung der Quelle wäre eine schöne Ergänzung. |
| Scraping vorhanden                                                         | 1  | Nur grundlegendes Scraping umgesetzt | Zusätzliche Datenpunkte (z. B. Volumen, Open/Close) oder mehrere Assets könnten den Umfang erweitern.                      |
| Scraping automatisiert                                                     | 1  | Scraping wird manuell angestossen | Automatisierung z. B. über `schedule` oder `cronjob` wäre ein nächster Schritt. |
| Datensatz vorhanden                                                        | 3  | – | Datensatz ist sauber strukturiert und gut nachvollziehbar. |
| Erstellung Datensatz automatisiert, Verwendung Datenbank                   | 1  | Speicherung erfolgt lokal, keine DB-Anbindung | Optional: Anbindung an SQLite oder einfache NoSQL-Datenbank wäre skalierbarer. |
| Datensatz-Grösse ausreichend, Aufteilung Train/Test, Kennzahlen vorhanden  | 1  | Keine sichtbare Trennung von Train/Test | Ergänzung eines Split z. B. 80/20 mit Metriken zur Modellgüte wäre sinnvoll. |
| Modell vorhanden                                                           | 1  | Modell wird verwendet, aber nicht ausführlich beschrieben | Erklärung des Modells und gewählte Parameter wären hilfreich. |
| Modell-Versionierung vorhanden (ModelOps)                                  | 1  | Keine Versionierung sichtbar | Einsatz von z. B. `MLflow` oder semantischem Tagging (v1.0 etc.) könnte helfen. |
| App: auf lokalem Rechner gestartet und funktional                          | 2  | Läuft lokal, aber wenig User-Feedback | Visuelles Feedback oder Fehlermeldungen verbessern die UX. |
| App: mehrere unterschiedliche Testcases durch Reviewer ausführbar          | 1  | Nur Basisfälle funktionieren korrekt | Validierung und Edge Cases ergänzen (z. B. falsche Symbole, leere Felder). |
| Deployment: Falls bereits vorhanden, funktional und automatisiert          | 0  | Kein Deployment vorhanden | Deployment auf Azure Cloud, Render oder Streamlit Cloud oder wäre ein möglicher nächster Schritt. |
| Code: Git-Repository vorhanden, Arbeiten mit Branches / Commits            | 0  | Kein öffentliches Git-Repo angegeben | Repository erstellen, Versionskontrolle durch Branches nutzen. |
| Code: Dependency Management, Dockerfile, Build funktional                  | 1  | Requirements vorhanden, aber kein Dockerfile | Dockerfile oder Setup-Skript erleichtert Installation für Dritte. |

\* wenn fehlend: mögliche Schwierigkeiten und Lösungen besprechen

## Skala

| Skala |                 |
|-------|-----------------|
| 0     | fehlt           |
| 1     | mit Lücken      |
| 2     | alles vorhanden |
| 3     | übertroffen     |

## Hinweise


Der Projekt-Review fliesst nicht in die Beurteilung der Leistungsnachweise ein, soll aber dem Studierenden dazu dienen, bis zur Abgabe 
noch Verbesserungsmöglichkeiten zu erkennen. Der Review *des fremden Projektes* muss als Teil des *eigenen* Lernjournals abgegeben werden.

