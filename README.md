## Have Phriction? You need Grease!

Grease can safely and easily retrieve your Remarkup documents from your Phabricator server. It is a single-file shell script with few dependencies, a complete help screen (grease --help), and a ton of error handling and carefully written error messages. If something doesn't work right, it will try to point you in the right direction.


## Dependencies

curl is used to connect to the Phabricator server and get a respone. [jq](https://github.com/stedolan/jq) is used for parsing the response from the server.


## Compatibility

This should run okay on most Unix-like operating systems. It's been tested on Debian. I tried to avoid platform-specific binaries. It probably requires a bash shell, but may run in other shells. If you have trouble running it on a Unix-like operating system or on another shell *and* you can offer a fix that probably won't break it for other users, please open an issue and let me know.


## Using Grease

More complete documentation will be here very shortly.


## License

Like most of my other stuff, grease is being released under the [MIT license](https://opensource.org/licenses/MIT). See [LICENSE](https://github.com/robsheldon/grease/blob/master/LICENSE).


[license-shield]: https://img.shields.io/github/license/robsheldon/grease.svg?style=flat-square
[license-url]: https://github.com/robsheldon/grease/blob/master/LICENSE
