# Żłobki i kluby dziecięce w Gdańsku
<i>Power BI report about daycare facilities in Gdansk.</i>
<br><br>
:point_right: Zobacz raport na żywo w przeglądarce: 
<a href="https://app.powerbi.com/view?r=eyJrIjoiN2E1OWQ0N2EtNzFkNC00NTc0LTk4NjgtNWY5Y2I0NzRjOTM2IiwidCI6Ijk3NmI5Y2IwLWJiYjctNDg2NC04NjAwLTE1NTk4MzA5YjY3YiJ9&pageName=ReportSection40897505d313de90450e">LINK</a>
<br><br>
<img src="https://raw.githubusercontent.com/jusjag/ZlobkiGdansk/main/ZlobkiGdansk_raport.png" width = "600">

## 1. Projekt
<b>Cel:</b> praktyczne wykorzystanie umiejętności pracy z Power Query, Power BI i DAX.<br>
<b>Dane:</b> publicznie dostępne dane z Rejestru Żłobków, uzupełnione o inne informacje wymienione w sekcji poniżej. Stan na rok 2022/2023.<br>

## 2. Źródła danych
Ten projekt wymagał ode mnie bardzo dużo zakulisowej pracy z zebraniem, pobraniem, połączeniem oraz przekształcaniem danych.<br>
Po raz kolejny przekonałam się, jak bardzo lubię "dłubanie" w Power Query :)<br>

### Lista placówek i podstawowe dane:
Podstawą do stworzenia raportu były dane z <b>Rejestru Żłobków</b>, dostępne publicznie na stronie <a href="https://dane.gov.pl/pl/dataset/2086/resource/48743,rejestr-zobkow/table?page=1&per_page=20&q=&sort=">dane.gov.pl</a>.<br>
Tabela zawiera informacje o nazwie i numerze rejestru, danych adresowych, kontaktowych, ilości miejsc i zapisanych dzieci, opłatach oraz udogodnieniach i jest oznaczona jako aktualna na dzień 23.06.2023. Po wstępnym przejrzeniu danych zauważyłam jednak, że część z nich jest nieprawidłowa - na liście znajduje się np. placówka, o której wiem, że jest zamknięta od ponad roku.<br><br>
Aby uzyskać aktualną listę placówek, posiłkowałam się danymi z <b>portalu Empatia</b> (<a href="https://empatia.mpips.gov.pl/dla-swiadczeniobiorcow/rodzina/d3/rejestr-zlobkow-i-klubow">empatia.mpips.gov.pl</a>). Tu pobranie listy było już trudniejsze: pozycje są ładowane na bieżąco w ramce zamieszczonej wewnątrz strony. Udało mi się jednak dotrzeć do źródła ramki, ręcznie rozwinąć całą potrzebną listę, zapisać stronę na dysku i takie dane załadować do Power Query. Jak widać poniżej nie były one w zbyt przyjaznym formacie:<br><br>
<img src="https://raw.githubusercontent.com/jusjag/ZlobkiGdansk/main/Zlobki_Empatia2.png"><br><br>
Dzięki temu, że każda placówka ma przypisany unikalny numer w rejestrze, możliwe było połączenie tabeli z danymi z rejestru żłobków i uzyskanie ostatecznej listy 102 placówek. Trudno samodzielnie określić stopień rzetelności danych, szczególnie że informacje dot. opłat i liczby wolnych miejsc mogą zmieniać się wielokrotnie w ciągu roku. Zakładam jednak, że oddają one ogólny obraz sytuacji żłobków i klubów dziecięcych w Gdańsku.<br><br>
Czyszczenie danych objęło m.in:<br>
- usunięcie zbędnych kolumn (m.in. danych kontaktowych);<br>
- wyodrębnienie ulicy i numeru z adresu placówki;<br>
- ujednolicenie danych w tabelach dot. opłat (np. <i>null</i> lub 0 w przypadku pustych pól);<br>
- ustawienie odpowiednich typów danych.

### Lista placówek publicznych
Informacje nt. listy placówek publicznych oraz liczby grup pochodzą ze strony Gdańskiego Zespołu Żłobków (<a href="https://zlobki.gda.pl/index.php/kontakt/nasze-zlobki/">gzz.gda.pl)</a>.

### Lista dzielnic Gdańska
Aby przeanalizować sytuację w poszczególnych dzielnicach, wykorzystałam listę sektorów i dzielnic dostępną na stronie <b>Czyste Miasto Gdańsk</b> (<a href="https://czystemiasto.gdansk.pl/zdizgdanskfiles/image/gdansk_ulice_sektory_201307.pdf">czystemiasto.gdansk.pl</a>).<br>
Po wyodrębnieniu z Rejestru Żłobków osobnej kolumny zawierającej nazwę ulicy (bez numeru i przedrostków jak Al., ul., itp) mogłam użyć tej kolumny jako klucza do połączenia z tabelą dot. dzielnic. W przypadku dłuższych ulic, przypisanych do więcej niż jednej dzielnicy, przyporządkowałam informacje ręcznie - takich przypadków było raptem kilkanaście.<br><br>
Dane dot. liczby mieszkańców poszczególnych dzielnic pobrałam ze strony Miasta Gdańsk (<a href="https://www.gdansk.pl/gdansk-w-liczbach/mieszkancy,a,108046">gdansk.pl</a>).<br>
Czyszczenie danych: w przypadku wszystkich powyższych danych konieczne było ujednolicenie pisowni nazw dzielnic (np. Zaspa Rozstaje vs Zaspa-Rozstaje), tak aby pokrywały się z nazwami dzielnic zakodowanymi w pliku z mapą, co umożliwi ich późniejszą prawidłową wizualizację.<br>

