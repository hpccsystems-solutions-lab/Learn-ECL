# Dataset

A Dataset is a set of records that an ECL program can manipulate. A Dataset can be initialized using a logical file (File Dataset), inline data (Inline Dataset) or by another ECL function that filters, queries or transforms data (Derived Dataset).

## Inline Dataset

A temporary dataset that's created and used while job is running. Inline dataset definition can be used for small datasets.

```java
attr_layout := RECORD
    data_type    field1;
    ...
    ...
    ...
    data_type    field100;
END;

//Inline Dataset
attr_name := DATASET(
                        [
                            {'', '', 0, '', FALSE, ..., ''},
                            {'', '', 0, '', FALSE, ..., ''},
                            ...
                            ...
                            {'', '', 0, '', FALSE, ..., ''}
                        ],
                        attr_layout
                    );
```

- attr_name
  - Dataset name
- DATASET
  - ECL Keyword
- (...)
  - Contains all dataset information
- [...]
  - Contains all rows for dataset
- attr_layout
  - Record structure or format of each record in the dataset

![record set example](./Images/RecordLayout.JPG)

The layout for above dataset:

```java
//Record layout
salaryLayout := RECORD
    STRING job;
    STRING category;
    STRING city;
    STRING2 state;
    INTEGER avg_Salary;
    INTEGER lowerBand;
    INTEGER upperband;
END;

salaryDS := DATASET([
                {'Manager', 'IT', 'Atlanta', 'GA', 87000, 62000, 114000},
                {'Director', 'IT', 'Atlanta', 'GA', 119000, 84000, 156000},
                {'Director', 'Art-Entertainment', 'Atlanta', 'GA', 100000, 70000, 133000},
                {'CIO', 'IT', 'Tampa', 'FL', 112000, 69000, 131000},
                {'Sales', 'General', 'Chicago', 'IL', 55000, 32000, 121000}
                ], salaryAvg_Layout //Layout definition
                );
```

## File Dataset

The File Dataset is initialized by data from a logical file that is stored on the cluster

```java
attr_layout := RECORD
    data_type    field1;
    ...
    ...
    ...
    data_type    field100;
END;

path := '~some::sample::path';

//File Dataset
attr_name := DATASET(path,
                       attr_layout,
                       file_type);
```

- attr_name
  - Dataset name
- DATASET
  - ECL Keyword
- ( )
  - Contains all dataset information
- path
  - Logical address of file, where file is stored on the cluster.
  - ~ : Global search for the file
- attr_layout
  - Record structure or format of each record in the dataset
- file_type
  - Type of file (XML, CSV, logical, etc). Please see below for file types.

```java

salaryLayout := RECORD
    STRING job;
    STRING category;
    STRING city;
    STRING2 state;
    INTEGER avg_Salary;
    INTEGER lowerBand;
    INTEGER upperband;
END;

salaryDS := DATASET
              (
                '~salary_file.csv', //File Name
                 salaryLayout, //defined record structure/layout
                 CSV     //File Type
              );

```

### File Type

- **FLAT**: Native file type for Thor; also used for fixed-length raw records
- **CSV**: Any kind of delimited data, including CSV-encoded data
- **JSON**: Data stored as a series of JSON objects
- **XML**: Data stored as a series of XML documents
- **PIPE**: Data obtained dynamically via process calls

## Derived Dataset

This is a Dataset that is derived by performing a filter, query or transform on another dataset.

filteredSalaryDS := salaryDS(job='Manager');

## Resources

Put it into practice [dataset.ecl](https://ide.hpccsystems.com/workspaces/share/291d17d9-e5cb-4fac-83c2-ac5997c28a31)

Please visit [DATASET](https://hpccsystems.com/training/documentation/ecl-language-reference/html/DATASET.html) for more information.
