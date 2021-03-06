# Docker image for Google Cloud Directory Sync

![Security](https://github.com/DNXLabs/docker-gcds/workflows/Security/badge.svg)
![Lint](https://github.com/DNXLabs/docker-gcds/workflows/Lint/badge.svg)

Google Cloud Directory Sync - GSuite synchronization tool: [https://support.google.com/a/answer/106368?hl=en](https://support.google.com/a/answer/106368?hl=en)

## Google requirements

* A role account should be created with Super Admin privileges under which this tool will run so that it is segregated from all other *normal* accounts

## Running GCDS commands

The normal commands that come with GCDS can be run using normal `docker run` commands.  However, due to the way that GCDS was built, there are several intricacies involved in running these applications inside a container, so it is best to use the scripts provided in this repository for running each of the commands.  For example, to run the `config-manager` command, you need to have an X11 session running, which can be problematic from inside a Docker container. Each of the other commands also have nuance in what is needed inside the container when they run, etc., so it is best to use the provided scripts when running them

## Java preferences

One of the trickiest parts of GCDS is the fact that it uses the Java Preferences library to save keys, etc. that make decrypting the passwords and secrets inside the XML config files possible.  Therefore, it is necessary to include the preferences directory whenever running commands that need to decrypt values within the files (`config-manager`, `encrypt-util`, `sync-cmd`, and `upgrade-config`).  If transitioning from an old system to this Docker container, you will need to do the following:

* Find out what user GCDS is running as
* Go to that user's home directory and get the `.java/.userPrefs` directory and its contents
* Make a `.java` directory in the directory where this repository is checked out and place the `.userPrefs` directory there
* Mount the `.java` directory into the container in /root, or use the provided scripts, as they will do this for you.

## Author

Managed by [DNX Solutions](https://github.com/DNXLabs).

## License

Apache 2 Licensed. See [LICENSE](https://github.com/DNXLabs/docker-gcds/blob/master/LICENSE) for full details.