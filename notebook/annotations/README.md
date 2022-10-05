# Core Entity Annotations

The 300 articles of the Europe PMC Corpus have been semantically annotated with three core entity types:

- Gene/Protein: refers to Uniprot and Protein Ontology.
- Disease: refers to ULMS and EFO disease.
- Organism: refers to NCBI Taxonomy.

Considering updating of ontology databases, we use the expertise of curator instead of controlled vocabulary for annotations. 
However, we advise curators to refer to the listed databases if necessary.   

## Raw [Hypothes.is](https://web.hypothes.is) annotations
- ```hyppthesis/csv/```: contains raw annotations fetched from the annotation platform [Hypothes.is](https://web.hypothes.is) in comma-separated values (CSV) format.
    - ```GROUP0/```: each CSV file contains raw manual annotations made by curator GROUP0.
    - ```GROUP1/```: each CSV file contains raw manual annotations made by curator GROUP1.
    - ```GROUP2/```: each CSV file contains raw manual annotations made by curator GROUP2.

NOTE: Each CSV file contains raw manual annotations of all the three core entity types. 

#### CSV Structure
Each CSV file has 17 columns:

| Column name       | Description                             | Column name     | Description                                           |
| :----------------:|:---------------------------------------:| :-------------: | :----------------------------------------------------:|
|```id```           | unique ID of an annotation              |```end```        | the end index of an annotation                        |
|```group_id```     | unique ID of a group                    |```tags```       | the tag of an annotation                              |
|```source```       | link of an article                      |```all```        | indicator, if all the occurrences should be annotated |
|```exact```        | the annotation                          |```origin_tags```| the original raw tags, without preprocessing          |
|```prefix```       | characters appear before the annotation |```comment```    | comment left by curator                               |
|```suffix```       | characters appear after the annotation  |```user```       | user ID of curator                                    |
|```anno_type```    | text selector                           |```title```      | title of an article                                   |
|```position_type```| text position selector                  |```created```    | time the annotation is created                        |
|```start```        | the start index of an annotation        |                 |                                                       |

NOTE: ```anno_type```, ```position_type```, ```start``` and ```end``` are platform (Hypothes.is) specific and therefore not really useful for annotation extraction.

## IOB: Annotations in IOB format
- ```IOB/```: contains automatically extracted annotations using raw manual annotations in ```hypothesis/csv/```, which is in Inside–Outside–Beginning tagging format.
    - ```dev/```: contains IOB format annotations of 45 articles, suppose to be used a dev set in machine learning task.
    - ```test/```: contains IOB format annotations of 45 articles, suppose to be used a test set in machine learning task.
    - ```train/```: contains IOB format annotations of 210 articles, suppose to be used a training set in machine learning task.

#### IOB Tagging Format:
For the meaning of IOB tags used in Europe PMC corpus, please see following descriptions:

```I```: Inside; ```O```: Outside; ```B```: Beginning

```GP```: Gene/Protein; ```DS```: Disease; ```OG```: Organism

#### TSV Structure
Each TSV file has 2 columns and they are separated by a Tab:

Example:
```
Minimum	O
requirements	O
for	O
ookinete	O
to	O
oocyst	O
transformation	O
in	O
Plasmodium	B-OG
```
    
## JSON: Annotations in JSON format

- ```JSON/```: contains automatically extracted annotations using raw manual annotations in ```hypothesis/csv/```, which is in JSON format.

NOTE: JSON format contains both NER annotations and Gene-Disease relationship annotations. Similar to IOB tagging format, there are three types of ```NER``` tags: ```GP```, ```DS``` and ```OG```. 
Gene-Disease relationship annotations has two types of values: ```YGD``` denotes has Gene-Disease relationship but ```NGD``` denotes no Gene-Disease relationship.

#### JSON Format
The annotations organised in JSON format have following structure:

Example:
```json
{
  "PMC2474741": 
    {
      "annotations": 
        [
          {
            "sid": 0, 
            "sent": "Minimum requirements for ookinete to oocyst transformation in Plasmodium ", 
            "section": "TITLE", 
            "ner": [[62, 72, "Plasmodium", "OG"]], 
            "rel": null
          },
          ...
        ],
      "doc_len": 481
    }
}
```

```ner```: contains NER annotations. Could be ```null``` or a list of NER annotations. Each NER annotation has four entires ```[start_index, end_index, annotation, tag]```

```rel```: contains Gene-Disease relationship annotation. Could be ```null```, ```YGD``` or ```NGD```. 