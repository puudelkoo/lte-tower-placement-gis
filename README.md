# LTE Tower Placement in Mountainous Terrain

## Opis projektu

Projekt przedstawia wykorzystanie narzędzi GIS do planowania lokalizacji anten LTE na obszarze górzystym na przykładzie gminy Maków Podhalański.

Analiza obejmuje dwa główne zagadnienia:

- wyznaczenie lokalizacji anten zapewniających odpowiedni poziom pokrycia terenów zabudowanych,
- wyznaczenie możliwie korzystnych tras dojazdowych do projektowanych masztów z uwzględnieniem ukształtowania terenu oraz ograniczeń przestrzennych.

Projekt łączy analizę rastrową, modelowanie terenu, analizę widoczności oraz analizę najmniejszego kosztu.

Projekt powstał pierwotnie w ramach zajęć akademickich i został następnie opracowany w formie portfolio case study.

---

## Cel projektu

Głównym celem projektu było zaprojektowanie rozmieszczenia anten LTE (4G) na obszarze gminy Maków Podhalański.

Przyjęto następujące założenia:

- minimalizacja kosztów budowy infrastruktury,
- minimalna wysokość anteny: 10 m,
- maksymalny zasięg anteny: 4 km,
- docelowe pokrycie terenów zabudowanych: około 70% ± 2%,
- lokalizacja masztów poza terenami zabudowanymi, wodami powierzchniowymi i terenami podmokłymi,
- uwzględnienie wpływu rzeźby oraz pokrycia terenu na widoczność.

Drugim elementem projektu było wyznaczenie tras dojazdowych łączących projektowane maszty z istniejącą siecią drogową.

---

## Narzędzia

W projekcie wykorzystano:

- **QGIS** – przygotowanie danych, analiza przestrzenna i wizualizacja,
- **GRASS GIS** – analiza kosztowa oraz wyznaczanie tras,
- **r.cost** – obliczenie skumulowanego kosztu przejścia przez teren,
- **r.drain** – wyznaczenie tras o najmniejszym koszcie,
- analizę rastrową,
- reklasyfikację rastrów,
- analizę widoczności (*viewshed*),
- analizę nachylenia terenu,
- analizę *least-cost path*.

---

## Dane

W analizie wykorzystano dane przestrzenne obejmujące:

- granice administracyjne gminy Maków Podhalański,
- Numeryczny Model Terenu (NMT),
- dane o pokryciu i użytkowaniu terenu,
- dane dotyczące istniejącej sieci drogowej,
- maski rastrowe ograniczające analizę do badanego obszaru.

Na podstawie danych o pokryciu terenu określono również orientacyjne wysokości przeszkód:

- lasy iglaste – 30 m,
- lasy liściaste – 15 m,
- tereny antropogeniczne – 8 m.

Dane wejściowe wykorzystane w pierwotnym projekcie zostały udostępnione w ramach ćwiczenia akademickiego.

---

## Metodyka

### 1. Przygotowanie modelu terenu

Numeryczny Model Terenu został połączony z informacjami o wysokości przeszkód wynikającymi z pokrycia terenu.

Na tej podstawie utworzono **Numeryczny Model Pokrycia Terenu (NMPT)**, który wykorzystano jako podstawę analizy widoczności.

### 2. Wyznaczenie terenów zabudowanych

Z danych o pokryciu terenu wyodrębniono obszary antropogeniczne.

Warstwę przekształcono do postaci binarnej:

- `1` – teren zabudowany,
- `0` – pozostałe obszary.

### 3. Wyznaczenie ograniczeń lokalizacyjnych

Z potencjalnych lokalizacji anten wykluczono:

- tereny zabudowane,
- wody powierzchniowe,
- tereny podmokłe.

Pozwoliło to ograniczyć rozmieszczenie masztów do obszarów spełniających przyjęte kryteria.

### 4. Analiza widoczności

Dla projektowanych lokalizacji anten wykonano analizę **viewshed**.

