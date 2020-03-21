---
pinned: true
tags: [Notebooks/Py4DA_Wes]
title: Table of Contents
created: '2020-03-11T17:37:33.378Z'
modified: '2020-03-12T00:31:59.774Z'
---

# Table of Contents

## Preface
- New for the Second Edition
- Conventions Used in This Book
- Using Code Examples
- O’Reilly Safari
- How to Contact Us
- Acknowledgments

## 1. Preliminaries
### 1.1 What Is This Book About?
- What Kinds of Data?
### 1.2 Why Python for Data Analysis?
- Python as Glue
- Solving the “Two-Language” Problem
- Why Not Python?
### 1.3 Essential Python Libraries
- NumPy
- pandas
- matplotlib
- IPython and Jupyter
- SciPy
- scikit-learn
- statsmodels
### 1.4 Installation and Setup
- Windows
- Apple (OS X, macOS)
- GNU/Linux
- Installing or Updating Python Packages
- Python 2 and Python 3
- Integrated Development Environments (IDEs) and Text Editors
### 1.5 Community and Conferences
### 1.6 Navigating This Book
- Code Examples
- Data for Examples
- Import Conventions
- Jargon

## 2. Python Language Basics, IPython, and Jupyter Notebooks
### 2.1 The Python Interpreter
### 2.2 IPython Basics
- Running the IPython Shell
- Running the Jupyter Notebook
- Tab Completion
- Introspection
- The %run Command
- Executing Code from the Clipboard
- Terminal Keyboard Shortcuts
- About Magic Commands
- Matplotlib Integration
### 2.3 Python Language Basics
- Language Semantics
- Scalar Types
- Control Flow

## 3. Built-in Data Structures, Functions, and Files
### 3.1 Data Structures and Sequences
- Tuple
- List
- Built-in Sequence Functions
- dict
- set
- List, Set, and Dict Comprehensions
### 3.2 Functions
- Namespaces, Scope, and Local Functions
- Returning Multiple Values
- Functions Are Objects
- Anonymous (Lambda) Functions
- Currying: Partial Argument Application
- Generators
- Errors and Exception Handling
### 3.3 Files and the Operating System
- Bytes and Unicode with Files
### 3.4 Conclusion

## 4. NumPy Basics: Arrays and Vectorized Computation
### 4.1 The NumPy ndarray: A Multidimensional Array Object
- Creating ndarrays
- Data Types for ndarrays
- Arithmetic with NumPy Arrays
- Basic Indexing and Slicing
- Boolean Indexing
- Fancy Indexing
- Transposing Arrays and Swapping Axes
### 4.2 Universal Functions: Fast Element-Wise Array Functions
### 4.3 Array-Oriented Programming with Arrays
- Expressing Conditional Logic as Array Operations
- Mathematical and Statistical Methods
- Methods for Boolean Arrays
- Sorting
- Unique and Other Set Logic
### 4.4 File Input and Output with Arrays
### 4.5 Linear Algebra
### 4.6 Pseudorandom Number Generation
### 4.7 Example: Random Walks
- Simulating Many Random Walks at Once
### 4.8 Conclusion

## 5. Getting Started with pandas
### 5.1 Introduction to pandas Data Structures
- Series
- DataFrame
- Index Objects
### 5.2 Essential Functionality
- Reindexing
- Dropping Entries from an Axis
- Indexing, Selection, and Filtering
- Integer Indexes
- Arithmetic and Data Alignment
- Function Application and Mapping
- Sorting and Ranking
- Axis Indexes with Duplicate Labels
### 5.3 Summarizing and Computing Descriptive Statistics
- Correlation and Covariance
- Unique Values, Value Counts, and Membership
### 5.4 Conclusion

## 6. Data Loading, Storage, and File Formats
### 6.1 Reading and Writing Data in Text Format
- Reading Text Files in Pieces
- Writing Data to Text Format
- Working with Delimited Formats
- JSON Data
- XML and HTML: Web Scraping
### 6.2 Binary Data Formats
- Using HDF5 Format
- Reading Microsoft Excel Files
### 6.3 Interacting with Web APIs
### 6.4 Interacting with Databases
### 6.5 Conclusion

## 7. Data Cleaning and Preparation
### 7.1 Handling Missing Data
- Filtering Out Missing Data
- Filling In Missing Data
### 7.2 Data Transformation
- Removing Duplicates
- Transforming Data Using a Function or Mapping
- Replacing Values
- Renaming Axis Indexes
- Discretization and Binning
- Detecting and Filtering Outliers
- Permutation and Random Sampling
- Computing Indicator/Dummy Variables
### 7.3 String Manipulation
- String Object Methods
- Regular Expressions
- Vectorized String Functions in pandas
### 7.4 Conclusion

## 8. Data Wrangling: Join, Combine, and Reshape
### 8.1 Hierarchical Indexing
- Reordering and Sorting Levels
- Summary Statistics by Level
- Indexing with a DataFrame’s columns
### 8.2 Combining and Merging Datasets
- Database-Style DataFrame Joins
- Merging on Index
- Concatenating Along an Axis
- Combining Data with Overlap
### 8.3 Reshaping and Pivoting
- Reshaping with Hierarchical Indexing
- Pivoting “Long” to “Wide” Format
- Pivoting “Wide” to “Long” Format
### 8.4 Conclusion

