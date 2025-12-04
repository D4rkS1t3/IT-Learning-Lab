ROUTER — KOMPLETNE KOMPENDIUM WIEDZY
#
#
Router to centralne urządzenie sieciowe, które łączy różne sieci i kieruje pakiety zgodnie z adresacją IP.
W praktyce router domowy to 3–4 urządzenia w jednej obudowie:
Router (warstwa 3)
Switch (warstwa 2)
Access Point (Wi-Fi, warstwa 1/2)
(czasem) Modem – DSL (miedziane linie telefoniczne), światłowód lub kablówka
#
#
Router wykonuje:

Routing – sprawdza IP docelowe pakietu, wybiera trasę (next hop), przekazuje pakiet do właściwej sieci.

NAT – translacja adresów. Pozwala, aby cała sieć LAN korzystała z jednego publicznego IP. Zamienia prywatne IP (np. 192.168.1.20) na publiczne IP od operatora.
Komputer wysyła pakiet do routera. Router zmienia:
– źródłowy IP: 192.168.1.20 -> 89.77.52.100
– źródłowy port (tzw. PAT – Port Address Translation).
Zapisuje tę parę w tablicy NAT (tłumaczeń). Odpowiedzi wracają do routera, router wie, do którego komputera je przekazać. (Adres IP docelowy pakietu nie jest zmieniany – cały czas wskazuje serwer w Internecie).

DHCP – automatyczne przydzielanie adresów. Router rozdaje IP, maskę, bramę i DNS komputerom w sieci. Router działa jako serwer DHCP, czyli:
Komputer wysyła DHCP Discover (broadcast).
Router odpowiada DHCP Offer.
Komputer akceptuje DHCP Request.
Router finalizuje DHCP Acknowledge.

Firewall – filtrowanie ruchu.
Router domowy ma firewall, który:
– blokuje połączenia przychodzące z Internetu,
– przepuszcza ruch wychodzący,
– chroni przed skanowaniem portów,
– może stosować zasady filtracji (ACL).

Nowoczesne firewalle potrafią też:
– filtrować treści (rodzicielska kontrola),
– blokować strony,
– wykrywać ataki.

Switch (wbudowany) – łączy komputery w tej samej sieci LAN.
Switch LAN wewnątrz routera:
– działa wyłącznie na MAC,
– sam buduje tablicę MAC,
– rozsyła broadcasty (np. ARP, DHCP Discover),
– kieruje ruch między komputerami bez udziału routera (CPU).

Dwa komputery wpięte kablami do routera komunikują się tylko przez switch, nie przez CPU routera.

Access Point (Wi-Fi) – punkt dostępowy, umożliwiający bezprzewodową komunikację za pomocą Wi-Fi.

Modem – zamienia sygnał operatora na Ethernet.

Serwery pomocnicze – DNS cache, NTP (czas), UPnP, QoS itp.
#
#
ONT (modem optyczny) – zamienia impuls optyczny na sygnał elektryczny (Ethernet). Routery mogą mieć wbudowany ONT. Jeżeli router ma wbudowane ONT, to pełni także rolę modemu.
W Polsce bardzo często operator daje oddzielne ONT, które:
– zamienia światło na Ethernet,
– autoryzuje łącze,
– ma unikalny numer (SN/LOID),
a router połączony jest z nim zwykłym kablem Ethernet.

OLT w centrali operatora musi rozpoznać ONT, bo inaczej go odrzuci.

Jak można podłączyć własny router (przypadki):

Przypadek 1:
ONT działa w trybie bridge -> można podłączyć swój router bezpośrednio do ONT.
Schemat:
Światłowód -> ONT (modem optyczny) -> Twój router -> LAN/Wi-Fi

Przypadek 2:
Operator ograniczył ONT -> wymaga routera operatorowego między ONT a Twoim routerem.
Wtedy należy ustawić router operatora w tryb BRIDGE (o ile operator na to pozwala).
Schemat:
Światłowód -> ONT -> Router operatora -> Twój router (WAN)

Tryb bridge wyłącza funkcje routera, a urządzenie operatora staje się przejściówką między: światłowodem / Internetem (WAN) a naszym routerem (WAN port).

Wtedy:
– operatorowy sprzęt nie robi NAT,
– nie daje Wi-Fi,
– nie przydziela IP (DHCP),
– nie działa firewall operatorowego routera dla ruchu LAN,
– nie tworzy sieci LAN (jego porty LAN są jak „kabel przedłużający”).

