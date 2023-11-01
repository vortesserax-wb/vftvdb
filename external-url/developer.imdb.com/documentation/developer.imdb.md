# Documentation & Data Dictionary:
# IMDb and Box Office Mojo

## IMDb Developer


## Documentation & Data Dictionary: IMDb and Box Office Mojo

Access IMDb’s metadata for every movie, TV series and video game title as well as performers and creators, along with full lifetime box office grosses from IMDbPro’s Box Office Mojo. Our bulk data access products help entertainment fans share their passion with the world, including IMDb’s 1-10 star rating, a daily-computed average of votes from IMDb’s global audience of over 250 million fans.

Our full range of bulk data products are delivered through Amazon Data Exchange and updated daily. This document explains schema and documentation for all IMDb content available in bulk via Amazon Data Exchange. Data types are packaged across several different data products ranging from IMDb’s world-renowned 1-10 star rating, to full industry Box Office grosses from IMDbPro’s Box Office Mojo. If you have any questions on our documentation, or would like to access a free sample of our data products, please get in touch via developer.imdb.com.


## Getting Support

For technical support or licensing questions please email imdb-licensing@imdb.com.


## Key Concepts

### IMDb IDs

IMDb uses unique identifiers for each of the entities referenced in IMDb data. For example we have “Name IDs” identifying name entities (people) and “Title IDs” identifying title entities (movies, series, episodes and video games). IMDb’s identifiers always take the form of two letters, which signify the type of entity being identified, followed by a sequence of at least seven numbers that uniquely identify a specific entity of that type. For example:

* tt0050083 is the unique identifier for the movie “12 Angry Men (1957)”, where tt signifies that it’s a title entity and 0050083 uniquely indicates “12 Angry Men (1957)”.
* nm0000020 is the unique identifier for the actor “Henry Fonda”, where nm signifies that it’s a name entity and 0000020 uniquely indicates “Henry Fonda”.

Within the data set, each entry relates to a single IMDb identifier.

### JSON Lines File Format

IMDb’s data set is provided in JSON Lines file format. The files are UTF-8 encoded text files, where each line in the file is a valid JSON string. Each JSON document, one per line, relates to a single entity, uniquely identified by an IMDb ID. We also provide a JSON schema that documents the format that is used for each JSON document within the file.

### Versioning

Every published revision of IMDb’s data set contains data file(s), documentation for that data, and a schema which validates that data. Each of these is associated with a correlated version number, which can be found at the end of their filenames.

At any time we may change the format of new data set revisions and their accompanying schema, but previously published data set revisions will remain unchanged. If data from a new revision of the data set is not compatible with the previous schema (i.e. a breaking change) then we will increment the version number for the data files, schema, and documentation. In this case we will publish both formats of the data set for some period of time before we stop publishing the older one. The data set format and schema may change without incrementing the schema version number if the change is compatible with previous revisions (i.e. a non-breaking change).

The following are examples of non-breaking changes to the schema:

* Adding a new key anywhere in the structure.
* Removing an optional key.
* Changing a key from optional to required.
* Changing the validation rules for a specific key such that all values still validate against previous validation rules.

The following are examples of breaking changes to the schema:

* Changing a key from required to optional.
* Removing a required key.
* Changing the validation rules for a specific key such that newly published values may exist that do not validate against previous validation rules.

### Data Structure Conventions

There are some conventions you should be aware of when using IMDb’s data set:

* There are no null values in the data set. If we do not have a value for a particular key we omit publishing that key. Keys which are required by the schema will never have a null value.
* There are no empty objects in the data set. If an object would have contained no keys we omit publishing that object.
* There are no empty arrays in the data set. If an array value would have contained no items we omit publishing the corresponding key.

### Changes to Entities and Resolving IDs

#### Duplicate IDs

IMDb’s data set is constantly being updated, adding more data and improving the quality of the data we have. While there is only ever one entry per IMDb ID, we sometimes find that we have duplicate IMDb IDs for an entity within our system. For example, we may learn that two people we have iden-tified separately are actually the same person. When this happens, we maintain the data associated with both identifiers in the data set, duplicating the data. This allows you to continue using any match-ing you have between IMDb identifiers and other identifiers. To identify when this is the case we in-clude a remappedTo field on one of the copies which gives you the new preferred identifier for that entity.

#### Deleted IDs

Sometimes we delete entities from the data set. The most prominent example of this is the deletion of titles that have been canceled during development and will therefore never be released. When we delete an entity it is no longer included in the data set. The identifier associated with it is never reused for a different entity.

### Data Consistency Model

IMDb’s data is constantly being expanded and updated, and it can take seconds or minutes for a change to propagate throughout the entire catalog. This means that the snapshot of IMDb’s data published may contain temporary inconsistencies. For example, it is possible that we report an ac-tor appearing in a title in their filmography, but it has not yet propagated to that title’s credits. Each individual inconsistency will be resolved in the next published revision of the data set.

