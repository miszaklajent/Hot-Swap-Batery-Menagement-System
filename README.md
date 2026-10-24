# Alizka power management system

## założenia
- modułowy system
- możliwość instalacji od 1 do 4 baterii
- redundancja baterii
- zabezpieczenie nadmiernego rozładowania baterii
- rozładowuje wszystkie podłączone baterie po kolei, w kolejności od najbardziej rozładowanej do najbardziej naładowanej.
- w trakcie działania przekazuje moc podłączonych baterii w sposób nie przerwany
- pozwala na przesył do 400W mocy (25A 16V przy prawie rozładowanej baterii)

## działanie 

### Załączenie:
1. Podczas pierwszego załączenia system wybiera najbardziej rozładowaną baterię, której napięcie przekracza minimalny poziom napięcia
2. System wykonuje soft-start aby ograniczyć prąd ładowania pojemności wyjściowej.
3. Zwiększanie wypełnienia przebiegu sterującego układem przetwornicy, do momentu osiągnięcia pracy ciągłej górnego tranzystora.

### Praca ciągła:
1. Podczas pracy ciągłej, przetwornica modułu baterii która aktualnie jest wykorzystywana, ma na stałe załączony górny tranzystor.
2. Praca ciągła trwa do momentu kiedy:
    - Napięcie na aktualnie wykorzystywanej baterii spadnie poniżej napięcia minimalnego
    - Bateria aktualnie wykorzystywana zostanie odłączona

### Przełączenie baterii:
1. Przed przełączeniem, system wybiera jedną z pozostałych baterii, na którą zaraz się przełączy. Będzie to następna bateria o najniższym napięciu, ale przekraczającym napięcie minimalne.
2. Przetwornica baterii wybranej do przełączenia jako następna, cały czas kluczuje w trybie stand-by (jej wyjście jest aktywnie ustalone na poziomie ok 100mV poniżej aktualnego wyjścia działającej baterii przez mikrokontroler)
3. Podczas przełączenia, przetwornica baterii o wyższym napięciu, zaczyna zwiększać wypełnienie do momentu osiągnięcia pełnego wypełnienia.
4. Podczas zwiększania wypełnienia przetwornicy baterii o wyższym napięciu, układ diody idealnej zawarty za wyjściem przetwornicy zapobiegnie przepływaniu prądów wyrównawczych.
5. Przypadku nagłego wypięcia baterii, diody idealne automatycznie przełączając tor mocy w taki sposób, aby moc była pobierana z przetwornicy która była w trybie stand-by, co powoduje jedynie nieznaczne obniżenie napięcia wyjścia. Po wykryciu odłączenia, mikrokontroler podnosi wypełnienie sygnału sterującego tak jak w punkcie wyżej.