On tylko przepuszcza ruch.
ONT/modem operatora -> NAT i routerowanie robi nasz router.

Tryb bridge sprawia, że operatorowy router staje się tylko „przejściówką optyczną”, a cały Internet, bezpieczeństwo, NAT, Wi-Fi i konfigurację przejmuje nasz własny, mocniejszy router.

Jak sprawdzić, który przypadek ma się u siebie:

Odłączyć router operatora.

Podłączyć komputer kablem do ONT.

Sprawdzić, jaki adres IP dostaje komputer.

– Jeżeli dostajemy adres publiczny, ONT działa w bridge i można podłączyć swój router bezpośrednio.
– Jeżeli komputer dostaje adres prywatny (np. 192.168.x.x, 10.x.x.x, 100.64.x.x) albo w ogóle nie dostaje adresu, to ONT nie daje publicznego adresu i trzeba podłączyć swój router do routera operatora (lub poprosić operatora o tryb bridge, jeśli to możliwe).
#
#
Budowa routera:
CPU – przetwarza pakiety, obsługuje NAT, routing, firewall
RAM – tablice routingu, sesje NAT, ARP, bufor pakietów
Flash – system routera (firmware)
Switch LAN (chip) – obsługuje porty Ethernet
Radio Wi-Fi – 2.4/5/6 GHz
Port WAN – do modemu / ONT / łącza operatora
Anteny – jeśli jest Wi-Fi
Zasilacz

Router domowy = mały komputer sieciowy.
#
#
Routing krok po kroku:
Router odbiera ramkę Ethernet z pakietem IP.
Odczytuje IP docelowe.
Patrzy w tablicę routingu (routing table).
Ustala next hop (następny router lub sieć lokalna).
Odpakowuje starą ramkę (usuwa MAC źródła i celu).
Tworzy nową ramkę Ethernet/Wi-Fi:
– MAC źródłowy -> jego własna karta wyjściowa
– MAC docelowy -> następny router / urządzenie
Wysyła dalej.

IP docelowy pozostaje taki sam przez całą drogę. Zmieniają się tylko adresy MAC, czyli „przystanki po drodze”.
#
#
Protokoły routingu (router uczy się tras i sieci automatycznie):

Routery profesjonalne (a czasem biznesowe) używają protokołów routingu do wymiany informacji o trasach. Zamiast ręcznie wpisywać, którędy wysyłać pakiety, routery same negocjują najlepsze trasy.
#
Najczęściej używane protokoły to:

RIP (Routing Information Protocol) - najprostszy, używa „liczby przeskoków” (hop count), max 15 hopów, powolna konwergencja, mało bezpieczny, używany tylko w małych/legacy sieciach.

OSPF (Open Shortest Path First) - dynamiczny protokół link-state (stan łączy), szybki, stabilny, skalowalny, routery wymieniają informacje o topologii, obliczają najlepszą drogę algorytmem Dijkstra, stosowany w firmach, korporacjach, data center.

BGP (Border Gateway Protocol) „protokół Internetu” – łączy sieci operatorów (ISP) i duże organizacje, bardzo skalowalny, kontroluje ruch między autonomicznymi systemami AS, używany na styku operatorów, w trasach międzykrajowych i międzykontynentalnych, absolutny standard w globalnym routingu.

EIGRP (Cisco) - hybrydowy protokół (distance-vector + link-state), działa głównie na routerach Cisco, szybszy od RIP, prostszy od OSPF.
#
Routery domowe nie używają tych protokołów, bo nie mają potrzeby dynamicznego routingu — ich jedyną trasą jest „wyślij wszystko do operatora”.

Routery profesjonalne mogą mieć jednocześnie:
– tysiące tras
– wiele interfejsów
– wiele sieci LAN/WAN
– dynamiczne monitorowanie i modyfikacje tras

#
#

Różnice między routerem domowym a profesjonalnym:

Router domowy i profesjonalny różnią się konstrukcją, wydajnością, funkcjami oraz przeznaczeniem. W praktyce to dwa zupełnie różne światy.
#
Wydajność i hardware:
Router domowy:
– słaby CPU (1–2 rdzenie),
– mała pamięć,
– ograniczone możliwości NAT przy dużych prędkościach,
– wbudowany switch i Wi-Fi.

