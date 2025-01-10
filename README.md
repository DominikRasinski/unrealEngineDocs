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

TODO: opisać zachowanie kontrolera gracza dla gry multiplayer

Powiązana klasa C++ to PlayerController

---

`AI Controller` podobnie jak `Player controller` choć w tym wypadku opanowuje klase `Pawn` aby reprezentować NPC w grze.

Z Automatu dodanie `Pawn` lub `Character` skończy się wpięciem podstawowej funkcjonalności z kontrolera `AI Controller` chyba, że zostanie wskazane innaczej podczas dodawania ów klas.

Powiązana klasa C++ to AIController

---

TODO: dokończyć

- `Player State`
- `Game Mode`
- `Game State`
- `Brush`
- `Volume`
- `Level`
- `World`
