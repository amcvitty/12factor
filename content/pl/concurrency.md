## VIII. Współbieżność

### Skaluj przez odpowiednio dobrane procesy

Każdy program komputerowy od momentu uruchomienia jest reprezentowany przez jeden lub więcej procesów. Aplikacje internetowe mogą być uruchamiane w różnorodny sposób. Dla przykładu - procesy PHP uruchamiane są na żądanie (w zależności od potrzeby obsługi odpowiednio dużej liczby zapytań) jako podrzędne procesy Apache'a. W Javie procesy obsługiwane są zupełnie inaczej, z JVM zapewniającym jeden nadrzędny proces, który rezerwuje zasoby systemu (CPU oraz pamięć) na starcie oraz współbieżnością zarządzaną wewnętrznie i opartą na wątkach. Dla developerów aplikacji różnica jednak nie będzie szczególnie odczuwalna.

![Skala wyrażana jest przez działające procesy, natomiast różnorodność obciążenia wyrażana jest w typach procesów](/images/process-types.png)

**W aplikacji 12factor, procesy są typem pierwszoklasowym** Zachowanie tych procesów jest mocno wzorowane na [modelu procesów unixowych dla usług działających w wewnątrz systemu operacyjnego](https://adam.herokuapp.com/past/2011/5/9/applying_the_unix_process_model_to_web_apps/). Używając tego modelu programista może zaprojektować aplikację by radziła sobie z różnorodnym obciążeniem przez przypisywanie każdej czynności do _typu procesu_. Przykłady to m.in obsługa procesów sieciowych przez HTTP oraz długotrwałe działanie zadań w tle opierających się na procesach roboczych.

Mimo tego procesy wciąż mogą się zwielokrotnić przez wątki w środowisku maszyny wirtualnej lub w asynchronicznym modelu wydarzeń, którego implementację możemy znaleźć wśród narzędzi takich jak [EventMachine](https://github.com/eventmachine/eventmachine), [Twisted](http://twistedmatrix.com/trac/), albo [Node.js](http://nodejs.org/). Należy pamiętać, że pojedyncza maszyna wirtualna może z czasem wymagać coraz więcej zasobów (skala pionowa), dlatego aplikacja musi być również w stanie pracować w oparciu o wiele procesów działających na wielu fizycznych maszynach.

Największa zaleta modelu procesów objawia się w momencie skalowania. [Niezależność oraz dzielenie się na podprocesy](./processes) umożliwia proste i bezproblemowe dodawanie wiekszej liczby równolegle działajacych procesów. Tablica typów procesów i liczba procesów nazywana jest ich _formacją_.

Procesy aplikacji 12factor [nigdy nie powinny być uruchamiane w tle](https://dustin.sallings.org/2010/02/28/running-processes.html) i nie mogą zapisywać plików PID. Zamiast tego opierają się na narzędziach systemu operacyjnego: do zarządzania procesami (np. [systemd](https://www.freedesktop.org/wiki/Software/systemd/), do zarządzania rozproszonymi procesami w chmurze, lub [Foreman](http://blog.daviddollar.org/2011/05/06/introducing-foreman.html) w developmencie) do zarządzania [stumieniami wyjściowymi](./logs), do obsługi zatrzymanych procesów, restartu i zakończenia działań zainicjowanych przez użytkownika.
