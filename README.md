Source for <https://ibm-bluemix.github.io>

The site is currently built based on the files:

* `people.toml`
* `projects.toml`
* `index-template.html`

See the [`toml`](https://github.com/toml-lang/toml) site for more information
on the structure of `.toml` files.

To hack on this site, first run an npm install to install the pre-req tools:

    npm install

To rebuild the site, run:

    npm run build

If you want have a build run as you edit and save the files, run:

    npm run watch
