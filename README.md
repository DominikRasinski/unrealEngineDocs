# Repozytorium przeznaczone do zapisywania notatek na temat silnika UnrealEngine

## UnrealEngine terminologia

`Project` projekt w silniku Unreal Engine przechowuje wszystkie pliki jakie znajdują się na dysku takie jak `Blueprints` oraz `Materials`.

- Organizacja plików i ich nazewnictwo wewnątrz projektu jest dowolna
- Panel `Content Browser` pokazuje tę samą strukturę plików jakie zostały utworzone wewnątrz projektu.

Każdy projekt posiada plik `.uproject` który jest powiązany z projektem. Plik `.uproject` jest odpowiedzialny za to jak tworzony, otwierany czy zapisywany jest projekt.

Możliwe jest tworzenie dowolnej ilości projektów i pracy na nich równolegle.

---

`BluePrint` umożliwia tworzenie gry za pomocą wizualnej części programowania. `BluePrint'y` udostępniają już gotowe systemy rozszerzające rozgrywkę, struktura gotowców opiera się na interfejsie opartym na węzłach.

Węzły są wyskorzystywane do definiowania obiektów, klas zorientowanych obiektowo (OO) w silniku.

---

`Object` obiekty to podstawa wykorzystywana wewnątrz silnika Unreal. Każda logika jest oparta na obiektach, sam silnik rozszerza lub dziedziczy nowe z podstawowych obiektów.

W języku C++ `UObject` jest bazą dla wszystkich klass obiektów, implementuje takie podstawowe mechanizmy jak `garbage collector`, `metadata` wspiera udostępnianie zmiennych, serializację, ładowanie oraz zapisywanie (`UProperty`)

---

`Class` klasy jak zazwyczaj w programowaniu definiują zachowanie oraz właściwości dla wskazanego `Actor'a` lub obiektu.
**Silnik Unreal wyskorzystuje klasy chierarchiczne** co oznacza, że klasy dziedziczą inforamcje z klas rodziców oraz przekazują inforamcje do swoich dziedzi.

Klasy można stworzyć za pomocą języka C++ lub wewnątrz `BluePrint'u`.

---

`Actor` aktorem jest dowolny objekt, który może zostać dodany do poziomu taki jak `Camera`, `static mesh` lub pozycja startowa gracza.
`Actors` wspierają 3D transformacje takie jak obracanie, skalowanie, translacja. Mogą być stworzeni (spawned), zniszczeni dzięki logicie C++ lub `BluePrint`

\*W dokumantacji będę na przemiennie wykorzystywać pojęcie Actor z Aktorem w zależności co lepiej pasuje do zdania.

---

`Casting` kastowanie jest operacją, która pobiera `Aktora` danej klasy i próbuje go potraktować jako część zupełnie innej klasy. Kastowanie `Aktora` może się powieść albo i nie, jeżeli kastowanie się powiedziecie to w takim momencie `Aktor` który został skastowany do innej klasy będzie posiadał dostęp do właśności nowej klasy.

Kastowanie **NIE** jest prostym sprawdzeniem czy dany `Aktor` należy do danej klasy, kastowanie dodaje `Aktorowi` dostęp do wybranej klasy. Sprawdzenie czy dany `Aktor` istnieje w wskazanej klasie zwróciłoby wartość `true` lub `false`, bez możliwości reagowania z własnościami klasy.

---

`Component` jest logiką, która może zostać dodana do `Aktora` rozszerzając jego możliwości. Komponenty muszę być dodane do `Aktora`, nie mogą istnieć bez bez niego.

---

`Pawn` (pionek) są podklasami `Aktora` i służą jako awatar albo osobowość wewnątrz gry, przykładem pionka może być NPC w grze. `Pionki` mogą być kontrolowane przez gracza albo AI jako NPC.

W momencie kiedy `Pionek` jest kontrolowany przez gracza albo AI jest uważany, że został `Opęntany` i w drugą stronę jeżeli `Pionek` nie jest kontrolowany przez gracza lub AI jest uważany jako `Nie opęntany`

---

`Character` jest podklasą klas `Pawn` oraz `Aktor`, która jest przeznaczona do użytku jako gracz. Podklasa `Character` dostarcza już wstępnie skonfigurowaną kolizję, ustawienia przycisków dla dwunożnego poruszania się oraz dodatkowy kod umożliwiający poruszanie się gracza.

---

`Player Controller` jest wyspecjalizowaną klasą która przyjmuje `input` gracza i tłumaczy go na interackję wewnątrz gry. `Player Controller` jest zazwyczaj opanowany przez klasę `Pawn` lub `Character` jako reprezentanta gracza w świecie gry.

