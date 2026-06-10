# Testprotokoll

|                       |                                                    |
| --------------------- | -------------------------------------------------- |
| **Aufgabe**           | Testen der Klasse `Netzplan` (JAR netplan-1_1.jar) |
| **Datum**             | 2024-06-10                                         |
| **Testdurchführung**  | (wird nach Ausführung eingetragen)                 |
| **Autor / Jahrgang**  | Khasu Akhmadov 3CHIT                               |
| **Tester / Jahrgang** | Khasu Akhmadov 3CHIT                               |

---

## Äquivalenzklassen (Übersicht)

| ID     | Art | Beschreibung                                     | Relevanz für Fragestellung |
| ------ | --- | ------------------------------------------------ | -------------------------- |
| ÄK‑1.1 | NF  | Genau ein Start‑, ein Endknoten                  | 1                          |
| ÄK‑1.2 | GF  | Kein Startknoten (alle haben Vorgänger)          | 1                          |
| ÄK‑1.3 | GF  | Mehrere Startknoten                              | 1                          |
| ÄK‑1.4 | GF  | Kein Endknoten                                   | 1                          |
| ÄK‑1.5 | GF  | Mehrere Endknoten                                | 1                          |
| ÄK‑1.6 | GF  | Nur ein Knoten (Start=Ende)                      | 1                          |
| ÄK‑1.7 | FF  | Leerer Netzplan (keine Knoten)                   | 1                          |
| ÄK‑2.1 | NF  | Lineare Kette – Reihenfolge                      | 2                          |
| ÄK‑2.2 | NF  | Beispielnetzplan (11 Knoten) – Reihenfolge       | 2                          |
| ÄK‑3.1 | FF  | Direkter Zirkel (A→B→A)                          | 3                          |
| ÄK‑3.2 | FF  | Indirekter Zirkel (A→B→C→A)                      | 3                          |
| ÄK‑3.3 | FF  | Selbstbezug (A→A)                                | 3                          |
| ÄK‑4.1 | NF  | Knoten mit zwei Vorgängern (FAZ = max)           | 4                          |
| ÄK‑4.2 | NF  | Varargs‑Methode für mehrere Vorgänger            | 4                          |
| ÄK‑4.3 | NF  | Gesamtpuffer am Merge‑Knoten                     | 4                          |
| ÄK‑5.1 | NF  | Knoten mit zwei Nachfolgern                      | 5                          |
| ÄK‑5.2 | NF  | Kausale Reihenfolge (FEZ ≤ FAZ der Nachfolger)   | 5                          |
| ÄK‑5.3 | NF  | Kritischer Pfad nur mit GP=0                     | 5                          |
| ÄK‑5.4 | NF  | Projektdauer des Beispielnetzplans (Excel = 178) | 5                          |

---

## Testfälle (nach Fragestellungen)

### Fragestellung 1 – Start‑ und Endknoten (10 Testfälle)

#### TC‑1.1 Gültiger Netzplan mit genau einem Start‑ und Endknoten

|                         |                                                                              |
| ----------------------- | ---------------------------------------------------------------------------- |
| **Test-ID / ÄK-ID**     | TC‑1.1 / ÄK‑1.1                                                              |
| **Beschreibung**        | Ein korrekter DAG (z. B. A→B→C) löst keine Exception aus.                    |
| **Testvoraussetzungen** | –                                                                            |
| **Testschritte**        | 1. Knoten A,B,C anlegen.<br>2. A→B→C verbinden.<br>3. `calcPath()` aufrufen. |
| **Erwartetes Ergebnis** | Keine Exception; `getDuration()` positiv.                                    |

#### TC‑1.2 Netzplan mit nur einem Knoten

|                         |                                                  |
| ----------------------- | ------------------------------------------------ |
| **Test-ID / ÄK-ID**     | TC‑1.2 / ÄK‑1.6                                  |
| **Beschreibung**        | Ein einzelner Knoten (Start = Ende) ist erlaubt. |
| **Testvoraussetzungen** | –                                                |
| **Testschritte**        | 1. Nur Knoten A anlegen.<br>2. `calcPath()`.     |
| **Erwartetes Ergebnis** | Keine Exception; `getDuration() == Dauer(A)`.    |

#### TC‑1.3 Zwei Startknoten → Exception

|                         |                                                                                      |
| ----------------------- | ------------------------------------------------------------------------------------ |
| **Test-ID / ÄK-ID**     | TC‑1.3 / ÄK‑1.3                                                                      |
| **Beschreibung**        | Zwei Knoten ohne Vorgänger sind nicht erlaubt.                                       |
| **Testvoraussetzungen** | –                                                                                    |
| **Testschritte**        | 1. Start1, Start2, Ende anlegen.<br>2. Start1→Ende, Start2→Ende.<br>3. `calcPath()`. |
| **Erwartetes Ergebnis** | `IllegalArgumentException` mit Meldung über mehrere Startknoten.                     |

