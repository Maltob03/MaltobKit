---
title: "Configuration"
date: 2020-08-14
draft: false
description: "All the configuration variables available in Blowfish."
slug: "configuration"

---

Blowfish is a highly customisable theme and uses some of the latest Hugo features to simplify how it is configured.

The theme ships with a default configuration that gets you up and running with a basic blog or static website.

> Configuration files bundled with the theme are provided in TOML format as this is the default Hugo syntax. Feel free to convert your config to YAML or JSON if you wish.

The default theme configuration is documented in each file so you can freely adjust the settings to meet your needs.

{{< alert >}}
As outlined in the [installation instructions] you should adjust your theme configuration by modifying the files in the `config/_default/` folder of your Hugo project and delete the `config.toml` file in your project root.
{{< /alert >}}

## Site configuration

Standard Hugo configuration variables are respected throughout the theme, however there are some specific things that should be configured for the best experience.

The site configuration is managed through the `config/_default/config.toml` file. The table below outlines all the settings that the Blowfish takes advantage of.

Note that the variable names provided in this table use dot notation to simplify the TOML data structure (ie. `outputs.home` refers to `[outputs] home`).



The default translations can be overridden by creating a custom file in `i18n/[code].yaml` that contains the translation strings. You can also use this method to add new languages. If you'd like to share a new translation with the community, please 

### Configuration

In order to be as flexible as possible, a language configuration file needs to be created for each language on the website. By default Blowfish includes an English language configuration at `config/_default/languages.en.toml`.

The default file can be used as a template to create additional languages, or renamed if you wish to author your website in a language other than English. Simply name the file using the format `languages.[language-code].toml`.

```
yaml
---
title: "Welcome to Blowfish!"
description: "This is a demo of adding content to the homepage."
---
Welcome to my website! I'm really happy you stopped by.
```



### Menus



## Theme parameters

Blowfish provides a large number of configuration parameters that control how the theme functions. The table below outlines every available parameter in the `config/_default/params.toml` file.





## Other configuration files

The theme also includes a `markup.toml` configuration file. This file contains some important parameters that ensure that Hugo is correctly configured to generate sites built with Blowfish.

Always ensure this file is present in the config directory and that the required values are set. Failure to do so may cause certain features to function incorrectly and could result in unintended behaviour.
