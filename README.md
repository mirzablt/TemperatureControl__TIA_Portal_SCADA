# Regulacija temperature prostorije i implementacija SCADA HMI interfejsa u TIA Portal razvojnom okruženju 

## Uvod
U ovom repozitoriju je data implementacija upravljanja temperaturom prostorije i implementacija odgovarajućeg SCADA HMI interfejsa. 
Upravljanje je implementirano u Siemens-ovom TIA Portal razvojnom okruženju.  
Korišten je temperaturni PID reguator iz TIA biblioteke blokova, i implementiran je tropozicioni regulator sa histerezom, a iz HMI interfejsa je omogućen njihov izbor. U praktičnom slučaju algoritam upravljanja se izvršava na PLC-u. U radu je izabran Siemens S7-1500 PLC uređaj, sa dodatnim ulaznim i izlaznim modulima na koje se spajaju senzori i aktuatori.
U izvedbi HMI interfejsa u SCADA sistemu je korišten WinCC RT Advanced program unutar TIA Portala. Implementirane su funkcionalnosti SCADA sistema: grafika procesa, trendovi, alarmi i događaji, arhiviranje izabranih trendova i alarma u SQL bazu podataka.   
U radu je korištena mogučnost simulacije u pomenutom razvojnom okruženju. Pa je simuliran sistem upravljanja sa implementiranim algoritmom i objektom upravljanja čiji model je zadan softverski, odgovarajućim blokom. U regulacionoj konturi u simuaciji, izlazi i ulazi objekta upravljanja su povezani sa odgovarajućim ulazima i izlazima PLC-a preko simboličkih tagova. U praktičnom slučaju za razliku od simulacije bi imali odgovarajuće fizičke tagove za odgovarajuće ulaze i izlaze PLC-a, na koje su spojeni senzori i aktuatori sa objekta upravljanja. U simulaciji algoritam upravljanja se izvršava na PLC-u, simulira se mreža koja povezule PLC sa PC računarom na kojem izvršava SCADA aplikacija. Simulacija daje potpun uvid u funkcionalnost izvedenog HMI interfejsa.  
Implementacija sistema upravljanja, implementacija SCADA HMI intrface-a i rezultati simulacije su detaljno dati u [Documentation](./Documentation.pdf)

## Sadržaj
- [Instalacija](#instalacija)
- [Pokretanje simulacije](#pokretanje-simulacije)
- [Simulacija](#simulacija)
  - [TIA Portal i S7-PLCSIM](#tia-portal-i-s7-plcsim)
  - [SCADA HMI Interfejs](#scada-hmi-interfejs)
  - [Alarmni sistem i arhiviranje podataka](#alarmni-sistem-i-arhiviranje-podataka)
- [Licenca](#licenca)

## Instalacija
Za pokretanje ovog projekta potrebno je instalirati sljedeći softver:
- Siemens TIA Portal v16
- WinCC RT Advanced (za SCADA HMI interface)
- S7-PLCSIM (za simulaciju PLC-a)
- Automation License Manager
- Microsoft SQL Server (SQL baza podataka). Poželjno je imati instalisan i SQL Server Management Studio.

### Pokretanje simulacije
1. Klonirati repozitorij;
2. Otvoriti TIA Portal;
3. Dvoklikom na fajl SeminarskiRad26n.ap16 iz repozitorija, otvaramo projekat sa implementiranim upravljanjem i HMI interfaceom;
4. U mapi Project tree klinuti na Controller te na dugme Start Simulation koje se nalazi u alatnoj traci TIA razvojnog okruženja. Ovim se pokreće aplikacija S7-PLCSIM.
5. Konfigurisati simulirani PLC u S7-PLCSIM-u i postavite ga na RUN.
6. Prebacite projekt na simulirani PLC putem opcije Download to device.
7. U Project tree mapi kliknuti na SCADA fajl, te na dugme Start Runtime on the PC koje se nalazi u alatnoj traci razvojnog okruženja. Ovim se pokreće aplikacija WinCC RT Advanced i simulacija implementiranog SCADA/HMI interfacea.

## Simulacija
### TIA Portal i S7-PLCSIM
Simulacija upravljačkog sistema izvodi se na Siemens S7-1500 PLC-u pomoću S7-PLCSIM aplikacije. Uključuje sljedeće elemente:
- PID regulator: Omogućava precizno održavanje temperature u prostoriji prema zadanoj vrijednosti.
- On/Off regulator sa histerezom: Omogućava održavanje temperature u prostoriji unutar zadane donje i gornje granice temperature prostorije.
- Simulacija fizičkog procesa (objekta upravljanja): Objekat upravljanja je u simulaciji zadan kao blok LSim_PT3HeatCool. Blok je uzet iz Siemens-ove biblioteke Lsim. Senzori, ventili i ventilatori su simulirani kao simbolički tagovi ulaza i izlaza pomenutog bloka, dok u stvarnom sistemu upravljački signali sa PLC-a idu prema fizičkim uređajima.

### SCADA HMI Interfejs
SCADA interfejs je realizovan pomoću WinCC RT Advanced i daje sljedeće funkcionalnosti:
- Prikaz grafike procesa: Upravljačke i procesne varijable vizualizovane su kroz grafički prikaz u stvarnom vremenu (ventili, ventilatori, temperature).
- Trenutni podaci i trendovi: Referentna i stvarna temperatura prostorije, upravljački signali (grijanje/hlađenje) prikazani su u obliku trendova.
- Podešavanje parametara: Operater može prilagoditi parametre PID regulatora direktno sa SCADA HMI ekrana. 
- Alarmi: HMI sadrži ekran alarma koji prikazuje trenutna i prethodna upozorenja o greškama i nepravilnostima u sistemu.

Rezultati simulacije su detaljno dati u Documentation.

### Alarmni sistem i arhiviranje podataka
SCADA sistem arhivira podatke u SQL bazi podataka, što omogućava praćenje historijskih trendova i događaja. Alarmni sistem uključuje:
- Aktivni i prošli alarmi: Svi alarmi su klasificirani kao greške ili događaji i prikazani u realnom vremenu.
- Pragovi za aktiviranje alarma: Postavljeni pragovi u HMI interfejsu omogućavaju aktiviranje alarma kada procesne varijable pređu određene granice.
- Arhiviranje: Svi alarmi i relevantni procesni podaci arhiviraju se u SQL bazu podataka, što omogućava naknadnu analizu.

## Licenca
Ovaj projekt je licenciran pod MIT licencom. Detalji licence su dati u datoteci [LICENSE](./LICENSE).
