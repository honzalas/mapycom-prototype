Generátor dokumentace Mapy.com (minimální, ale škálovatelné)

Cíl



Naplnit předpřipravený repozitář mapycom/developer stručnou, konzistentní a snadno rozšiřitelnou dokumentací k REST API Mapy.com a k URL schématům Mapy.com.

Nepřenášet OpenAPI do repa; místo toho odkazovat na Swagger UI a OpenAPI YAML hostované na api.mapy.com.

U URL Mapy.com odkazovat na uživatelskou dokumentaci na: https://developer.mapy.com/further-uses-of-mapycz/.



Předpřipravená struktura (neměň název ani umístění složek)

mapycom/developer/

├─ README.md

├─ llms.txt

└─ docs/

&nbsp;  ├─ README.md

&nbsp;  ├─ rest-api/

&nbsp;  │  ├─ README.md

&nbsp;  │  ├─ getting-access.md

&nbsp;  │  ├─ map-tiles.md

&nbsp;  │  ├─ static-maps.md

&nbsp;  │  ├─ geocoding.md

&nbsp;  │  ├─ route-planning.md

&nbsp;  │  ├─ elevation.md

&nbsp;  │  ├─ static-panorama.md

&nbsp;  │  └─ time-zones.md

&nbsp;  └─ url-mapy/

&nbsp;     ├─ README.md

&nbsp;     ├─ showmap.md

&nbsp;     ├─ search.md

&nbsp;     └─ route.md



Zásady a konvence



Jazyk: Česky (stručně, prakticky).



Formát: Markdown (.md).



Názvy souborů: kebab-case.



Odkazy: používej absolutní URL pro externí zdroje (developer.mapy.com, api.mapy.com).



Technické detaily (parametry, schémata) neopisuj celé z OpenAPI; stačí výběr klíčových parametrů + odkazy na Swagger/YAML.



Kódové ukázky: cURL + JavaScript fetch. U routing/geocoding přidej i C# HttpClient (struhý snippet).



Metadata: Na konec každé stránky dej řádek Last verified: YYYY-MM-DD.



Související: Na konec každé stránky sekce „Související“ s lokálními odkazy (např. Geocoding ↔ Autocomplete/URL Search).



Žádné duplikace OpenAPI v repu. Jen odkazuj (Swagger/YAML).



Zdroje pravdy (použij v odkazech)



REST API – textová dokumentace (funkce):



Map Tiles — https://developer.mapy.com/en/rest-api-mapy-cz/function/map-tiles/



Static maps — https://developer.mapy.com/en/rest-api-mapy-cz/function/static-maps/



Geocoding — https://developer.mapy.com/en/rest-api-mapy-cz/function/geocoding/



Route Planning — https://developer.mapy.com/en/rest-api-mapy-cz/function/routing/



Elevation — https://developer.mapy.com/en/rest-api-mapy-cz/function/elevation-api/



Static Panorama — https://developer.mapy.com/en/rest-api-mapy-cz/function/api-for-static-panorama/



Time zones — https://developer.mapy.com/en/rest-api-mapy-cz/function/api-for-time-zones/



OpenAPI – Swagger UI + YAML:



Map Tiles — Swagger: https://api.mapy.com/v1/docs/maptiles/

&nbsp;• YAML: https://api.mapy.com/v1/docs/maptiles/openapi.yaml



Static maps — Swagger: https://api.mapy.com/v1/docs/static/

&nbsp;• YAML: https://api.mapy.com/v1/docs/static/openapi.yaml



Geocoding — Swagger: https://api.mapy.com/v1/docs/geocode/

&nbsp;• YAML: https://api.mapy.com/v1/docs/geocode/openapi.yaml



Route Planning — Swagger: https://api.mapy.com/v1/docs/routing/

&nbsp;• YAML: https://api.mapy.com/v1/docs/routing/openapi.yaml



Elevation — Swagger: https://api.mapy.com/v1/docs/elevation/