Instancja kontrolera dla gracza jest podstawowym punktem interackcji gracza z grą multiplayer.
Podczas gry multiplayer serwer posiada jedną instację `Player Controller` dla każdego gracza w grze, ze względu na wykonywanie wymaganych funkcji umożliwających granie po sieci dla każdego gracza.
Każdy klient posiada tylko jeden `Player Controller` indedyfikujący gracza podczas komunikacji z serwerem.

Powiązana klasa C++ to `PlayerController`

---

`AI Controller` podobnie jak `Player controller` choć w tym wypadku opanowuje klase `Pawn` aby reprezentować NPC w grze.

Z Automatu dodanie `Pawn` lub `Character` skończy się wpięciem podstawowej funkcjonalności z kontrolera `AI Controller` chyba, że zostanie wskazane innaczej podczas dodawania ów klas.

Powiązana klasa C++ to `AIController`

---

`Player State` stan jest uczestnikiem w grze takim jak gracz lub bot który symuluje gracza.
Nie grywalne AI które jest częścia świata gry nie posiada dostępu do `Player State`

Przykładowe dane jakie `Player State` może przechowywać:

- Nazwa
- Aktualny poziom
- Ilość punktów życia
- Ilość punktów

Dla gier multiplayer `Player State` wszystkie maszyny uzyskują ten stan i replikują dane z serwera do klienta aby trzymać dane w synchronizacji. Zachowanie `stanu gracza` jest inne od zachowania `kontrolera gracza` ze względu na to, że kontroler tylko istnieje na maszynie gracza które reprezentuje.

Powiązana klasa C++ `PlayerState`

---

`Game Mode` tryb gry pozwala na ustawienie zasad gry jaka jest aktualnie grana.
Zasady mogą zawierać takie rzeczy jak:

- Jak gracze dołączają do gry
- W jakim momencie gra może zostać zapauzowana
- Każde zachowanie warunkujące wygraną

`Game Mode` domyślnie jest ustawiany w ustawieniach projektu (Project Settings) i zasady gry mogą być nadpisane dla każdego poziomu. Chociaż jest to opcjonalne, gra może powstać na podstawie tylko jednego `Trybu gry` dla każdego poziomu.

W grze multiplayer `tryb gry` tylko istnieje dla serwera i zasady są replikowane i wysyłane do połączonych klientów z serwerem.

Powiązana klasa C++ `GameMode`

---

`Game State` stan gry jest kontenerem, który przechowuje informacje jakie powinny zostać zreplikowane dla każdego klienta w grze.
W prostrzym języku to jest stan gry dla każdego podłaczonego gracza.

Przykładowe wartości jakie może `Stan gry` przetrzymywać:

- Informacje na temat punktacji gry
- Czy mecz się zaczął, czy jednak nie
- Ile graczy AI zrespawnować

Dla gier multiplayer jest tylko jedna instacja `Statnu gry` dla każdej maszyny gracza. Lokalnie przechowywana wartość `stanu gry` uzyskuje informacje z instacji `stanu gry` przetrzymywanego na serwerze.

Powiązana klasa C++ `GameState`

---

`Brush` jest `Aktorem` który opisuje kształt 3D taki jak sześcian albo kula.
`Pędzle` można ustawiać w projekcie aby definiować geometrię poziomu.

Zwane są również jako `Binary Space Partion` lub `BSP brushes`.

Aktorzy tego typu są pomocne do szybkiego ułożenia poziomu w projekcie.

---

`Volume` jest objętością ograniczającą przestrzeń 3D, która ma różne zastosowanie w zależności od tego z jakimi efektami jest powiązane.

Przykład:

- Blocking Volumes są nie widzialne i wykorzystywne są do tego aby `Aktor` nie mógł przejść przez dany `Volumen` (ograniczanie przejść)
- Pain Caousing Volumes przyczynia się do zadawania obrażeń gdy `Aktor` przechodzi przez
- Trigger Volumes sa zaprogramowane aby wywoływać wydarzenie kiedy `Aktor` wchodzi albo wychodzi z niego

---

`Level` jest strefą rozgrywki którą definiujemy. `Poziomy` zawierają wszystkie elementy z jakie gracz może widzieć oraz ingerować.

Silnik Unreal zapisuje każdy poziom jako oddzielnyu `.umap` plik. Te pliki są czasami nazywane jako `Maps`

---

`World` jest kontenerem dla wszystkich poziomów jakie tworzą grę.
Zajmuje się całym procesem strumieniowania poziomów oraz sprawnowania/tworzenia dynamicznych `Aktorów`
