# Medical Data Generation using Transformers


Healthcare Industry has been long facing the issue of data scarcity due to the stringent government policies to ensure privacy of an individual and prevent misuse. Our research is focused on building a model to generate clean useful data and preserve the confidentiality of original data while maintaining the statistical, syntactical, and semantic knowledge of the data.

### Dataset

#### Dataset used: MIMIC III 

MIMIC-III is a large, freely-available database comprising deidentified health-related data associated with over forty thousand patients who stayed in critical care units of the Beth Israel Deaconess Medical Center between 2001 and 2012.

#### Data Description
- 26 relational tables
- Tables are linked by identifiers which have suffix 'ID'
- Charted events such as notes, laboratory tests, and fluid balance are stored in a series of ‘events’ tables.
- For example the `OUTPUTEVENTS` table contains all measurements related to output for a given patient, while the `LABEVENTS` table contains laboratory test results for a patient.
- Tables prefixed with ‘D_’ are dictionary tables and provide definitions for identifiers. 
- Broadly speaking, five tables are used to define and track patient stays: `ADMISSIONS`; `PATIENTS`; `ICUSTAYS`; `SERVICES`; and `TRANSFERS`.
- Another five tables are dictionaries for cross-referencing codes against their respective definitions: `D_CPT`; `D_ICD_DIAGNOSES`; `D_ICD_PROCEDURES`; `D_ITEMS`; and `D_LABITEMS`.
- The remaining tables contain data associated with patient care, such as physiological measurements, caregiver observations, and billing information.
