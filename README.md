# Żłobki i kluby dziecięce w Gdańsku
<i>Power BI report about daycare facilities in Gdansk.</i>
<br><br>
:point_right: Zobacz raport na żywo w przeglądarce: 
<a href="https://app.powerbi.com/view?r=eyJrIjoiN2E1OWQ0N2EtNzFkNC00NTc0LTk4NjgtNWY5Y2I0NzRjOTM2IiwidCI6Ijk3NmI5Y2IwLWJiYjctNDg2NC04NjAwLTE1NTk4MzA5YjY3YiJ9">LINK</a>
<br><br>
<img src="https://raw.githubusercontent.com/jusjag/ZlobkiGdansk/main/ZlobkiGdansk_raport.png" width = "600">

## Projekt
<b>Cel:</b> praktyczne wykorzystanie umiejętności pracy z Power Query, Power BI i DAX.<br>
<b>Dane:</b> publicznie dostępne dane z Rejestru Żłobków, uzupełnione o inne informacje wymienione w sekcji poniżej. Stan na rok 2022/2023.<br>

## 1. Źródła danych
Ten projekt wymagał ode mnie bardzo dużo zakulisowej pracy z zebraniem, pobraniem, połączeniem oraz przekształcaniem danych.<br>
Po raz kolejny przekonałam się, jak bardzo lubię "dłubanie" w Power Query :)<br>
### Lista placówek i podstawowe dane:
Podstawą do stworzenia raportu były dane z <b>Rejestru Żłobków</b>, dostępne publicznie na stronie <a href="https://dane.gov.pl/pl/dataset/2086/resource/48743,rejestr-zobkow/table?page=1&per_page=20&q=&sort=">dane.gov.pl</a>.<br>
Tabela zawiera informacje o nazwie i numerze rejestru, danych adresowych, kontaktowych, ilości miejsc i zapisanych dzieci, opłatach oraz udogodnieniach i jest oznaczona jako aktualna na dzień 23.06.2023. Po wstępnym przejrzeniu danych zauważyłam jednak, że część z nich jest nieprawidłowa - na liście znajduje się np. placówka, o której wiem, że jest zamknięta od ponad roku.<br><br>
Aby uzyskać aktualną listę placówek, posiłkowałam się danymi z <b>portalu Empatia</b> (<a href="https://empatia.mpips.gov.pl/dla-swiadczeniobiorcow/rodzina/d3/rejestr-zlobkow-i-klubow">empatia.mpips.gov.pl</a>). Tu pobranie listy było już trudniejsze: pozycje są ładowane na bieżąco w ramce zamieszczonej wewnątrz strony. Udało mi się jednak dotrzeć do źródła ramki, ręcznie rozwinąć całą potrzebną listę, zapisać stronę na dysku i takie dane załadować do Power Query. Jak widać poniżej nie były one w zbyt przyjaznym formacie:<br><br>
<img src="https://raw.githubusercontent.com/jusjag/ZlobkiGdansk/main/Zlobki_Empatia2.png"><br><br>
Dzięki temu, że każda placówka ma przypisany unikalny numer w rejestrze, możliwe było połączenie tabeli z danymi z rejestru żłobków i uzyskanie ostatecznej listy 102 placówek. Choć trudno samodzielnie określić stopień rzetelności danych, zakładam że oddają one ogólny obraz sytuacji żłobków i klubów dziecięcych w Gdańsku. Szczególnie informacje dot. opłat i liczby wolnych miejsc mogą zmieniać się wielokrotnie w ciągu roku.<br>
Ponieważ tabela z rejestru żłobków była całkiem nieźle przygotowana do pracy, czyszczenie danych ograniczyło się do usunięcia zbędnych kolumn (m.in. danych kontaktowych), ustawienia odpowiednich typów danych oraz sformatowania adresów placówek.

### Lista placówek publicznych
Informacje nt. listy placówek publicznych oraz liczby grup pochodzą ze strony Gdańskiego Zespołu Żłobków (<a href="https://zlobki.gda.pl/index.php/kontakt/nasze-zlobki/">gzz.gda.pl)</a>.

### Lista dzielnic Gdańska
Aby przeanalizować sytuację w poszczególnych dzielnicach, wykorzystałam listę sektorów i dzielnic dostępną na stronie <b>Czyste Miasto Gdańsk</b> (<a href="https://czystemiasto.gdansk.pl/zdizgdanskfiles/image/gdansk_ulice_sektory_201307.pdf">czystemiasto.gdansk.pl</a>).<br>
Po wyodrębnieniu ulicy z adresu placówki w Rejestrze Żłobków mogłam za pomocą "inner joina" dołączyć spis ulic i dzielnic. W przypadku dłuższych ulic, przypisanych do więcej niż jednej dzielnicy, przyporządkowałam dzielnice ręcznie - takich przypadków było raptem kilkanaście.<br><br>
Dane dot. liczby mieszkańców poszczególnych dzielnic pobrałam ze strony Miasta Gdańsk (<a href="https://www.gdansk.pl/gdansk-w-liczbach/mieszkancy,a,108046">gdansk.pl</a>).<br>

### Mapa dzielnic Gdańska
Odrębną kwestią było przedstawienie danych na mapie dzielnic miasta. Nietety Power BI nie posiada dla Polski wbudowanych map na tak szczegółowym poziomie.<br>
Skorzystałam z udostępnionych przez Miasto Gdańsk plików SHP z granicami dzielnic (<a href="https://dane.gov.pl/pl/dataset/1821,granice-dzielnic-w-gdansku">dane.gov.pl</a>), a następnie za pomocą narzędzia mapshaper.org przekształciłam je do formatu .json obsługiwanego przez Power BI.<br>

### Dane ogólnopolskie
Na potrzeby obliczenia wskaźnika liczby żłobków na 1 tysiąc mieszkańców wykorzystałam:<br>
- Dane dot. ludności kraju na 31.12.2022 - z publikacji "Sytuacja demograficzna Polski do roku 2022" dostępnego na <a href="https://stat.gov.pl/obszary-tematyczne/ludnosc/ludnosc/sytuacja-demograficzna-polski-do-roku-2022,40,3.html">stronie GUS</a><br>
- Dane dot. ogólnopolskiej liczby żłobków w 2022 - z publikacji "Żłobki i Kluby Dziecięce w 2022 roku" dostępnej na <a href="https://stat.gov.pl/obszary-tematyczne/dzieci-i-rodzina/dzieci/zlobki-i-kluby-dzieciece-w-2022-roku,3,10.html">stronie GUS</a>
