![logo](https://raw.githubusercontent.com/pediRAM/DirectoryFingerPrinting.CLI/main/Documentation/icon.png)

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)
[![Release](https://img.shields.io/github/release/pediRAM/DirectoryFingerPrinting.CLI.svg?sort=semver)](https://github.com/pediRAM/DirectoryFingerPrinting.CLI/releases)

**D**irectory **F**inger**P**rinter (abbreviation: **dfp**) is a CLI (`dfp.exe`), which creates and compares **directory fingerprints**: a *directory fingerprint* is the aggregate of all names, timestamps, sizes, and hash sums of files within a specified directory.

The main **purpose** and **intention** of **dfp** is to recognize any changes to any directory and/or compare two directories, containing mostly non-text files, without the need for any versioning management software like git or subversion, etc.

# Contents
- [Contents](#contents)
- [Introduction](#introduction)
- [How does the comparison look like?](#how-does-the-comparison-look-like)
- [Usage and Syntax:](#usage-and-syntax)
- [1. Parameters](#1-parameters)
  - [1.1 Self-Parameters](#11-self-parameters)
  - [1.2 Parameters Combination Synonyms:](#12-parameters-combination-synonyms)
  - [1.3 All Parameters:](#13-all-parameters)
- [2. Help Parameters:](#2-help-parameters)
- [3. Version Parameters:](#3-version-parameters)
- [4. Calculation:](#4-calculation)
  - [4.1 Filter parameters:](#41-filter-parameters)
  - [4.2 Hash/Checksum Options:](#42-hashchecksum-options)
  - [4.3 Report Level Options:](#43-report-level-options)
  - [4.4 Display Options:](#44-display-options)
  - [4.5 Save Options:](#45-save-options)
- [5. Compare:](#5-compare)
  - [5.1 Type of Comparison:](#51-type-of-comparison)
  - [5.2 Ignore Options:](#52-ignore-options)
- [6. Extensions List:](#6-extensions-list)
  - [6.1 Delimeters/Separators:](#61-delimetersseparators)
  - [6.2 Example:](#62-example)
- [7. Filenames:](#7-filenames)
  - [7.1 Filename formats of saved fingerprint files:](#71-filename-formats-of-saved-fingerprint-files)
  - [7.2 EXAMPLES:](#72-examples)
- [8. Save/Load Parameters:](#8-saveload-parameters)
  - [8.1 Saving Parameters (to file)](#81-saving-parameters-to-file)
  - [8.2 Loading Parameters (from file)](#82-loading-parameters-from-file)
- [9. Usage Examples](#9-usage-examples)
  - [9.1 Calculation](#91-calculation)
    - [Show FP of only toplevel files in current directory:](#show-fp-of-only-toplevel-files-in-current-directory)
    - [Process only assemblies (will ignore anything else than \*.dll, \*.exe):](#process-only-assemblies-will-ignore-anything-else-than-dll-exe)
    - [Process only \*.json, \*.txt, \*.xml and \*.yaml files:](#process-only-json-txt-xml-and-yaml-files)
    - [Ignore \*.log and \*.ini and hidden files:](#ignore-log-and-ini-and-hidden-files)
    - [Show use SHA256 algorithm, don't show header:](#show-use-sha256-algorithm-dont-show-header)
    - [Show FP for all files (recursive) in C:\\MyDir:](#show-fp-for-all-files-recursive-in-cmydir)
    - [Save FPF as \*.dfp into 'C:\\MyDFP Files\\Test':](#save-fpf-as-dfp-into-cmydfp-filestest)
    - [Save FPF as \*.csv into 'C:\\MyDFP Files\\Test':](#save-fpf-as-csv-into-cmydfp-filestest)
  - [9.2 Compare:](#92-compare)
    - [Compare directories "C:\\MyDir1" to "C:\\MyDir2" and print result in color:](#compare-directories-cmydir1-to-cmydir2-and-print-result-in-color)
    - [Compare two FPFs with different formats and print verbose result:](#compare-two-fpfs-with-different-formats-and-print-verbose-result)
    - [Compare FPF 'temp\\fingerprint.dfp' with directory C:\\MyDir but ignore file timestamps:](#compare-fpf-tempfingerprintdfp-with-directory-cmydir-but-ignore-file-timestamps)
- [9. Error Codes](#9-error-codes)
- [10. Symbols and Colors](#10-symbols-and-colors)

# Introduction

**What is a "directory fingerprint"?**\
A **directory fingerprint** is the aggregate of all names, timestamps, sizes, and hash sums of files within a specified directory.

**What can I do with the directory-fingerprinter `dfp.exe`?**
- View a detailed list of files in a directory (including versions of assemblies and hash sums).
- Save the fingerprint of a directory for comparing and detecting changes in the future.
- Compare two folders.
- Compare two directory fingerprints.
- Compare a fingerprint (file) against a directory or vice versa.

**How can I use `dfp.exe`?**\
By calling it in the prompt (cmd/console) as shown below:

```cmd
C:\> dfp --versions -d c:\MyBinaries
-------------------------------------------------
 Name                            | Version
-------------------------------------------------
 CheckNet.exe                    | 1.0.1.0
 dfp.dll                         | 1.0.2
 dfp.exe                         | 1.0.2
 DirectoryFingerPrinting.dll     | 1.0.0.0
 log4net.dll                     | 2.0.8.0
 Newtonsoft.Json.dll             | 12.0.3.23909
 nuget.exe                       | 5.7.0.6726
 

 C:\> dfp --versions -d c:\MyBinaries
 ---------------------------------------------------------------------------
 Name                            | Hashsum (SHA1)
---------------------------------------------------------------------------
 CheckNet.exe                    | eba4ceb1cd17caed003ea8b34cec36de5d7ebd63
 dfp.dll                         | 01f2a6785ee2015d252e79c17bdca5b312423c38
 dfp.exe                         | 7636abe0730ce3af6b0d12080636e9bf3c6cb54d
 DirectoryFingerPrinting.dll     | e55e52aadc1e62b5e6b385b657395235f7dfd5a7
 log4net.dll                     | 92683e4fb8d3c7a544dce21e12f24dcc8b600e9c
 Newtonsoft.Json.dll             | 1248142eb45eed3beb0d9a2d3b8bed5fe2569b10
 nuget.exe                       | 6d2302dc9c562078b1c089372c078d5cd589a59a


C:\> dfp -ao -icd -ila -d C:\SomeDirectory
--------------------------------------------------------------------------------------------------------------------------
 Name                            | Modified at         | Size    | Version      | Hashsum (SHA1)
--------------------------------------------------------------------------------------------------------------------------
 CheckNet.exe                    | 2021-11-20 15:24.30 | 239616  | 1.0.1.0      | eba4ceb1cd17caed003ea8b34cec36de5d7ebd63
 dfp.dll                         | 2023-05-20 06:07.58 | 53760   | 1.0.2        | 01f2a6785ee2015d252e79c17bdca5b312423c38
 dfp.exe                         | 2023-05-20 06:08.00 | 115200  | 1.0.2        | 7636abe0730ce3af6b0d12080636e9bf3c6cb54d
 DirectoryFingerPrinting.dll     | 2023-05-20 05:40.12 | 27648   | 1.0.0.0      | e55e52aadc1e62b5e6b385b657395235f7dfd5a7
 log4net.dll                     | 2017-03-08 18:26.22 | 276480  | 2.0.8.0      | 92683e4fb8d3c7a544dce21e12f24dcc8b600e9c
 Newtonsoft.Json.dll             | 2019-11-08 23:56.44 | 700336  | 12.0.3.23909 | 1248142eb45eed3beb0d9a2d3b8bed5fe2569b10
 nuget.exe                       | 2021-02-23 19:16.54 | 6661528 | 5.7.0.6726   | 6d2302dc9c562078b1c089372c078d5cd589a59a


C:\> dfp -d C:\SomeDirectory
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
 Name                            | Created at          | Modified at         | Last Access at      | Size     | Version      | Hashsum (SHA1)
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------
 CheckNet.exe                    | 2023-09-11 15:41.00 | 2021-11-20 15:24.30 | 2024-04-01 13:48.32 | 239616   | 1.0.1.0      | eba4ceb1cd17caed003ea8b34cec36de5d7ebd63
 delreg.cmd                      | 2023-09-11 15:41.00 | 2020-12-01 21:34.50 | 2024-04-01 13:48.32 | 14478    |              | c1de89c2b434452d849c7ca4a320e3848133b330
 dfp.deps.json                   | 2023-05-20 06:08.00 | 2023-05-20 06:08.00 | 2024-04-01 13:48.32 | 3246     |              | 67506dcb6863bd96b14b9ec3099db9e47538bb09
 dfp.dll                         | 2023-05-20 06:07.58 | 2023-05-20 06:07.58 | 2024-04-01 13:48.32 | 53760    | 1.0.2        | 01f2a6785ee2015d252e79c17bdca5b312423c38
 dfp.exe                         | 2023-05-20 06:08.00 | 2023-05-20 06:08.00 | 2024-04-01 13:48.32 | 115200   | 1.0.2        | 7636abe0730ce3af6b0d12080636e9bf3c6cb54d
 dfp.runtimeconfig.json          | 2023-05-20 06:08.00 | 2023-05-20 06:08.00 | 2024-04-01 13:48.32 | 253      |              | 9160a009cb381e044ba4c63e4435da6bfeb9dc6d
 DirectoryFingerPrinting.dll     | 2023-05-20 05:40.12 | 2023-05-20 05:40.12 | 2024-04-01 13:48.32 | 27648    | 1.0.0.0      | e55e52aadc1e62b5e6b385b657395235f7dfd5a7
 google.cmd                      | 2023-09-11 15:41.00 | 2021-01-06 18:44.42 | 2024-04-01 13:48.32 | 1446     |              | 72b50e74d7b3182ab0d55a6225db60185986ff39
 License.txt                     | 2023-05-13 17:30.10 | 2023-05-13 17:30.10 | 2024-04-01 13:48.32 | 35149    |              | 31a3d460bb3c7d98845187c716a30db81c44b615
 log4net.dll                     | 2023-09-11 15:41.00 | 2017-03-08 18:26.22 | 2024-04-01 13:48.32 | 276480   | 2.0.8.0      | 92683e4fb8d3c7a544dce21e12f24dcc8b600e9c
 Manual.pdf                      | 2023-09-11 15:41.00 | 2020-03-03 14:00.29 | 2024-04-01 13:48.32 | 75018    |              | 0626f4842d46baa750b5bad7bd2acdcf13607534
 Newtonsoft.Json.dll             | 2023-09-11 15:41.00 | 2019-11-08 23:56.44 | 2024-04-01 13:48.32 | 700336   | 12.0.3.23909 | 1248142eb45eed3beb0d9a2d3b8bed5fe2569b10
 nuget.exe                       | 2023-09-11 15:41.00 | 2021-02-23 19:16.54 | 2024-04-01 13:48.32 | 6661528  | 5.7.0.6726   | 6d2302dc9c562078b1c089372c078d5cd589a59a
 ```

 # How does the comparison look like?
 There are three modes to list the differencies:
 - Essential (default)
 - Informative
 - Verbose
Section 4.3 contains more details. Here is an output example for using the "--report-informative" or short "-ri" parameter:

 ```
- File2.txt (File removed)
- Sub_Dir\File4.txt (File removed)
~ Sub_Dir\File6.txt (Hashsums differs)
+ File3.txt (File added)
+ Sub_Dir\File5.txt (File added)
```

# Usage and Syntax:
Call ***dfp.exe*** in console as follow:

**dfp** ( (**HELP** | **VERSION**) | (**CACLULATION** | **COMPARE** ) [**PARAMETERS**]+ )

# 1. Parameters

## 1.1 Self-Parameters
These parameters output the help text or license/version information about **dfp.exe**:

| Parameter                      | Examples (short)                                                      | Description                                          |
|--------------------------------|-----------------------------------------------------------------------|------------------------------------------------------|
| --help                         | dfp -h                                                                | Displays help text                                   |
| --version                      | dfp -v                                                                | Displays version information about **dfp.exe**       |

## 1.2 Parameters Combination Synonyms:
These parameters are synonyms for multiple parameters, so in most cases you type less:

| Parameter                      | Examples (short)                                                      | Description                                          |
|--------------------------------|-----------------------------------------------------------------------|------------------------------------------------------|
| --checksums                    | dfp --checksums                                                       | Displays only checksums of files                     |
| --sizes                        | dfp --sizes                                                           | Displays only sizes of files                         |
| --versions                     | dfp --versions                                                        | Displays available versions of assemblies (\*.dll, \*.exe)|

## 1.3 All Parameters:

| Parameter                      | Examples (short)                                                      | Description                                          |
|--------------------------------|-----------------------------------------------------------------------|------------------------------------------------------|
| --assemblies-only              | dfp -ao --directory .\                                                | Processes only *.dll and *.exe files                |
| --directory                    | dfp --directory .\                                                    | Specifies the base directory for calculations       |
| --help                         | dfp -h                                                                | Displays help text                                    |
| --ignore-timestamps            | dfp -its -d .\                                                        | Ignores all timestamps of files                     |
| --ignore-size                  | dfp -is -d .\                                                         | Ignores file sizes (in bytes)                        |
| --ignore-creation-date         | dfp -icd -d .\                                                        | Ignores created-at-timestamp                         |
| --ignore-last-modification     | dfp -ilm -d .\                                                        | Ignores last-modification-at-timestamp               |
| --ignore-last-access           | dfp -ila -d .\                                                        | Ignores last-access-timestamp                        |
| --ignore-version               | dfp -iv -d .\                                                         | Ignores versions (only *.dll and *.exe files)       |
| --ignore-checksum              | dfp -ics -d .\                                                        | Ignores hashsums                                     |
| --ignore-hidden-files          | dfp -ihf -d .\                                                        | Ignores hidden files                                 |
| --ignore-access-errors         | dfp -iae -d .\                                                        | Ignores access violation errors                      |
| --ignore-case                  | dfp -ic -d .\                                                         | Compares filenames case insensitive                  |
| --recursive                    | dfp -r -d .\                                                          | Searches recursive                                   |
| --positive-list                | dfp -p -d .\ -x "json,txt,xml,yaml"                                   | Specifies positive list of extensions                |
| --negative-list                | dfp -n -d .\ -x "log,md" -ihf                                         | Specifies negative list of extensions                |
| --extensions                   | dfp -x "json,txt,xml,yaml" -d .\                                      | Specifies list of extensions                         |
| --use-crc32                    | dfp -crc32 -d .\                                                      | Uses CRC32 algorithm                                 |
| --use-md5                      | dfp -md5 -d .\                                                        | Uses MD5 algorithm                                   |
| --use-sha1                     | dfp -d .\ -sha1                                                       | Uses SHA1 algorithm (DEFAULT)                        |
| --use-sha256                   | dfp -d .\ -sha256                                                     | Uses SHA256 algorithm                                |
| --use-sha512                   | dfp -d .\ -sha512                                                     | Uses SHA512 algorithm                                |
| --report-essential             | dfp -re -d .\                                                         | Prints only +/-/~ and filename                      |
| --report-informative           | dfp -ri -d .\                                                         | Prints essential with matter of change (human friendly) |
| --report-verbose               | dfp -rv -d .\                                                         | Prints everything                                    |
| --print-colored                | dfp -pc -d .\                                                         | Prints result in colors (red = removed, blue = added, yellow = changed) |
| --no-header                    | dfp -nh -d .\                                                         | Suppresses header printing                           |
| --no-format                    | dfp -nf -d .\                                                         | Prints unformatted DFP                               |
| --print-sorted-ascendent       | dfp -asc -d .\                                                        | Prints fingerprints sorted by filepaths/filenames ascendant |
| --print-sorted-descendent      | dfp -desc -d .\                                                       | Prints fingerprints sorted by filepaths/filenames descendant |
| --print-only-filename          | dfp -pof -d .\                                                        | Prints filenames instead of relative paths           |
| --save                         | dfp -s "C:\MyDFP Files\Test" -d "C:\MyDir" --recursive                | Saves calculated fingerprint to file                 |
| --format-dfp                   | dfp -dfp "C:\MyDFP Files\Test" -d "C:\MyDir" -r                       | Saves fingerprint in *.dfp format                   |
| --format-xml                   | dfp -xml "C:\MyDFP Files\Test" -d "C:\MyDir" -r                       | Saves fingerprint in *.xml format                   |
| --format-json                  | dfp -json "C:\MyDFP Files\Test" -d "C:\MyDir" -r                      | Saves fingerprint in *.json format                  |
| --format-csv                   | dfp -csv "C:\MyDFP Files\Test" -d "C:\MyDir" -r                       | Saves fingerprint in *.csv format                   |
| --compare-directories          | dfp -cd "C:\MyDir1" "C:\MyDir2"                                       | Compares two directories                             |
| --compare-fingerprints         | dfp -cf "temp\fingerprint1.dfp" "temp\fingerprint2.json"              | Compares two fingerprints                           |
| --compare                      | dfp -c "temp\fingerprint.dfp" "C:\MyDir"                              | Compares a fingerprint against a directory          |
| --load-options                 | dfp -lo "options.txt" -d .\                                           | Loads options from a file                            |
| --save-options                 | dfp -so "options.txt" -d .\                                           | Saves options to a file                              |
| --checksums                    | dfp --checksums                                                       | Displays checksums                                   |
| --sizes                        | dfp --sizes                                                           | Displays sizes                                       |
| --versions                     | dfp --versions                                                        | Displays available versions                          |
| --version                      | dfp -v                                                                | Displays version information about **dfp.exe**       |


# 2. Help Parameters:
|PARAMETER: |SHORT:    | DESCRIPTION: |
|-----------|----------|--------------|
|--help     | -h or /? | Shows this help text.|

# 3. Version Parameters:
|PARAMETER: |SHORT:    | DESCRIPTION: |
|-----------|----------|--------------|
|--version  | -v       | Shows version and copyright information.|
|--versions |          | Shows only versions of assembly files.|

# 4. Calculation:
## 4.1 Filter parameters:
|PARAMETER:                 |SHORT:| DESCRIPTION: |
|---------------------------|------|--------------|
|--directory                | -d   | Base directory for calculating fingerprints.|
|--recursive                | -r   | Search recursive.|
|--assemblies-only          | -ao  | Processes only *.dll and *.exe files (anything else will be ignored).|
|--ignore-hidden-files      | -ihf | Ignore hidden files.|
|--ignore-access-errors     | -iae | Ignore access violation errors.|
|--extensions               | -x   | List of extensions (read ***EXTENSION_LIST*** in section ***6***!).|
|--positive-list            | -p   | ***EXTENSION_LIST*** is ***positive*** list (files will be ***included***).|
|--negative-list            | -n   | ***EXTENSION_LIST*** is ***negative*** list (files will be ***excluded***).|

## 4.2 Hash/Checksum Options:
|PARAMETER:                |SHORT:   | DESCRIPTION:|
|---------------------------|---------|-------------|
|--use-crc32                | -crc32  | CRC32.|
|--use-md5                  | -md5    | MD5.|
|--use-sha1                 | -sha1   | SHA1 ***(DEFAULT)***.|
|--use-sha256               | -sha256 | SHA256.|
|--use-sha512               | -sha512 | SHA512.|

## 4.3 Report Level Options:
|PARAMETER:                 |SHORT:| DESCRIPTION:|
|---------------------------|------|-------------|
|--report-essential         | -re  | Prints only +/-/~ and filename ***(DEFAULT)***.|
|--report-informative       | -ri  | Prints essential with matter of change (human friendly).|
|--report-verbose           | -rv  | Prints everything.|

## 4.4 Display Options:
|PARAMETER:                 |SHORT: |DESCRIPTION:|
|---------------------------|-------|------------|
|--print-colored            | -pc   | Prints result in colors (red = removed, blue = added, yellow = changed).|
|--no-header                | -nh   | No header will be printed.|
|--no-format                | -nf   | Prints unformatted DFP.|
|--print-sorted-ascendent   | -asc  | Prints fingerprints sorted by filepaths/filenames ascendent.|
|--print-sorted-descendent  | -desc | Prints fingerprints sorted by filepaths/filenames descendent.|
|--print-only-filename      | -pof  | Prints filenames instead of relative paths.|

## 4.5 Save Options:
|PARAMETER:                 |SHORT: |DESCRIPTION:|
|---------------------------|-------|------------|
|--save                     | -s    | Saves calculated fingerprint to file (read section ***7.1***!).|
|--format-dfp               | -dfp  | *.dfp ***(DEFAULT)***|
|--format-xml               | -xml  | *.xml|
|--format-json              | -json | *.json|
|--format-csv               | -csv  | *.csv (separated with ';').|

# 5. Compare:
## 5.1 Type of Comparison:
|PARAMETER:                 |SHORT:|DESCRIPTION:|
|---------------------------|------|------------|
|--compare-directories      | -cd  | Compares two directories.|
|--compare-fingerprints     | -cf  | Compares two FPFs.|
|--compare                  | -c   | Compares a FPF against a directory.|

## 5.2 Ignore Options:
|PARAMETER:                 |SHORT:|DESCRIPTION:|
|---------------------------|------|-------------|
|--ignore-case              | -ic  | Compares filenames case insensitive.|
|--ignore-timestamps        | -its | Ignores all timestamps of files.|
|--ignore-creation-date     | -icd | Ignores created-at-timestamp.|
|--ignore-last-modification | -ilm | Ignores last-modification-at-timestamp.|
|--ignore-last-access       | -ila | Ignores last-access-timestamp.|
|--ignore-size              | -is  | Ignores filesizes (in bytes).|
|--ignore-version           | -iv  | Ignores versions (only *.dll and *.exe files!).|
|--ignore-hashsum           | -ihs | Ignores hashsums.|

# 6. Extensions List:
Single- or double-quoted list of separated file-extensions, without asterisk ('*') and/or dot ('**.**'), like:\
- **'dll exe xml'** or **'dll,exe,xml'** or **'dll;exe;xml'**\
or
- **"dll exe xml"** or **"dll,exe,xml"** or **"dll;exe;xml"**

## 6.1 Delimeters/Separators:
You can separate the extensions with following delimeters:
- Semicolorn ('**;**')
- Comma ('**,**')
- Space (' ')

## 6.2 Example:
"config,dll,exe" or "config;dll;exe" or "config dll exe".

# 7. Filenames:
## 7.1 Filename formats of saved fingerprint files:
***yyyy-MM-dd_HH.mm.ss.*** (***csv*** | ***dfp*** | ***json*** | ***xml***)
## 7.2 EXAMPLES:
```
2023-08-15_00.00.00.csv
2023-09-11_08.46.11.dfp
2023-12-31_23.59.59.xml
```

# 8. Save/Load Parameters:
Since **dfp** provides many options, you may want to save them once and use them later just by loading them, instead of typing them again and again.

|PARAMETER:                 |SHORT:                       | DESCRIPTION: |
|---------------------------|-----------------------------|--------------|
| --load-options            | dfp -lo "options.txt" -d .\ | Loads options from a file|
| --save-options            | dfp -so "options.txt" -d .\ | Saves options to a file|

## 8.1 Saving Parameters (to file)

For saving currently used parameters, just add the parameter **--save-options** or short **-so** followed by a filepath when you call **dfp**, like this:

```cmd
C:\> dfp --recursive --directory "C:\Directory\SubDir\Release" --assemblies-only --ignore-timestamps --ignore-size --ignore-hashsum --print-colored --save-options "C:\dfp-opts-for-Release.json"
```
or by using short parameter-names:

```cmd
C:\> dfp -r -d "C:\Directory\SubDir\Release" -ao -its -is -ihs -pc -so "C:\dfp-opts-for-Release.json"
```

## 8.2 Loading Parameters (from file)
After you have saved the paramters (as described in 8.1), you only need to use 2 parameters to load all other parameters:

```cmd
C:\> dfp --load-options "C:\dfp-opts-for-Release.json"
```
or by using short parameter-name:

```cmd
C:\> dfp -lo "C:\dfp-opts-for-Release.json"
```

# 9. Usage Examples
## 9.1 Calculation
### Show FP of only toplevel files in current directory:
```cmd
dfp --directory .\
dfp -d .\
```

### Process only assemblies (will ignore anything else than *.dll, *.exe):
```cmd
dfp --directory .\ --assembly-only
dfp -d .\ -ao
```

### Process only *.json, *.txt, *.xml and *.yaml files:
```cmd
dfp --directory .\ --positive-list --extensions "json,txt,xml,yaml"
dfp -d .\ -p -x "json,txt,xml,yaml"
```

### Ignore *.log and *.ini and hidden files:
```cmd
dfp --directory .\ --negative-list -extensions "log,md" --ignore-hidded-files
dfp -d .\ -n -x "log,md" -ihf
```

### Show use SHA256 algorithm, don't show header:
```cmd
dfp --directory .\ --use-sha256 --no-header
dfp -d .\ -sha256 -nh
```

### Show FP for all files (recursive) in C:\MyDir:
```cmd
dfp --directory "C:\MyDir" --recursive
dfp -d "C:\MyDir" -r
```

### Save FPF as *.dfp into 'C:\MyDFP Files\Test':
```cmd
dfp --directory "C:\MyDir" --recursive --save "C:\MyDFP Files\Test"
dfp -d "C:\MyDir" -r -s "C:\MyDFP Files\Test"
```

### Save FPF as *.csv into 'C:\MyDFP Files\Test':
```cmd
dfp --directory "C:\MyDir" --recursive --save "C:\MyDFP Files\Test" --format-csv
dfp -d "C:\MyDir" -r -s "C:\MyDFP Files\Test" -csv
```

## 9.2 Compare:
### Compare directories "C:\MyDir1" to "C:\MyDir2" and print result in color:
```cmd
dfp --compare-directories "C:\MyDir1" "C:\MyDir2" --print-colored
dfp -cd "C:\MyDir1" "C:\MyDir2" -pc
```

### Compare two FPFs with different formats and print verbose result:
```cmd
dfp --compare-fingerprints "temp\fingerprint1.dfp" "temp\fingerprint2.json" --report-verbose
dfp -cf "temp\fingerprint1.dfp" "temp\fingerprint2.json" -rv
```

### Compare FPF 'temp\fingerprint.dfp' with directory C:\MyDir but ignore file timestamps:
```cmd
dfp --compare "temp\fingerprint.dfp" "C:\MyDir" --ignore-timestamps
dfp -c "temp\fingerprint.dfp" "C:\MyDir" -its
```

# 9. Error Codes
Following error codes are returned after cli executable has terminated.
You can output them in prompt/cmd by:

```cmd
echo %errorlevel%
```

| Error Code | Description                                                    |
|------------|----------------------------------------------------------------|
| 0          | OK (no error).                                                 |
| 1          | No parameters.                                                 |
| 2          | Missing parameter.                                             |
| 3          | Unknown parameter.                                             |
| 4          | Internal error.                                                |
| 5          | Illegal value.                                                 |
| 6          | Single parameter.                                              |
| 7          | File already exists.                                           |
| 8          | Writing fingerprint file failed.                               |
| 9          | File not found.                                                |
| 10         | Directory not found.                                           |
| 11         | Calculate, save, and compare at once are not provided.         |
| 12         | Illegal/Unknown fingerprint file extension.                    |
| 13         | Unequal hashsum algorithms.                                    |

# 10. Symbols and Colors

| Symbol | Color                      | Meaning   | Description      |
|--------|----------------------------|-----------|------------------|
| +      | $${\color{blue}Blue}$$     | New/Added | New file found   |
| -      | $${\color{red}Red}$$       | Removed   | File is missing  |
| ~      | $${\color{yellow}Yellow}$$ | Changed   | File has changes |

