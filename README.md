#  Understanding citation contexts with structural position awareness Dataset

This repository contains the dataset and resources used in this paper:

**"Understanding citation contexts with structural position awareness"** (Submitted to MIWAI 2025)

---

##  Dataset 

The dataset is derived from 818 full-text articles from the Journal of Informetrics (Vol. 7–16), retrieved via the Scopus API. It is designed for training and evaluating citation intent classification models by incorporating section-level information.

###  Dataset used in this research 

Preprocessed Dataset.csv: This CSV file is the final processed dataset used in this experiment. It has the following schema:

```json

{
  "paper_id": "10.1016_j.joi.2012.07.006",
  "sentence": "In other words, it is a pattern of contacts which are created due to the flow of information among the participating actors ([bib0295]).",
  "sec_id": "sec0005",
  "section_name": "5.Conclusion",
  "section_cat": "Conclusion",
  "intent": "background"
}

```
'paper_id':       Unique article identifier (e.g., DOI or internal ID)                    
'sentence':        The citation sentence from the article                                  
'sec_id':            Section ID of the citation sentence                                     
'section_name':      Raw section name (e.g., "2. Related Work")                              
'section_cat':      Normalized section category (e.g., Introduction, Method, Results, etc.) 
'intent':         Citation intent label (e.g., background, uses, motivation, etc.)   

### Intent Label Distribution


Total Instances: 26,246
```json
{
  "background": 21657,
  "uses": 2684,
  "similarities": 738,
  "differences": 527,
  "motivation": 433,
  "extends": 134,
  "future_work": 61
}
```
*Note: Some minor label noise exists (e.g., misparsed section names), but no filtering was applied after preprocessing.

### Citation sentence dataset Components

### sentence.zip
Contains citation-level annotations from 818 papers. It has the following schema:

```json
{
  "sentence": "The method described by Smith et al. [3] was adopted in our study.",
  "bib_id": "bib3",
  "intent": "uses",
  "offset_start": 1985,
  "offset_end": 2023
}
```
### section.zip
Contains structural metadata for each paper. One JSON per article. It has the following schema:

```json
{
  "section_id": "sec0003",
  "title": "2. Related Work",
  "start": 1345,
  "end": 1894,
  "depth": 1
}
```


### controlled_outline.json
Maps noisy or variable section names to standardized categories, It has the following schema:
```json
{
  "2.Relatedwork": "Introduction",
  "5.Experimentalsetup": "Method"
}
```
### bibid_check.json
Links in-text citation markers to external identifiers like DOIs, It has the following schema:
```json
{
  "bib3": "10.1000/xyz123",
  "bib7": "10.1016/j.artint.2020.103423"
}
```
## Data Preprocessing 

The following steps were applied to create Preprocessed Dataset.csv:

The construction of Preprocessed_Dataset.csv involved the following steps:

1. Extract citation sentences from sentence.zip, *_sentence_intent.json files, which contain both the citation sentence and its labeled intent. In order to avoid ambiguity and ensure a more balanced dataset for classification—particularly given the limited number of multi-intent examples, only the first listed intent was retained as the primary label.

2. Map each citation to its corresponding section using metadata from section.zip, adding fields such as sec_id and section_name.

3. Normalize section names using controlled_outline.json, resulting in a standardized section category (section_cat) such as Introduction, Method, etc.

4. Resolve citation IDs by linking in-text citation markers to the corresponding bibliography entries using bibid_check.json.

5. Merge all information into a unified DataFrame, preserving paper_id, sentence, section_cat, and the simplified intent, and export it as Preprocessed_Dataset.csv.
