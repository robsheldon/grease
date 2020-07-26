## Have Phriction? You need Grease!

Grease can safely and easily retrieve your Remarkup documents from your Phabricator server. It is a single-file shell script with few dependencies, a complete help screen (grease --help), and a ton of error handling and carefully written error messages. If something doesn't work right, it will try to point you in the right direction.


## Dependencies

* curl, used to connect to the Phabricator server and get a respone.
* [jq](https://github.com/stedolan/jq), for parsing the response from the server.


## Compatibility

This should run okay on most Unix-like operating systems. It's been tested on Debian. I tried to avoid platform-specific binaries. It probably requires a bash shell, but may run in other shells. If you have trouble running it on a Unix-like operating system or on another shell *and* you can offer a fix that probably won't break it for other users, please open an issue and let me know.


## Status

At this point, grease does everything I want it to do and limited testing hasn't uncovered anything that needs fixing. No further features are planned. Grease is "temporarily completed".


## Using Grease

Grease includes complete man-page-like documentation; just run `grease --help`. (`-h` is reserved for other use.)

### Quickstart

`cd` into the directory you want your Phriction documents copied in to, and then:

```bash
/path/to/grease -h phabricator.example.com -k api-yvnxjoomfcqnuhe1ofa1ifen597i /w/mynotes
```
This would connect to a Phabricator installation at `phabricator.example.com`, authenticate using the API key "api-yvnxjoomfcqnuhe1ofa1ifen597i", and download the "mynotes" document at the top level of the wiki.

At a minimum, grease needs a hostname, an API key, and the path to download. This should be a good way to test your first connection attempts.

### Downloading multiple documents

Using the previous example, you could download all documents under "mynotes" like this:

```bash
/path/to/grease -h phabricator.example.com -k api-yvnxjoomfcqnuhe1ofa1ifen597i "/w/mynotes/*"
```
**IMPORTANT:** When using a wildcard path, like this example, make sure it's quoted so that your shell doesn't try to expand it and mangle the arguments.

grease will create subdirectories as needed.

### Using a configuration file

Your Phabricator API key counts as an authentication secret and it's bad form to use those in shell arguments. Let's put it into a configuration file instead. You can store your grease configuration file in `~/.grease` or in `/path/to/grease/config/local`. grease configuration files are simple "key=value" lines, with each key matching a possible argument to the script. Any line starting with "#" *or not including "="* will be ignored. Example, using the options from the quickstart above:

```
# This is your grease configuration file.

# Hostname, required:
hostname="phabricator.example.com"

# API key, also required:
api_key="api-yvnxjoomfcqnuhe1ofa1ifen597i"
```

### Possible configuration file values and their equivalent commandline switches

* **hostname** (**-h**): the hostname for your Phabricator server. This should be a bare hostname, i.e. no "https://" in front. **SSL is required.** Tunneling and other fancy things aren't supported directly by grease, unfortunately. I really wanted to use ssh config files for this but they don't support custom keys.

* **api_path** (**--api**): the path to the Conduit API for Phriction documents in your Phabricator installation. This is `/api/phriction.document.search` by default. If you *did* need to specify a non-https connection, or any other URL supported by curl, you can specify it here and leave the `hostname` value blank: `--api http://my.insecure.phabricator/api/phriction.document.search` for example. If you're doing something super weird, like proxying your Conduit API requests, this option should help.

* **api_key** (**-k**): your Conduit API key ("token"). You can find your Phabricator user account's API tokens at `/settings/panel/apitokens/` in your Phabricator server. Phabricator expects its api tokens to begin with `api-`.

* **path_prefix** (**--prefix**): this prefix will get added to the path given on the commandline. For example, you might use grease to download different groups of documents under the "mynotes" document; a `path_prefix` of "/w/mynotes" would mean that `path/to/grease "tech/*" would retrieve everything under "/w/mynotes/tech", `path/to/grease "personal/*"` would download everything under "/w/mynotes/personal", and so on.

* **destination** (**-o**): set the output (destination) directory for your retrieved Phriction documents. **IMPORTANT:** grease expects this path to already exist (and be writable).

* **title** (**--title**): by default, grease adds the document's title to the top of each document as a markdown H1 element. You can set the **title** option to `no` to disable this behavior (or `yes` on the commandline to override a configuration file; see below).

### Custom configuration file paths

You can use the `-c` option on the commandline to provide a custom path to a grease configuration file. If you do, grease will not attempt to look for or load its default configuration file locations.

### Overriding configuration options

grease can merge configuration options from a few different files (or the commandline). If you specify a hostname and api key on the commandline and no `-c`, grease has enough to run with and won't try looking for a configuration file. It assumes that if you were using configuration files, the first thing you'd put in them is your hostname and API key.

If there's no hostname or no api key specified and no `-c` for a custom config file path, it will look first in its own directory for a file at `config/local`. This is considered a "global" configuration. Then, it looks in your home directory for a `.grease` file; values in this file will override values in the global configuration. Finally, any commandline values will override values provided in any config file.

### Error messages

I've been troubleshooting computery stuff for over three decades now. There are few things that crack my chill more than programs with useless, vague, or obtuse error messages. Grease tries to provide helpful error messages if something goes wrong and it exits with a non-zero status if it prints an error message, so you can safely do things like, `grease "essays/*" && deploy ...`.


## Helpful Tips

Perl can help translate Phabricator's `lang=...` blockquote syntax into the syntax that is expected by other Markdown software: `perl -0777 -i.original -pe 's/```\nlang=/```/igs' <path-to-your-file.md>`

## Contributing

Find a bug? Want to add a feature? Just [open an issue](https://github.com/robsheldon/grease/issues) or send a pull request.


## License

Like most of my other stuff, grease is being released under the [MIT license](https://opensource.org/licenses/MIT). See [LICENSE](https://github.com/robsheldon/grease/blob/master/LICENSE).


[license-shield]: https://img.shields.io/github/license/robsheldon/grease.svg?style=flat-square
[license-url]: https://github.com/robsheldon/grease/blob/master/LICENSE
