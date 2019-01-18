# ConnectedHumber-Air-Quality-Interface

> The web interface and JSON api for the ConnectedHumber Air Quality Monitoring Project.

This project contains the web interface for the ConnectedHumber air Quality Monitoring system. It is composed of 2 parts:

 - A PHP-based JSON API server (entry point: api.php) that's backed by a MariaDB server
 - A Javascript client application that runs in the browser

The client-side browser application is powered by [Leaflet](https://leafletjs.com/).

Note that this project is _not_ responsible for entering data into the database. This project's purpose is simply to display the data.

## System Requirements
In order to run this program, you'll need the following:

 - Git
 - Bash (if on Windows, try [Git Bash](https://gitforwindows.org/)) - the build script is written in Bash
 - [composer](https://getcomposer.org/) - For the server-side packages
 - [Node.JS](https://nodejs.org/)
 - [npm](https://npmjs.org/) - comes with Node.JS - used for building the client-side code
 - A [MariaDB](https://mariadb.com/) server with a database already setup with the schema data in it. Please get in contact with [ConnectedHumber](https://connectedhumber.org/) for information about the database schema and structure.

## Getting Started
The client-side code requires building. Currently, no pre-built versions are available (though these can be provided upon request), so this must be done from source. A build script is available, however, which automates the process - as explained below.

### Building From Source
The build script ensures that everything it does will not go outside the current directory (i.e. all dependencies are installed locally).

To build from source, start off by running the `setup` and `setup-dev` build commands like this:

```bash
./build setup setup-dev
```

This will initialise any git submodules and install both the server-side and client-side dependencies. Once done, all you need to do is build the client-side code:

```bash
./build client
```

For development purposes, the `client-watch` command is available.

### Configuration
Some configuration must be done before the application is ready for use. The first time `api.php` is called from a browser, it will create a new blank configuration file at `data/settings.toml`, if it doesn't already exist. See the `settings.default.toml` file in this repository for a list of configurable settings, but do **not** edit `settings.default.toml`! Instead, enter your configuration details into `data/settings.toml`, which overrides `settings.default.toml`. In particular, you'll probably want to change the settings under the `[database]` header - but ensure you give the entire file a careful read.

## API
The server-side API is accessed through `api.php`, and supports a number of GET parameters. The most important of these is the `action` parameter, Which determines what the API will do. The following values are supported:

### fetch-data
> Fetches air quality data from the system for a specific data type at a specific date and time.

Parameter			| Type		| Meaning
--------------------|-----------|---------------------
`datetime`			| date/time	| Required. Specifies the date and time for which readings are desired.
`reading_type`		| string	| Required. Specifies the type of reading desired.

Examples:

```
https://example.com/path/to/api.php?action=fetch-data&datetime=2019-01-03%2007:52:10&reading_type=PM10
```

### list-devices
> Fetches a list of devices currently in the system.

Parameter			| Type		| Meaning
--------------------|-----------|---------------------
`only-with-location`| bool		| Optional. If present only devices with a defined location will be returned. Useful for getting a list of devices to place on a map.

Examples:

```
https://example.com/path/to/api.php?action=list-devices
https://example.com/path/to/api.php?action=list-devices&only-with-location=yes
```

### device-info
> Gets (lots of) information about a single device.

Parameter			| Type		| Meaning
--------------------|-----------|---------------------
`device-id`         | int		| Required. The id of the device to get extended information for. See the `list-device` action for how to get a hold of one.

Examples:

### list-reading-types
> Lists the different types of readings that can be specified.

_No parameters are currently supported by this action._

```
https://example.com/path/to/api.php?action=list-reading-types
```

## Notes
 - Readings are taken every 6 minutes as standard.

## Contributing
Contributions are welcome - feel free to [open an issue](https://github.com/ConnectedHumber/Air-Quality-Web/issues/new) or (even better) a [pull request](https://github.com/ConnectedHumber/Air-Quality-Web/compare).

The [issue tracker](https://github.com/ConnectedHumber/Air-Quality-Web/issues) is the place where all the tasks relating to the project are kept.

## License
This project is licensed under the _Mozilla Public License 2.0_. The full text of this license can be found in the [LICENSE](https://github.com/ConnectedHumber/Air-Quality-Web/blob/master/LICENSE) file of this repository, along with a helpful summary of what you can and can't do provided by GitHub.
