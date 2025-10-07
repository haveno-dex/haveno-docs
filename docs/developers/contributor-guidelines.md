# Contributing to Haveno

Thanks for wishing to help! Here there are some guidelines and information about the development process. We suggest you join the [matrix](https://matrix.to/#/#haveno-development:monero.social) room `#haveno-development` (relayed on [IRC/Libera](irc://irc.libera.chat/#haveno-development)) and have a chat with the devs, so that we can help get you started.

Issues are tracked on GitHub. We use [a label system](https://github.com/haveno-dex/haveno/issues/50) and GitHub's [project boards](https://github.com/haveno-dex/haveno/projects) to simplify development. Make sure to take a look at those and follow the priorities suggested.

## General guidelines

- Be verbose. Remember this is collaborative development and we need to make life as easy as possible for future developers and the current maintainers.
- All formatting needs to be consistent with the current code.
- Changes must be done 'the right way'. No hacks to make things work, if you are having issues, let the other devs know so that we can help you out.
- Rebase and squash your commits

## Development process

When you have something new built for Haveno, submit a pull request for review to be merged.

- Pull requests should contain as many details as possible about what you are going to change and why. Avoid "title only" PRs, unless they are self-explanatory.
- Pull requests should contain one single commit, unless it makes sense to have more. Please become familiar with git if you are not.
- Pull requests won't be merged before 24 hours have passed. This timeframe will be extended when Haveno will have more active developers.

## Translation

Existing translation files are in [core/src/main/resources/i18n/](https://github.com/haveno-dex/haveno/tree/master/core/src/main/resources/i18n), feel free to update or improve them if needed.

To add a new locale translations, follow these steps:

- Add your [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639_language_codes) standard country language code to [core/src/main/java/haveno/core/locale/LanguageUtil.java](https://github.com/haveno-dex/haveno/blob/master/core/src/main/java/haveno/core/locale/LanguageUtil.java) and remove it from "// not translated yet" if it is there.

- Copy [displayStrings.properties](https://github.com/haveno-dex/haveno/blob/master/core/src/main/resources/i18n/displayStrings.properties), create new file in [core/src/main/resources/i18n/](https://github.com/haveno-dex/haveno/tree/master/core/src/main/resources/i18n) in this format: `displayStrings_[insertLocaleName].properties` and then add translations.
