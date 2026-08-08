# Sportsoft Developer API Dokumentace

Vítejte v oficiální vývojářské dokumentaci pro veřejná REST API rozhraní platformy Sportsoft. Tato rozhraní poskytují přístup ke konfiguracím závodů, listinám přihlášených závodníků a živým výsledkům z časomíry.

---

## Přehled koncových bodů (Endpoints)

| # | Název API | Koncový bod | Metoda | Popis |
|---|---|---|---|---|
| 1 | **Race Setup** | `https://live.sportsoft.cz/api/setup/race/{raceId}` | `GET` | Konfigurace události, trasy, úseky, kategorie a startovní časy |
| 2 | **Competitors** | `https://live.sportsoft.cz/api/competitors/race/{raceId}` | `GET` | Seznam přihlášených závodníků a týmů rozdělených po soutěžích |
| 3 | **Results** | `https://live.sportsoft.cz/api/results/race/{raceId}` | `GET` | Výsledková listina, mezitasy, pořadí a finální časy |

---

## 1. Race Setup API (Nastavení závodu)

Poskytuje kompletní strukturu události, seznam soutěží, měřené úseky (splits), definice věkových/výkonnostních kategorií a plánované i reálné startovní časy vln.

* **URL:** `https://live.sportsoft.cz/api/setup/race/{raceId}`
* **Metoda:** `GET`
* **Autentizace:** Žádná

### Parametry cesty (Path Parameters)

| Parametr | Typ | Povinný | Popis |
|---|---|---|---|
| `raceId` | `integer` | **Ano** | Unikátní ID závodu v systému Sportsoft (např. `880`). |

### Datová struktura (Response Schema)

#### Hlavní objekt (Race Object)
| Pole | Typ | Popis |
|---|---|---|
| `Id` | `integer` | Unikátní identifikátor události. |
| `Name` | `string` | Název sportovní události. |
| `Competitions` | `array[Competition]` | Seznam soutěží/tratí v rámci události. |

#### Závod / Soutěž (Competition Object)
| Pole | Typ | Popis |
|---|---|---|
| `Id` | `integer` | ID soutěže. |
| `Name` | `string` | Název soutěže (např. `"Full Distance Triathlon"`). |
| `Ranks` | `boolean` | Příznak, zda se vyhodnocuje pořadí (`true`/`false`). |
| `Distance` | `number` | Celková vzdálenost trati v km. |
| `Splits` | `array[Split]` | Měřené úseky a mezičasy. |
| `Categories` | `array[Category]` | Věkové a výkonnostní kategorie. |
| `StartTimes` | `array[StartTime]` | Plánované a reálné časové výstřely vln. |

#### Úseky a Mezičasy (Split Object)
| Pole | Typ | Popis |
|---|---|---|
| `Name` | `string` | Název úseku (např. `"Swim"`, `"T1"`, `"Bike"`). |
| `Distance` | `string` | Vzdálenost měřeného bodu od startu (v km). |
| `IsStart` | `boolean` | `true`, pokud jde o startovní čáru. |
| `IsFinish` | `boolean` | `true`, pokud jde o cílovou čáru. |

#### Kategorie (Category Object)
| Pole | Typ | Popis |
|---|---|---|
| `LongName` | `string` | Celý název kategorie (např. `"Men Elite"`). |
| `ShortName` | `string` | Zkratka kategorie (např. `"ME"`). |
| `MinYear` | `integer` | Minimální věk závodníka. |
| `MaxYear` | `integer` | Maximální věk závodníka. |
| `Gender` | `string` | Pohlaví (`"male"`, `"female"`, `"mixed"`). |
| `Entries` | `integer` | Počet přihlášených závodníků v kategorii. |

#### Startovní časy (StartTime Object)
| Pole | Typ | Popis |
|---|---|---|
| `Name` | `string` | Název startovní vlny / skupiny. |
| `Planned` | `string` | Plánovaný čas startu (`HH:mm:ss`). |
| `GunTime` | `string` | Reálný čas výstřelu startovní pistole (`HH:mm:ss`). |

### Příklad volání (cURL)
```bash
curl -X GET "[https://live.sportsoft.cz/api/setup/race/880](https://live.sportsoft.cz/api/setup/race/880)"
