
# Template syntax for citation styles in OSIRIS

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


## 2. Fallback / Priority {field|fallback}

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


## 3. Conditional blocks %...%

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


## 4. Automatic cleanup

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


## 5. The language of the templates

Some fields can be displayed in multiple languages, e.g., the month or the type of thesis. By default, the language of the OSIRIS user interface is used, but this can lead to inconsistent citations among different users. We therefore recommend explicitly specifying the language of the templates. You can do this in the general settings. 


## 6. Limitations of the current syntax

The following are not currently supported:
- Nested conditions
- Mathematical comparisons
- Explicit else branches

➡️ The goal is traceability and maintainability, not Turing completeness.
