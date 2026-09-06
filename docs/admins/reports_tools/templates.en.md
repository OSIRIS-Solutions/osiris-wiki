
# Template syntax

This documentation describes the template syntax that can be used to define individual citation styles in OSIRIS.
It is intended for people who want to replicate the official citation style of their institution (e.g., for publications) as accurately as possible.


## Basic principle

A citation template is a text with placeholders that is filled with the data of an activity at runtime.

**Example:**

```
{authors}. {title}. %journal ({year})% {journal}, {year}.
```

OSIRIS replaces:
- Field placeholders ({…}) with specific values
- Conditional blocks (%…%) only if certain fields exist
- Empty brackets, double punctuation marks, etc. automatically

For new activities, a standard template consisting of authors, title, and year is used. This template can (and should) be customized. This is particularly useful if your activities are only displayed as empty templates.


## 1. Field placeholders {field}

Simple field access

- `{title}`
- `{year}`
- `{journal}`

→ Replaced by the content of the respective field.
- If the field does not exist or is empty → empty string
- Arrays (e.g., author lists) are automatically concatenated with ,
- MongoDB objects are correctly resolved internally

> To find out which fields are available, you can use the [Template Builder](tools.md). All fields are listed there with sample data.
> 
> For **user-defined fields**, the field name is the ID you assigned when creating the user-defined field.


## 2. Author, editor, and supervisor templates

Authors, editors, and supervisors use a dedicated template syntax. This makes it possible to format lists of people flexibly.

The basic structure is:

```
authors-{name format}-{options}
editors-{name format}-{options}
supervisors-{name format}-{options}
```

`authors-`, `editors-`, or `supervisors-` must always come first. This is followed by the name format. Additional separator and person limit options can be appended with `-` and combined flexibly.

### Name formats

| Code | Example output |
|---|---|
| `last f.` | Koblitz J. |
| `last f` | Koblitz J |
| `f last` | J Koblitz |
| `f. last` | J. Koblitz |
| `last first` | Koblitz, Julia |
| `first last` | Julia Koblitz |
| `last, f.` | Koblitz, J. |
| `last, f` | Koblitz, J |
| `last, first` | Koblitz, Julia |

### Separators

| Code | Effect |
|---|---|
| *(no code)* | Comma and “and”: A, B and C |
| `amp` | Replaces “and” with “&”: A, B & C |
| `amp+comma` | Uses “, &” as the final separator: A, B, & C |
| `semicolon` | Uses semicolons instead of commas: A; B and C |

### Person limit

| Code | Meaning |
|---|---|
| *(no code)* | Display all people |
| `etal6` | Display no more than 6 people, followed by “et al.” |
| `ellipses5` | Display up to 4 people and the final person, using “...” between them where necessary |

The number can be adjusted to the required limit, for example `etal3` or `ellipses10`.

### Editor suffix

Editor suffixes apply **only to `editors-`**. They are not used with `authors-` or `supervisors-`.

| Code | Effect |
|---|---|
| `eds` | Always append “(eds.)” to the list |
| `ed` | Append “(ed.)” for one person and “(eds.)” otherwise |
| `Eds` | Always append “(Eds.)” to the list |
| `Ed` | Append “(Ed.)” for one person and “(Eds.)” otherwise |

### Example combinations

| Format | Example output |
|---|---|
| `authors-last f.` | Koblitz J., Stark T. and Miller L. |
| `editors-first last-amp-ed` | Julia Koblitz, Tony Stark & Lois Miller (eds.) |
| `authors-last, f-etal3` | Koblitz, J, Stark, T, Miller, L et al. |
| `authors-last first-amp+comma` | Koblitz, Julia, Stark, Tony, & Miller, Lois |
| `editors-f. last-semicolon-Eds` | J. Koblitz; T. Stark and L. Miller (Eds.) |
| `supervisors-last f.-semicolon-ellipses5` | Koblitz J.; Stark T.; Miller L.; Wayne B. ... Parker P. |

If a required format cannot yet be represented, you can create a ticket for it on GitHub.


## 3. Fallback / Priority {field|fallback}

A fallback can be defined with `|`.

### Literal as fallback

```
{doi|"without DOI"}
```

- If doi is empty → Text without DOI
- Literals must be enclosed in quotation marks (`"`)
- Important: This only applies to the fallback, not to conditional blocks! There, literals do not need to be enclosed in quotation marks and fields must be set in `{}`. The reason for this inconsistency is that nesting multiple `{}` would be difficult to read and prone to errors.

