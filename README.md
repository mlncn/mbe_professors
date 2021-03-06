# Drupal 8 Migrations by Example

A demo module created by Mauricio Dinarte (@dinarcon) to explain migrations concepts in Drupal 8.

http://bit.ly/migrations-by-example

## Dependencies

The following projects are required to run this demo. The version number indicates which version was last used for testing.
 
* Drupal 8.5.1
* Address 1.3
* Entity reference revisions 1.3
* Migrate plus 4.0
* Migrate source csv 2.1
* Migrate tools 4.0
* Paragraphs 1.2

## CSV files location

This demo assumes that the CSV files provided in the `sources` folder are moved to a `migrate` folder in the same level where the Druapl configuration lives.

This can be accomplished starting a project using the https://github.com/drupal-composer/drupal-project template and creating `migrate` folder. This would look similar to:

```
.
|-- composer.json
|-- composer.lock
|-- config
|-- migrate
|-- vendor
`-- web
``` 

If you want to place the files in a different location, you need to update the path in the corresponding configuration files.
## Examples

This demo includes 3 migration configurations.

* mbe_book_paragraph for importing data into paragraphs entities. There are no dependencies on other migrations.
* mbe_photo_field for importing data into file entities. There are no dependencies on other migrations.
* mbe_professors for importing data into node entities. This depend on other migrations to be run before: mbe_book_paragraph and mbe_photo_field.

## Instructions

* Install dependencies (composer command: `composer require drupal/paragraphs drupal/address drupal/migrate_plus drupal/migrate_source_csv drupal/migrate_tools`)
* Download demo into /modules/custom directory.
* Enable module.
* Run migrations using the UI or drush.

### Running the migrations

The Migrate Plus module provides a UI and drush commands to run the migrations.

The UI is located at /admin/structure/migrate. Alternatively, the following drush commands can be used:

```
drush migrate:import mbe_book_paragraph
drush migrate:import mbe_photo_field
drush migrate:import mbe_professors
```

After the migrations are run successfully, visit /professors to see the content that was imported.

### Gotcha

There is an issue when rolling back a migration that depends on another one that interacts with entity revisions. This affects paragraphs migrations.

For this particular demo, if the mbe_professors is rolled back and later imported again, the book paragraphs will not be associated with the nodes.

To fix this, the dependent migration mbe_book_paragraph also needs to be rolled back and migrated again. This can be done via the UI or with the following drush commands:

```
drush migrate:rollback mbe_professors
drush migrate:rollback mbe_book_paragraph
drush migrate:import mbe_book_paragraph
drush migrate:import mbe_professors
```
