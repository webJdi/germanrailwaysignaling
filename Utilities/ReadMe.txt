------------------------------------------------------------
How to Download the GERALD Dataset
------------------------------------------------------------

The GERALD dataset is publicly available and can be downloaded directly from
the official RWTH Aachen University publications archive.

Direct download link for the dataset ZIP file:

    https://publications.rwth-aachen.de/record/980030/files/GERALD.zip

This link begins downloading the full archive:
    
    GERALD.zip

Steps:

1. Open the direct download URL in your browser:
       https://publications.rwth-aachen.de/record/980030/files/GERALD.zip

2. Save the file 'GERALD.zip' to your local machine.

3. Place 'GERALD.zip' in the same directory as the subset creation script or
   Jupyter notebook.

4. Do NOT unzip it -> the subset generator reads directly from the ZIP archive.

Once the ZIP is in place, simply run the notebook/script to generate the
balanced subset.

------------------------------------------------------------------------
GERALD Balanced Subset Creator
------------------------------------------------------------------------

This tool creates a balanced, size limited subset of the GERALD dataset,
grouped across weather x light conditions and re mapped into a simplified
signal taxonomy.

File: gerald_subset_creator.ipynb
Output folder: GERALD_subset/


------------------------------------------------------------
Overview
------------------------------------------------------------

The ipynb extracts a balanced subset from the GERALD traffic signal dataset
directly from the GERALD.zip archive. It ensures:

1. Weather x light conditions remain proportionally represented.
2. Only the classes:
      - main_signal
      - distant_signal
   are kept.
3. The final dataset stays under a configurable size limit (default: 2.4 GB).
4. Each annotation gets metadata (weather + light) injected into the XML.
5. A validation stage checks:
      - image/annotation consistency
      - class names
      - class statistics
6. A distribution plot 'subset_distribution.png' is saved.

The final output includes:
- /images/           -> filtered images
- /annotations/      -> filtered & cleaned PascalVOC XML files
- info.json          -> copied from source
- class_mapping.json -> mapping + class statistics
- subset_distribution.png


------------------------------------------------------------
Class Remapping
------------------------------------------------------------

A set of original GERALD class names (aliases) are grouped into
two target categories:

main_signal:
    Hp_0_HV, Hp_1, Hp_2, Hp_0_Sh, Ks_1, Ks_2

distant_signal:
    Vr_0, Vr_1, Vr_2

Aliases are matched by substring (case-insensitive).


------------------------------------------------------------
Automatic Weather x Light Balancing
------------------------------------------------------------

The script indexes all (weather, light) combinations, counts the
XML files in each group, then computes the median group size.

MAX_PER_PAIR is chosen automatically:

    MAX_PER_PAIR = clamp(median(group_sizes), 50, 300)

This ensures more balanced selection without manual tuning.

Example output:
    Group sizes: [923, 1348, 506, 559, 73, 277, 5, 685, 383, 6, 71, 8, 156]
    Median = 277
    Auto selected MAX_PER_PAIR = 277
    Total images selected = 2258


------------------------------------------------------------
Typical Output from a Successful Run
------------------------------------------------------------

Copying dataset...
Final dataset size: 1485.87 MB

Dataset summary:
    Images saved:       1967
    Annotations:        1967
    Total objects:      3942

Objects per class:
    main_signal:        2551
    distant_signal:     1391

All checks passed! Dataset is fully consistent.


------------------------------------------------------------
How to Run
------------------------------------------------------------

1. Place 'GERALD.zip' in the same directory as the notebook or script.

2. Open and run:
       gerald_subset_creator.ipynb
   OR run the script directly (if exported as .py).

3. The output will appear in:
       GERALD_subset/


------------------------------------------------------------
Configuration
------------------------------------------------------------

Edit these variables in the script if needed:

    OUT_DIR     -> output folder name
    TARGET_MB   -> max final dataset size (default 2400 MB)
    CLASSES_KEEP -> classes to retain
    CATEGORY_ALIASES -> alias rules for class mapping


------------------------------------------------------------
Validation
------------------------------------------------------------

The validation step checks:
- every annotation has a matching image
- all classes are valid
- object counts match class_mapping.json

If anything does not match, validation fails with AssertionError.




