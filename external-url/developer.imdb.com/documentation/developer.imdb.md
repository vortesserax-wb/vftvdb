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

{
 "awards": &#91;
 {
 "year": 1958,
 "awardName": "Golden Globe",
 "category": "Best Actor - Drama",
 "winner": false,
 "event": "Golden Globes, USA"
 },
 ...
 &#93;
}

### filmography

### knownFor
