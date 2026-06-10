# cybersecurity-home-lab
# Isolated Cybersecurity Testing Environment (Home Lab)

## 1. Opis Projektu (Project Overview)
Projekt polega na zaprojektowaniu, wdrożeniu i zabezpieczeniu odizolowanego laboratorium metodą wirtualizacji (Home Lab). Środowisko zostało stworzone w celu bezpiecznego przeprowadzania testów penetracyjnych aplikacji webowych oraz analizy podatności, bez ryzyka wpływu na fizyczną sieć lokalną (LAN).

**Główne komponenty środowiska:**
* **Hiperwizor:** Oracle VM VirtualBox
* **Maszyna Atakująca (Attacker):** Kali Linux (najnowsza wersja dystrybucyjna)
* **Maszyna Podatna (Victim):** Ubuntu Server z uruchomioną celowo podatną aplikacją webową **OWASP Juice Shop** w kontenerze Docker.

---

## 2. Architektura Sieci (Network Architecture)
Kluczowym założeniem projektu było zapewnienie pełnej izolacji maszyn wirtualnych. Zastosowano sieć typu **NAT Network** (Sieć NAT) w VirtualBox, o adresacji `10.0.2.0/24`.

* **Kali Linux IP:** `[TUTAJ WPISZ IP KALI, np. 10.0.2.4]`
* **Ubuntu Server (Juice Shop) IP:** `[TUTAJ WPISZ IP UBUNTU, np. 10.0.2.5]`

Dzięki takiej konfiguracji:
1. Maszyny wirtualne mogą komunikować się między sobą.
2. Maszyny mają dostęp do Internetu (w celu pobierania aktualizacji/pakietów).
3. **Sieć lokalna hosta (LAN) jest chroniona** – ruch z maszyn wirtualnych nie ma bezpośredniego dostępu do urządzeń w domowej sieci.

*[TUTAJ WSTAW SCHEMAT SIECI - Wygeneruj go np. na draw.io, zapisz jako PNG, wrzuć do repozytorium i dodaj link poniżej]*
![Schemat Architektury Sieci](sciezka_do_obrazka/schemat.png)

---

## 3. Procedura Wdrożenia Krok po Kroku (Deployment Steps)

### Krok 1: Konfiguracja Sieci w VirtualBox
1. W ustawieniach globalnych VirtualBox przeszedłem do zakładki *Sieć* (Network).
2. Utworzyłem nową sieć NAT (NAT Network) o nazwie `LabNetwork` i przypisałem jej CIDR `10.0.2.0/24` z włączoną obsługą DHCP.

### Krok 2: Konfiguracja Maszyny Atakującej (Kali Linux)
1. Pobrałem oficjalny obraz VM dla VirtualBox ze strony offensive-security.com.
2. W ustawieniach maszyny zmieniłem kartę sieciową na `Sieć NAT` (wybierając `LabNetwork`).
3. Przeprowadziłem podstawowy hardening: zmiana domyślnego hasła systemowego oraz aktualizacja pakietów:
   ```bash
   sudo apt update && sudo apt upgrade -y