#### TC‑1.4 Zwei Endknoten → Exception

|                         |                                                                                     |
| ----------------------- | ----------------------------------------------------------------------------------- |
| **Test-ID / ÄK-ID**     | TC‑1.4 / ÄK‑1.5                                                                     |
| **Beschreibung**        | Zwei Knoten ohne Nachfolger sind nicht erlaubt.                                     |
| **Testvoraussetzungen** | –                                                                                   |
| **Testschritte**        | 1. Start, Ende1, Ende2 anlegen.<br>2. Start→Ende1, Start→Ende2.<br>3. `calcPath()`. |
| **Erwartetes Ergebnis** | `IllegalArgumentException`.                                                         |

#### TC‑1.5 Kein Startknoten → Exception

|                         |                                                           |
| ----------------------- | --------------------------------------------------------- |
| **Test-ID / ÄK-ID**     | TC‑1.5 / ÄK‑1.2                                           |
| **Beschreibung**        | Alle Knoten haben Vorgänger → kein Startknoten.           |
| **Testvoraussetzungen** | –                                                         |
| **Testschritte**        | 1. A→B→C und zusätzlich C→A (Zirkel).<br>2. `calcPath()`. |
| **Erwartetes Ergebnis** | `IllegalArgumentException` (Start fehlt oder Zirkel).     |

#### TC‑1.6 Leerer Netzplan → Exception

|                         |                                                    |
| ----------------------- | -------------------------------------------------- |
| **Test-ID / ÄK-ID**     | TC‑1.6 / ÄK‑1.7                                    |
| **Beschreibung**        | Keine Knoten → keine Start‑/Endknoten → Exception. |
| **Testvoraussetzungen** | –                                                  |
| **Testschritte**        | 1. `new Netzplan()`.<br>2. `calcPath()`.           |
| **Erwartetes Ergebnis** | `IllegalArgumentException`.                        |

#### TC‑1.7 Startknoten hat FAZ = 0

|                         |                                                                                                |
| ----------------------- | ---------------------------------------------------------------------------------------------- |
| **Test-ID / ÄK-ID**     | TC‑1.7 / ÄK‑1.1                                                                                |
| **Beschreibung**        | Früheste Anfangszeit des Startknotens muss 0 sein.                                             |
| **Testvoraussetzungen** | Netzplan A→B→C, Dauer beliebig.                                                                |
| **Testschritte**        | 1. Aufbau, `calcPath()`.<br>2. Startknoten ermitteln.<br>3. `assertEquals(0, start.getFaz())`. |
| **Erwartetes Ergebnis** | FAZ == 0.                                                                                      |

#### TC‑1.8 Genau ein Endknoten (Nachfolger‑Liste leer)

|                         |                                                                   |
| ----------------------- | ----------------------------------------------------------------- |
| **Test-ID / ÄK-ID**     | TC‑1.8 / ÄK‑1.1                                                   |
| **Beschreibung**        | Es gibt genau einen Knoten ohne Nachfolger.                       |
| **Testvoraussetzungen** | Netzplan A→B→C.                                                   |
| **Testschritte**        | 1. Aufbau, `calcPath()`.<br>2. Zähle `getSuccessors().isEmpty()`. |
| **Erwartetes Ergebnis** | Anzahl == 1 (Knoten C).                                           |

#### TC‑1.9 Projektdauer == FEZ des Endknotens

|                         |                                                                  |
| ----------------------- | ---------------------------------------------------------------- |
| **Test-ID / ÄK-ID**     | TC‑1.9 / ÄK‑1.1                                                  |
| **Beschreibung**        | `getDuration()` liefert denselben Wert wie `endKnoten.getFez()`. |
| **Testvoraussetzungen** | A→B→C (Dauern 5,3,2).                                            |
| **Testschritte**        | 1. Berechnen.<br>2. Vergleiche.                                  |
| **Erwartetes Ergebnis** | Gleich.                                                          |

#### TC‑1.10 Korrekte Anzahl Knoten nach Einfügen

|                         |                                                             |
| ----------------------- | ----------------------------------------------------------- |
| **Test-ID / ÄK-ID**     | TC‑1.10 / ÄK‑1.1                                            |
| **Beschreibung**        | `size()` gibt die richtige Anzahl zurück.                   |
| **Testvoraussetzungen** | –                                                           |
| **Testschritte**        | 1. 5 Knoten hinzufügen.<br>2. `assertEquals(5, np.size())`. |
| **Erwartetes Ergebnis** | 5.                                                          |