### Linking to IMDb

IMDb’s data contains URLs that you can use to link back to the IMDb website in any experience you build for your users. Your license may require you to attach a “refmarker” to the end of the URL. The “refmarker” is a special sequence of characters that we use to identify the source of our traffic. Add the “refmarker” to the URL by appending ?ref_=xx_xxx_x to the URL, where xx_xxx_x is replaced by the code we have provided to you. A full URL could look something like https://www.imdb.com/title/tt0050083/?ref_=my_ref_marker.


## Data Dictionary - Name Essential

### nameId

The unique IMDb ID for the name in question. Each IMDb ID appears exactly once.

### name

The primary name by which this person is known, usually the one by which they are most often cred-ited. For more information about how IMDb defines the primary name see IMDb’s help site.

### awards

A list of awards that this person has won or been nominated for. This includes the name and category of the award, the name and year of the award event, and whether the person won the award. Note that ‘winner’ may be false because the person is known not to have won the award (where the awards event occured in the past) or because the winner is not yet known (where the awards event occurs in the future, but the nominations have been announced). If ‘winner’ is true that means the awards event has already occured, and the person won the award.

#### Example

```
{
  "awards": [
    {
      "year": 1958,
      "awardName": "Golden Globe",
      "category": "Best Actor - Drama",
      "winner": false,
      "event": "Golden Globes, USA"
    },
    ...
  ]
}
```

### filmography

The filmography for this name as a list of credits. Each credit is within a “category” such as “ac- tress”, “director” or “editorial_department”. For cast categories (e.g. “actor”), we include the roles that the person played and the billing they had in the end credits (if available). For crew categories (e.g. “writer”) we include the more specific “jobs” that the person was credited with if applicable. Lists of credits, roles, and jobs are each in on-screen credits order.

Credits can have a list of attributes. At the moment we provide the following attributes:

* “uncredited”: signals that while the person performed this role on the title, they were not present in the title’s end credits.
* “voice”: signals that this person provided a voice only performance for this title.

Additional information about these attributes can be found on IMDb’s help site.

For episodic credits we only include the series in the list. To get full information on which episodes were worked on, look at the episode credits in the title file.

#### Example

A cast credit

```
{
  "filmography": [
    {
      "titleId": "tt0050083",
      "category": "actor",
      "billing": "8",
      "roles": ["Juror 8"]
    },
    ...
  ]
}
```

A crew credit

```
{
  "filmography": [
    {
      "titleId": "tt0052462",
      "category": "producer",
      "jobs": ["executive producer"]
    },
    ...
  ]
}
```

An uncredited credit

```
{
  "filmography": [
    {
      "titleId": "tt0050083",
      "category": "actor",
      "roles": ["Judge"],
      "attributes": ["uncredited"]
    },
    ...
  ]
}
```

### knownFor

A short list of IMDb title IDs for the titles in which this person is most well known for being involved, and the category of job that they had on that title (e.g. “actor” or “director”). This is always a subset of filmography but the selection and order is determined by IMDb. For more details see IMDb’s help site. For further details on their involvement see the filmography entry, or the creditsByCat- egory entry on the title in question.

#### Example

```
{
  "knownFor": [
    {
      "titleId": "tt0050083",
      "category": "actor"
    },
    ...
  ]
}
```


## Data Dictionary - Title Essential

### titleId

The unique IMDb ID for the title in question. Each IMDb ID appears exactly once.

### remappedTo

If there are duplicate title entities for this title, the IMDb ID of the primary title entity for this title. See “Duplicate IDs” in the “Key Concepts” section of this document for more information.

### originalTitle

The original title text of the title, normally what the title is known as in its original country of release.

### akas

A list of alternative title texts by which this title is also known. Each title is listed with additional in- formation about the usage of that title text, e.g. what region it is from, and what language it is used in.

#### Example

```
{
  "akas": [
    {
      "title": "12 Homens e uma Sentença",
      "region": "BR"
    },
    ...
  ]
}
```

### awards

A list of awards that this title has won or been nominated for. This includes the name and category of the award, the name and year of the award event, and whether the title won the award. Note that ‘winner’ may be false because the title is known not to have won the award (where the awards event occured in the past) or because the winner is not yet known (where the awards event occurs in the future, but the nominations have been announced). If ‘winner’ is true that means the awards event has already occured, and the title won the award.

#### Example

```
{
  "awards": [
    {
      "awardName": "Oscar",
      "category": "Best Picture",
      "event": "Academy Awards, USA",
      "winner": false,
      "year": 1958
    },
    ...
  ]
}
```

### creditsByCategory

