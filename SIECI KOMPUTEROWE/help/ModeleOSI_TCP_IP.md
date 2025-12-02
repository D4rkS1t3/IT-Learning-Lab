[Modele TCP/IP i OSI]

Do czego służą modele TCP/IP i OSI?


Modele te pomagają: uporządkować działanie sieci komputerowych, zrozumieć zadania poszczególnych warstw, dopasować protokoły do odpowiednich warstw, zrozumieć, jak dane przechodzą od aplikacji do przewodu i z powrotem, ułatwić projektowanie, analizowanie i naprawianie sieci.
OSI – model teoretyczny (7 warstw).
TCP/IP – model praktyczny (4 warstwy), używany w Internecie.

#
#

MODEL TCP/IP (4 warstwy)

To praktyczny model sieciowy, według którego działa Internet. Składa się z:

#
#

1. Warstwa Aplikacji

Dostarcza usługi i interfejs dla użytkownika i programów, określa sposób wymiany danych między aplikacjami. Tu powstaje treść, zanim zostanie podzielona na mniejsze jednostki.

Protokoły:
HTTP/HTTPS – strony WWW
DNS – tłumaczenie nazw domen
SMTP, IMAP, POP3 – poczta
FTP/SFTP – przesyłanie plików
SSH, Telnet – zdalne logowanie

#
#

2. Warstwa Transportowa

Tworzy połączenia między aplikacjami, dzieli dane na segmenty, może zapewniać niezawodność i kontrolę błędów.

Protokoły:
TCP – połączeniowy, niezawodny, używa ACK, retransmisji (WWW, e-mail)
UDP – bezpołączeniowy, szybki, bez kontroli błędów (gry, VoIP)

Jednostki danych: segmenty (TCP), datagramy (UDP).
(Uwaga: UDP nie dzieli danych logicznie, ale gdy datagram jest większy niż MTU (~1500 bajtów), warstwa IP fragmentuje go fizycznie.)

#
#

3. Warstwa Internetu (Sieci)

Zajmuje się adresacją IP, wybiera drogę pakietu (routing), umożliwia komunikację między różnymi sieciami.

Protokoły:
IP (IPv4/IPv6) – adresowanie
ICMP – diagnostyka (ping)
ARP – kojarzenie IP <-> MAC w sieci lokalnej
NAT – translacja adresów

Jednostka danych: pakiety IP.

#
#

4. Warstwa Dostępu do Sieci (Łącza danych + Fizyczna)

Opisuje sposób fizycznego przesyłania danych, definiuje adresy MAC, określa format ramek i rodzaje mediów transmisyjnych.

Protokoły/technologie:
Ethernet
Wi-Fi (802.11)
PPP
Frame Relay

Jednostka danych: ramki (frames) -> wysyłane po kablu lub radiowo.

#
#



Przepływ danych w modelu TCP/IP
#
#
1. Warstwa Aplikacji – tworzenie treści

Użytkownik uruchamia przeglądarkę i wpisuje adres strony.
Aplikacja tworzy żądanie HTTP/HTTPS i określa, jakie dane chce pobrać.
Na tym etapie powstaje czysta treść.
#
#
2. Warstwa Transportowa – pakowanie i przygotowanie do transmisji

Warstwa transportowa (TCP lub UDP):

Jeśli używamy TCP:

– dzieli dane na mniejsze kawałki – segmenty,
– dodaje numery sekwencyjne,
– ustawia numer portu źródłowego i docelowego (np. 443 dla HTTPS),
– może ustawić mechanizmy kontroli błędów i potwierdzeń.

Jeśli używamy UDP:

– po prostu dokłada nagłówek z portami i wysyła,
– nie segmentuje danych,
– ale jeśli datagram jest większy niż MTU (~1500B), zostanie podzielony przez warstwę IP (fragmentacja).

Powstaje segment/datagram z danymi aplikacji + portami źródłowym i docelowym.
#
#
3. Warstwa Internetu (IP) – ustalanie kierunku

Warstwa IP:
– dodaje adres IP nadawcy,
– dodaje adres IP odbiorcy (np. serwera Google),
– decyduje, czy odbiorca jest w tej samej sieci czy w innej.
Jeśli w innej – pakiet trzeba wysłać do routera (bramy domyślnej).

Powstaje pakiet IP, który wie:
– skąd pochodzi (IP źródłowe),
– dokąd ma dotrzeć (IP docelowe),
– jak ma być przekazywany (TTL, protokoły).
#
#
4. Warstwa Dostępu do Sieci – dodanie adresów MAC

Pakiet trafia do warstwy odpowiedzialnej za lokalne dostarczenie danych.