---

### Fragestellung 2 – Einfluss der Eingabereihenfolge (3 Testfälle)

#### TC‑2.1 Lineare Kette: Reihenfolge egal

|                         |                                                                                                                                            |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| **Test-ID / ÄK-ID**     | TC‑2.1 / ÄK‑2.1                                                                                                                            |
| **Beschreibung**        | A→B→C: Projektdauer gleich, egal ob Knoten in A,B,C oder C,B,A eingefügt werden.                                                           |
| **Testvoraussetzungen** | –                                                                                                                                          |
| **Testschritte**        | 1. Zwei Netzpläne mit gleichen Kanten, unterschiedlicher `addNode`-Reihenfolge.<br>2. Beide `calcPath()`.<br>3. Vergleich `getDuration()`. |
| **Erwartetes Ergebnis** | Dauern identisch.                                                                                                                          |

#### TC‑2.2 Beispielnetzplan (A–K): Reihenfolge egal

|                         |                                                                                                                |
| ----------------------- | -------------------------------------------------------------------------------------------------------------- |
| **Test-ID / ÄK-ID**     | TC‑2.2 / ÄK‑2.2                                                                                                |
| **Beschreibung**        | Der komplexe Beispielnetzplan liefert dieselbe Dauer (178) unabhängig von der Einfügereihenfolge.              |
| **Testvoraussetzungen** | Aufbau wie in Excel (Abhängigkeiten A→B→D→F→I→K, etc.).                                                        |
| **Testschritte**        | 1. Normal (A..K) vs. umgekehrt (K..A).<br>2. `assertEquals(erwartet, np1.getDuration())` und Vergleich beider. |
| **Erwartetes Ergebnis** | Gleiche Dauer (178).                                                                                           |

#### TC‑2.3 Kritischer Pfad (Länge) unabhängig von Reihenfolge

|                         |                                                          |
| ----------------------- | -------------------------------------------------------- |
| **Test-ID / ÄK-ID**     | TC‑2.3 / ÄK‑2.1                                          |
| **Beschreibung**        | Anzahl Knoten auf dem kritischen Pfad ändert sich nicht. |
| **Testvoraussetzungen** | Linearer Plan A→B→C.                                     |
| **Testschritte**        | 1. Zwei Reihenfolgen.<br>2. `cp.size()` vergleichen.     |
| **Erwartetes Ergebnis** | Beide `criticalPath().size()` gleich (hier 3).           |

---

### Fragestellung 3 – Zirkelbezüge erkennen (3 Testfälle)

#### TC‑3.1 Direkter Zirkel (A→B→A)

|                         |                                                        |
| ----------------------- | ------------------------------------------------------ |
| **Test-ID / ÄK-ID**     | TC‑3.1 / ÄK‑3.1                                        |
| **Beschreibung**        | Zyklus der Länge 2 → Exception.                        |
| **Testvoraussetzungen** | –                                                      |
| **Testschritte**        | 1. A,B anlegen.<br>2. A→B und B→A.<br>3. `calcPath()`. |
| **Erwartetes Ergebnis** | `IllegalArgumentException` (Zirkel).                   |

#### TC‑3.2 Indirekter Zirkel (A→B→C→A)

|                         |                                                            |
| ----------------------- | ---------------------------------------------------------- |
| **Test-ID / ÄK-ID**     | TC‑3.2 / ÄK‑3.2                                            |
| **Beschreibung**        | Zyklus über drei Knoten → Exception.                       |
| **Testvoraussetzungen** | –                                                          |
| **Testschritte**        | 1. A,B,C anlegen.<br>2. A→B, B→C, C→A.<br>3. `calcPath()`. |
| **Erwartetes Ergebnis** | `IllegalArgumentException`.                                |

#### TC‑3.3 Selbstbezug (A→A)

|                         |                                                               |
| ----------------------- | ------------------------------------------------------------- |
| **Test-ID / ÄK-ID**     | TC‑3.3 / ÄK‑3.3                                               |
| **Beschreibung**        | Knoten als eigener Vorgänger → Zirkel → Exception.            |
| **Testvoraussetzungen** | –                                                             |
| **Testschritte**        | 1. Knoten A.<br>2. `A.addPredecessor(A)`.<br>3. `calcPath()`. |
| **Erwartetes Ergebnis** | `IllegalArgumentException`.                                   |

---

### Fragestellung 4 – Mehrere Vorgänger (3 Testfälle)

#### TC‑4.1 Knoten mit zwei Vorgängern (FAZ = max FEZ)

