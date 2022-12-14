# Onsides
A resource of adverse drug effects extracted from FDA structured product labels.

## V01

Initial release of the Onsides database of adverse reactions and boxed warnings extracted from the FDA structured product labels. All labels available to download from DailyMed (https://dailymed.nlm.nih.gov/dailymed/spl-resources-all-drug-labels.cfm) as of April 2022 were processed in this analysis. In total 2.7 million adverse reactions were extracted from 42,000 labels for just under 2,000 drug ingredients or combination of ingredients.

Onsides was created using the ClinicalBERT language model and 200 manually curated labels available from [Denmer-Fushman et al.](https://pubmed.ncbi.nlm.nih.gov/29381145/). The model achieves an F1 score of 0.86, AUROC of 0.88, and AUPR of 0.91 at extracting effects from the ADVERSE REACTIONS section of the label and an F1 score of 0.66, AUROC of 0.71, and AUPR of 0.60 at extracting effects from the BOXED WARNINGS section.

### Download

The data are available as a set of SQL tables or as flat files in CSV format.

#### SQL File
[onsides_v01_20220430.sql.gz](https://github.com/tatonetti-lab/onsides/releases/download/v01/onsides_v01_20220430.sql.gz) (81MB, md5:b386e9485e943120c9a783edf843d68e)

#### CSV Files
[onsides_v01_20220430.tar.gz](https://github.com/tatonetti-lab/onsides/releases/download/v01/onsides_v01_20220430.tar.gz) (81MB, md5:f73ded83cf5edc63447f6ca8b80add66)


### Description of Tables

Below is a brief description of the tables. See [`SCHEMA.md`](SCHEMA.md) for column descritpions and [`src/load_onsides_db.sql`](src/load_onsides_db.sql) for more details.

`adverse_reactions` - Main table of adverse reactions. This table includes adverse reactions extracted from the ADVERSE REACTIONS section of the most recent label for each ingredient or combination of ingredients. 94,029 rows.

`adverse_reactions_bylabel` - All extracted adverse reactions from the ADVERSE REACTIONS section of all labels. Each drug will have multiple labels over its lifetime (revisions, generic alternatives, etc.). This table contains the results of extracting adverse reactions from every label available for download from DailyMed. 2,764,338 rows.

`boxed_warnings` - Main table of boxed warnings. This table includes adverse reactions extracted from the BOXED WARNINGS section of the most recent label for each ingredient or combination of ingredients. 2,907 rows.

`boxed_warnings_bylabel` - All extracted adverse reactions from the BOXED WARNINGS section of all labels (including revisions, generics, etc). 67,984 rows.

`ingredients` - Active ingredients for each of the parsed labels. If the label is for a drug with a single active compound then there will be only a single row for that label.  If the label is for a combination of compounds then there will be multiple rows for that label. 52,646 rows.

`label_map` - Map between the xml_id (xml filename), zip_id (zip filename), and set_id (drug identifier). 45,989 rows.

`latest_labels_bydrug` - Most recent label (as determined from the zip filename for each drug or combination of drugs. 2,051 rows.

`rxnorm_map` - Map between set_id and rx_cui (product level) and also includes the structured product label version and a string description of the product. 443,620 rows.

`rxnorm_product_to_ingredient` - Map between the RxNorm product to the active ingredients in that product. 254,036 rows.

`rxnorm_to_setid` - Map between set_id and rx_cui. 141,915 rows.

### Caveats

The Onsides database is intended for research purposes only. The extraction process is far from perfect, side effects will be missed and some identified will be incorrect. Patients seeking health information should not trust these data and instead refer to the FDA's website (fda.gov) and consult their doctor.

The project is under active development. Validation of extracted information is yet to be independently verified and the data, methods, and statistics are subject to change at any time. Check back to this page for updates. If you would like to to contribute to the project or have ideas on how the methods, data, or evaluation can be improved please reach out to Dr. Tatonetti via email or [Twitter](http://twitter.com/nicktatonetti).

### Citation

A manuscript describing the data, methods, and results is in preparation. In the mean time, please reference the github repository. [![DOI](https://zenodo.org/badge/479583027.svg)](https://zenodo.org/badge/latestdoi/479583027)
