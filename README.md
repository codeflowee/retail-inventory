# retail-inventory

**Jaekaupluste müügimahtude ja laovarude kirjeldav analüüs.** Seminaritöö 1 õppeaines RAM0580 Andmeanalüüs (TalTech).

Töö uurib, millest sõltub toote päevane müügimaht (`Units.Sold`). Andmed loetakse R-i sisse ja puhastatakse, tunnused teisendatakse ning sihttunnuse seoseid arvuliste ja kategoriaalsete tunnustega kirjeldatakse joonistel ja arvkarakteristikute abil.

## Andmestik

[Retail Store Inventory Forecasting Dataset](https://www.kaggle.com/datasets/anirudhchauhan/retail-store-inventory-forecasting-dataset) (Kaggle, autor Anirudh Chauhan).

- 73 100 rida: 5 kauplust × 20 toodet × 731 päeva (01.01.2022 – 01.01.2024)
- 15 tunnust: laoseis, müük, tellimused, nõudluse prognoos, hind, allahindlus, konkurendi hind, kategooria, piirkond, ilm, pühad/kampaaniad, aastaaeg
- Andmestik on **sünteetiline**, st genereeritud müügi prognoosimise harjutamiseks

## Failid

| Fail | Sisu |
| --- | --- |
| `retail-inventory-seminar1.Rmd` | Analüüs (R Markdown) |
| `retail-inventory-seminar1.html` | Renderdatud raport (esitatav dokument) |
| `retail_store_inventory.csv` | Algandmed Kaggle'ist |
| `retail-inventory.Rproj` | RStudio projekt |

## Käivitamine

Vaja on R-i (≥ 4.0), RStudiot või pandoc'i ning pakette:

```r
install.packages(c("rmarkdown", "DT", "skimr"))
```

Ava `retail-inventory.Rproj` RStudios ja vajuta failis `retail-inventory-seminar1.Rmd` nuppu **Knit**. Käsurealt saab raporti renderdada nii:

```r
rmarkdown::render("retail-inventory-seminar1.Rmd")
```

Andmed asuvad juba repositooriumis. Kui soovid need uuesti alla laadida, saad seda teha ilma Kaggle'i kontota:

```bash
curl -L -o dataset.zip "https://www.kaggle.com/api/v1/datasets/download/anirudhchauhan/retail-store-inventory-forecasting-dataset"
unzip dataset.zip
```

## Töö käik

1. **Ülevaade.** `str()`, tunnuste tüübid, sihttunnuse ja tegurite valik, `skim()`.
2. **Teisendused.** Tekstina loetud kuupäevast saab `Date` tüüpi tunnus, millest tuletatakse kuu (`Month`) ja aastaaeg (`Season`). `Holiday.Promotion` (0/1) muudetakse faktoriks.
3. **Puhastamine.** Kontrollitakse duplikaate ja puuduvaid väärtusi. Peidetud puuduvad väärtused on negatiivsed nõudluse prognoosid: need asendatakse `NA`-ga ja tunnus jäetakse välja.
4. **Seosed.** Arvuliste tunnuste seoseid vaadeldakse hajuvusdiagrammide maatriksil ja korrelatsioonide abil, kategoriaalsete tunnustega seost karpdiagrammidel.
5. **Valitud tunnuste analüüs.** Analüüsitakse `Units.Sold`, `Inventory.Level` ja `Category`: histogrammid, sagedustabelid, arvkarakteristikud ja erindid.

## Peamised tulemused

- Andmestikus ei ole duplikaate ega puuduvaid lahtreid, kuid tunnuste vahel on loogilisi vastuolusid:
  - `Seasonality` ei vasta kuupäevale;
  - sama kauplus esineb kõigis piirkondades;
  - sama toode kuulub kõigisse kategooriatesse.
- `Demand.Forecast` on sisuliselt tegelik müük, millele on lisatud müra (r = 0,997). Mudelis tekitaks see andmelekke.
- Müük sõltub ainult **laoseisust** (r ≈ 0,59). Laoseis on müügi ülempiir.
- Hind, allahindlus, kategooria, kauplus, piirkond, ilm, kampaaniad ja aastaaeg müüki ei mõjuta. See kinnitab, et andmed on genereeritud.

## Autor

Andre Toodre, 2026
