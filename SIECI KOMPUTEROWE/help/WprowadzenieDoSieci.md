[Wprowadzenie do sieci komputerowych]

Cel: Nauka podstawowych pojęć

Wymagania / środowisko:
-urządzenie do wyświetlenia tego pliku



Najważniejsze pojęcia:


Sieć komputerowa - to zbiór połączonych ze sobą urządzeń (komputerów, serwerów, drukarek, routerów itp.), które mogą wymieniać dane za pomocą ustalonych zasad komunikacji (protokołów).
Celem sieci jest udostępnianie zasobów, komunikacja, współdzielenie danych oraz zapewnienie dostępu do usług.


Host - urządzenie końcowe, z którego korzysta użytkownik i które generuje lub odbiera dane.
Host ma adres IP i komunikuje się bezpośrednio w sieci.Przykłady: komputer, laptop, smartfon, drukarka sieciowa, serwer.


Węzeł (node) - każde urządzenie, które jest częścią sieci i bierze udział w przesyłaniu danych.
Może to być urządzenie, które nie musi generować ani odbierać danych użytkownika, ale przekazuje ruch dalej. Przykłady węzłów:router, switch, punkt dostępowy (AP), serwer, komputer, drukarka sieciowa.


Serwer - komputer świadczący usługi (np. WWW, poczta, pliki) innym urządzeniom zwanym klientami.
Klient wysyła zapytanie -> serwer je przetwarza -> odsyła odpowiedź. Serwer może być jednocześnie: urządzeniem fizycznym, programem uruchomionym na komputerze, wirtualną maszyną w chmurze.


Klient - to urządzenie lub program, który inicjuje komunikację z serwerem i korzysta z jego usług. Klient wysyła żądanie -> serwer odpowiada. Klientem może być: urządzenie (komputer, telefon, tablet), aplikacja (przeglądarka, program pocztowy), inny serwer (np. mikroserwisy). Przykłady klientów: klient WWW aplikacja: Chrome, Firefox, Edge - łączy się z serwerami WWW i pobiera strony (HTTP/HTTPS). Klient pocztowy aplikacja: Outlook, Thunderbird - odbiera/wysyła e-maile przez serwery SMTP, IMAP lub POP3.Aplikacje mobilne - każda aplikacja korzystająca z internetu jest klientem (np. TikTok, Messenger).


Protokół sieciowy to zbiór zasad, reguł i standardów, które mówią urządzeniom w sieci, jak mają się ze sobą komunikować. Określa m.in.: jak wygląda wiadomość (struktura, nagłówki), w jakiej kolejności urządzenia wymieniają dane, jak wykrywać błędy i jak ponawiać transmisję, jak nawiązywać i kończyć połączenie, co robić, gdy wystąpi błąd lub przerwa w komunikacji. Dzięki protokołom różne urządzenia i oprogramowanie mogą współpracować. Przykłady: TCP, IP, HTTP, DNS, DHCP.


Adres IP to unikalny, logiczny numer identyfikujący każde urządzenie podłączone do sieci korzystającej z protokołu IP (np. Internetu). Działa jak „adres domu”, który pozwala kierować pakiety we właściwe miejsce. IP = Internet Protocol Address. Każdy host w sieci musi mieć swój adres IP. Adres IP określa lokalizację w sieci, a nie fizyczne urządzenie. Może być prywatny (wewnątrz domu) lub publiczny (widoczny w Internecie). Może zmieniać się dynamicznie (DHCP).


Adres MAC (Media Access Control) to fizyczny, fabrycznie nadany identyfikator karty sieciowej (np. Wi-Fi lub Ethernet). Ma 48 bitów (6 bajtów), np. 00:1A:2B:3C:4D:5E. Pierwsze 3 bajty oznaczają producenta (OUI), ostatnie 3 — konkretne urządzenie. Służy do identyfikacji urządzeń w sieci lokalnej (LAN). Nie jest używany w Internecie — każdy router „zdejmuje” ramkę z MAC i tworzy nową na kolejne połączenie. IP określa cel globalny, a MAC służy tylko do przekazania danych między dwoma sąsiednimi urządzeniami w tej samej sieci.


Router to urządzenie łączące różne sieci i kierujące pakiety z jednej sieci do drugiej. Router łączy różne sieci IP, a nie tylko urządzenia — dlatego jest kluczowy dla działania Internetu. Jego główne zadania to:
Routing – decydowanie, którędy mają iść pakiety.
NAT – tłumaczenie adresów prywatnych (LAN) na publiczny adres Internetu.
DHCP – automatyczne przydzielanie adresów IP w sieci domowej.
Firewall – ochrona sieci poprzez filtrowanie ruchu.
Access Point Wi-Fi – udostępnianie sieci bezprzewodowej (jeśli ma moduł Wi-Fi).
Lokalny DNS cache – buforowanie odpowiedzi DNS dla szybszego ładowania stron.


