# classicaplus-hugo <!-- omit in toc -->

[Hugo](https://gohugo.io) source of the [classicaplus.com](http://classicaplus.com) website.

This project is in Work in progress status, the current production site is created using another static site generator.

- [Initial setup](#initial-setup)
- [Content editing](#content-editing)
  - [Team members](#team-members)
    - [Team member data file](#team-member-data-file)
    - [Team member content file](#team-member-content-file)
  - [Composers](#composers)
    - [Composer data file](#composer-data-file)
  - [Composer's works](#composers-works)
    - [Work data file](#work-data-file)
  - [Places](#places)
    - [Place data file](#place-data-file)
  - [Events](#events)
    - [Event data file](#event-data-file)
    - [Event content file](#event-content-file)

## Initial setup

The best way to edit/create the site content on a local PC is to use [VS Code](https://code.visualstudio.com), an open-source code editor from Microsoft. It has validation capabilities for YAML data files using JSON schema, and the project contains relevant schema files as well as the `.vscode` folder with relevant project settings.

List of optional VS Code extensions for comfortable work:

1. [YAML Support by Red Hat](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml) - to validate YAML data files
2. [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one) - for comfort content editing
3. [markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint) - to check content file syntax

## Content editing

The site content consists of 2 major parts: data files in the `data` folder and content files in the `content` folder. Data files use YAML format, and content files are in markdown format with YAML front-matter. The site is multilingual (currently in English and Rissian), which is supported by:

- two language version of each content file with different language code extensions like `.ru.md` and `.en.md`
- language-specific sub-sections in YAML data files

The below sections describe how to add/edit content of various types.

### Team members

Any performer mentioned in evens should be added as a team member. A team member could have an own page on the site, and could be published on the common team page, which is controlled by the `priority` field in a team member data file, see below. Create a team member data file first, then create content files, if required.

#### Team member data file

1. Go to `data/team` folder and create a file with `firstname.lastname.yaml` filename format
2. Add `priority` field; a positive value defines a position of a team member on the common team page (ordered from lowest to highest). Put a negative value to exclude a person from the team page.
3. Add a `poster` section with `small` and `big` fields for poster file names. The `small` poster is displayed on the common team page, the `big` poster is published on an individual page of a team member. The `poster` section is optional for persons with negative `priority`, see above. Poster files should be stored in `static/resources/img/team/firstname.lastname` folder of a specific team member.
4. Add `name` section with multiple language code sub-sections like `ru` and `en`. In each sub-section add `first` and `last` fields for the first name and last name respectively.
5. Add `instrument` section for the person's instrument names in different languages; names are given in fields with respective language codes like `ru` and `en`.

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

Content files are mandatory for those team members who are published on the common team page (with positive priority field, see above in the [Team member data file](#team-member-data-file) section). Content files of team members with negative priority are still used to create an individual member page, but this page is not referenced on the common team page.

1. Go to the `content/team` folder and create a content file with `firstname.lastname.ru.md` filename format for content in the Russian language, and corresponding files for other language codes.
2. Add a YAML front-matter with `title` and `description` fields, separated by lines with a triple dash, see the below example

**Team member content file example:**
`content/team/eugenia.boginskaya.en.md`

```markdown
---
title: "Eugenia Boginskaya"
description: "Eugenia Boginskaya, cello"
---
A few words from Eugenia
```

### Composers

Any composer whose compositions are used in event programs should be described in a data file in `data/composers` folder. There are no content files for composers at the moment, but they might be added in the future.

#### Composer data file

1. Go to the `data/composers` folder and create a file with `firstname.lastname.yaml` filename format.
2. Add a `name` section with multiple language code sub-sections like `ru` and `en`. In each sub-section add `first`, `last` and `full` fields for the first name, last name and full name respectively. You can also add `short` field for a short name of the composer.

**Composer data file example:**
`data/composers/alberto.ginastera.yaml`

```yaml
name:
  ru:
    first: Даниель
    last: Шнидер
    full: Даниель Шнидер
    short: Д. Шнидер
  en:
    first: Daniel
    last: Schnyder
    full: Daniel Schnyder
    short: D. Schnyder
```

### Composer's works

Any composition used in event programs should be described in a data file in `data/works` folder. Works (aka compositions) are stored in sub-folders named according to a corresponding composer's name. There are no content files for works at the moment, but they might be added in the future.

#### Work data file

1. Go to the `data/works/firstname.lastname` folder (where `firstname.lastname` belongs to the respective composer) and create a file with `opus.id.yaml` filename format, where `opus.id` is a composition identifier, so-called "opus number". Each composer has his own opus numbering, you can refer to a corresponding [Wikipedia article](https://en.wikipedia.org/wiki/Alberto_Ginastera) or to a list of works published by [International Music Score Library Project](https://imslp.org). The following points should be taken into account:
   - some composers like [Johann Sebastian Bach](https://imslp.org/wiki/List_of_works_by_Johann_Sebastian_Bach) have several opus numbering systems. In this case, just use anyone you like and use consistently. Prepend an opus number by the corresponding code of the opus numbering system *in lower case*, separated by dot, for example: `bwv.846.yaml`
   - if opus numbers have only digits and the numbering system does not have any special name, just prepend the digital opus number with "op" letters, for example, `op.22.yaml`
   - if a composer doesn't have any opus numbering system, then some meaningful ID should be created. For example, it could look like `op.1920.lbe.yaml`, where `op` is just a common prefix, `1920` is the work creation year, and `lbe` is an abbreviated work name
2. Add an `opus` field with the work opus number, which in most cases should match the data file name without `.yaml` extension. It will be used on events pages in concert programs, so it might have uppercase letters and spaces. This field is optional, so if a work doesn't have a real opus id like in the case of `op.1920.lbe.yaml` from the above example, then the `opus` field can be skipped.
3. Add a `title` section for the work's title in different languages, titles are given in fields with respective language codes like `ru` and `en`.
4. Add a `date` section. It's supposed to have different dates assoiciated with the work, but the only creation date is supported at the moment, see the below example for the details. The `date` section is optional, but once it present, the `year` field is mandatory in the `created` sub-section.

**Work data file example:**
`data/works/alberto.ginastera/op.22.yaml`

```yaml
opus: op.22
title:
  ru: 'Соната №1 для фортепиано'
  en: 'Piano Sonata No.1'
date:
  created:
    year: 1952
```

### Places

Any place for events like a concert hall etc should be described in a data file in `data/places` folder. There are no content files for places at the moment, but they might be added in the future.

#### Place data file

1. Go to the `data/places` folder, into a sub-folder of a corresponding city (`data/places/moscow`, for example. Just create a sub-folder for a city if it doesn't exist yet), and create a file with `place-name.yaml` filename format.
2. Add a `name` section for the place name in different languages, names are given in fields with respective language codes like `ru` and `en`.
3. Add an `address` section for the place address in different languages, addresses are given in fields with respective language codes like `ru` and `en`.
4. Add an optional `phones` section, which contains a list of different phone contact numbers of the place. Each element of the list has a complex structure given in the below example. It allows publishing different sets of phone numbers for various purposes, like general inquiries, ticket office etc. In the below example the only one set of phone numbers is given, for the ticket office.

**Place data file example:**
`data/places/moscow/glinks-museum.yaml`

```yaml
name:
  ru: 'Всероссийское музейное объединение музыкальной культуры имени М.И. Глинки'
  en: 'The Glinka National Museum Consortium of Musical Culture'
address:
  ru: 'Москва, ул. Фадеева, д.4 (метро «Маяковская», «Новослободская»)'
  en: '4 Fadeeva st, Moscow'
phones:
  - desc:
      ru: 'Заказ билетов'
      en: 'Ticket office'
    numbers:
      - url: '+74957396226'
        disp: '(495) 739-62-26'
      - url: '+74957393987'
        disp: '(495) 739-39-87'
```

### Events

Any event like concert etc should be described in a data file `data/events/YYYY-MM-DD/event-name.yaml` and in content files for different languages in `content/events/YYYY-MM-DD` folder. Create an event data file first, then create content files with the event title and description.

#### Event data file

The event data file contains general information about the event like date of the event and a reference to the place of the event. It also has a list of performers of the event and list of works in the concert programme.

The event programme is printed on the event's page in two places:

 * as the list of composers (the full names of composers are used)
 * as the list of works (short names of composers are used where available)

1. Go to the `data/events` folder, and create a sub-folder of a corresponding date in `YYYY-MM-DD` format (`data/events/2016-04-13`, for example), and then create a file with `event-name.yaml` filename format, where `event-name` is a short name of the event in lowercase.
2. Add a `datetime` field of the event date and time. The format for the field should be as in the below example.
3. Add a `poster` section with `small` and `big` fields for poster file names. A `small` poster is displayed on the common events page, a `big` poster on an individual page of an event. `poster` section is mandatory for any event. Poster files should be stored in `static/resources/img/events/YYYY-MM-DD/event-name/` folder of the event.
4. Add a `place` section with a reference to a place of the event. It has 2 fields: `city` and `hall`, which should correspond to the folder and the file name of the place, see the above description in the [Place data file](#place-data-file) section.
5. Add an optional `timepad` field with the event ID from timepad.ru to add the "Buy tickets" button
6. Add an optional `performers` section with the list of performance short names, which should match file names in `data/team` folder
7. Add an optional `programme` section for concert programme items. Each item has two fields: `composer` and `work`, which should correspond to a folder and a file name of composition, see the above description in the [Work data file](#work-data-file) section.

**Event data file example:**
`data/events/2016-05-18/3x3.yaml`

```yaml
datetime: 2016-05-18T19:00:00
poster:
  small: '3x3-800.jpeg'
  big: '3x3-800.jpeg'
place:
  city: moscow
  hall: glinka-museum
performers:
  - natalia.zhukova
  - valeria.esaulenko
  - eugenia.boginskaya
programme:
  - composer: sergey.prokofiev
    work: op.82
  - composer: sergey.prokofiev
    work: op.94
  - composer: alberto.ginastera
    work: op.22
  - composer: bohuslav.martinu
    work: h.300
```

#### Event content file

Content files are mandatory for events; they have an event title, a short and a detailed description of the event.

1. Go to the `content/events` folder and create a create a sub-folder of a corresponding date in `YYYY-MM-DD` format (`content/events/2016-04-13`, for example). In that sub-folder create a content file with `event-name.ru.md` filename format for content in the Russian language and corresponding files for other language codes.
2. Add a YAML front-matter with `title` and `description` fields, separated by lines with the triple dash, see the below example

**Team member content file example:**
`content/events/2016-05-18/3x3.en.md`

```markdown
---
title: "3✕3"
description: "3✕3 - 3 composers, 3 sonatas, and trio"
---
Concert dedicated to the anniversary of Sergey Prokofiev
```