### Field as fallback

```
{doi|link}
```

- If doi is empty → value from the link field
- If link were not a known field, it would be interpreted as a literal and output as such.

### Resolution rule
1. The first element is always interpreted as a field.
2. If this is empty:
   - Quoted → literal
   - Unquoted + known field → field value
   - otherwise → literal


## 4. Conditional blocks %...%

Conditional blocks are only output if certain fields exist.

The syntax is as follows:

```
%CONDITION Text%
```



### Single field

```
%journal ({journal})%
```

→ Only output if journal exists and is not empty. This avoids empty brackets. (Empty brackets are automatically removed later, so this is only an example of how to use conditions.)


### AND condition (&)

```
%journal&year ({journal}, {year})%
```

→ Only output if all fields exist.

Typical use case:
- Year only if journal also exists
- Volume/issue only if pages exist


### OR condition (|)

```
%doi|link (Available at: {doi|link})%
```

→ Output if at least one of the fields exists.


### Negation (!)

```
%!title (No title available)%
```

→ Only output if title **does not** exist or is empty.


### Special case: Placeholder value -

A field with the value `-` is considered empty.
→ Condition fails.


## 5. Date formatting `{date:format}`

<!-- md:version 2.1.0 -->

Date fields can be displayed in a custom format. Add the desired date format after the field name, separated by a colon.

Available date fields:

- `date`: Date of an activity
- `start`: Start date of a date range
- `end`: End date of a date range

### Commonly used format characters

| Character | Meaning | Example |
|---|---|---|
| `Y` | Four-digit year | `2026` |
| `y` | Two-digit year | `26` |
| `F` | Full month name | `July` |
| `M` | Abbreviated month name | `Jul` |
| `m` | Month with leading zero | `07` |
| `n` | Month without leading zero | `7` |
| `d` | Day with leading zero | `04` |
| `j` | Day without leading zero | `4` |

Additional format characters are described in the [PHP date formatting documentation](https://www.php.net/manual/en/datetime.format.php).

### Examples

ISO date:

```
{date:Y-m-d}
```

Result:

```
2026-07-24
```

Date following APA conventions:

```
({date:Y}, {date:F j})
```

Result:

```
(2026, July 24)
```

### Date ranges

The start and end dates can be formatted independently:

```
{start:Y, F j}–{end:Y, F j}
```

Result:

```
2026, July 24–2026, July 26
```

#### Optional end date

For a regular date range, a missing end date means that the activity took place only on the start date. The end date and the en dash can therefore be placed in a conditional block:

```
{start:Y, F j}%end –{end:Y, F j}%
```

If there is no end date, only the start date is displayed.

#### Ongoing date range

For an ongoing date range, a missing end date means that the activity is still in progress. A literal fallback can be used to insert any desired text:

```
{start:Y, F j}–{end:Y, F j|"Today"}
```

For a German-language template, the wording itself can be specified:

```
{start:j. F Y}–{end:j. F Y|"heute"}
```

This allows the language and capitalization to be selected individually for each template.

### Compact date ranges with `end-compact`

With `end-compact`, matching parts of the start and end date are not repeated:

```text
({start:Y, F j}%end-compact –{end-compact:Y, F j}%)
```

The following output is generated automatically depending on the date range:

- Same day: `(2026, July 24)`
- Same month: `(2026, July 24–26)`
- Different month: `(2026, July 30–August 2)`
- Different year: `(2026, December 30–2027, January 2)`

If the end date is missing or matches the start date, `end-compact` remains empty. The en dash should therefore also be placed inside a conditional block.


## 6. Automatic cleanup

After replacement, OSIRIS performs automatic format cleanup:

The following are removed, among other things:
- Empty brackets: () or []
- Superfluous spaces
- Multiple periods or commas
- Commas before the end of a sentence
- Commas after `<br />` (line break)

**Example template:**
```
{authors}. {title}. ({journal}) {year}.
```

**Without journal:** 
Müller J. Title. 2024.

No manual intervention required ✅


## 7. The language of the templates

Some fields can be displayed in multiple languages, e.g., the month or the type of thesis. By default, the language of the OSIRIS user interface is used, but this can lead to inconsistent citations among different users. We therefore recommend explicitly specifying the language of the templates. You can do this in the general settings.


## 8. Limitations of the current syntax

The following are not currently supported:
- Nested conditions
- Mathematical comparisons
- Explicit else branches

➡️ The goal is traceability and maintainability, not Turing completeness.
