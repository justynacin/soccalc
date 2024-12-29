# soccalc
Calculating cost and profit margin for SOC Sales Department
Założenia: 
Firma X poprosiła mnie o przygotowanie kalkulatora dla działu handlowego obliczającego koszt usługi dla trzech oferowanych pakietów. W pakietach MDR i SOC koszt jest uzależniony od ilości deklarowanych przez klienta użytkowników. Do pakietu SOC Premium można dokupić dodatki, których koszt wyrażony jest w USD lub EUR. 
Arkusze:
1. dane: zawiera przykładowe obliczenia dla pakietu MDR używając przelicznika dostępnego poniżej (przydatne do użytku wewnętrznego); b. kurs USD i EUR dla klientów rozliczających się w innej walucie niż złotówki i dla kalkulacji dodatków SOC Premium.
Arkusz zawiera wyłącznie dane do użytku wewnętrznego, w przypadku udostępnienia handlowcowi lub innej osobie postronnej arkusz byłby ukryty.
2. user conv: zawiera tabelę uwzględniającą przelicznik z arkusza Dane, skalującą koszt do liczby użytkowników usługi. Wybrałam vlookup, żeby uniknąć tworzenia wielopoziomowego wyrażenia IF. Komórka Raw Cost odpowiada kosztowi jednego pracownika dla danego klienta: 
- koszt zatrudnienia pracownika firmy X wynosi 10500 PLN
- firma zakłada, że 1 pracownik jest w stanie opiekować się 3 klientami, stąd koszt jego zatrudnienia jest dzielony przez 3
Podobnie jak user conv, arkusz byłby ukryty przed innymi użytkownikami.  
3. MDR view - możliwość edycji dostępna jest tylko dla komórek C2:C5. Użytkownik podaje: liczbę użytkowników, procent marży i marżę partnera / strony trzeciej. Google Sheets umożliwia zapisywanie formuł jako własnych funkcji, więc dla MDR zapisałam dwie w celu uproszczenia zapisywanych danych:
MDR_RAW =vlookup(komórka;'user conv'!$A:$B;2)*'user conv'!$F$2 - koszt, jaki firma musi ponieść dla usługi o podanej liczbie użytkowników
MDR_TOTAL=vlookup(komórka;'user conv'!$A:$B;2)*'user conv'!$F$2*(1+('MDR view'!$C$3+'MDR view'!$C$4)) - całkowity koszt z marżami - MDR_RAW przemnażam przez 1,x (gdzie x to procent nadanej marży)
4. SOC view -  analogicznie jak w MDR, zapisałam dwie customowe funkcje:
SOC_TOTAL=vlookup(komórka;'user conv'!$A:$C;3)*'user conv'!$F$2*(1+('SOC view'!$C$3+'SOC view'!$C$4))
SOC_RAW=vlookup(komórka;'user conv'!$A:$C;3)*'user conv'!$F$2
Arkusze różnią się jedynie kolumną, z której pobierany jest przelicznik liczby użytkowników.
5. SOC Premium view - do przelicznika użytkowników i marży należy tu dodać koszt każdego dodatku. W komórce C8 zastosowałam formułę SUMIF, która weryfikuje kolumnę E. Jeśli E jest zaznaczone (TRUE), przelicza koszt dodatku z kolumny G. SUMIF użyłam także w komórce C6. Klient nalicza marżę osobno przy wybraniu dodatku (ze względu na koszt obsługi) i osobno przy przygotowywaniu pełnej oferty dla klienta.  Zysk firmy według wymagań klienta być wyszczególniony osobno, a więc wartość marży w komórce C8 musi więc wyrażać sumę dwóch zebranych marż.
