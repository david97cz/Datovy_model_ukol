# Datový model pro reporting pro pobočkovou síť
## Vypracoval: David Mûller
![datovy_model](CSOB_ukol.jpg "datovy_model")
Každá tabulka musí obsahovat minimálně jeden sloupec, který bude unikátní - Bude mít primírní klíč

Primární klíče byly zvoleny následující:

 - Tabulka ACCOUNTS -  acn - číslo účtu musí být unikátní
 - Tabulka LOANS -  nový záznam idl - ID jednotlivého úvěru
 - Tabulka CLIENT - cid - id klienta
 - Tabulka BRANCH - pob - id pobočky
 - Tabulka WORKER - zam - id zaměstance

Pro zajištění možnosti reportovat data zpětně lze využít koncept tzv. dočasných tabulek (temporal tables)

Pro prezentaci výsledných reportů je vhodné využít nástroje business intelligence (BI) - například Power BI od Microsoftu
