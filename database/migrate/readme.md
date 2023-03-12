# Migration

***there are several key concepts and techniques to learn when it comes to database migrations:***

## create migration

    php artisan make:migration name

## Running Migrations

    php artisan migrate

## Rolling Back Migrations

    php artisan migrate:rollback

## Modifying Tables

You can use migrations to modify existing database tables, such as adding or removing columns or indexes

## Seeding Data

In addition to defining schema changes, migrations can also be used to seed data into your database. Laravel provides a built-in Seeder class that you can use to populate your database with sample data

## Foreign Key Constraints

You can use migrations to define foreign key constraints between tables, which can help ensure data integrity.

## Handling Errors

If a migration encounters an error, Laravel provides mechanisms for handling and reporting those errors, such as rolling back the migration and displaying an error message.


