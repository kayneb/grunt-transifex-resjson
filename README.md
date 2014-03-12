# grunt-transifex-resjson

Grunt tasks for managing RESJSON resource files in [Transifex](https://www.transifex.com/). 

These tasks have been developed to interact with the Transifex API in a Windows Store Javascript app project where RESJSON resource file are used for handling different translations. 

See the [MSISDN](http://msdn.microsoft.com/en-us/library/windows/apps/hh465254.aspx) for details about using RESJSON in Javascript apps.

## Getting Started
This plugin requires Grunt `~0.4.3`

You may install this plugin with this command:

```shell
npm install grunt-transifex-resjson --save
```

Once the plugin has been installed, it may be enabled and configured inside your `Gruntfile.js`.

Enable the tasks:

```js
grunt.loadNpmTasks("grunt-transifex-resjson");
```

Define the configuration file name in `initConfig`:

```js
    grunt.initConfig({
	...
        "transifex-resjson": {
            transifex_resjson_config: "project-tx-config-file.resjson"
        }
	...
	}
```

And finally setup the configuration file as described in the following section.

### <span id="Configuration file">Configuration file</span>

The tasks provided in `grunt-transifex-resjson` read configuration information from a file located at the root of your Grunt project. The name  of the config file is defined in the `transifex_resjson_config` -property described above.

The config file contains your Transifex project info for accessing the [Transifex API](http://support.transifex.com/customer/portal/articles/995872-overview) and information regarding the local project file structure in order to locate the resources files. The configuration is defined in  [Relaxed JSON](http://oleg.fi/relaxed-json/) format, so comments are allowed. Below is a sample configuration file:

```js
{
  /*
	Settings for Transifex API credentials and Transifex project specific info.
  */
  transifex: {
    /*
        URL for the Transifex API
    */
    api: "https://www.transifex.com/api/2",

    /*
        Authentication information for the API
    */
    auth: {
      user: "yourtransifexuser",
      pass: "anditspassword",
    },

    projectSlug: "transifex-projectslug",

    /*
	   List of Transifex users used as coordinators for languages created 
       using the tasks
	*/
    langCoordinators: ["user1", "user2"],
    /* 
	  The slug name for the main resources file, the slug must match the
     local file name without the .json file extension.
	*/

    mainResourceSlug: "resources",

    /*
	 Source language code used in Transifex project. The language code
     is in Transifex format.
	*/
	sourceLanguage: "en_US",
  },
  /*
	 Settings for your local project 
  */
  localProject: {

    /*
	   Directory in your local project structure containing
       RESJSON files 
	*/ 
    stringsPath: "src/strings",

    /*
	   Directory containing the resource files containing the 
       source language strings
	*/
    sourceLangStringsPath: "src/strings/en-US",

    /*
	   File containing additional comments (copyrights, instructions, etc.) you wish to concatenate to your resources files that
       are received from Transifex.
	*/
    commentPreambleFile: "src/strings comment-preamble.resource"
  }
}
```

### Provided Grunt Tasks and Usage

The `grunt-transifex-resjson` module provides the following Grunt tasks for interacting with your Transifex repository:

- [tx-project-resources](#tx-project-resources)
- [tx-pull-translations](#tx-pull-translations)
- [tx-push-resources](#tx-push-resources)
- [tx-create-translation-language](#tx-create-translation-language)
- [tx-add-resource](#tx-add-resource)
- [tx-add-instruction](#tx-add-instruction)


#### <span id="tx-project-resources">tx-project-resources</span>


##### Description

Returns a list of project resources from Transifex.


##### Usage

```js
grunt tx-project-resources
```

#### <span id="tx-create-translation-language">tx-create-translation-language</span>

##### Description

Add a new language for translation in Transifex project. Takes language code or the string `all` as argument. `all` tries to create a language for translation in Transifex for all languages found under the `stringsPath`.

The language codes used as arguments are those used in the local project as directory names, i.e. **not** Transifex language codes.

##### Usage

```js
grunt tx-create-translation-language:jp-JP 
```

Or provision all languages at once.

```js
grunt tx-create-translation-language:all
```



#### <span id="tx-add-resource">tx-add-resource</span>

##### Description

Creates a new resource in Transifex. The basename of the file is used as the slug name in Transifex. The optional `--name` option can be used to give the file a more descriptive name used in the Transifex UI.

##### Usage

```js
grunt tx-add-resource --file <basename-for-resource.resjson> --name="Display name in Transifex"
```


#### <span id="tx-push-resources">tx-push-resources</span>

##### Description

Pushes `*.resjson` resource files under the `sourceLangStringsPath` to Transifex project labeled with `projectSlug`.

The files are expected to be created in the Transifex project prior pushing.

##### Usage

```js
grunt tx-push-resources
```

#### <span id="tx-pull-translations">tx-pull-translations</span>

##### Description


Retrieves translations from Transifex project. The task can be given a list of language codes as argument to limit which translations are downloaded. Only translations that are marked as reviewed are downloaded, non-reviewed strings are returned identical to the original strings.

The language codes used as arguments are those used in the local project as directory names, i.e. **not** Transifex language codes.


##### Usage

```js
grunt tx-pull-translations
```

or to download specific translations:

```js
grunt tx-pull-translations:fi-FI, es-ES
```


#### <span id="tx-push-translations">tx-push-translations</span>

##### Description

Upload local translation files to Transifex. The task can be given a list of language codes as argument to limit which translations are uploaded.

The language codes used as arguments are those used in the local project as directory names, i.e. **not** Transifex language codes.


##### Usage


```js
grunt tx-push-translations
```

or to upload specific translations:

```js
grunt tx-push-translations:fi-FI, es-ES
```


#### <span id="tx-add-instruction">tx-add-instruction</span>

##### Description

Includes detailed instructions in addition to the comments in resources file for translators for a translation key. The detailed instruction can include HTML markup for easier readability in Transifex. The detailed instruction is not part of the data read back from Transifex with `tx-pull-translations`.

##### Usage

```sh
grunt tx-add-instruction --key='<resource-key>' --comment='<comment-str>'
```

e.g.

```sh
grunt tx-add-instruction --key='my.little.resource' --comment='<strong>Important!</strong> Comment can include HTML markup and <a href="http://www.google.com">links</a>'
```

## License

Licensed under the MIT license.

## Release History

0.1.0 Initial version.