&nbsp;• YAML: https://api.mapy.com/v1/docs/elevation/openapi.yaml



Static Panorama — použij stejnou OpenAPI sekci jako Static maps (/static/).



Time zones — Swagger: https://api.mapy.com/v1/docs/timezone/

&nbsp;• YAML: https://api.mapy.com/v1/docs/timezone/openapi.yaml



URL Mapy.com – uživatelská dokumentace (centrální rozcestník):



https://developer.mapy.com/further-uses-of-mapycz/

&nbsp;← na toto odkazuj ve všech souborech URL



Co má Cursor vygenerovat do jednotlivých souborů

1\) README.md (kořen)



Účel repozitáře (zjednodušený mirror/rozcestník pro vývojáře a LLM).



Dva hlavní odkazy: REST API (/docs/rest-api/README.md) a URL Mapy.com (/docs/url-mapy/README.md).



Poznámka, že OpenAPI žije na api.mapy.com (viz odkazy níže).



„Last verified“.



2\) llms.txt (kořen)



Jednoduchý rozcestník pro LLM (cesty k README a k jednotlivým funkcím).



Zahrň položky:



ROOT, DOCS\_INDEX



REST\_INDEX + cesty na všechny REST .md



URL\_INDEX + cesty na showmap/search/route



PORTAL\_REST = https://developer.mapy.com/rest-api-mapy-cz/



PORTAL\_URLS = https://developer.mapy.com/further-uses-of-mapycz/



3\) docs/README.md



Stručný „landing“ s odkazem na rest-api/README.md a url-mapy/README.md.



Vysvětli, že detaily = odkazy na oficiální portál + OpenAPI.



„Last verified“.



4\) docs/rest-api/README.md



Krátký úvod, jak číst REST dokumentaci (principy, odkazy na OpenAPI).



Vlož tabulku funkcí (viz níže) – přesně s odkazy na textové stránky i Swagger/YAML.



Odkaz na getting-access.md.



„Last verified“.



Tabulka vložit DOSLOVA:



| Funkce | Dokumentace (developer.mapy.com) | Swagger UI | OpenAPI YAML |

|:--|:--|:--|:--|

| \*\*Map Tiles\*\* | https://developer.mapy.com/en/rest-api-mapy-cz/function/map-tiles/ | https://api.mapy.com/v1/docs/maptiles/ | https://api.mapy.com/v1/docs/maptiles/openapi.yaml |

| \*\*Static maps\*\* | https://developer.mapy.com/en/rest-api-mapy-cz/function/static-maps/ | https://api.mapy.com/v1/docs/static/ | https://api.mapy.com/v1/docs/static/openapi.yaml |

| \*\*Geocoding\*\* | https://developer.mapy.com/en/rest-api-mapy-cz/function/geocoding/ | https://api.mapy.com/v1/docs/geocode/ | https://api.mapy.com/v1/docs/geocode/openapi.yaml |

| \*\*Route Planning\*\* | https://developer.mapy.com/en/rest-api-mapy-cz/function/routing/ | https://api.mapy.com/v1/docs/routing/ | https://api.mapy.com/v1/docs/routing/openapi.yaml |

| \*\*Elevation\*\* | https://developer.mapy.com/en/rest-api-mapy-cz/function/elevation-api/ | https://api.mapy.com/v1/docs/elevation/ | https://api.mapy.com/v1/docs/elevation/openapi.yaml |

| \*\*Static Panorama\*\* | https://developer.mapy.com/en/rest-api-mapy-cz/function/api-for-static-panorama/ | https://api.mapy.com/v1/docs/static/ | https://api.mapy.com/v1/docs/static/openapi.yaml |

| \*\*Time zones\*\* | https://developer.mapy.com/en/rest-api-mapy-cz/function/api-for-time-zones/ | https://api.mapy.com/v1/docs/timezone/ | https://api.mapy.com/v1/docs/timezone/openapi.yaml |



