# Entity, Class Definitions

---

**Version:** 0.1.0
**Author:** Artemy Malkov
**Fidelity:** `#good-enough`

---

**Key Concepts:** 
- [Entity](#entity)
- [Class](#class)
- [Subclass](#subclass)
- [Property](#property)
- [Class Guidelines](#class-guidelines)
- [Entity Template](#entity-template) 
- [Typecasting](#typecasting) 
- [Inheritance of Classes](#inheritance-of-classes)

---

## Entity 

Alterative names: `CoEntity`, `CogniEntity` if the word Entity is ambigous in the current context.

`Entity` is a structured piece of data representing an object (product, person, file, organization, etc.). `Entities` are typically used in business processes and workflows. `Entities` are quite similar to objects or class isntances in traditional programming. However in `Cognitive Components` entity is not necessarily represent a traditional fixed data structure (set of fields or a database entry). Instead, `Entities` may contain semi-structured data. Think of `Entity` as of an Markdown file describing the object with the ability to augment the data structure with additional information put into the file. `Entities` can represent `Cognitive Classes` and support `Typecasting` so that the `Entity` can be mapped into the data format required for a particular business process expecting instances of a particular `Cognitive Class`. 

---

## Class 

Alterative names: `CoClass`, `CogniClass`, `Cognitive Class` if the word Class is ambigous in the current context.

`Cognitive Class` defines a type of `Entities` united by common characteristics, purpose, or application area. `Cognitive Classes` are formally introduced and declared within a `Cognitive Package`. According to convention, `Cognitive Classes` should be named with a capital letter and contain no spaces or special characters, for example "MyFirstClass". The main purpose of `Classes` is to define data structures expected by business processes and workflows defined in the `Cognitive Package`. `Cognitive Classes` define only data structure and do not support methods which makes this concept different from the traditional classes in Object-Oriented Programming.

`Cognitive Class files` (for example `MyWonderfulClass.md`) should be placed into `{component-root}/concepts/classes` folder.

### Subclass

`Subclass` is a specialized `Cognitive Class` that inherits data structure from a parent class and adds specific properties or features for a narrower application area. `Subclasses` are formally introduced and declared within a `Cognitive Component`, similar to base `Cognitive Classes`. According to convention, `Subclasses` should be named with a capital letter and contain no spaces or special characters, for example "MySubclass". 

`Subclasses` are used to create a class hierarchy where each level adds more specific properties or characteristics. `Cognitive Classes` support multiple inheritance, which means that the instance of the `Subclass` can be `Typecasted` to the one or another parent class and can participate in the business processes operating with one of these parent classes.

In the context of `Taxonomies`, subclasses can be mapped to specific `Categories`, allowing formal definition of important classification nodes that require strict typing and use in workflows and automation.

---

## Property

`Properties` of the `Class` contain information about individual elements of the data structure that are required for every instance, have a defined format or data type, and are inherited by all `Subclasses`. A `Property` may be a date, string, number, value from an extensible set of options, a value from a `Taxonomy` (values selected from taxonomy categories), or a link. 

In general, `Properties` do not require strict typing and allow fuzzy-logic `Typecasting`.

### Properties naming conventions

Property naming convention: Properties are expected to be named as one or several English words. As the `Cognitive Components` are built for cognitive logic and are friendly to semi-structured information, there is no one single must-have convention for property naming. Every project may have their preferred way to name the properties, if that does not cause unnecessary ambiguity. The recommended conventions for properties are: 

````
  camelCasePropertyName

  PascalCasePropertyName
  
  snake_case_property_name
  
  kebab-case-property-name
  
  `Inline code style property name` (may be used in entity md files with `backticks`)
  
  **Bold label style property name** (may be used in entity md files with **Bold** - use carefully to avoid ambiguity)
````

The `Cognitive Project`, `Cognitive Package` or `Cognitive Component` can choose which convention to use as long as properties can be clearly interpreted as a sequence of words and this convention is consistent across one `Cognitive Component` and `Cognitive Package`.

Acronyms should be spelled all capital or all lowercase within a property name. Avoid ambiguity. Valid examples:

````
  URL
  
  pdf
  
  httpStatus
  
  RFP_document
````

### Property type, format, default value and semantic

If not specified, all `Properties` have undefined type, format and default value, while the semantic of the property is defined by its name.

Recommended format of property definition (the choice of snake_case and camelCase used in these examples here is not obligatory):

````
  property_name : property_type (property_format) = default_value - property_semantic
  propertyName : propertyType (propertyFormat) = defaultValue - propertySemantic
````

Property type, format and semantic are not obligatory; if you declare just a property name, it will have undefined `Property type`, no specified `Property format`, and `Property semantic` will be the same as the property name.

`Property type` defines what type the property expects:
- undefined or just no type : no explicit type, can be anything
- text/string : text 
- number/integer : numeric value
- date/date-time : date 
- period : time period (days, months, years, minutes, seconds) 
- url : link to a webpage 
- `SomeClass` : link to an entity of a defined `SomeClass`
- `#SomeTaxonomy` : link to a particular `Category` in `SomeTaxonomy`

`Property format` defines expected format, there is no one single required way to describe format, just avoid ambiguous declaration:
- (description of expected format) - format described verbally
- (regexp) - regular expression defining the format
- (XX-XXX-X-XXXX) - template
- (small | medium | large) - options of possible values

`Default value` defines the default value of the property.

`Property semantic` explains the meaning of the property which simplifies `Typecasting` and makes entities easier to interpret for humans and machines. 

Examples of valid declarations (property names in snake_case):
````
  email_address : text (^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$) - contact email for communication

  priority_level : number (1 | 2 | 3 | 4 | 5) = 3 - urgency ranking for task scheduling

  created_at : date/date-time (YYYY-MM-DD or ISO 8601) - timestamp when the entity was first created

  document_url : url (valid HTTP/HTTPS link) - web location of the referenced document

  author : Person (person entity) - person who authored or created the file

  product_category : #ProductTypeTaxonomy - product group in the catalog

  internal_reference (alphanumeric ID) - reference to external system

  page_name : text - displayed title of the page

  status (draft | published | archived) = draft

  user_name
````

### Properties keywords

There are 3 groups of `Properties` in the `Class` declaration:

- 1. **IDENTIFIER** : Identification Properties - properties that are used to identify the `Entity` matching different entities to make sure these are the same or different `Entities` when getting semi-structured data from different sources. It can be a database ID or a passport number, first, second name and the date of birth for employees, etc.
- 2. **MANDATORY** : Properties that are mandatory for the majority of workflows and business processes
- 3. **OPTIONAL** (Default if no keyword is applied at the property declaration) : Properties that are nice to have but not required for most of the workflows, they bring additional information but the absense of this information is not critical most of the time.
- 4. **COMMON** : This property value is defined in the class, and is the same for all instances (`Entities`) of this class. This is similar to `static` keyword in programming languages meaning that the property is defined on the class level, not on the individual instances.

Example of property declaration with keywords:
````
  database_id IDENTIFIER : text (UUID) - unique identifier for entity matching across sources

  passport_number IDENTIFIER : text (XX-XXXXXXX) - government-issued ID for person identification

  title MANDATORY: text - displayed title required for workflows

  created_at MANDATORY: date/date-time (ISO 8601) - timestamp required for audit

  notes OPTIONAL : text - additional notes for context

  species COMMON : #MammalsTaxonomy = #felis-catus - biological species shared by all cats, instances of the Cat class
````

Using combination of fields as IDENTIFIER: A combination of field may be used as an IDENTIFIER. Example:
````
  name : text - user name

  surname : text - user surname

  birth_date : date - user birth date

  name & surname & birth_date IDENTIFIER
````

---

## Class Guidelines

Besides `Properties`, a `Cognitive Class` can also contain **Guidelines**. `Guidelines` are text descriptions (short or long) that provide additional information about the `Class`. They may include external links for more details, explanations, use cases (how the class is used), and separate sections that describe logic, application of the class, and features or characteristics of its instances (`Entities`). In effect, `Class Guidelines` are part of the documentation and the prompt context for the `Class`.

This text serves as context for language models that process business processes and `Rulebooks` should know about the class. When a large language model performs a business process that uses entities representing the `Class`, it will need to get acquainted with the `Class Guidelines` described in the class, and then execute the `Rulebook` of the business process.

### Best practice for locating Guidelines in the Component

**Where to describe Guidelines.** The class file is not the best place to describe guidelines. It is better to describe concepts and principles, including class-related documentation, in the concepts folder of the `Cognitive Component`, specifically in **definitions** and **principles** subfolders. When creating a `Cognitive Component`, prefer putting definitions and principles in the definitions area rather than inside the class files themselves.

**Exception.** However, some projects may have many subclasses or categories (for example, 10 or 20 product categories). Creating separate definitions for each may not be practical. In such cases, `Class Guidelines` within the class file can describe some of this information, specific to that exact subclass. The `Class Guidelines` within the class file then provide the necessary context for the language model when it works with that class and its instances.

### What is considered a Guideline and what is not in Class declaration file

We consider Guidelines to be information that contains essential details (besides the formal description of the data structure and properties) necessary for applying classes in business processes; we do not include technical details. Guidelines complement properties in business logic.

**These are Guidelines:**
- explanation of the essence of this class, why it was introduced, what entities it describes, its ontology and relationship with other classes
- use cases of how this class is applied in business processes
- characteristics common to all instances of the class (described verbally, not as properties)
- policies and principles for processing this class and its instances in business processes and `Rulebooks`
- examples of using the class in business logic

**These are NOT Guidelines:**
- property declarations. They describe the data structure foundation; we do not count them as Guidelines in order to separate formal definition (Properties) from informal (Guidelines)
- descriptions of technical implementation details, including mappings of the class to databases or to classes in programming languages (they belong to concrete implementation, not business logic, and should not be placed in context to avoid conflicts and clutter)
- refactoring instructions or design-time details at the `Cognitive Component` design stage (not included in Guidelines to avoid cluttering the context window and creating conflicts during business process execution)
- file metadata (author, status, change history, etc.)

### Where Guidelines are located in Class File

**Example of Class Guidelines in Code-style:**

In this example, a Guideline is placed explicitly in the comment of the Product class and implicitly in the comment to the price `Property`; both of them are considered `Class Guidelines`.

````
class Product:
  # Guideline
  # This is a base class for all products on our acme.com website. The products are stored in a large catalog and the website will be recommending the products based on customer search queries and behavior. The user may be interested in similar products or products with particular characteristics which are stored in the description property.

  name : string (not more than 30 letters) - name of the product in the catalog
  vendor : #VendorTaxonomy - the name of the vendor should match the taxonomy of vendors 
  sku IDENTIFIER : string (unique id) - unique sku code in the catalog
  description : text (not more than 120 words) - description of the product containing value proposition and characteristics
  price : number - price in US dollars.   # prices are stored in US dollars and should be recalculated in local currency if the user visits the website from another country. For conversion use xe.com for reference rates or a licensed API such as exchangerate-api.com or openexchangerates.org for automated workflows. 

````

**Example of Class Guidelines in Markdown-style:**

In this example, a Guideline is placed explicitly in the Guidelines section of the `Cognitive Class file` in markdown format, and implicitly in the class definition in the root section (This is a base class...) and also in the `Property` Price. All three placements are considered `Class Guidelines`. 

````markdown
# Product Class

This is a base class for all products on our acme.com website. 

## Properties
`Name` : string (not more than 30 letters) - name of the product in the catalog
`Vendor` : #VendorTaxonomy - the name of the vendor should match the taxonomy of vendors 
`SKU` IDENTIFIER : string (unique id) - unique sku code in the catalog

### Description 
Description of the product containing value proposition and characteristics.
**Type**: text
**Format**: not more than 120 words

### Price 
Price in US dollars. Prices are stored in US dollars and should be recalculated in local currency if the user visits the website from another country. For conversion use xe.com for reference rates or a licensed API such as exchangerate-api.com or openexchangerates.org for automated workflows. 
**Type**: number

## Guidelines

The products are stored in a large catalog and the website will be recommending the products based on customer search queries and behavior. The user may be interested in similar products or products with particular characteristics which are stored in the description property.

````

---

## Entity Template

`Entity templates` can reside either inside the `Class file` or in separate files, for example in the folder `{component-root}/definitions/templates`. On the one hand, a template defines an example format for storing entity class data; on the other hand, it may contain instructions for `Typecasting` - converting unstructured data into the required format for processing as entities within business processes and rulebooks.

The template is a combination of text element that should be present in the output file and instructions. Instructions should be placed inside square brackets `[instruction]` or curly braces `{instruction}` — both options are acceptable if they do not create conflict or ambiguity in interpretation. 

Instructions are written in natural language and may reference variable values.

Generally you may use the names of `Properties` from the `Cogntivie Class` in the template where it does not cause ambiguity. In complex situations in order to specify the property names you may use on of these options:

- property_name (bare reference use when context is clear)
- this.property_name
- self.property_name
- `property_name`
- _property_name
- {property_name}
- {{property_name}}
- ${property_name}

If you choose to use curly braces for variables, then square brackets are recommended for enclosing instructions to avoid ambiguity and interpretation errors.

If you chose one of these conventions, keep it consistent across all files in your `Cognitive Component`.

### Example of Employee template

employee-template.md
````markdown
# [this.Name this.Surname]

## Class: Employee

## General Information

Name : [name, check that the name is in the correct form (Bob vs Robert) as per the Corporate employee registry]
Surname : [surname]
Email : [email]
Birth Date : [birth_date, format: YYYY-MM-DD] 

---

## Job

Department : [department_name in #DepartmentTaxonomy category, e.g. "Finance"] ([this.department : #DepartmentTaxonomy, e.g. "#finance-department"])
Job Title : [job_title_name in #JobTitleTaxonomy, e.g. "Senior Developer"] ([this.job_title : #JobTitleTaxonomy, e.g. "#senior-developer"])
Start Date : [employment_start_date, e.g. "2020-01-15"]
End Date : [employment_end_date or "present" for current role, e.g. "2024-12-31" or "present"]

---

## Performance Review

Supervisor : [name and surname of employee_supervisor, and email e.g. "John Doe, j.doe@corporation.com"]

### Performance Feedback

( [perfornamce reviews in this section, ordered from the most recent to the oldest] )

[date - feedback text, e.g. "2025-01-15 - Exceeded expectations on project delivery"]
[date - feedback text, e.g. "2024-06-20 - Strong collaboration skills, recommended for lead role"]
[date - SUPERVISOR CHANGE - previous name surname - new supervisor name surname, email]
[date - feedback text, e.g. "2023-12-10 - Met all quarterly objectives"]
[date - SUPERVISOR ASSIGNED - first supervisor name surname, email]

---

# END OF TEMPLATE, EXAMPLES BELOW THIS LINE

---

# Jason Lee

## Class: Employee

## General Information

Name : Jason
Surname : Lee
Email : jason.lee@acme.com
Birth Date : 1985-03-01

---

## Job

Department : IT (#it-department)
Job Title : CIO (#cio)
Start Date : 2017-04-12
End Date : present

---

## Performance Review

Supervisor : Daniel Flores, dflores@acme.com

### Performance Feedback

2024-11-20 - Outstanding performance review; exceeded revenue and efficiency targets.
2023-09-05 - Met all annual objectives; recommended for expanded regional responsibilities.
2022-03-10 - Successfully led digital transformation initiative; recognized by board for strategic vision.
2021-06-15 - SUPERVISOR CHANGE: Sarah Chen - Daniel Flores, dflores@acme.com.
2020-12-15 - Strong leadership during remote transition; team engagement scores improved by 15%.
2019-07-23 - Exceeded expectations on infrastructure migration; delivered ahead of schedule with zero downtime.
2017-04-12 - SUPERVISOR ASSIGNED: Sarah Chen, schen@acme.com.
````

### How Typecasting works via Templates 

Templates are used actively in `Typecasting`. When you have a target `Class` used in business processes and data arrives in another format (from the Internet, a database, or a text file) `Typecasting` takes this raw data, applies the template to it, and produces `Entity` data that conforms to your class structure.

The `Enity Template` defines variables that connect values from the external source with the entity. The entity is recorded in the `Entity Template` format; the template contains fields that correspond to the class properties. A file produced by typecasting can be treated as an `Entity` - an instance of the `Class`.

For `Typecasting` to work reliably, the `Entity Template` should expose all properties of the class — at minimum all IDENTIFIERs and MANDATORY fields, and ideally the rest as well. Each class field must be mappable from the template so that we can reverse-engineer what value it holds. This is especially important for MANDATORY fields.

#### Entity Identification during Typecasting

Some class properties are marked as **IDENTIFIER**. These fields are used to determine whether an incoming record corresponds to an entity we already have in our specific `Data Collection` — for example, a list of employees, products, or cities.

When new information arrives in unstructured or semi-structured form, we apply `Typecasting` to map it to our class structure. Two outcomes are possible:

1. **New Entity** — The record does not match any existing object in the catalog. We treat it as a new `Entity` and work with it accordingly.
2. **Existing Entity** — The record matches an entity we already have, e.g. we identify the existing user or propduct. Depending on the business process, the `Rulebook`, we may treat the incoming information as complementary data (e.g., updating the entity and enriching its data with new information).

`Entity Identification` can occur during `Typecasting`. The business process or `Rulebooks` then decide how to handle the result: whether we have identified an existing entity or created a new one and add it to the `Data Collection` and how we resolve contradicting data for the same `Enitity`.

---

## Typecasting

`Typecasting` is a mechanism for mapping an `Entity` (an instance of a `Cognitive Class` or a free-form data structure) into the data format defined by a particular `Cognitive Class`. It allows an `Entity` to participate in business processes and workflows that expect instances of a specific `Cognitive Class`—whether the `Entity` belongs to a `Subclass`, or was obtained from unstructured sources such as a third-party database, webpage scraping, or a free-form description in a user query.

`Typecasting` works like slot-filling in natural language processing: the target `Cognitive Class` defines the properties (slots) that an instance should have, and the mechanism fills these slots from the available data of the source `Entity`.

---

## Inheritance of Classes

Class inheritance is a mechanism for transfer of `Properties` from a parent class to subclasses. When inheriting, all `Properties` of the parent class automatically become part of the subclass. A subclass can refine the format or extend the set of parent properties, but cannot cancel their presence. This ensures data structure compatibility, and formal validation in the class hierarchy. Inheritance creates vertical connections in the class hierarchy and is the basis for working with databases, compilers, and formal algorithms.

**Example:** The `Document` class has a property `creation_date` (type: date). The `Contract` subclass automatically inherits this property and cannot remove it, but can refine the date format or add additional properties, for example `signing_date`.

For better transparency when you describe a subclass it may be reasonable to list all inherited properties as well.


Example of Classes with inheritance declaration in the code style:
````
Class User: 

  name : string
  surname : string
  email IDENTIFIER : text (valid email) - personal or corporate email 


Class Employee (User):

  inherited from User
  - name
  - surname
  - email - corporate email

  department : #DepartmentTaxonomy
  job_title : #JobTitleTaxonomy
  supervisor : Employee
  personal_email (valid email) - personal not corporate email of the employee
  name & surname IDENTIFIER - combination of name and surname can be used for matching
````

Example of Classes with inheritance declaration in the markdown style:

User.md
````markdown
# User Class

## Properties
`Name` : string
`Surname` : string
`Email` IDENTIFIER : text (valid emal) - personal or corporate email 
````

Employee.md
````markdown
# Employee Class

## Properties Inherited from User
- `Name`
- `Surname`
- `Email` - corporate email

## Properties
- `Department` : #DepartmentTaxonomy - department from the department catalog
- `Job Title` : #JobTitleTaxonomy - job title from the corporate job title catalog
- `Supervisor` : Employee

### Personal Email
Personal not corporate email of the employee. Every employee must have a personal email in the system during the onboarding. 
**Format** : valid email

## IDENTIFIERS 
###  Name & Surname
A combination of name and surname can be used for matching. If the process is not critical, and does not require strict person identification, it is allowed to use the combination of the name and surname as an identifier. In non-sensitive cases it is allowed to aslo match the first names in full and short form e.g. Alex=Alexander, Elisabeth=Liz=Liza, Bob=Robert, etc. However if the corporate database have several people with similar Names and Surnames that may cause confusion, additional properties should be used for exact identification.
````

