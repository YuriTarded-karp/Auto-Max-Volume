# BT Max 1.0

Aplikacja na Androida 13 lub nowszego, która utrzymuje maksymalną głośność multimediów dla wybranego wyjścia Bluetooth. Interfejs jest po polsku. Nie wymaga roota, konta ani Internetu.

## Instalacja i uruchomienie

1. Pobierz `BT-Max-1.0.apk` na telefon i otwórz plik. Jeśli Android poprosi, pozwól aplikacji, z której otwierasz APK, instalować aplikacje z tego źródła.
2. Połącz głośnik lub słuchawki z telefonem w ustawieniach Bluetooth. Włącz dla urządzenia dźwięk multimediów.
3. Otwórz **BT Max**. Wpisz dokładną nazwę urządzenia lub użyj **Wybierz połączone urządzenie**. Wybór z listy jest pewniejszy: zapisuje także adres urządzenia, gdy Android go udostępnia.
4. Naciśnij **Włącz ochronę · 100%**. Przyznaj dostęp do urządzeń w pobliżu oraz powiadomień i potwierdź włączenie maksymalnej głośności.
5. Uruchom muzykę. W aplikacji powinien pojawić się stan **Chronione**. Obniż głośność telefonu — aplikacja powinna przywrócić maksimum, zwykle w około sekundę.
6. Zatrzymaj ochronę przyciskiem w aplikacji albo **STOP** w powiadomieniu, kiedy chcesz normalnie regulować głośność.

Ochrona działa także po zamknięciu ekranu aplikacji i przy zgaszonym ekranie. Usługa pierwszoplanowa i czasowo odnawiana blokada uśpienia zwiększają zużycie baterii. Jeśli producent telefonu ubija aplikację, ustaw dla BT Max nieograniczone użycie baterii w ustawieniach systemowych. Po wymuszonym zatrzymaniu, restarcie telefonu lub odebraniu uprawnień otwórz aplikację i uruchom ochronę ponownie. Automatycznego startu po restarcie nie ma.

## Co dokładnie wykrywa

Aplikacja odczytuje systemową głośność multimediów co 350 ms i w razie obniżenia próbuje ustawić maksymalny poziom. Ponowne próby są ograniczone do jednej na sekundę. Podgłaśnia także przy pierwszym aktywowaniu ochrony oraz po ponownym połączeniu wskazanego wyjścia.

Obsługiwane są wyjścia A2DP i LE Audio. Aplikacja kontroluje głośność multimediów, a nie rozmów, alarmów ani dzwonka. Nie rozpoznaje, kto obniżył głośność — reaguje na każdą widoczną dla Androida zmianę.

Zmiana przyciskiem lub pokrętłem urządzenia Bluetooth jest wykrywalna, gdy urządzenie synchronizuje głośność z telefonem (Bluetooth Absolute Volume). Jeśli pokrętło zmienia tylko lokalną głośność wzmacniacza, Android może nie otrzymać informacji. Wtedy ten program nie wykryje ani nie cofnie tej zmiany. Nie istnieje uniwersalna obsługa niezależnych regulatorów wszystkich głośników.

Ograniczenia głośności narzucone przez Androida nie są omijane. Gdy system nie pozwala osiągnąć maksimum, aplikacja pokazuje ograniczenie. Na urządzeniach ze stałą głośnością funkcja nie działa.

## Sprawdzanie wyjścia audio

Przed zmianą głośności aplikacja sprawdza publicznym API Androida przewidywane wyjście dla multimediów. Jeśli wskazane urządzenie jest rozłączone, wybrane jest inne wyjście albo nazwa jest niejednoznaczna, ochrona czeka. Podczas rozmowy lub innego trybu połączenia audio jest wstrzymana. API zwraca przewidywaną trasę dla standardowych atrybutów multimediów; aplikacje stosujące własne kierowanie dźwięku i rozwiązania producentów telefonów mogą zachowywać się inaczej. Samo sprawdzenie i ustawienie głośności nie jest jedną atomową operacją systemu.

## Budowanie projektu

Otwórz folder `BTMax` w Android Studio, zainstaluj Android SDK Platform 35 i Build Tools 35.0.0, poczekaj na synchronizację Gradle, następnie użyj opcji budowania APK w menu Build. Projekt zawiera Gradle Wrapper 8.11.1, Android Gradle Plugin 8.9.1 i używa Java 17.

Z terminala w folderze projektu:

```sh
./gradlew assembleDebug lintDebug
```

Windows:

```bat
gradlew.bat assembleDebug lintDebug
```

Plik wynikowy: `app/build/outputs/apk/debug/app-debug.apk`.

APK dołączony do wydania jest podpisaną wersją debug do bezpośredniej instalacji. Własna kompilacja z innym kluczem nie zaktualizuje istniejącej instalacji — w takim przypadku odinstaluj poprzednią wersję lub podpisz nową tym samym kluczem. Publikacja w Google Play wymaga osobnego podpisania wydania i spełnienia zasad sklepu dla usług pierwszoplanowych.

## Weryfikacja

- Kompilacja APK: wykonana.
- Android Lint: końcowy wynik w `WERYFIKACJA.txt`.
- Weryfikacja podpisu APK: wykonana.
- 10 testów polityki dopasowania urządzenia i trasy: wykonane (`python3 tests/test_devices.py`; wymaga JDK 17).
- Testy na fizycznym telefonie i urządzeniu Bluetooth: nie wykonano w tym środowisku. Testy hostowe używają uproszczonych atrap API i nie dowodzą zachowania konkretnego telefonu.

Próba na telefonie: obniż głośność przyciskiem telefonu, następnie przyciskiem głośnika, sprawdź ochronę po zgaszeniu ekranu, rozłącz Bluetooth i sprawdź, że telefon nie jest podgłaśniany, połącz ponownie, a na końcu użyj STOP i sprawdź, że głośność pozostaje obniżona. Jeśli przycisk głośnika nie zmienia suwaka telefonu, głośnik najprawdopodobniej używa niezależnej regulacji.

## Dokumentacja Androida

- AudioManager: https://developer.android.com/reference/android/media/AudioManager
- Wyjścia audio: https://developer.android.com/media/platform/output
- Usługi pierwszoplanowe: https://developer.android.com/develop/background-work/services/fgs/service-types
- Bluetooth i synchronizacja głośności: https://source.android.com/docs/core/connect/bluetooth/services
