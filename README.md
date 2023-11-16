# Żłobki i kluby dziecięce w Gdańsku
<i>Power BI report about daycare facilities in Gdansk.</i>
<br><br>
:point_right: Zobacz raport na żywo w przeglądarce: 
<a href="https://app.powerbi.com/view?r=eyJrIjoiN2E1OWQ0N2EtNzFkNC00NTc0LTk4NjgtNWY5Y2I0NzRjOTM2IiwidCI6Ijk3NmI5Y2IwLWJiYjctNDg2NC04NjAwLTE1NTk4MzA5YjY3YiJ9">LINK</a>
<br><br>
<img src="https://raw.githubusercontent.com/jusjag/ZlobkiGdansk/main/ZlobkiGdansk_raport.png">

## Projekt
Cel: praktyczne wykorzystanie umiejętności pracy z Power Query, Power BI i DAX.<br>
Dane: publicznie dostępne dane z Rejestru Żłobków, uzupełnione o inne informacje wymienione w sekcji poniżej. Stan na rok 2022/2023.<br>

## 1. Źródła danych
Rejestr Żłobków i Klubów Dziecięcych (dane.gov):<br>
Dane: Numer rejestru, dane adresowe, liczba miejsc, zapisanych dzieci, informacje nt. opłat. Data dostępu: 01.11.2023<br>
Link: https://dane.gov.pl/pl/dataset/2086/resource/48743,rejestr-zobkow/table?page=1&per_page=20&q=&sort=<br><br>
<b>WAŻNE: </b><br>
1. Choć dane są oznaczone jako aktualne na 23.06.2023, zawierają błędy bądź nieaktualne dane - na liście znajduje się np. żłobek zamknięty od ponad roku. Trudno samodzielnie ocenić skalę nieprawidłowości w pliku. W raporcie wykorzystałam dane dot. opłat i ilości miejsc, samą listę czynnych żłobków oparłam jednak o dane z Empatii.<br>
2. Warto pamiętać, że informacje nt. ilości wolnych miejsc oraz opłat mogą zmieniać się bardzo dynamicznie i nie być aktualne już w dniu tworzenia raportu - zebrane informacje oddają jednak ogólną tendencję w skali miasta.<br>
<br>
Rejestr Żłobków i Klubów Dziecięcych (Empatia, mpips.gov.pl):<br>
Dane: Numer rejestru, dane adresowe. Data dostępu: 02.11.2023<br>
Link: https://empatia.mpips.gov.pl/dla-swiadczeniobiorcow/rodzina/d3/rejestr-zlobkow-i-klubow﻿<br>
<br>
Lista dzielnic z przypisanymi im ulicami (Czyste Miasto Gdańsk):<br>
Dane: Przyporządkowanie ulic lub ich fragmentów do dzielnic miasta. Data dostępu: 12.11.2023<br>
Link: https://czystemiasto.gdansk.pl/zdizgdanskfiles/image/gdansk_ulice_sektory_201307.pdf <br>
<br>
Liczba mieszkańców Gdańska oraz poszczególnych dzielnic (gdansk.pl):<br>
Dane: Nazwy dzielnic, liczba mieszkańców. Data dostępu: 12.11.2023. Aktualne na dzień: 31.12.2022<br>
Link: https://www.gdansk.pl/gdansk-w-liczbach/mieszkancy,a,108046 <br>
<br>
Granice dzielnic Gdańska (dane.gov.pl):<br>
Dane: plik SHP zawierający nazwy i przebieg granic dzielnic Gdańska; przekonwertowany do formatu json. Data dostępu: 12.11.2023.<br>
Link: https://dane.gov.pl/pl/dataset/1821,granice-dzielnic-w-gdansku<br>
<br>
Dane dot. ludności kraju na 31.12.2022 (GUS):<br>
Link: https://stat.gov.pl/obszary-tematyczne/ludnosc/ludnosc/sytuacja-demograficzna-polski-do-roku-2022,40,3.html<br>
<br>
Dane dot. ogólnopolskiej liczby żłobków w 2022 (GUS):<br>
Link: https://stat.gov.pl/obszary-tematyczne/dzieci-i-rodzina/dzieci/zlobki-i-kluby-dzieciece-w-2022-roku,3,10.html<br>
<br>

## 2. Przygotowanie danych
Ten projekt wymagał ode mnie bardzo dużo zakulisowej pracy z zebraniem oraz przygotowaniem danych. Po raz kolejny przekonałam się, jak bardzo lubię pracę z Power Query :)<br><br>
1. Podstawą raportu jest zestaw danych z Rejestru Żłobków (źródło). Zawiera szereg szczegółowych informacji nt. numeru rejestru, danych adresowych, kontaktowych, liczby dostępnych i zajętych miejsc, kosztów, źródeł finansowania oraz dostosowań do specjalnych potrzeb.<br>
Przygotowanie danych obejmowało głównie odfiltrowanie placówek spoza Gdańska, usunięcie niepotrzebnych bądź nieużywanych kolumn (dane kontaktowe, udogodnienia) oraz ustawienie odpowiednich typów danych.
2. Przeglądając dane zauważyłam, że nie są one aktualne, na liście znajduje się np. żłobek o którym wiem, że jest zamknięty od ponad roku. Dlatego zdecydowałam się na pobranie danych również z drugiego źródła: portalu Empatia (link). Choć dane są przedstawione w sposób wygodny do przeglądania, nie są jednak możliwe do pobrania w prosty sposób - są ładowane na bieżąco w ramce zamieszczonej wewnątrz strony. Udało mi się jednak dotrzeć do źródła ramki, ręcznie rozwinąć całą potrzebną listę, zapisać stronę na dysku i takie dane załadować do Power Query.
3. Za pomocą inner joina połączyłam dane z obu powyższych tabel, otrzymując listę 102 placówek. Choć trudno samodzielnie określić stopień rzetelności danych, zakładam że oddają one ogólny obraz sytuacji żłobków i klubów dziecięcych w Gdańsku.
4. Zależało mi na tym, aby pokazać dane na mapie dzielnic Gdańska. Power BI nie obsługuje jednak bezpośrednio takich danych, a mimo wielu prób nie udało mi się stworzyć prawidłowej mapy dzielnic z użyciem wbudowanych narzędzi.
