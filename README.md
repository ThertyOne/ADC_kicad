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