Switch to urządzenie, które łączy hosty w tej samej sieci lokalnej (LAN) i przesyła dane na podstawie adresów MAC, a nie adresów IP (tych nie analizuje na swojej warstwie). Pracuje w 2. warstwie modelu OSI – warstwie łącza danych. Switch uczy się adresów MAC: gdy host wyśle ramkę, switch zapisuje w swojej tablicy MAC adres źródłowy i port, z którego przyszła – tak tworzy mapę MAC -> port. Dzięki temu przesyła ramki tylko na ten port, gdzie znajduje się odbiorca, a nie do wszystkich. Może tworzyć VLAN-y – czyli logicznie oddzielone sieci w jednym fizycznym urządzeniu (np. VLAN 10 = biuro, VLAN 20 = goście). W odróżnieniu od routera nie oddziela sieci, tylko tworzy jedną sieć lokalną (chyba że używamy VLAN-ów).


Hub to bardzo proste urządzenie sieciowe, dziś prawie nieużywane. Pracuje w 1. warstwie OSI – warstwie fizycznej. Nie rozpoznaje adresów MAC. Ramkę wysyła do wszystkich portów, niezależnie od tego, do kogo jest skierowana. Każdy odbiorca musi zdecydować, czy ramka jest do niego. Tworzy jedną dużą domenę kolizji -> wolny i niebezpieczny.


Pakiet danych to podstawowa jednostka przesyłania informacji w sieci opartej na protokole IP. Zawiera nagłówek (np. adres źródłowy i docelowy IP, informacje o routingu) oraz dane (ładunek – payload). Pakiet to jednostka warstwy 3 (IP). Później pakiet jest zamykany w ramkę Ethernet (z adresami MAC) podczas transmisji w sieci lokalnej. Można to przedstawić jako „opakowania”: ramka (MAC) -> w środku pakiet (IP) -> w środku segment (TCP/UDP) -> w środku dane aplikacji.


LAN to lokalna sieć komputerowa, obejmująca niewielki obszar, np.: dom, mieszkanie, biuro, małą firmę, szkołę lub jedno piętro budynku. Charakterystyka: wysoka prędkość (Ethernet, Wi-Fi), własna infrastruktura właściciela (administrator zarządza całą siecią), zwykle prywatne adresy IP, urządzenia są fizycznie blisko siebie.


MAN to sieć miejska, obejmująca większy obszar niż LAN, ale mniejszy niż WAN — zwykle obszar jednego miasta lub aglomeracji. Przykłady: sieć światłowodowa operatora w mieście, miejska sieć łącząca szkoły, urzędy, biblioteki, sieć kampusu uniwersyteckiego obejmująca wiele budynków. Charakterystyka: łączy wiele LAN-ów w jedną większą sieć, często należy do operatora telekomunikacyjnego lub instytucji miejskiej.


WAN to sieć rozległa, obejmująca bardzo duży obszar: kraj, kontynent, wiele państw, a nawet cały świat. Największym przykładem WAN jest Internet. Charakterystyka: łączy ze sobą różne sieci lokalne (LAN) i miejskie (MAN), działa na infrastrukturze wielu operatorów, prędkość i opóźnienia zależą od odległości i rodzaju łączy.



Jednostki przesyłu danych


Rozmiary danych:


bit (b) – podstawowa jednostka

bajt (B) – 8 bitów

kilobajt (KB) – 1 024 B

megabajt (MB) – 1 024 KB

gigabajt (GB) – 1 024 MB

terabajt (TB) – 1 024 GB



Przepustowość łącza (szybkość transmisji) – zawsze w bitach na sekundę:


bit na sekundę (bps) – podstawowa jednostka

kilobit na sekundę (kbps) – 1 000 bps

megabit na sekundę (Mbps) – 1 000 kbps

gigabit na sekundę (Gbps) – 1 000 Mbps


Media transmisyjne (rodzaje połączeń):



Przewodowe:

Skrętka (UTP - nieekranowana / STP) - najpopularniejszy kabel sieci LAN, tania, łatwa w montażu, zasięg do kilkuset metrów

Światłowód - największa szybkość i odporność na zakłócenia, bardzo duże zasięgi, przepustowości rzędu setek Gbps i więcej

Kabel koncentryczny - starsza technologia (telewizja, sieci kablowe). Obecnie rzadziej używany w LAN.



Bezprzewodowe:

Wi-Fi - połączenia lokalne do kilkudziesięciu metrów

Bluetooth - bardzo krótki zasięg, do urządzeń peryferyjnych (myszki, słuchawki, klawiatury)

LTE / 5G - sieci komórkowe, duże zasięgi i wysokie prędkości




Topologie sieciowe


Magistrala (Bus) - wszystkie urządzenia są podłączone do jednego wspólnego kabla. Tania, ale awaria kabla powoduje problem całej sieci.


Gwiazda (Star) - każde urządzenie łączy się ze switchem. Najpopularniejsza topologia w sieciach LAN. Awaria jednego kabla nie psuje całej sieci.


Pierścień (Ring) - urządzenia tworzą zamkniętą pętlę. Dane przesyłane w jednym kierunku. Obecnie rzadziej stosowana.


Siatka (Mesh) - każdy węzeł połączony z wieloma innymi. Bardzo odporna na awarie. Stosowana w dużych i kluczowych sieciach.


Drzewo (Tree) - połączenie wielu topologii gwiazdy w strukturę hierarchiczną. Wykorzystywana w większych organizacjach.

