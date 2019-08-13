---
category: Principy pro vývoj
expires: 2019-03-31
---

# Principy vývoje

Tyto principy neříkají, jakou strukturu by měl mít váš kód, konkrétní kroky procesu nebo jak použít určité technologie - tato se bude měnit na kontextu službu nebo projektu.

Tyto principy nejsou v jasném pořadí, a jsou vždy otevřené k debatě. V případě, že však půjdete vlastní cestou, očekávejte otázky.

## Obecné principy

### 1. Veškerý kód a konfigurace je udržována v profesionálně spravovaných repositářích

**Proč je to důležité**

Ztráta kódu, nechtěné přepsání kódu, zmatek jaká verze kódu je správná a nemožnost opakovaně budovat aplikace, bude v budoucnu zpomalovat vývoj a bude mít dopad na kvalitu systému. Kód a konfigurace systému musí být v na bezpečném místě a musí být lidem dostupná, aby tím mohli spolupracovat.

**Jak to může vypadat**

Jestli mít kód a config můžete mít v jednom nebo více repositářích záleží na povaze služby a její vyspělosti. Repositář s kódem a configem musí splňovat to, že je:

  - to centrální repositář
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

Do prostředí je manualní nasazení riskantní a více náchylné na chyby. Zvláště ve chvílích, kdy je nasazení kritické. Automatizované nasazení většinu těchto rizik odstraňuje.

**Jak to může vypadat**

Nasazování do jakéhokoliv spravovaného nebo kontrolovaného prostředí jsou automatizována pomocí nástrojů jako GitLab nebo Jenkins. Spravované prostředí jako: produkce, pre-prod, QA, nebo systémové integrační testování. Automatizované nasazení je spuštěno podle kroků z předchozí build pipeliny, nebou je supštěno manuálně. Pokud budete interagovat manuálně, dělejte tak co možná nejméně. Ptejte se například pouze na: 

  -  číslo buildu
  -  cílové prostředí

Všechny jiné environment-specific informace musí přijít z daného místa jako repositář nebo Jenkins build job.

**Kontrola**


Můžete:

  -  někomu ukázat, jak nasazejte do vašeho prostředí?
  -  sebevědomě říct, jak dlouho nasazení zabere?
  -  popsat, jaké nástroje používáte pro automatizované nasazování?
  -  ukázat, kde jsou vaše deployment skripty zálohovány?