Router profesjonalny:
– wielordzeniowy CPU lub dedykowane procesory sieciowe (ASIC/NP),
– gigabajty pamięci RAM,
– wydajność rzędu dziesiątek/ setek Gb/s (bez strat),
– modularna konstrukcja (sloty, interfejsy).
#
Funkcje sieciowe:
Router domowy:
– podstawowy NAT, DHCP, firewall, UPnP, Wi-Fi
– często brak VLAN
– ograniczona konfiguracja

Router profesjonalny:
– zaawansowane VLAN-y (802.1Q, trunking, routing między VLAN)
– dynamiczne protokoły routingu (OSPF, BGP, RIP)
– MPLS, VPN IPSec/SSL, QoS klasy operatorskiej
– ACL warstwy 3/4 i często 7
– precyzyjne monitorowanie (NetFlow, SNMP, syslog)
– pełna kontrola nad topologią sieci
#
Firmware i zarządzanie:
Router domowy:
– proste GUI w przeglądarce,
– ograniczone opcje konfiguracji,
– automatyczne aktualizacje operatora.

Router profesjonalny:
– konfigurowany przez CLI (konsola tekstowa)
– wymaga znajomości składni (Cisco IOS, MikroTik RouterOS, Juniper JunOS)
– wsparcie dla automatyzacji (Ansible, API)
– możliwość tworzenia kopii zapasowych, wielu konfiguracji
#
Zastosowanie:
Router domowy:
– Internet w domu, proste Wi-Fi, kilka/kilkanaście urządzeń

Router profesjonalny:
– firmy, kampusy, operatorzy, serwerownie
– setki lub tysiące urządzeń
– krytyczne wymagania SLA (działa 24/7, bez restartów)
#
Stabilność i bezpieczeństwo:
Router domowy:
– ograniczony firewall
– słaba ochrona przed zaawansowanymi atakami
– sprzęt potrafi się przeciążyć

Router profesjonalny:
– bezpieczeństwo klasy enterprise
– zaawansowany firewall, IDS/IPS, segmentacja
– działa stabilnie przy gigantycznych obciążeniach

#
Routery profesjonalne oferują monitoring:

– NetFlow / IPFIX – statystyki przepływu
– SNMP – statystyki urządzeń
– Ping/latency monitoring
– wykresy zużycia CPU, RAM, pasma
– analiza pakietów (Packet Sniffer)
#
#
VLAN – logiczne podziały sieci. VLAN (Virtual LAN) pozwala podzielić jedną fizyczną sieć na wiele logicznych sieci.

VLAN służą do:
– izolacji urządzeń (np. IoT, goście, kamery)
– bezpieczeństwa
– uporządkowania sieci w firmach
– zmniejszenia broadcastów
– łatwego zarządzania dużymi sieciami

Przykład:
VLAN 10 – komputery
VLAN 20 – Wi-Fi gości
VLAN 30 – kamery
VLAN 40 – IoT (smart home)

Router profesjonalny może routować między VLAN-ami (tzw. routing między VLAN).

Switch musi wspierać VLAN (standard 802.1Q), aby je obsługiwać.
#
#
QoS (Quality of Service) – zarządzanie jakością ruchu. Pozwala ustalić priorytety dla różnych typów ruchu.

QoS używa się do:
– zapewnienia stabilnego pingu w grach
– poprawy działania połączeń VoIP
– priorytetu dla wideokonferencji (Teams/Zoom)
– ograniczenia prędkości dla gości
– priorytetu dla serwerów

Przykładowe klasy:
– High priority – gry, VoIP
– Medium – przeglądanie stron
– Low – pobieranie, torrenty

Dobre routery potrafią rozpoznawać ruch automatycznie (m.in. Asus Adaptive QoS, Mikrotik Queue Tree).
#
VPN tworzy szyfrowane połączenie między dwoma punktami w Internecie. Działa tak, że zwykły pakiet IP zostaje zaszyfrowany, a następnie włożony w nowy pakiet, który jest wysyłany do serwera VPN. Dzięki temu routery po drodze widzą tylko zaszyfrowaną „otoczkę”, a nie prawdziwe dane.
#
Działa to tak:
#
Bez VPN:

nasz komputer -> pakiet IP -> Internet -> serwer

Każdy router po drodze widzi: nasz adres IP, adres docelowy, dane (czasem jawne, czasem szyfrowane HTTPS).
#
Z VPN:

weź zwykły pakiet IP -> zaszyfruj go ->	włóż go do nowego pakietu -> wyślij do serwera VPN -> serwer VPN odpakowuje -> Internet -> serwer