Komputer:
– sprawdza w swojej tablicy ARP MAC routera (jeśli pakiet idzie do Internetu)
lub MAC urządzenia docelowego (jeśli znajduje się w tej samej sieci),
– tworzy ramkę Ethernet/Wi-Fi,
– dodaje do niej:
– MAC źródłowy (swojej karty),
– MAC docelowy (routera albo innego hosta).

Powstaje ramka gotowa do wysłania po kablu lub radiowo.
#
#
5. Medium transmisyjne – wysyłka fizyczna

Ramka w postaci impulsów elektrycznych, światła lub fal radiowych biegnie:
– po kablu Ethernet,
– przez światłowód,
– lub poprzez Wi-Fi.
#
#
6. Switch – sprawdzanie adresu MAC

Ramka trafia do switcha.

Switch:
– odczytuje adres MAC docelowy,
– sprawdza w swojej tablicy MAC, na którym porcie znajduje się urządzenie,
– jeśli zna port → wysyła ramkę tylko tam,
– jeśli nie zna → wykonuje flooding, wysyła ramkę na wszystkie porty oprócz wejściowego.

Switch działa po MAC, a nie po IP.
#
#
7. Przekazanie do kolejnego urządzenia

Ramka trafia:
– do routera (jeśli komunikacja wychodzi poza LAN),
– albo bezpośrednio do urządzenia docelowego (np. do innego komputera).
#
#
8. Odbiorca – najpierw warstwa MAC

Karta sieciowa odbiorcy sprawdza:
„Czy adres MAC w ramce jest mój?”

Jeśli tak:
– ramka zostaje zaakceptowana,
– karta usuwa nagłówek Ethernet,
– przekazuje pakiet IP wyżej.
#
#
9. Warstwa Internetu – IP sprawdza cel

System operacyjny odbiorcy:
– odczytuje adres IP docelowy,
– sprawdza, czy pakiet jest do niego,
– przekazuje segment/datagram do warstwy transportowej.
#
#
10. Warstwa Transportowa – składanie danych

TCP/UDP odczytuje porty:

TCP:

– układa segmenty we właściwej kolejności,
– odrzuca uszkodzone,
– potwierdza odbiór (ACK).

UDP:

– przekazuje dane bez kontroli i bez retransmisji.

Segment/datagram staje się z powrotem danymi aplikacji.
#
#
11. Warstwa Aplikacji – dostarczenie treści

Dane trafiają do właściwego programu:
– przeglądarka otrzymuje odpowiedź HTTP,
– program pocztowy otrzymuje wiadomość,
– gra otrzymuje pakiet z ruchem gracza.

To jest moment, gdy użytkownik „widzi” efekt transmisji.

#
#
#
#
MODEL OSI (7 warstw)

Model OSI (Open Systems Interconnection) to teoretyczna struktura opisująca sposób, w jaki dane przechodzą przez sieć – od aplikacji aż do fizycznego medium transmisji.
#
#
Warstwa 7 – Aplikacji

Dostarcza usługi i interfejs dla użytkownika i programów, określa sposób wymiany danych między aplikacjami.

Protokoły:
HTTP/HTTPS, DNS, SMTP, IMAP, POP3, FTP/SFTP, SSH, Telnet
#
#
Warstwa 6 – Prezentacji

Konwersja danych między formatami, kompresja, szyfrowanie.

Przykłady:
SSL/TLS (częściowo), ASCII, Unicode, JPEG, MP3
#
#
Warstwa 5 – Sesji

Nawiązywanie, utrzymywanie i kończenie sesji komunikacyjnych, synchronizacja dialogu.

Przykłady:
sesje w RPC, NetBIOS
#
#
Warstwa 4 – Transportowa

Przesyła dane między aplikacjami (TCP/UDP), dzieli dane na segmenty, zapewnia kontrolę błędów, realizuje retransmisje, obsługuje numery portów (multiplexing).
#
#
Warstwa 3 – Sieciowa

Adresowanie IP, routing pakietów między sieciami.

Protokoły:
IP, ICMP, ARP, OSPF, BGP
#
#
Warstwa 2 – Łącza danych

Adresowanie MAC, tworzenie ramek, komunikacja w obrębie jednej sieci LAN.

Technologie:
Ethernet, Wi-Fi (802.11), VLAN (802.1Q), PPP
#
#
Warstwa 1 – Fizyczna

Fizyczny przesył bitów, określa złącza, sygnały, częstotliwości, medium transmisyjne.

Przykłady:
światłowód, skrętka, fale Wi-Fi, USB, sygnały elektryczne

Zastosowanie OSI

Model OSI jest używany głównie jako model edukacyjny i referencyjny — do nauki i analizy działania sieci.
