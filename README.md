# mapping-methodology
This repository contains a script to create a crosswalk between Codemeta and another specification or vocabulary, as well as some examples of generation.

The methodology for generating the files required to create the crosswalk is as follows:
1. **Identify the Terms to Map**<br>
Identify the terms you want to map in both semantic artifacts (source and target). Source applies to Codemeta while the target is another metadata schema.
2. **Create a YAML Metadata File**<br>
Create a YAML metadata file that contains the metadata of the mapping using the provided template: mapping-template.yml.
Name the file appropriately, for example, codemeta-bibtext-mapping.yml.
3. **Complete the Required Metadata Fields**<br>
- mapping_date: The date when the mapping was created (use ISO XXXX format).
- mapping_set_description: A description of the mapping, including references to the involved domains and the purpose of the mapping.
- mapping_provider: Information about the entity responsible for the mapping. Include a URI if possible.
- mapping_set_id: The URI of the mapping (see next step). Consider using persistent identifiers instead of standard URIs. A persistent identifier can be minted using the w3id.org project. Please, add it in the project codemeta/crosswalks
- object_source: The URI of the specification or vocabulary. Use persistent identifiers whenever possible.
4. **Create the Data File (CSV Format)**<br>
Name the CSV file appropriately, for example, codemeta-bibtext-mapping.csv.
The CSV file should contain the following columns:
- source_term: The term in CodeMeta.
- target_term: The corresponding term in the target specification or vocabulary.
- type_relation: The type of relation between the two terms. Possible values:
    - exact_match: The terms are equivalent concepts.
    - part_of: The target concept is part of the source concept (e.g., a publication year in the target vocabulary is part of a publication date in the source vocabulary).
    - more_specific_than: The target vocabulary term is more specific than the target codemeta term. For example, "codemeta:identifier narrowerMatch hal_id" means that the hal id is a more concrete term than the "identifier" property in CodeMeta. 
    - more_generic_than:  The target vocabulary term is more generic than target codemeta term. For example, "codemeta:developmentStatus broaderMatch status" means that the status is a more generic term than the "developmentStatus" property in CodeMeta.
    - close_match: The subject and the object are sufficiently similar that they can be used interchangeably in some information retrieval applications. We do NOT recommend using this.
- combined_mapping: It applies to a part-of relationship. This column defines the formula used to combine terms between schemes.  For example, if the source vocabulary has the term date, and the target vocabulary has the terms year and month, the CSV should contain two separate lines. This column will contain the formula date: $(year) + $(month)

5. **Generate the SSSOM File**<br>
Execute the script (generate_crosswalk.py) to generate an .sssom.tsv file from the CSV.
```bash
python .\generate_crosswalk.py examples/bibtex-codemeta-mappings.yaml examples/bibtex-codemeta-mappings.csv
