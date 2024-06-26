# Datový model pro reporting pro pobočkovou síť
## Vypracoval: David Mûller
![datovy_model](CSOB_ukol.jpg "datovy_model")

Každá tabulka musí obsahovat minimálně jeden sloupec, který bude unikátní a nebude nulový (NOT NULL) - Bude mít primírní klíč

Primární klíče jsem zvolil následující:

- Tabulka ACCOUNTS -  acn - číslo účtu musí být unikátní
- Tabulka LOANS -  vytvořil bych nový sloupec lid (Id půjčky), který by zajišťoval, ža každý záznam bude unikátní
- Tabulka CLIENT - cid - id klienta
- Tabulka BRANCH - pob - id pobočky
- Tabulka WORKER - zam - id zaměstance

Pro zajištění integrity dat je také potřeba zvolit cizí klíče (foregin key), které se budou odkazovat na primární klíče v jiné tabulce

Cizí klíče jsou následující:

- Id klienta v tabluce CLIENT odkazuje na id klienta v tabulce ACCOUNTS - Zajistí, že bude účet klienta přiřazen existujícímu klientovi
- Číslo účtu v tabulce LOANS odkazuje na číslo účtu v tabulce ACCOUNTS - Zajistí, že půjčka odpovídá příslušnému účtu
- Id klienta v tabluce LOANS odkazuje na id klienta v tabulce CLIENT - To zajistí, že půjča odpovídá příslušnému klientovi (teoreticky to není nutné, protože odkaz už je vytvořen přes číslo účtu)
- Id zaměstanace v Tabulce WORKER odkazuje na id zaměstanace, který účet založil v tabulce ACCOUNT - Zajistí, že zaměstnanec bude odpovídat příslušnému účtu, který založil
- Id pobočky v tabulce WORKER Odkazuje na id pobočky v tabulce BRANCH. -  Zajistí, že každý zaměstnanec je přiřazen existující pobočce.

Pro zajištění možnosti reportovat data zpětně lze využít koncept tzv. dočasných tabulek (temporal tables)

Pro prezentaci výsledných reportů je vhodné využít nástroje business intelligence (BI) - například Power BI od Microsoftu
