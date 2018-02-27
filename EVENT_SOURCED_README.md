# Developing Event Sourced Neos Content Repository (README)

## Getting Started

**PREREQUISITES**: You need Mariadb at least 10.2.2; as we need recursive CTEs!

- checkout [neos/neos-development-collection](https://github.com/neos/neos-development-collection/) with branch `event-sourced`
- run `composer update` in the checked out folder.
  - This will check out the `neos-development-collection` on the `event-sourced`-branch
  - it will check out the `Neos.EventSourcing` base package


## Running Behat tests

- run `./flow behat:setup`

- add configuration in `Configuration/Testing/Behat/Settings.yaml`:

    ```
    Neos:
      Flow:
        persistence:
          backendOptions: &backendOptions
            driver: pdo_mysql
            host: 127.0.0.1
            dbname: neos-eventsourced-behat
            user: root
            password: null
            charset: utf8
      ContentGraph:
        DoctrineDbalAdapter:
          persistence:
            backendOptions: *backendOptions
    ```

    Adjust the DB name etc as needed.

- Set up the event store using `FLOW_CONTEXT=Testing/Behat ./flow eventstore:setupall`

- Now, you can run behat tests using `fbehattest Tests/Behavior/behat.yml.dist Tests/Behavior/Features/ --tags ~skip`
- alternatively, use `bin/behat -c Packages/Neos/Neos.ContentRepository/Tests/Behavior/behat.yml.dist Packages/Neos/Neos.ContentRepository/Tests/Behavior/Features/ --tags ~skip`


## Migrate Data from old UI

- configure `Settings.yaml`
- copy an existing Neos master DB
- `./flow doctrine:migrate`
- `./flow eventstore:setupall`
- `./flow contentrepositorymigrate:run`