5\) docs/rest-api/getting-access.md



Jak se registrovat (odkaz na portál), získat API klíč, základní bezpečnost (nikdy necommitovat klíč, server-side proxy pro citlivé volání).



Základní „Hello, API“ s cURL a JS fetch (volání kterékoliv veřejné funkce, např. geocoding).



Odkaz na sazby/limity na portálu (neopisovat).



6\) Jednotlivé REST soubory



Pro každý z těchto souborů: map-tiles.md, static-maps.md, geocoding.md, route-planning.md, elevation.md, static-panorama.md, time-zones.md vygeneruj stejnou kostru:



Šablona (použij ji a doplň konkrétní odkazy výše):



\# <Název funkce>



Krátký popis, kdy použít.



\## Rychlé odkazy

\- Textová dokumentace: <odkaz na developer.mapy.com>

\- Swagger UI: <odkaz na /v1/docs/.../>

\- OpenAPI (YAML): <odkaz na /v1/docs/.../openapi.yaml>



\## Typické endpointy

Krátký výčet hlavních endpointů dle OpenAPI (1–3 řádky).



\## Klíčové parametry (výběr)

\- `param` (typ) — co dělá, příklad.

\- `param2` (typ) — co dělá, příklad.



> Kompletní seznam parametrů viz Swagger / YAML výše.



\## Příklady

\### cURL

```bash

\# stručný, realistický příklad



JavaScript (fetch)

// stručný, realistický příklad



C# (HttpClient)

// stručný, realistický příklad



Chyby a limity



Stručně zmínit běžné chyby (401, 403, 429) a odkázat na portál/OpenAPI.



Související



Lokální odkazy na příbuzné stránky (např. Geocoding ↔ URL Search)



Last verified: YYYY-MM-DD





\*\*Specifika:\*\*

\- `static-panorama.md` používej \*\*stejné OpenAPI odkazy jako Static maps\*\* (`/v1/docs/static/`).  

\- `route-planning.md` přidej podsekci „Matrix routing (stručně)“.  

\- `geocoding.md` přidej podsekce „Forward“, „Reverse“, „Autocomplete“.



\### 7) `docs/url-mapy/README.md`

\- Co jsou URL schémata, kdy je použít (deeplinky, rychlé otevření mapy).  

\- \*\*Odkazuj jen na uživatelskou dokumentaci\*\*: `https://developer.mapy.com/further-uses-of-mapycz/`.  

\- Seznam lokálních stránek: `showmap.md`, `search.md`, `route.md` (tyto stránky budou stručné, s příklady a s odkazem na \*\*tento\*\* rozcestník).



\### 8) `docs/url-mapy/showmap.md`, `search.md`, `route.md`

\- Krátký popis syntaxe (1–2 odstavce) + 2–3 příklady URL (bez citlivých klíčů).  

\- \*\*Vždy\*\* vlož nahoře výrazný odkaz:  

&nbsp; „\*\*Úplná uživatelská dokumentace URL Mapy.com je zde:\*\* https://developer.mapy.com/further-uses-of-mapycz/“  

\- Na konci „Související“ (odkazy mezi showmap/search/route).  

\- „Last verified“.



---



\## Akceptační kritéria

\- Všechny výše uvedené soubory existují a jsou vyplněné podle šablon.  

\- `docs/rest-api/README.md` obsahuje \*\*přesně\*\* vloženou tabulku s odkazy (Swagger + YAML).  

\- Každá stránka má „Související“ a „Last verified: YYYY-MM-DD“.  

\- Všechny externí odkazy jsou \*\*absolutní\*\* a fungují.  

\- Žádné OpenAPI soubory nejsou kopírovány do repa.  

\- V URL sekci je \*\*všude odkaz na\*\* `https://developer.mapy.com/further-uses-of-mapycz/`.



---



> Pozn.: Pokud je k dispozici datum, vlož dnešní datum do „Last verified“. Pokud ne, použij aktuální.

