---
category: Principy pro vývoj
expires: 2019-03-31
---

# Principy vývoje

Tyto principy neříkají, jakou strukturu by měl mít váš kód, konkrétní kroky procesu nebo jak použít určité technologie - to se bude měnit v kontextu služby nebo projektu.

Tyto principy nejsou v jasném pořadí, a jsou vždy otevřené k debatě. V případě, že však půjdete vlastní cestou, očekávejte otázky.

## Obecné principy

### 1. Veškerý kód a konfigurace je udržována v profesionálně spravovaných repositářích

**Proč je to důležité**

Ztráta kódu, nechtěné přepsání kódu, zmatek jaká verze kódu je správná a nemožnost opakovaně budovat aplikace, bude v budoucnu zpomalovat vývoj a bude mít dopad na kvalitu systému. Kód a konfigurace systému musí být na bezpečném místě, které musí být ostatním dostupné, aby nad tím mohli spolupracovat.

**Jak to může vypadat**

Jestli mít kód a config v jednom nebo více repositářích záleží na povaze služby a její vyspělosti. Repositář s kódem a configem musí splňovat následující:

  - jedná se centrální repositář
  - přístupný těm členům týmu, který přístup potřebují
  - v souladu se standardy bezpečnosti MSp
  - dostupný, když tým nebo služba potřebují
  - často zálohován

**Kontrola**

Můžete:

  -  někomu ukázat daný repositář nebo repositáře?
  -  vysvětlit, jak udržujete konfiguraci systému?
  -  vysvětlit, jak často je váš kód a/nebo config zálohován?
  -  předvést, že běžně kontrolujete kód a/nebo config, ideálně alespoň jednou za den?
  -  někomu ukázat, kde ukládáte build skripty, deployment skripty a infrastrukturní build skripty?

### 2. Všechny systémy mají automatizované testy

**Proč je to důležité**

Dobré automatizovné testy umožňují týmu postupovat vpřed, jelikož ví, že nic nerozbil.

**Jak to může vypadat**

Typy testů, počet testů a co testují se může měnit na základě systému a/nebo záviset na použité technologii.

**Kontrola**

Můžete:

  -  někomu automatizované testy v vašem repositáři nebo repositářích ukázat?
  -  ukázat, že vaše testy (skoro) vždy projdou?
  -  vysvětlit, co se stane, když se vaše testy selžou?
  -  vysvětlit, kde testy spouštíte a ukázat výsledek předešlých testů, co jste spustili?
  -  testy spustit teď hned?


### 3. Jakékoliv nasazení (deployment) aplikace je automatizovaný

Do prostředí je manualní nasazení nejen riskantní, ale i více náchylné na chyby. Zvláště ve chvílích, kdy je nasazení potřeba. Automatizované nasazení většinu těchto rizik odstraňuje.

**Jak to může vypadat**

Nasazování do jakéhokoliv spravovaného nebo kontrolovaného prostředí jsou automatizována pomocí nástrojů jako GitLab nebo Jenkins. Spravovaným prostředím je myšleno: produkce, pre-prod, QA, nebo systémové integrační testování. Automatizované nasazení je spuštěno buď podle kroků z předchozí build pipeliny, nebo je spuštěno manuálně. Pokud už budete s nasazením interagovat manuálně, dělejte tak co možná nejméně. Ptejte se například pouze na: 

  -  číslo buildu
  -  cílové prostředí

Všechny jiné environment-specific informace musí přijít z místa jako je repositář nebo build job Jenkinsu.

**Kontrola**


Můžete:

  -  někomu ukázat, jak nasazejte do vašeho prostředí?
  -  sebevědomě říct, jak dlouho nasazení potrvá?
  -  popsat, jaké nástroje používáte pro automatizované nasazování?
  -  ukázat, kde jsou vaše deployment skripty zálohovány?

### 4. Systém může být vrácen zpět (roll-back) do poslední fungujícího stavu

**Proč je to důležité**

Pokud nasazení do spravovaného prostředí selže, služba musí přesto dál bezpečně pracovat nebo být obnovena do původní stavu co možná nejdříve. Pokud služba nefunguje, nemá hodnotu.

**Jak to může vypadat**

Obnovením může být automatizovaný roll-back nebo krátký popis manuálních kroků, například, vypnout aplikaci, obnovit databázi, re-deploy předchozí verzi, restartovat aplikaci. Jakýkoliv rollback musí zaručit, že data jsou obnovena do bezpečného, fungujícího stavu a že se z důvodu selhání nasazení data neztratila.

**Kontrola**

Můžete:

  -  ukázat opatření, která během vývoje děláte, abyste zajistili, že při selhání nasazení můžete bezpečně udělat roll-back do předchozícho funkčního stavu?
  -  popsat, co se stane, pokud selže nasazení do produkce?
  -  indentifikovat, kdo autorizuje roll-back?
  -  vysvětlit, jak byste validovali systém po roll-backu?
  -  dokázat, že jste testovali roll-back v prostředí podobné produkci?

### 5. Chyby a eventy jsou loggovány tak, aby bylo možné je centrálně monitorovat

**Proč je to důležité**

Primární způsob, jak zjistit zdraví aplikace je skrz logy. Tyto logy by měly být v čitelném a pochopitelném formátu a měly by být odesílány na místo, kde mohou být monitorovány všemi příslušnými stakeholdery.

**Jak to může vypadat**


**Kontrola**

Můžete:

  -  ukázat někomu poslední zápisy do logu vygenerované aplikací?
  -  ukázat, jako jsou vaše logy monitorovány a kdo je monitoruje?
  -  vysvětlit, co se stane, pokud je do logu zapsána chyba?

### 6. Architektura systému je zdokumentována

**Proč je to důležité**

Postupně jak se systém vyvíjí, tak vznikají a objevují se spousty informací. Tyto informace jsou podstatné jak pro stakeholdery, tak do budoucna obecně. Informace, které nelze zachytit, nelze zdokumentovat.

**Jak to může vypadat**

Informace o hlavních komponentách systému a jeho širší architektuře jsou zachyceny na wiki stránkách. Tyto stránky obsahují:

  -  celkovou strukturu systému
  -  hlavní architektonická rozhodnutí
  -  způsoby, jak pomoc lidem se v architektuře navigovat, obsahuje i architekturu infrastruktury, architekturu nasazovaní (deployment), a proces vývoje, stejně tak i datovou strukturu

**Kontrola**

Můžete:

  -  odkázat někoho na místo s wiki, kde mohu najít dokumentaci k architektuře systému?
  -  dokázat, že je dokumentace aktuální k dnešnímu dni?

