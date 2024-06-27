# Datový model pro reporting pro pobočkovou síť

## Vypracoval: David Mûller

Tento datový model byl naržen tak aby sledoval a reportoval aktuální i historické údaje o účtech, půjčkách, klientech, zaměstanancích a pobočkách. V modelu je zahrnuto 5 tabulek: ACCOUNTS, LOANS, CLIENT, WORKER a BRANCH.

![datovy_model](CSOB_ukol.jpg "datovy_model")

Každá tabulka musí obsahovat minimálně jeden sloupec, který bude unikátní a nebude nulový (NOT NULL) - Bude mít primírní klíč

Primární klíče jsem zvolil následující:

- Tabulka ACCOUNTS - sloupec acn - číslo účtu musí být unikátní
- Tabulka LOANS -  vytvořil bych nový sloupec lid - Id půjčky, který by zajišťoval, ža každý záznam bude unikátní. Teoreticky by se mohlo stát, že bude existovat více záznamů LOANS se stejným číslem účtu.
- Tabulka CLIENT - sloupec cid - id klienta
- Tabulka BRANCH - sloupec pob - id pobočky
- Tabulka WORKER - sloupec zam - id zaměstance

Pro zajištění integrity dat jsem v modelu také popsal cizí klíče (foregin key), které se odkazují na primární klíče v jiné tabulce a zajišťují tak vzájemné propojení

Cizí klíče jsou následující:

- Id klienta v tabluce CLIENT odkazuje na id klienta v tabulce ACCOUNTS - Zajistí, že bude účet klienta přiřazen existujícímu klientovi
- Číslo účtu v tabulce LOANS odkazuje na číslo účtu v tabulce ACCOUNTS - Zajistí, že půjčka odpovídá příslušnému účtu
- Id klienta v tabluce LOANS odkazuje na id klienta v tabulce CLIENT - To zajistí, že půjča odpovídá příslušnému klientovi (teoreticky to není nutné, protože odkaz už je vytvořen přes číslo účtu)
- Id zaměstanace v Tabulce WORKER odkazuje na id zaměstanace, který účet založil v tabulce ACCOUNT - Zajistí, že zaměstnanec bude odpovídat příslušnému účtu, který založil
- Id pobočky v tabulce WORKER Odkazuje na id pobočky v tabulce BRANCH. -  Zajistí, že každý zaměstnanec je přiřazen existující pobočce.

Pro přehled nově prodaných produktů v členění dle zaměstananců lze použít následující zjednodušený SELECT s využitím UNION příkazu:

```sql
SELECT
    acn AS product_id
FROM
    ACCOUNTS
WHERE
    zam = @konkretni_zam

UNION ALL

SELECT
    lid AS product_id
FROM
    LOANS
WHERE
   zam = @konkretni_zam

ORDER BY product_id
```

Pro zajištění možnosti reportovat data zpětně lze využít koncept tzv. dočasných tabulek (temporal tables). Dočasné tabulky mohou zvýšit velikost databáze více než běžné tabulky, zejména pokud uchovávávají historická data po delší dobu. Zásady uchovávání historických dat jsou proto důležitým aspektem plánování a správy životního cyklu každé dočasné tabulky. Pro vytváření dočsných tabulek lze využít službu Azure SQL Database, která umožňuje zadat dobu uchovávání.

Pro prezentaci výsledných reportů je vhodné využít nástroje business intelligence (BI) - například Power BI od Microsoftu