The credits for this title organized by category. Each entry in this list represents a single category and gives you a list of credits within that category. For cast credits we include the roles that the person played and the billing they had in the end credits (if available). For crew credits we include the more specific “jobs” that the person was credited with if applicable. Lists of credits, roles, and jobs are each in on-screen credits order.

Credits can have a list of attributes. At the moment we provide the following attributes:

* “uncredited”: signals that while the person performed this role on the title, they were not present in the title’s end credits.
* “voice”: signals that this person provided a voice only performance for this title.

Additional information about these attributes can be found on IMDb’s help site.

For series we include anyone who is credited on any episode. To get full information on which episodes were worked on by a specific person, look at the episode credits.

#### Example

A crew category

```
{
  "creditsByCategory": [
    {
      "category": "sound_department",
      "credits": [
        {
          "nameId": "nm0322302",
          "jobs": ["sound"]
        },
        {
          "nameId": "nm0334505",
          "jobs": ["re-recording mixer"],
          "attributes": ["uncredited"]
        }
      ]
    },
    ...
  ]
}
```

A cast category

```
{
  "creditsByCategory": [
    {
      "category": "cast",
      "credits": [
        {
          "nameId": "nm0000842",
          "roles": ["Juror 1"],
          "billing": 1
        },
        {
          "nameId": "nm0275835",
          "roles": ["Juror 2"],
          "billing": 2
        },
        ...
      ]
    },
    ...
  ]
}
```

### principalCastMembers

A short list of the most important cast credits for this title. This is always a subset of the cast from the creditsByCategory list, but the selection and order is determined by IMDb. Often it is similar to top-billed cast but it can be different, for example if the title credits are in order of appearance or alphabetical. For more details see IMDb’s help site. Also includes the role or roles played (in on-screen credits order) and the billing in the full cast list.

#### Example

```
{
  "principalCastMembers": [
    {
      "nameId": "nm0000020",
      "category": "actor",
      "roles": ["Juror 8"],
      "billing": "8"
    },
    ...
  ]
}
```

### principalCrewMembers

A short list of the most important crew credits for this title. This is always a subset of the crew from the creditsByCategory list, but the selection and order is determined by IMDb. Also includes the category and job which qualified the credit for this list.

#### Example

```
{
  "principalCrewMembers": [
    {
      "nameId": "nm0741627",
      "category": "writer",
      "job": "story"
    },
    ...
  ]
}
```

### certificates

A list of content rating certifications that have been given to a title, and the region where the rating applies or applied. For example a title may be given a ‘PG-13’ rating in the ‘US’ region (by the MPAA). There may be additional attributes about the certificate or reasons for the rating provided by the rating organization.

#### Example

```
{
  "certificates": [
    {
      "region": "US",
      "rating": "TV-PG"
    },
    ...
  ]
}
```

### companies

Lists of the names of distribution, production, special-effects, and other miscellaneous companies as- sociated with the making or subsequent distribution of this title. This list includes all companies that have ever been involved with the title, even if their involvement has now ended. Additional informa- tion about companies associated with titles can be found on IMDb’s help site. These are ordered by on-screen credit order, or in the case of distribution companies by distribution release date.

#### Example

```
{
  "companies": {
    "distribution": ["United Artists", "CBS/Fox", "Warner Home Video"],
    "production": ["Orion-Nova Productions"],
    "miscellaneous": [
      "International Alliance of Theatrical Stage Employees (IATSE)",
      "Solters & Digney"
    ]
  }
}
```

### countries

A list of ISO 3166 country codes for the countries in which the production companies for the title are based. For more details see IMDb’s help site.

### episodeInfo

For titles that are episodes, this contains information about the series, such as the series title ID, sea- son number and episode number. It also includes the season and episode numbers where relevant.

#### Example

```
{
  "episodeInfo":
    {
      "seriesTitleId": "tt0944947",
      "episodeNumber": 1,
      "seasonNumber": 8
    }
}
```

### seriesInfo

For titles that are series, this contains additional information about the series, such as the year it started airing, the year it finished airing (if it has finished), and a list of all the episode title IDs in the series ordered by episode number (e.g. season 1 episode 1, season 1 episode 2, etc.).

#### Example

```
{
  "seriesInfo": {
    "startYear": 2011,
    "endYear": 2019,
    "episodeTitleIds": [
        "tt1480055",
        "tt1668746",
        "tt1829962"
        ...
    ]
  }
}
```

### episodeTitleIds

For titles which are series, the IMDb title IDs for all the episodes of that series.

### officialSiteLinks

A list of URLs (and optionally their link titles) linking to this title’s official website.

#### Example

```
{
  "officialSiteLinks": [
    {
      "url": "www.example.com/official/example-title",
      "linkTitle": "Example official website for title"
    },
    ...
  ]
}
```

### genres

