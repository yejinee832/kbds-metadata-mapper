# CA19-9 worked example and limits

The user's September 2026 structure report describes 91 clinical rows and 73 columns (A–BU), plus 178 FASTQ files corresponding to 89 R1/R2 pairs. Two QC-failed clinical records, PDAC637 and PDAC646, have `mRNA = "-"` and no corresponding raw reads in that inventory. Recompute all counts from the actual chosen inputs on every run; these are historical observations, not universal rules.

Source categories include `SAMPLE`, `mRNA`, `QC_Macrogen` (identity/QC); `AGE`, `SEX`, `ASA`; surgery; recurrence; CEA/CA19-9; survival (`OS`, `RFS`); tumor location/stage; PET (`PRE_PET`, `SUVmax`); pathology; neoadjuvant, adjuvant and post-recurrence treatment. Preserve original headers and clarify units, codes and clinical meaning with the research team.

The file-organizing report dated 2026-09-26 verified three copied documents by SHA-256 and set `submission_ready=false`; its study name remained `UNCONFIRMED`. That verification did not check whether the K-BDS BioSample or KRA fields were correctly mapped. A previous draft workbook explicitly used candidate IDs and lacked evidence for tissue, organism, library, instrument and protocol; do not promote those placeholders to facts.

For each cohort, request the currently approved BioSample and KRA workbook versions and independently inspect their actual field definitions. The local examples named `BioSample_metadata.xlsx` and `KRA_metadata.xlsx` are reference inputs, not proof of current portal validation rules.
