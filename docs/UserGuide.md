# User Guide

CuddleCare is a pet management app that helps keep track of your pets' treatment schedules via a Command Line
Interface (CLI). You can add, view, and manage treatments efficiently through simple commands, ensuring that no pet’s
care routine is overlooked.

- [Quick Start](#quick-start)
- [Features](#features)
    - [Add Pet — `add-pet`](#add-pet--add-pet)
    - [List Pets — `list-pets`](#list-pets--list-pets)
    - [Edit Pet — `edit-pet`](#edit-pet--edit-pet)
    - [Delete Pet — `delete-pet`](#delete-pet--delete-pet)
    - [Add Treatment — `add-treatment`](#add-treatment--add-treatment)
    - [Delete Treament — `delete-treatment`](#delete-treatment--delete-treatment)
    - [Mark a Treatment as Done — `mark`](#mark-a-treatment-as-done--mark)
    - [Unmark a Treatment — `unmark`](#unmark-a-treatment--unmark)
    - [Group Treatments by Type — `group-treatments`](#group-treatments-by-type--group-treatments)
    - [Filter Treatments by Date — `treatment-date`](#filter-treatments-by-date--treatment-date)
    - [Find Treatments — `find`](#find-treatments--find)
    - [List All Treatments — `list-all-treatments`](#list-all-treatments--list-all-treatments)
    - [List a Pet's Treatments — `list-treatments`](#list-a-pets-treatments--list-treatments)
    - [View Summary of Completed Treatments — `summary`](#view-summary-of-completed-treatments--summary)
    - [View Overdue Treatments — `overdue-treatments`](#view-overdue-treatments--overdue-treatments)
    - [View Help — `help`](#view-help--help)
    - [Exit the Program— `bye`](#exit-the-program--bye)
- [FAQ](#faq)
- [Command Summary](#command-summary)

## Quick Start

- **Java**: Ensure you have Java 17.
- **Run**: `java -jar cuddlecare.jar`
- **Prompt**: You should see a greeting and a prompt symbol `> `.

```
Hello! Welcome to CuddleCare.
>
```

## Features

### Add Pet — `add-pet`

Adds a new pet to the tracker.

**Format**

```
add-pet n/PET_NAME s/SPECIES_NAME a/AGE
```

* n/ (required): name of the pet
* s/ (required): species of the pet
* a/ (required): age of the pet

**Example**

```
> add-pet n/Peanut s/Golden Retriever a/7
Peanut has been successfully added.

> add-pet n/Peanut s/Husky a/3
A pet with that name already exists.
```

**Notes**

* Name and species can be 1 or more words.
* Name cannot be longer than 20 chars.
* Species cannot be longer than 30 chars.
* Age cannot be more than 200 years old.
* Name and species cannot include anything except: A-Z, a-z,
  hyphen and space
* Each pet's name must be unique or user will have to re-enter
  (as seen above).
* Age must be a number and is in units of years.
* If input is invalid: `Invalid input. Usage: add-pet n/PET_NAME s/PET_SPECIES a/PET_AGE`

---

### List Pets — `list-pets`

Lists all pets.

**Format**

```
list-pets
```

**Example**

```
> list-pets
Here are your pets:
1. Milo (Species: Dog, Age: 3 years old)
2. Luna (Species: Cat, Age: 2 years old)
```

- If no pets: `No pets found.`

**Notes**

- Output uses **1-based index**.
- Command word is **`list-pets`** (with a dash).

---

### Edit Pet — `edit-pet`

Edit a pet’s **name**, **species**, and/or **age** by addressing it with its **current name**.

**Format**

```
edit-pet n/OLD_NAME [nn/NEW_NAME] [s/SPECIES] [a/AGE]
```

- **n/** *(required)*: current name to identify the pet
- **nn/** *(optional)*: new name
- **s/** *(optional)*: new species
- **a/** *(optional)*: new age (positive integer)

**Examples**

```
edit-pet n/Milo a/4
edit-pet n/Milo nn/Millie s/Dog
edit-pet n/Luna s/Cat a/3
```

**Constraints & Pitfalls**

- Flags must be **space-separated** (e.g., `n/Milo a/4`, not `n/Miloa/4`).
- Age must be a **positive integer**.
- If no optional field is provided, command is invalid (nothing to change).
- Usage help (on invalid input): `Usage: edit-pet n/OLD_NAME [nn/NEW_NAME] [s/SPECIES] [a/AGE]`.

---

### Delete Pet — `delete-pet`

Deletes a pet from the tracker by **name**.

**Format**

delete-pet n/PET_NAME

* **n/** *(required)*: the name of the pet to delete

**Examples**

    delete-pet n/Peanut
    Successfully removed Peanut (Golden Retriever, 7) from the list.

    delete-pet n/Unknown
    No Pet named "Unknown" exists

**Notes**

* The pet name must **match exactly** (case-insensitive) with an existing pet in the tracker.
* If no `n/` flag is provided or syntax is malformed:

Invalid Syntax: delete-pet n/<pet name>

* If the pet cannot be found, an error message is displayed.
* If deletion fails unexpectedly, the following message appears:

```
Something went wrong. Could not delete the pet
```

---

### Add Treatment — `add-treatment`

Adds a new treatment record for a specified pet.

**Format**

```
add-treatment n/PET_NAME t/TREATMENT_NAME d/DATE [note/NOTE]
```

* n/ (required): name of the pet
* t/ (required): name of the treatment
  * Maximum 50 characters.
  * Only letters, hyphens, and spaces allowed.
* d/ (required): date in yyyy-MM-dd format
  * Between 10 years in the past and 100 years in the future from the day of.
* note/ (optional): additional notes about the treatment

**Example**

```
> add-treatment n/Milo t/Vaccination d/2025-10-06
Added treatment "Vaccination" on 2025-10-06 for Milo.

> add-treatment n/Luna t/Dental Cleaning d/2025-11-15 note/Dr. Smith, $200
Added treatment "Dental Cleaning" on 2025-11-15 for Luna.
Note: Dr. Smith, $200
```

**Notes**

* Date must be in yyyy-MM-dd format (e.g., 2025-10-06).
* If pet not found: `Pet not found: <name>.`
* If date is invalid: `Invalid date format. Please use yyyy-MM-dd format (e.g., 2024-12-25).`
* On missing required fields: `Invalid input. Usage: add-treatment n/PET_NAME t/TREATMENT_NAME d/DATE note/{NOTE}.`

---

### Delete Treatment — `delete-treatment`

Deletes a treatment from a pet's record.

**Format**

```
delete-treatment n/PET_NAME i/INDEX
```

* n/ (required): name of the pet
* i/ (required): 1-based index of the treatment to delete

**Example**

```
> delete-treatment n/Milo i/1
Deleted treatment "Vaccination" for Milo.
```

**Notes**

* If pet not found: `Pet not found: <name>.`
* If index is invalid: `Invalid treatment index.`
* On malformed input: `Invalid input. Usage: delete-treatment n/PET_NAME i/INDEX.`

---

### Mark a Treatment as Done — `mark`

Marks a **per-pet treatment** as completed by **index** in that pet’s list.

**Format**

```
mark n/PET_NAME i/INDEX
```

**Example**

```
> mark n/Milo i/2
Marked Vaccination on 2025-10-05 as done for Milo
```

- On malformed args or non-integer index, usage help is shown: `Usage: mark n/PET_NAME i/INDEX`.
- If pet not found / index invalid: an error message is shown.
- Marking toggles the status in subsequent listings as `[X]` (done).

---

### Unmark a Treatment — `unmark`

Unmarks (sets **not completed**) by **1-based index** in the selected pet’s list.

**Format**

```
unmark n/PET_NAME i/INDEX
```

**Example**

```
> unmark n/Milo i/2
Unmarked Vaccination on 2025-10-05 as done for Milo
```

- On malformed args or non-integer index, usage help is shown: `Usage: unmark n/PET_NAME i/INDEX`.

---

### Group Treatments by Type — `group-treatments`

Groups treatments by **type** either **for a specific pet** or **across all pets**.

- **Type definition**: the **first word** of the treatment name (case-insensitive).  
  Examples: `Vaccine A` → *Vaccine*; `Checkup` → *Checkup*; `Dental Cleaning` → *Dental*.
- **Sorting**:
    - Type groups are sorted **alphabetically (case-insensitive)**.
    - Inside each type, items are sorted by **date (ascending)**.

### For a specific pet

**Format**

```
group-treatments n/PET_NAME
```

**Example (per-pet)**

```
> group-treatments n/Milo
== Checkup ==
1. Milo: [ ] Checkup on 2024-02-05
== Vaccine ==
1. Milo: [ ] Vaccine A on 2024-01-10
```

- If pet not found: `No such pet: <name>`.
- If pet has no treatments: `No treatments for <name> to group.`

### Across all pets

**Format**

```
group-treatments
```

*(If no name is supplied, the app groups treatments for **all pets**.)*

**Example (all pets)**

```
> group-treatments
== Checkup ==
1. Milo: [ ] Checkup on 2024-02-05
2. Luna: [X] Checkup on 2024-02-08
== Vaccine ==
1. Milo: [ ] Vaccine A on 2024-01-10
```

---

### Filter Treatments by Date — `treatment-date`
Displays all treatments that fall within a specified date range across all pets.

**Format**

```
treatment-date from/FROM_DATE to/TO_DATE
```

* from/ (required): start date in yyyy-MM-dd format
* to/ (required): end date in yyyy-MM-dd format
* Date range is inclusive (includes both start and end dates)

**Example**

```
> treatment-date from/2025-11-01 to/2025-11-30
Treatments between 2025-11-01 and 2025-11-30:
1. Annual Vaccine (Luna) - 2025-11-15
2. Dental Checkup (Milo) - 2025-11-22
```

If no treatments in range: `No treatments found between <FROM_DATE> and <START_DATE>.`

**Notes**

* Start date must be before or equal to end date.
* If start date is after end date: `Error: Start date cannot be after end date.`
* If date format is invalid: `Invalid date format. Please use yyyy-MM-dd format.`
* On missing parameters: `Invalid input. Usage: treatment-date start/START_DATE end/END_DATE.`

---

### Find Treatments — `find`
Searches for treatments across all pets by keyword. The search is case-insensitive and uses substring matching.

**Format**

```
find KEYWORD
```

**Example**

```
> find vaccine
Found 2 treatments containing 'vaccine':
1. Rabies Vaccine (Milo) - 2025-10-10
2. Annual Vaccine (Luna) - 2025-11-15

> find checkup
Found 1 treatments containing 'checkup':
1. Regular Checkup (Milo) - 2025-09-20
```

**Notes**

* Keyword cannot be empty or whitespace-only.
* If no matches: `No treatments found containing '<keyword>'.`
* If keyword is empty: `Error: Please provide a keyword to search for.`

---

### List All Treatments — `list-all-treatments`

Lists all treatments across all pets in ascending order.

**Format**

```
list-all-treatments
```

**Example**

```
> list-all-treatments
1. mimi: [ ] vaccine on 2025-10-10
2. snoopy: [ ] vaccine on 2025-10-11
```

**Notes**

* If no treatments: `No treatments logged.` will be displayed.

---

### List a Pet's Treatments — `list-treatments`

Lists all treatments specific to a pet.

**Format**

```
list-treatments n/PET_NAME
```

**Example**

```
> list-treatments n/mimi
mimi's treatment history:
1. [ ] vaccine on 2025-10-10
2. [ ] dental appointment on 2025-10-11

> list-treatments n/snoopy
snoopy has no logged treatments.
```

**Notes**

* The treatments listed are in the order it was added, with the latest addition at the bottom of the list.
* If no treatments: `<PET_NAME> has no logged treatments.` will be displayed.
* If pet is not found: `Pet not found: <PET_NAME>` will be displayed.
* Pet name is not case-sensitive.

---

### View Summary of Completed Treatments — `summary`

Displays a summary of all completed treatments within a specific date range.

**Format**

```
summary from/FROM_DATE to/TO_DATE
```

* from/ (required): start date in yyyy-MM-dd format
* to/ (required): end date in yyyy-MM-dd format
* Date range is inclusive (includes both start and end dates)

**Example**

```
> summary from/2025-10-10 to/2025-12-10
1. mimi: [X] vaccine on 2025-10-10
2. snoopy: [X] vaccine on 2025-10-11
```

**Notes**

* If no completed treatments: `No treatments found from <FROM_DATE> to <TO_DATE>' will be displayed.`

---

### View Overdue Treatments — `overdue-treatments`

Lists all overdue treatments for pets.

**Format**

overdue-treatments [n/PET_NAME]

* **n/PET_NAME** *(optional)* — filters overdue treatments for a specific pet.  
  If omitted, the command displays overdue treatments for **all pets**.

**Description**
Displays all treatments that are **overdue**, i.e. those that:

* Are **not marked as completed**, and
* Have a **scheduled date before today**.

If no pets have overdue treatments, a message confirming that is displayed.

**Examples**

    > overdue-treatments
    Overdue Treatments:
        Bella: "Vaccination" was due on 2023-10-10 (overdue for 5 days)
        Luna: "Check-up" was due on 2023-10-09 (overdue for 6 days)

    overdue-treatments n/Bella
    Overdue Treatments for Bella:
        "Vaccination" was due on 2023-10-10 (overdue for 5 days)

    overdue-treatments n/Max
    No pet found with the name: Max

    overdue-treatments n/
    Invalid arguments provided.
    Syntax: overdue-treatments [n/PET_NAME]

**Notes**

* A treatment is **overdue** only if:
    - It is **not completed** (`isCompleted = false`)
    - Its **date is before today** (`t.getDate().isBefore(LocalDate.now())`)
* The command automatically calculates how many days each treatment is overdue.
* Works for either:
    - **All pets** (`overdue-treatments`)
    - **A specific pet** (`overdue-treatments n/<pet name>`)
* Displays friendly messages when:
    - No pets exist in the system
    - The selected pet has no overdue treatments
    - An invalid or non-existent pet name is provided

---

### View Help — `help`

Displays information about available commands in the **CuddleCare** application.

**Format**

help [c/COMMAND_NAME]

* **c/COMMAND_NAME** *(optional)*: shows detailed information about a specific command.

**Examples**

    > help
    Here is the list of all commands supported by the application:
    General
        bye: Exits the application
        help: Displays All Commands

    Pet
        add-pet: Adds a new pet
        delete-pet: Deletes a pet from the application by name
        edit-pet: Edits a pet's name, species, and/or age.
        list-pets: Lists all pets in the application.

    Treatment
        add-treatment: Adds a treatment record for a pet
        delete-treatment: Deletes a treatment for a specific pet
        find: Finds treatments containing a keyword
        group-treatments: Groups treatments by type
        list-all-treatments: Lists all treatments across all pets
        list-treatments: Lists all treatments for a pet
        mark: Marks a treatment as completed for a pet.
        overdue-treatments: Lists overdue treatments for pets
        summary: Displays a summary of completed treatments.
        treatment-date: Filters treatments by date range
        unmark: Unmarks a treatment (sets it as not completed) for a pet.

    Run "help [c/COMMAND_NAME]" to find out more about a command.

**Notes**

* Running `help` without arguments lists **all commands**, grouped by category.
* Using the `c/` tag shows **detailed information** for a specific command.
* If an invalid tag or command name is used:

```
Invalid Syntax.
help [c/COMMAND_NAME]
```

or

```
Command "xyz" not found. Run "help" for a list of all available commands.
```

---

### Exit the Program — `bye`

Exits the **CuddleCare** application safely.

**Format**

```
bye
```

**Example**

    > bye
    Bye bye, Have a wonderful day ahead :)

**Notes**

* This command **terminates the program** immediately after displaying the farewell message.
* It does **not accept any arguments** — typing anything after `bye` (e.g., `bye now`) is ignored.
* All data up to the last valid command is already saved automatically before exit.

---

## FAQ

**Q**: What is the format for the dates?  
**A**: All commands which require date use this format: `YYYY-MM-DD`, example: `2025-10-31`

**Q**: Do I have to write tags in the same order as mentioned in the command syntax?  
**A**: No. Tags can be used in any order. Example: `add-pet n/Milo s/Cat a/2` and `add-pet s/Cat a/2 n/Milo` both work
the same.

**Q**: Are pet names case-sensitive?  
**A**: No. `milo` and `Milo` refer to the same pet.

**Q**: Can I add two pets with the same name?  
**A**: No. Each pet name must be unique.

**Q**: Can I use flags as pet names, species, treatment names or other inputs?
**A**: No. Avoid using characters like `n/`, `s/`, `t/`, `d/`, `note/` in names.`/` is used to separate command 
parameters. Any text followed by `/` is interpreted as a command tag.

**Q**: What does [X] mean beside a treatment?
**A**: [X] means the treatment is completed.
[ ] means it is pending.

**Q**: Will my data be saved automatically?
**A**: Yes. All valid commands are saved immediately, even before exiting.

**Q**: How do I clear all data at once without deleting one by one?  
**A**: Head over to `/data/` and delete the `cuddlecare_save.txt` file.
> **⚠️️ CAUTION:** Deleting the save file will permanently erase all application data.


---

## Command Summary

* List Pets `list-pets`
* Edit Pet `edit-pet n/OLD_NAME [nn/NEW_NAME] [s/SPECIES] [a/AGE]`
* Add Treatment `add-treatment n/PET_NAME t/TREATMENT_NAME d/DATE [note/NOTE]`
* Delete Treatment `delete-treatment n/PET_NAME i/INDEX`
* Mark a Treatment as Done `mark n/PET_NAME i/INDEX`
* Unmark a Treatment `unmark n/PET_NAME i/INDEX`
* Group Treatments by Type `group-treatments [n/PET_NAME]`
* Filter Treatments by Date `treatment-date from/FROM_DATE to/TO_DATE`
* Find Treatments `find KEYWORD`
* List All Treatments `list-all-treatments`
* List a Pet's Treatments `list-treatments n/PET_NAME`
* Completed Treatment Summary `summary from/FROM_DATE to/TO_DATE`
* Help `help [c/COMMAND_NAME]`
* Delete pet `delete-pet n/PET_NAME`
* Overdue Treatments `overdue-treatments [n/PET_NAME]`
* Exit `bye`