Routery po drodze widzą tylko:

nasz adres IP

adres IP serwera VPN

zaszyfrowaną (nieczytelną) zawartość
#
Serwer VPN odbiera to, odpakowuje i dopiero on wysyła pakiet dalej do Internetu.

Dlatego VPN to „tunel”:
Routery widzą tylko ZEWNETRZNĄ otoczkę pakietu, a środek jest ukryty.

#
Rodzaje VPN:

IPSec – bardzo bezpieczny, standard korporacyjny

OpenVPN – stabilny, popularny

WireGuard – nowoczesny, bardzo szybki, lekki

L2TP/PPTP – stare protokoły (PPTP niezalecany)

Zastosowania:
– połączenie z siecią firmy
– zdalny dostęp do domu
– omijanie ograniczeń operatora
– bezpieczne Wi-Fi w miejscach publicznych

Profesjonalne routery mają wbudowane serwery VPN.
Domowe routery często też (Asus, Mikrotik, FritzBox).
#
#
NAT to zbiór technik, które zmieniają adresy IP i/lub porty w pakietach. Wszystkie działają w warstwie 3–4, ale robią inne rzeczy.

Najprostsze porównanie:

SNAT -> zmieniamy kto wysyła

DNAT -> zmieniamy do kogo wysyłamy

PAT -> pozwalamy wielu urządzeniom korzystać z jednego adresu

Hairpin -> umożliwiamy dostęp z własnej sieci do własnego publicznego IP

#

SNAT (Source NAT) – zmiana adresu źródłowego

Stosowany gdy pakiet wychodzi z LAN do Internetu.

Przykład:
Nasz komputer:

192.168.1.20 -> 172.217.22.14 (Google)


Router zamienia adres źródłowy na publiczny:

89.77.52.100 -> 172.217.22.14

SNAT = „gdy JA wysyłam pakiet, router zmienia mój adres na swój”.

Konieczny, bo prywatne IP nie istnieją w Internecie

#

DNAT (Destination NAT) – zmiana adresu docelowego

Stosowany gdy pakiet przychodzi z Internetu do domu.

Przykład:

192.168.1.20:25565


Osoba z Internetu wpisuje:

89.77.52.100:25565


Router wykonuje DNAT:

cel = 89.77.52.100 -> 192.168.1.20


DNAT = przekierowanie portów (port forwarding). To router zmienia adres docelowy i kieruje ruch do konkretnego komputera.

używane przy serwerach
używane w kamerach IP
używane w grach

#

PAT (Port Address Translation) – NAT + porty

PAT to odmiana SNAT, ale dodatkowo router zmienia port źródłowy, aby odróżnić wiele urządzeń.

Przykład:
W LAN mamy:

192.168.1.10
192.168.1.20
192.168.1.30

Wszystkie idą do Internetu przez jedno publiczne IP:

89.77.52.100


Router robi:

PC		Port źródłowy (LAN)	Port po PAT	Publiczne IP
192.168.1.10	50000			40001		89.77.52.100
192.168.1.20	50000			40002		89.77.52.100
192.168.1.30	50000			40003		89.77.52.100

Dzięki temu wie, do którego komputera odesłać odpowiedź.

To właśnie pozwala setkom urządzeń korzystać z jednego publicznego IP router używa tablicy NAT, aby to rozróżniać.

PAT to najczęściej używany rodzaj NAT na świecie.

#

NAT Hairpin / Loopback – gdy chcesz wejść na własne publiczne IP z LAN

Bez tej funkcji:

– nie wejdziesz na swoją kamerę wpisując:

89.77.52.100:8080


– nie wejdziesz na własny serwer z sieci lokalnej

Z NAT Hairpin:

Router pozwala „zawrócić” ruch:

LAN -> publiczny IP routera -> znowu LAN -> serwer


#

DMZ (Demilitarized Zone) w routerach domowych oznacza „przekieruj cały ruch przychodzący na jeden host”.

Używane gdy:
– host działa jako serwer,
– gry wymagają wielu portów,
– masz własny router za routerem operatora (tymczasowe rozwiązanie).

Urządzenie w DMZ jest praktycznie bez ochrony.
#

UPnP – automatyczne otwieranie portów. Automatycznie otwiera porty dla gier i aplikacji.

