# DPBI - Závěrečný projekt
Závěrečný projekt do předmětu **DPBI** – Zpracování dat s PowerBI.

## Data

[Brazilian E-Commerce Public Dataset by Olist](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (Kaggle)

8 CSV tabulek (objednávky, položky, platby, recenze, zákazníci, prodejci, produkty, překlad kategorií).

## Stránky reportu

| Stránka | Odpovídá na otázku |
|---|---|
| **Úvod** | O datech, o reportu a jak vznikl |
| **Přehled** | Jak se vyvíjejí tržby a objednávky? (přepínač graf / tabulka přes bookmarky) |
| **Geografie a doručení** | Ve kterých státech se doručuje pozdě a jak to ovlivní hodnocení? |
| **Produkty a prodejci** | Které kategorie a prodejci táhnou tržby? |
| **Zákazníci a hodnocení** | Jak jsou zákazníci spokojení a kdy nakupují? |

Slicery **Rok** a **Region** jsou synchronizované napříč stránkami, vizuály se vzájemně filtrují.

## Datový model

Hvězdicové schéma v režimu Import: fakta `Orders`, `OrderItems`, `Payments`, `Reviews`,
dimenze `Customers`, `Sellers`, `Products`, `States` a DAX tabulka `Date`.

- **Výpočtové sloupce** – doba doručení, zpoždění ve dnech, stav doručení (Včas / Pozdě / Nedoručeno), koše zpoždění, sentiment recenze, váhová třída produktu
- **Measures** – tržby, GMV, AOV, opakovaní zákazníci, YoY a running total, on-time rate, rozdíl hodnocení včas vs. pozdě, ranking, dynamické barvy pro podmíněné formátování

## Jak report vznikl

| Nástroj | K čemu |
|---|---|
| **PBIP** (Power BI Project) | projekt jako textové soubory místo binárního `.pbix` → verzování v gitu |
| **TMDL** | sémantický model – tabulky, relace, DAX |
| **PBIR** | report – stránky, vizuály a bookmarky jako JSON |
| **Claude Code** | AI agent v terminálu, který psal DAX, Power Query i definice vizuálů |
| **[power-bi-agentic-development](https://github.com/data-goblin/power-bi-agentic-development)** (Data Goblins, Kurt Buhler) | pluginy a skills `pbip`, `semantic-models`, `reports` – znalost TMDL/PBIR/DAX + validační hooky |
| **[pbir CLI](https://pypi.org/project/pbir-cli/)** (Kurt Buhler & Maxim Anatsko) | přidávání a formátování vizuálů, validace reportu |
| **Git / GitHub** | verzování |

Inspirováno přístupem Data Goblins k vývoji Power BI jako kódu.