Raster widoczności został następnie przekształcony do postaci binarnej:

- `1` – obszar widoczny z co najmniej jednej anteny,
- `0` – brak widoczności.

Raster widoczności połączono z rasterem terenów zabudowanych, co umożliwiło określenie części zabudowy znajdującej się w zasięgu projektowanej sieci.

### 5. Porównanie wariantów rozmieszczenia anten

Pierwszy wariant obejmował 10 anten i zapewniał około **74% pokrycia terenów zabudowanych**.

Następnie liczbę masztów zmniejszono do 7.

Finalny wariant zapewnił około **72,3% pokrycia terenów zabudowanych**, zachowując wartość bardzo zbliżoną do przyjętego poziomu docelowego przy jednoczesnym ograniczeniu liczby masztów.

### 6. Analiza tras dojazdowych

Na podstawie NMT obliczono nachylenie terenu.

Następnie przygotowano mapę tarcia reprezentującą koszt przejścia przez poszczególne komórki rastra.

Do modelu wprowadzono również ograniczenia przestrzenne związane z:

- zabudową,
- wodami powierzchniowymi,
- terenami podmokłymi.

Przy użyciu algorytmu `r.cost` obliczono skumulowany koszt dotarcia od istniejącej sieci drogowej do pozostałych części obszaru.

Następnie za pomocą `r.drain` wyznaczono trasy o najmniejszym koszcie prowadzące od projektowanych anten do istniejących dróg.

---

## Wyniki

Wariant końcowy obejmuje:

- **7 anten LTE**,
- około **72,3% pokrycia terenów zabudowanych**,
- trasy dojazdowe wyznaczone z wykorzystaniem analizy najmniejszego kosztu.

Analiza wykazała również, że najkrótsza trasa nie zawsze jest trasą najtańszą. Na koszt wpływa również nachylenie terenu oraz konieczność omijania obszarów wykluczonych.

Projekt pokazuje kompromis pomiędzy:

- poziomem pokrycia sygnałem,
- liczbą potrzebnych masztów,
- lokalizacją infrastruktury,
- kosztami realizacji,
- dostępnością komunikacyjną projektowanych lokalizacji.

---

## Koszty

Dla końcowego wariantu z 7 antenami oszacowano:

| Element | Wartość |
|---|---:|
| Liczba anten | 7 |
| Pokrycie terenów zabudowanych | 72,3% |
| Koszt budowy anten | 1 030,06 zł |
| Koszt dróg dojazdowych | 110,47 zł |
| **Łączny szacowany koszt wariantu** | **1 140,53 zł** |

Wartości kosztowe mają charakter modelowy i służą do porównania wariantów w ramach przyjętej metodyki.

---

## Ograniczenia

Projekt stanowi analizę GIS wspierającą wstępne planowanie lokalizacji infrastruktury i posiada kilka istotnych ograniczeń:

- analiza widoczności nie zastępuje pełnego modelowania propagacji fal radiowych,
- maksymalny zasięg anten został przyjęty jako stała wartość 4 km,
- wysokości przeszkód terenowych zostały przypisane na podstawie uproszczonych klas pokrycia terenu,
- model kosztów ma charakter orientacyjny i nie przedstawia rzeczywistych kosztów budowy infrastruktury,
- analiza tras uwzględnia przede wszystkim nachylenie terenu oraz wybrane ograniczenia przestrzenne,
- jakość wyników zależy od dokładności danych wejściowych,
- dane wykorzystane w projekcie pochodziły z materiałów udostępnionych w ramach ćwiczenia akademickiego.

Projekt należy więc traktować jako **GIS-based site selection and accessibility case study**, a nie kompletny projekt techniczny sieci telekomunikacyjnej.

---

## Pełny raport

Szczegółowy opis kolejnych etapów analizy, mapy oraz wyniki znajdują się w pełnym raporcie:

**[Pobierz / otwórz raport PDF](./lte_tower_placement_gis_case_study.pdf)**