## 9. Plotting and Visualization
### 9.1 A Brief matplotlib API Primer
- Figures and Subplots
- Colors, Markers, and Line Styles
- Ticks, Labels, and Legends
- Annotations and Drawing on a Subplot
- Saving Plots to File
- matplotlib Configuration
### 9.2 Plotting with pandas and seaborn
- Line Plots
- Bar Plots
- Histograms and Density Plots
- Scatter or Point Plots
- Facet Grids and Categorical Data
### 9.3 Other Python Visualization Tools
### 9.4 Conclusion

## 10. Data Aggregation and Group Operations
### 10.1 GroupBy Mechanics
- Iterating Over Groups
- Selecting a Column or Subset of Columns
- Grouping with Dicts and Series
- Grouping with Functions
- Grouping by Index Levels
### 10.2 Data Aggregation
- Column-Wise and Multiple Function Application
- Returning Aggregated Data Without Row Indexes
### 10.3 Apply: General split-apply-combine
- Suppressing the Group Keys
- Quantile and Bucket Analysis
- Example: Filling Missing Values with Group-Specific Values
- Example: Random Sampling and Permutation
- Example: Group Weighted Average and Correlation
- Example: Group-Wise Linear Regression
### 10.4 Pivot Tables and Cross-Tabulation
- Cross-Tabulations: Crosstab
### 10.5 Conclusion

## 11. Time Series
### 11.1 Date and Time Data Types and Tools
- Converting Between String and Datetime
### 11.2 Time Series Basics
- Indexing, Selection, Subsetting
- Time Series with Duplicate Indices
### 11.3 Date Ranges, Frequencies, and Shifting
- Generating Date Ranges
- Frequencies and Date Offsets
- Shifting (Leading and Lagging) Data
### 11.4 Time Zone Handling
- Time Zone Localization and Conversion
- Operations with Time Zone−Aware Timestamp Objects
- Operations Between Different Time Zones
### 11.5 Periods and Period Arithmetic
- Period Frequency Conversion
- Quarterly Period Frequencies
- Converting Timestamps to Periods (and Back)
- Creating a PeriodIndex from Arrays
### 11.6 Resampling and Frequency Conversion
- Downsampling
- Upsampling and Interpolation
- Resampling with Periods
### 11.7 Moving Window Functions
- Exponentially Weighted Functions
- Binary Moving Window Functions
- User-Defined Moving Window Functions
### 11.8 Conclusion

## 12. Advanced pandas
### 12.1 Categorical Data
- Background and Motivation
- Categorical Type in pandas
- Computations with Categoricals
- Categorical Methods
### 12.2 Advanced GroupBy Use
- Group Transforms and “Unwrapped” GroupBys
- Grouped Time Resampling
### 12.3 Techniques for Method Chaining
- The pipe Method
### 12.4 Conclusion

## 13. Introduction to Modeling Libraries in Python
### 13.1 Interfacing Between pandas and Model Code
### 13.2 Creating Model Descriptions with Patsy
- Data Transformations in Patsy Formulas
- Categorical Data and Patsy
### 13.3 Introduction to statsmodels
- Estimating Linear Models
- Estimating Time Series Processes
### 13.4 Introduction to scikit-learn
### 13.5 Continuing Your Education

## 14. Data Analysis Examples
### 14.1 USA.gov Data from Bitly
- Counting Time Zones in Pure Python
- Counting Time Zones with pandas
### 14.2 MovieLens 1M Dataset
- Measuring Rating Disagreement
### 14.3 US Baby Names 1880–2010
- Analyzing Naming Trends
### 14.4 USDA Food Database
### 14.5 2012 Federal Election Commission Database
- Donation Statistics by Occupation and Employer
- Bucketing Donation Amounts
- Donation Statistics by State
### 14.6 Conclusion

## Appendix A. Advanced NumPy
### A.1 ndarray Object Internals
- NumPy dtype Hierarchy
### A.2 Advanced Array Manipulation
- Reshaping Arrays
- C Versus Fortran Order
- Concatenating and Splitting Arrays
- Repeating Elements: tile and repeat
- Fancy Indexing Equivalents: take and put
### A.3 Broadcasting
- Broadcasting Over Other Axes
- Setting Array Values by Broadcasting
### A.4 Advanced ufunc Usage
- ufunc Instance Methods
- Writing New ufuncs in Python
### A.5 Structured and Record Arrays
- Nested dtypes and Multidimensional Fields
- Why Use Structured Arrays?
### A.6 More About Sorting
- Indirect Sorts: argsort and lexsort
- Alternative Sort Algorithms
- Partially Sorting Arrays
- numpy.searchsorted: Finding Elements in a Sorted Array
### A.7 Writing Fast NumPy Functions with Numba
- Creating Custom numpy.ufunc Objects with Numba
### A.8 Advanced Array Input and Output
- Memory-Mapped Files
- HDF5 and Other Array Storage Options
### A.9 Performance Tips
- The Importance of Contiguous Memory

## Appendix B. More on the IPython System
### B.1 Using the Command History
- Searching and Reusing the Command History
- Input and Output Variables
### B.2 Interacting with the Operating System
- Shell Commands and Aliases
- Directory Bookmark System
### B.3 Software Development Tools
- Interactive Debugger
- Timing Code: %time and %timeit
- Basic Profiling: %prun and %run -p
- Profiling a Function Line by Line
### B.4 Tips for Productive Code Development Using IPython
- Reloading Module Dependencies
- Code Design Tips
### B.5 Advanced IPython Features
- Making Your Own Classes IPython-Friendly
- Profiles and Configuration
### B.6 Conclusion