|                         |                                                                                                                           |
| ----------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **Test-ID / ÄK-ID**     | TC‑4.1 / ÄK‑4.1                                                                                                           |
| **Beschreibung**        | C mit Vorgängern A(D=10), B(D=25). FAZ(C) muss 25 sein.                                                                   |
| **Testvoraussetzungen** | –                                                                                                                         |
| **Testschritte**        | 1. A,B,C anlegen, Dauern setzen.<br>2. `C.addPredecessor(A,B)`.<br>3. `calcPath()`.<br>4. `assertEquals(25, C.getFaz())`. |
| **Erwartetes Ergebnis** | FAZ = 25.                                                                                                                 |

#### TC‑4.2 Varargs‑Methode für mehrere Vorgänger

|                         |                                                                                                     |
| ----------------------- | --------------------------------------------------------------------------------------------------- |
| **Test-ID / ÄK-ID**     | TC‑4.2 / ÄK‑4.2                                                                                     |
| **Beschreibung**        | `addPredecessor(A,B,C)` fügt alle drei Vorgänger korrekt ein.                                       |
| **Testvoraussetzungen** | –                                                                                                   |
| **Testschritte**        | 1. A,B,C,D anlegen.<br>2. `D.addPredecessor(A,B,C)`.<br>3. Prüfe `D.getPredecessors().size() == 3`. |
| **Erwartetes Ergebnis** | Größe = 3.                                                                                          |

#### TC‑4.3 Gesamtpuffer am Merge‑Knoten

|                         |                                                                                                                                   |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| **Test-ID / ÄK-ID**     | TC‑4.3 / ÄK‑4.3                                                                                                                   |
| **Beschreibung**        | Bei zwei parallelen Pfaden (Start→A(10), Start→B(20)) zum Merge hat der längere Pfad GP=0, der kürzere GP>0.                      |
| **Testvoraussetzungen** | –                                                                                                                                 |
| **Testschritte**        | 1. Start, A, B, Merge (Dauer 0).<br>2. A→Merge, B→Merge.<br>3. `calcPath()`.<br>4. `assertTrue(A.getGp() > 0 && B.getGp() == 0)`. |
| **Erwartetes Ergebnis** | B (länger) kritisch, A hat Puffer.                                                                                                |

---

### Fragestellung 5 – Mehrere Nachfolger (3 Testfälle)

#### TC‑5.1 Knoten mit zwei Nachfolgern

|                         |                                                                                                 |
| ----------------------- | ----------------------------------------------------------------------------------------------- |
| **Test-ID / ÄK-ID**     | TC‑5.1 / ÄK‑5.1                                                                                 |
| **Beschreibung**        | A hat Nachfolger B und C (durch `B.addPredecessor(A)` und `C.addPredecessor(A)`).               |
| **Testvoraussetzungen** | –                                                                                               |
| **Testschritte**        | 1. A,B,C.<br>2. B und C als Nachfolger von A einrichten.<br>3. `A.getSuccessors().size() == 2`. |
| **Erwartetes Ergebnis** | Größe = 2.                                                                                      |

#### TC‑5.2 Kausale Reihenfolge (FEZ(Vorgänger) ≤ FAZ(Nachfolger))

|                         |                                                                                            |
| ----------------------- | ------------------------------------------------------------------------------------------ |
| **Test-ID / ÄK-ID**     | TC‑5.2 / ÄK‑5.2                                                                            |
| **Beschreibung**        | Für jede Kante im Beispielnetzplan muss die Bedingung gelten.                              |
| **Testvoraussetzungen** | Beispielnetzplan (A–K).                                                                    |
| **Testschritte**        | 1. Berechnen.<br>2. Iteriere über alle Kanten.<br>3. `assertTrue(fez(vorg) <= faz(nach))`. |
| **Erwartetes Ergebnis** | Alle Kanten erfüllen die Ungleichung.                                                      |

#### TC‑5.3 Projektdauer des Beispielnetzplans = 178 (aus Excel)

|                         |                                                                                       |
| ----------------------- | ------------------------------------------------------------------------------------- |
| **Test-ID / ÄK-ID**     | TC‑5.3 / ÄK‑5.4                                                                       |
| **Beschreibung**        | Der Excel‑Netzplan liefert genau die erwartete Dauer 178.                             |
| **Testvoraussetzungen** | Knoten, Dauern, Abhängigkeiten wie in `Netzplan_Beispiel.xlsx`.                       |
| **Testschritte**        | 1. Aufbau des Plans.<br>2. `calcPath()`.<br>3. `assertEquals(178, np.getDuration())`. |
| **Erwartetes Ergebnis** | Dauer = 178.                                                                          |
