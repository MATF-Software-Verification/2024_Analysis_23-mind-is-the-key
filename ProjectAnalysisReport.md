# Izveštaj analize projekta

## Clang <img src="https://llvm.org/img/LLVMWyvernSmall.png" width="40" height="30">
**Clang** je kompilator zasnovan na `LLVM (Low-Level Virtual Machine)` projektu koji se koristi za jezike `C, C++, Objective C i Objective C++`.
Predstavlja frontend koji kao ulaz uzima kod napisan u prethodno navedem jezicima i prevodi ga u međureprezentaciju tj. *llvm IR* i to predstavlja ulaz za središnji deo na kojem se vrše optimizacije nezavisne od jezika i arhitekture. Na kraju backend vrši optimizacije vezane za konkretnu arhitekturu i prevodi kod na mašinski jezik. Clang je implementiran u `C++`-u i dizajniran da bude brz, efikasan i modularan.

### Clang-Tidy
**Clang-Tidy** predstavlja jedan od `Clang` zasnovanih alata koji obavlja statičku analizu koda i proveru stila (vrši analiziranje izvornog koda bez njegovog izvršavanja sa ciljem pronalaženja grešaka, poboljšanja kvaliteta koda i ispravljanja neoptimalno napisanih delova koda). Takođe može da pomogne u modernizaciji koda u skladu sa najnovijim standardima i najboljim praksama.

### Clazy
**Clazy** je alat specifičan za projekte bazirane na `Qt` okruženju, koji koristi `Clang` kao osnovu za statičku analizu. Prilagođen je `Qt` bibliotekama i otkriva neefikasne upotrebe `QString, QList, QHash` -a, pogrešnu upotrebu `Qt API`-ja (npr. korišćenje `signals` i `slots`) i mnoge druge propuste.

Ovi alati su integrisani u `QtCreator`-u i mogu se pokrenuti sa podrazumevanim (*default*) ili sa prilagođenom (*custom*) konfiguracijom.
1) Pokretanje sa `default` (podrazumevanom) konfiguracijom zahteva odabir kartice `Analyze` i opcije `Clang-Tidy and Clazy`
   
![img](clang/clang-tidy_clazy/ClangTidyAndClazy1.png)

Nakon toga se izaberu fajlovi nad kojima će se vršiti analiza:

![img](clang/clang-tidy_clazy/ClangTidyAndClazy2.png)

Pritiskom na dugme `Analyze` pokreće se analiza. Kada se završi, vidimo rezultat analize svih fajlova. Neka upozorenja se odnose na curenje memorije, neka na nekorišćene promenljive, na potencijalno problematično imenovanja slotova itd.

![img](clang/clang-tidy_clazy/ClangTidyAndClazy3.png)

2) Pokretanje `custom` definisane konfiguracije zahteva odabir kartice `Tools`, zatim `Options`, pa `Analyzer` i klik na `Default Clang-Tidy and Clazy checks`

![img](clang/clang-tidy_clazy/ClangTidyAndClazy4.png)

Nakon toga je potrebno kopirati default-nu konfiguraciju, preimenovati je (u mom slučaju `Default Clang-Tidy and Clazy checks (Custom)`) i u njoj izmeniti željene opcije. Npr. ja sam u okviru *Clang-Tidy checks* kartice dodala pored podrazumevane `clang-*` opcije, `modernize-*` i `performance-*`

![img](clang/clang-tidy_clazy/ClangTidyAndClazy5.png)

Nakon ponovnog pokretanje analize primećujemo da ima mnogo više upozorenja i potencijalnih izmena u cilju poboljšanja koda:

![img](clang/clang-tidy_clazy/ClangTidyAndClazy6.png)

### ClangAnalyzer
**ClangAnalyzer** je alat za statičku analizu koda koji efikasno otkriva curenje memorije, probleme sa neinicijalizovanim promenljivama i dereferenciranje `null` pokazivača.
Fokusiran je na otkrivanje logičkih grešaka i pogodan je za velike projekte gde je teško ručno identifikovati greške.
Za pokretanje alata potrebno je pozicionirati se u `build` direktorijum i izvršiti sledeću komandu `scan-build make`.
Rezultat je `No bugs found` što se može videti na slici:

![img](clang/clang-analyzer/ClangAnalyzer.png)
