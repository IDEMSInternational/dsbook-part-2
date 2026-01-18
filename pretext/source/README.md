# PreText Textbook Structure

This directory contains the PreText source files for "Introduction to Data Science".

The main `project.ptx` configuration file is located in the repository root.

## Structure

- `../project.ptx` - Main project configuration file (in repository root)
- `publication/publication.ptx` - Publication settings
- `source/` - Source files for the textbook
  - `main.ptx` - Main book structure file
  - `frontmatter.ptx` - Front Matter (Preface and Acknowledgments)
  - `summary-statistics.ptx` - Summary Statistics chapter
  - `probability.ptx` - Probability chapter
  - `statistical-inference.ptx` - Statistical Inference chapter
  - `linear-models.ptx` - Linear Models chapter
  - `high-dimensional-data.ptx` - High Dimensional Data chapter
  - `machine-learning.ptx` - Machine Learning chapter

## Book Structure

**Title:** Introduction to Data Science

**Chapters:**
1. Front Matter
2. Summary Statistics
3. Probability
4. Statistical Inference
5. Linear Models
6. High Dimensional Data
7. Machine Learning

## Building the Book

To build this PreText book, you'll need to have the PreText CLI installed. 

### From the repository root:

```bash
pretext build html
```

The `project.ptx` file in the repository root will be automatically detected by PreText CLI.

For more information about PreText, visit: https://pretextbook.org/

## Notes

This is a skeleton structure. Content needs to be filled in for each chapter and section.