PLUSY: działa bardzo wygodnie (konsola, Fortnite, GTA Online itd.)
MINUSY: potencjalnie niebezpieczne – malware może otworzyć porty samo.

Profesjonaliści często:
– wyłączają UPnP,
– zamiast tego ustawiają ręczne port forwarding.

Ale w domu UPnP bywa wygodne i często konieczne dla gier.
#

IPv4 to najbardziej znany i używany protokół adresacji w Internecie. Każde urządzenie potrzebuje adresu IP, aby mogło komunikować się z innymi.

Adres IPv4 to 32-bitowy numer, zapisany w formacie: xxx.xxx.xxx.xxx np. 192.168.1.20. Każdy „xxx” to liczba 0–255.

Adres IPv4 składa się z dwóch części: część sieciowa (network), część hosta (host).

Maska podsieci wskazuje, gdzie kończy się sieć, a gdzie zaczyna host.

Przykład:

192.168.1.20 /24


Maska /24 = 255.255.255.0
Czyli:

pierwsze 24 bity -> sieć

ostatnie 8 bitów -> host w tej sieci

IPv4 ma za mało adresów. IPv4 ma tylko ok. 4,3 miliarda adresów, a to za mało dla całego świata. Dlatego wprowadzono: adresy prywatne, NAT, IPv6.

Adresy prywatne IPv4 (LAN) - w sieci domowej używamy „wewnętrznych” adresów, które NIE występują w Internecie:
10.0.0.0 – 10.255.255.255

172.16.0.0 – 172.31.255.255

192.168.0.0 – 192.168.255.255

Czyli nasz komputer ma np.:

192.168.1.20

Ale do Internetu wychodzi przez router, który robi NAT.

Publiczne adresy IPv4 - to adresy widoczne w Internecie. Router operatora ma publiczny adres typu:

89.77.xx.xx
213.189.xx.xx
195.116.xx.xx

Te adresy muszą być unikalne w całym Internecie.

Klasy adresowe IPv4:

Klasa A: 0.0.0.0 – 127.255.255.255

Klasa B: 128.0.0.0 – 191.255.255.255

Klasa C: 192.0.0.0 – 223.255.255.255

Dziś stosuje się CIDR — elastyczne podziały bez klas.

#
IPv6 – Internet nowej generacji

IPv6 wprowadza adresy 128-bitowe (ogromna pula adresów).

Zalety IPv6:
– każdy urządzenie może mieć publiczny adres
– nie potrzeba NAT
– szybsze zestawianie połączeń
– większe bezpieczeństwo (IPSec wbudowany)
– lepsza przyszłościowość

Wady IPv6:
– nie zawsze działa na 100% u wszystkich operatorów
– nie wszystkie aplikacje obsługują IPv6
– konfiguracja firewall IPv6 wymaga uwagi

Obecnie większość sieci działa w trybie „dual stack”: IPv4 + IPv6 jednocześnie.
#

DNS (Domain Name System) zamienia nazwy (np. google.com) na adresy IP.

Etapy działania DNS:

Komputer pyta lokalny resolver (zwykle router / operator).

Jeśli brak odpowiedzi – zapytanie idzie do DNS rekursywnego (ISP).

Ten odpyta serwery DNS najwyższych poziomów (root -> TLD -> domena).

Odpowiedź jest zapisywana w cache.

Router często pełni rolę:
– DNS relay
– DNS cache
co przyspiesza ładowanie stron.
#
Port forwarding – przekierowanie portów. Port forwarding to funkcja DNAT – umożliwia wystawienie usług z sieci domowej na Internet.

Przykłady użycia:
– serwer Minecraft
– kamery IP
– serwer WWW
– FTP
– zdalny pulpit

Wymaga:
– publicznego IP
– wyłączenia podwójnego NAT
– dobrze ustawionych reguł firewall

#
Segmentacja sieci – najlepsza praktyka bezpieczeństwa. Segregacja urządzeń to klucz do bezpieczeństwa.

Przykład nowoczesnej sieci domowej:

VLAN 10 – Komputery
VLAN 20 – Telefony
VLAN 30 – IoT (np. żarówki, TV)
VLAN 40 – Goście
VLAN 50 – Kamery IP
VLAN 99 – Zarządzanie

Każda grupa ma inne zasady bezpieczeństwa.vIoT i TV nie mają dostępu do komputerów. Kamery mają dostęp tylko do rejestratora. Sieć gości ma dostęp tylko do Internetu. Tak buduje się sieci „zero trust”.


