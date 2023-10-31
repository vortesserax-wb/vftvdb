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

* • tt0050083 is the unique identifier for the movie “12 Angry Men (1957)”, where tt signifies that it’s a title entity and 0050083 uniquely indicates “12 Angry Men (1957)”.
* • nm0000020 is the unique identifier for the actor “Henry Fonda”, where nm signifies that it’s a name entity and 0000020 uniquely indicates “Henry Fonda”.
