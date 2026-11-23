# DPBI – E-commerce dashboard Olist

Závěrečný projekt z předmětu DPBI. Report nad datasetem [Olist Brazilian E-Commerce](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (~100 tis. objednávek, 2016–2018):
kde, co a komu Olist prodává a jak zpoždění doručení snižuje hodnocení (včas 4,3 ★ vs. pozdě 2,3 ★).

| Nástroj | K čemu |
|---|---|
| **PBIP** (Power BI Project) | projekt uložený jako textové soubory místo binárního `.pbix` |
| **TMDL** | datový model – tabulky, relace, výpočtové sloupce a measures v DAX |
| **PBIR** | report – stránky a vizuály jako JSON |
| **[Claude Code](https://claude.com/claude-code)** | AI asistent, se kterým jsem soubory programaticky psal a upravoval |
| **Plugins & skills** z [power-bi-agentic-development](https://github.com/data-goblin/power-bi-agentic-development) (Data Goblins, Kurt Buhler) | `pbip`, `semantic-models`, `reports` – znalosti TMDL/PBIR/DAX a validační hooky pro Claude Code |
| **[pbir CLI](https://pypi.org/project/pbir-cli/)** (Kurt Buhler & Maxim Anatsko) | přidávání a úprava vizuálů, validace reportu |
| **Git / GitHub** | přenos mezi Macem a Windows |
| **Power BI Desktop** (Windows) | načtení dat, kontrola, finální `.pbix` |