### Mapa dzielnic Gdańska
Odrębną kwestią było przedstawienie danych na mapie dzielnic miasta. Nietety Power BI nie posiada dla Polski wbudowanych map na tak szczegółowym poziomie.<br>
Skorzystałam z udostępnionych przez Miasto Gdańsk plików SHP z granicami dzielnic (<a href="https://dane.gov.pl/pl/dataset/1821,granice-dzielnic-w-gdansku">dane.gov.pl</a>), a następnie za pomocą narzędzia mapshaper.org przekształciłam je do formatu .json obsługiwanego przez Power BI.<br>

### Dane ogólnopolskie
Na potrzeby obliczenia wskaźnika liczby żłobków na 1 tysiąc mieszkańców wykorzystałam:<br>
- Dane dot. ludności kraju na 31.12.2022 - z publikacji "Sytuacja demograficzna Polski do roku 2022" dostępnego na <a href="https://stat.gov.pl/obszary-tematyczne/ludnosc/ludnosc/sytuacja-demograficzna-polski-do-roku-2022,40,3.html">stronie GUS</a><br>
- Dane dot. ogólnopolskiej liczby żłobków w 2022 - z publikacji "Żłobki i Kluby Dziecięce w 2022 roku" dostępnej na <a href="https://stat.gov.pl/obszary-tematyczne/dzieci-i-rodzina/dzieci/zlobki-i-kluby-dzieciece-w-2022-roku,3,10.html">stronie GUS</a>

## 3. Przykładowe miary i obliczenia

Do zestawu danych dodałam takie kolumny jak:
- "Publiczny" (TAK / NIE) - dodane ręcznie, pozwalające wyodrębnić z listy żłobki publiczne;<br>
- "Przepełniony" (TAK / NIE) - kolumna sprawdzająca, czy liczba dzieci zapisanych przewyższa liczbę dostępnych miejsc;<br>
- "Wolnych miejsc" - kolumna warunkowa: różnica między liczbą miejsc a liczbą zapisanych dzieci, zwracająca 0 zamiast liczb ujemnych gdy żłobek jest przepełniony.<br>
- "Wielkość placówki" (Przedziały liczbowe: do 15, 16-20, 21-25, 26-30, 31-50, 50-80, 80-100 i 100+), przedziały ustalone samodzielnie na podstawie częstotliwości występowania.<br>

<b>Miary w DAX:</b><br>
Kalkulacja do kafelka pokazującego koszt żłobka publicznego, tak aby w/w wartość nie była filtrowana przez inne elementy raportu.
```
Miesięczny koszt PUBL = 
CALCULATE (
    AVERAGE(Finanse[Opłata za pobyt - miesięczna]),
    ALLEXCEPT(Finanse, Finanse[Publiczny]),
    Finanse[Publiczny] = "TAK",
    ALL('Żłobki_info')
    )
```
<br><br>
Kalkulacja do kafelka pokazującego liczbę miejsc w wybranej dzielnicy (zakładka mapy) - w taki sposób, aby przy braku placówek wyświetlał "0" zamiast "(blank)".
```
MD Liczba miejsc = CALCULATE(
    IF(SUM(Dzielnice_info[D Liczba miejsc])=0,"0",SUM('Żłobki_info'[Liczba miejsc])),
    CROSSFILTER('Żłobki_info'[Dzielnica], Dzielnice_info[Dzielnica], Both)
)
```
<br><br>
Kalkulacja do kafelka pokazującego przedział opłat - wartości zmieniające się w zależności od opcji zaznaczonej w pozostałych elementach na stronie.
```
Opłata mc przedział = 
CONCATENATE(MIN(Finanse[Opłata za pobyt - miesięczna]), 
    CONCATENATE(" - ", 
        CONCATENATE(MAX(Finanse[Opłata za pobyt - miesięczna]), " zł")))
```
<br>

## 4. Raport
Zobacz raport na żywo w przeglądarce: 
<a href="https://app.powerbi.com/view?r=eyJrIjoiN2E1OWQ0N2EtNzFkNC00NTc0LTk4NjgtNWY5Y2I0NzRjOTM2IiwidCI6Ijk3NmI5Y2IwLWJiYjctNDg2NC04NjAwLTE1NTk4MzA5YjY3YiJ9&pageName=ReportSection40897505d313de90450e">LINK</a>
<br><br>
<img src="https://raw.githubusercontent.com/jusjag/ZlobkiGdansk/main/ZlobkiGdansk_raport.png"><br><br>
<img src="https://raw.githubusercontent.com/jusjag/ZlobkiGdansk/main/ZlobkiGdansk_dzielnice.png"><br><br>
<img src="https://raw.githubusercontent.com/jusjag/ZlobkiGdansk/main/ZlobkiGdansk_publ.png"><br><br>
<img src="https://raw.githubusercontent.com/jusjag/ZlobkiGdansk/main/ZlobkiGdansk_koszty.png"><br><br>
<img src="https://raw.githubusercontent.com/jusjag/ZlobkiGdansk/main/ZlobkiGdansk_mapy.png"><br><br>
