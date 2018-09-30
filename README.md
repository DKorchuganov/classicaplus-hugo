# classicaplus-hugo <!-- omit in toc -->

[Hugo](https://gohugo.io) source of the [classicaplus.com](http://classicaplus.com) website.

This project is in Work in progress status, the current production site is created using another static site generator.

- [Initial setup](#initial-setup)
- [Content editing](#content-editing)
  - [Adding a team member](#adding-a-team-member)
    - [Team member data file](#team-member-data-file)
    - [Team member content file](#team-member-content-file)
  - [Adding a composer](#adding-a-composer)
    - [Composer data file](#composer-data-file)

## Initial setup

The best way to edit/create the site content on a local PC is to use [VS Code](https://code.visualstudio.com), an open-source code editor from Microsoft. It has validation capabilities for YAML data files using JSON schema, and the project contains relevant schema files as well as the .vscode folder with corresponding project settings.

List of optional VS Code extensions for a comfortable work:

1. [YAML Support by Red Hat](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml) - to validate YAML data files
2. [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one) - for comfort content editing
3. [markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint) - to check content file syntax

## Content editing

Site content consits of 2 major parts: data files in `data` folder and content files in `content` folder. Data files use YAML format, content files are in markdown format with YAML front-matter. The site is multilingual (currently in english and Rissian), which is supported by:

- two language version of the each content file with different language code extensions like `.ru.md` and `.en.md`
- language-specific sub-sections in YAML data files

The below sections describe how to add/edit content of diffrent types.

### Adding a team member

All performers mentioned in evens should be added as team members. They could have a separate team member pages on the side, and could be mentioned on the common team page, which is controlled by `priority` filed in a team member data file, see below. Create a team member data file first, then create content files, if required.

#### Team member data file

1. Go to `data/team` folder and create a file with `firstname.lastname.yaml` filename format
2. Add `priority` field, a positive value defines a position of a team member on the common team page (ordered from lowest to highest), put a negative value to exclude a person from the team page
3. Add `poster` section with `small` and `big` fields for poster file names. `small` poster is displayed on the common team page, `big` poster on an individual page of a team member. `poster` section is optional for persons with negative `priority`, see above. Poster files should be stored in `static/resources/img/team/firstname.lastname` folder of a specific team member.
4. Add `name` section with multiple language code sub-sections like `ru` and `en`. In each sub-section add `first` and `last` fields for the firsname and lastname respectively
5. Add `instrument` section for the person's instrument names in different languages, names are given in fields with respective languge codes like `ru` and `en`

**Team member data file example:**
`data/team/eugenia.boginskaya.yaml`

```yaml
priority: 2
poster:
  small: eugenia.boginskaya-small700.jpg
  big: eugenia.boginskaya-big700.jpg
name:
  ru:
    first: Евгения
    last: Богинская
  en:
    first: Eugenia
    last: Boginskaya
instrument:
  ru: виолончель
  en: cello
```

#### Team member content file

Content files are mandatory for team members which are published on the common team page (with positive priority field, see above in [Team member data file](#team-member-data-file) section). Content files of team members with negative priority are still used to create and individual member page, but this page is not referenced on the common team page.

1. Go to `content/team` folder and create a condent file with `firstname.lastname.ru.md` filename format for content in Russian language, and similar files for other language codes
2. Add YAML front-matter with `title` and `description` fields, separated by lines with triple dash, see the below example

**Team member content file example:**
`content/team/eugenia.boginskaya.en.md`

```markdown
---
title: "Eugenia Boginskaya"
description: "Eugenia Boginskaya, cello"
---
A few words from Eugenia
```

### Adding a composer

All composers used in event's programs should be described in data files in `data/composers` folder. There is no content files for composers at the moment, but they might be added in the future.

#### Composer data file

1. Go to `data/composers` folder and create a file with `firstname.lastname.yaml` filename format
2. Add `name` section with multiple language code sub-sections like `ru` and `en`. In each sub-section add `first`, `last` and `full` fields for the firsname, lastname and fullname respectively

**Composer data file example:**
`data/composers/alberto.ginastera.yaml`

```yaml
name:
  ru:
    first: Альберто
    last: Хинастера
    full: Альберто Эваристо Хинастера
  en:
    first: Alberto
    last: Ginastera
    full: Alberto Evaristo Ginastera
```
