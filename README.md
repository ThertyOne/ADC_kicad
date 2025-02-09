# mini-project ADC

## Opis projektu
Projekt przedstawia układ do precyzyjnej konwersji analogowo-cyfrowej oparty na przetworniku ADC ADS8881, wspomaganym przez wzmacniacze operacyjne oraz stabilizatory napięcia. Układ został zoptymalizowany względem poprzedniej wersji, uwzględniając poprawki dotyczące źródła napięcia referencyjnego, filtracji sygnału wejściowego oraz zasilania.

## Zmiany względem poprzedniej wersji
1. **Implementacja układu z rysunku 70 z dokumentacji ADS8881** – poprawiona konfiguracja układu przetwornika ADC.
2. **Napięcie referencyjne** pochodzi teraz ze stabilizatora LDO, które również zasila wzmacniacze operacyjne w układzie. Napięcie wejściowe wynosi 12V.
3. **Filtr wejściowy** został poprawiony – zastosowano górnoprzepustowy filtr o częstotliwości odcięcia **48 Hz**.
4. **Dodano stabilizator napięcia 3V**, który zasila przetwornik ADS8881, zgodnie z jego specyfikacją (dopuszczalne napięcie zasilania od 2.7V do 3.6V).

## Kluczowe elementy układu
- **Mikrokontroler**: Raspberry Pi Pico – sterowanie przetwornikiem ADC.
- **Przetwornik ADC**: ADS8881 – 18-bitowy, szybki i precyzyjny konwerter analogowo-cyfrowy.
- **Stabilizatory napięcia**:
  - **TPS7A49** – stabilizator napięcia referencyjnego.
  - **LM4040** – dodatkowe źródło referencyjne 3V dla ADS8881.
- **Wzmacniacze operacyjne**:
  - **OPA320** – wzmacniacz buforujący sygnał wejściowy.
  - **THS4521** – różnicowy wzmacniacz sygnału dla przetwornika ADC.

## Power Supply (LDO)
Układ zawiera kilka wzmacniaczy operacyjnych, które wymagają stabilnego zasilania. Wybrano zasilanie 5V, ponieważ wszystkie użyte wzmacniacze operacyjne mogą pracować przy tym napięciu. Stabilizator niskoszumowy (LDO) **TPS7A49** został wybrany ze względu na swoje niskie szumy (15,4 µV RMS) oraz prostą konfigurację napięcia wyjściowego przy użyciu jedynie dwóch rezystorów. Dodatkowo zastosowano diodę TVS do ochrony układu przed przepięciami oraz wskaźnik napięcia wyjściowego 5V.

## LM4040 Voltage Reference
Aby zasilić zarówno sekcję analogową, jak i cyfrową przetwornika **ADS8881** (który działa w zakresie 2,7V - 3,6V), użyto stabilizatora **LM4040**, który konwertuje 5V z LDO na 3V dla ADC. Dzięki temu poziomy logiczne na wyjściu ADC będą miały maksymalne napięcie 3V, co nie stanowi problemu dla **Raspberry Pi Pico**, gdyż powinien on niezawodnie rozpoznawać 2,7V jako wysoki poziom logiczny.

## Input Signal Processing
Sygnał wejściowy z fotodiody przechodzi przez filtr górnoprzepustowy **50 Hz**, a następnie przez wzmacniacz, który wzmacnia sygnał i izoluje go od obciążenia.

## Analog-to-Digital Conversion (ADC)
Zgodnie z zaleceniami z poprzedniego maila wdrożono konfigurację **ADS8881** zgodnie z **rysunkiem 70** z dokumentacji. Konfiguracja ta jest zasilana **5V z LDO**, które jednocześnie pełni rolę napięcia referencyjnego dla ADC. Jak wspomniano wcześniej, sam ADC jest zasilany napięciem **3V ze stabilizatora LM4040**, a obie sekcje – analogowa i cyfrowa – są wyposażone w **kondensatory odsprzęgające 1µF**. Linie komunikacyjne są podłączone do **Raspberry Pi Pico**, które odpowiada wyłącznie za wyzwalanie pomiaru i odczyt danych z ADC.

## Schemat
Projekt został opracowany w **KiCad** i zapisany w pliku `adc.kicad_sch`. Schemat zawiera podział na bloki funkcjonalne:
- Stabilizacja napięcia (LDO, stabilizator 3V)
- Filtracja sygnału wejściowego (filtr górnoprzepustowy)
- Konwersja analogowo-cyfrowa (ADS8881 + wzmacniacze operacyjne)
- Sterowanie i komunikacja z mikrokontrolerem

## Wymagane napięcia
- **Vin**: 12V
- **Vref**: 5V (ze stabilizatora LDO)
- **Zasilanie ADC**: 3V