A list of genres to which this title belongs. The full list of allowed genres and guidelines for how titles should be categorized can be found on IMDb’s help site. IMDb defines a limited list of genres but may add more in the future.

### image

A URL linking to an image associated with this title, such as a movie poster or still frame. Additionally includes the width and height of the image in pixels.

#### Example

```
{
  "image": {
    "url": "https://m.media-
     ↪  amazon.com/images/M/MV5BMWU4N2FjNzYtNTVkNC00NzQ0LTg0MjAtYTJlMjFhNGUxZDFmXkEyXkFqcGdeQXVyNjc1NTYyMjg@._V1_.jpg",
    "height": 1500,
    "width": 974
  }
}
```

### imdbUrl

A full URL to see the name or title on www.imdb.com.

### isAdult

Whether or not this title contains adult content. Useful if you would like to filter out all adult content from your copy of the data set.

### keywords

A list of keywords associated with the title. More information about keywords and guidance for how they might be associated with at title can be found on IMDb’s help site.

### languages

A list of ISO 639 language codes for the languages spoken in this title, in order of frequency that they are spoken in the title. For more details see IMDb’s help site.

### locations

A list of locations where scenes from this title were filmed and optionally names or descriptions of the scenes which used that location.

#### Example

```
{
  "locations": [
    {
      "scenes": ["studio"],
      "place": "Fox Movietone Studio, New York, USA"
    },
    ...
  ]
}
```

### movieConnections

A list of IMDb title IDs of other titles which have a connection to this title, and the type of connection, for example titles which reference or spoof this title. Optionally may include a description of the con- nection. A complete list of current connection types can be found on IMDb’s help site, although more may be added in future.

#### Example

```
{
  "movieConnections": [
    {
      "type": "referenced_in",
      "titleId": "tt2336547",
      "text": "Jack criticizes the film for depicting 11 Americans being
       ↪  swayed by Jane Fonda's father"
    },
    ...
  ]
}
```

### plot

A plot description of this title. Most plot descriptions will be just a couple of sentences long, however some may be longer, the ‘plot’ will contain the shortest of ‘plotShort’, ‘plotMedium’ or ‘plotLong’ and may be omitted. If you are displaying these plots you may need to consider truncation on longer plots.

### plotShort

A plot outline of this title, no longer than 239 characters. Plot outlines never contain spoilers.

### plotMedium

A plot summary of this title. Most plot summaries will be reasonably brief, a paragraph or two. If there are multiple plot summaries available on this title’s Plot page on IMDb.com, then the one provided here will have been selected to display prominently on the title’s main page by our users or manual vetting team.

### plotLong

A synopsis of this title. A long detailed description of the entire plot of the title.

### releaseDates

A list of the release dates (ISO 8601 date format) for this title, together with the region (an ISO 3166 country code) to which each release date applies.

Note that each release date may specify year, month and day (e.g. 1979-08-16), year and month (e.g. 1979-08) or only year (e.g. 1979).

#### Example

```
{
  "releaseDates": [
    {
      "date": "1957-04",
      "region": "GB"
    },
    {
      "date": "1957-04-10",
      "region": "US"
    },
    {
      "date": "2016-02-24",
      "region": "CZ"
    },
    ...
  ]
}
```

### productionStatus

A list of production statuses for this title in ascending order by date, with the last status being the current production status. The available statuses for in-production listings are available on IMDb’s help site.

```
{
  "productionStatus": [
    ...{
      "updated": "2008-12-02",
      "status": "pre production"
    },
    {
      "updated": "2008-12-02",
      "status": "pre production"
    },
    {
      "updated": "2008-12-02",
      "status": "pre production"
    }
  ]
}
```

### runtimeMinutes

### taglines

### titleType

### imdbRating

#### Example

```
```

### year


## Data Dictionary - Box Office

### boxoffice_title_budgets_v1

#### budgetItemType

#### amount

### boxoffice_title_grosses_v1

#### area

#### grossToDate

#### rank

### boxoffice_title_opening_weekends_v1

#### area

#### startDate

#### endDate

#### occasionId

#### gross

#### numTheaters

### boxoffice_title_grosses_timeseries_v1

#### area

#### startDate

#### endDate

#### occasionId

#### gross

#### grossToDate

#### numTheaters

#### rank

## Querying Data in Amazon Athena Tables

### What is Amazon Athena?

### Getting Started

### What Are the Highest-Rated Movies On IMDb?

### What Are the Title Texts for the Titles That a Person Is Known For?

### What Are the Names of the Principal Cast for a Title?

### What Awards was a Title Nominated for, and who were the Award Nominees?

### What Are the Title Texts for Episodes of a Series?

### What Is the Title Text of the Series for an Episode Title?


## Appendix 1: Box Office Mojo Areas

### Area Rollups and Special Areas

### Individual Areas
