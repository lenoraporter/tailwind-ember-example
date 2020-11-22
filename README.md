# tailwind-example

All the examples I found just after Tailwind 2.0s release cover getting Tailwind 1.x into an Ember app. The setup is just slightly different, enough to give some weird errors. For someone not very familiar with `PostCSS` it was confusing. I put this example repository together to figure out a minimal working example. Hope it helps someone else.

## Step 1 - Installing Dependencies

If you are already using `ember-cli-postcss` make sure to update to latest `v7` version. Tailwind 2.x switches to `PostCSS v8`. See the `ember-cli-postcss` README for specifics of ther version: (https://github.com/jeffjewiss/ember-cli-postcss)

```
ember install ember-cli-postcss
npm install --save-dev tailwindcss autoprefixer
```

## Step 2 - Create a tailwind config file

`npx tailwindcss init config/tailwindcss-config.js --full `

## Step 3 - Configure `ember-cli-build.js`

This is the part that's a big different. The configuration used to take an array of modules, and the tailwind plugin was passed the location of the config by calling the module. 

With tailwind 2.0, ember-cli-postcss v7, and PostCSS v8 the configuration excepts an array of objects each containing a `module` and an `options` property.

```
/* ember-cli-build.js */

'use strict';

const EmberApp = require('ember-cli/lib/broccoli/ember-app');
const autoprefixer = require('autoprefixer');
const tailwind = require('tailwindcss');

module.exports = function(defaults) {
  let app = new EmberApp(defaults, {
    postcssOptions: {
      compile: {
        plugins: [
          {
            module: autoprefixer,
            options: {}
          },
          {
            module: tailwind,
            options: {
              config: './config/tailwindcss-config.js'
            }
          }
        ],
      }
    }
  });

  return app.toTree();
};
```

## Step 4 - Import Tailwind in app.css

```
// app/stytles/app.css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

## Step 5 - Replace application.hbs with tailwind css

```
{{!-- app/templates/application.hbs --}}
<div class="container mx-auto">
  <h1 class="font-mono text-blue-500">Hello Tailwind</h1>
</div>
```

# TODO

Modifying the `tailwindcss-config.js` doesn't regenerate the css, which is kinda a pain when trying to tweak your config. You need to stop / start `ember s` to pickup the changes. If anyone knows how to resolve that please open a PR :)

With a real production app configuring something like `PurgeCSS` would also be useful to exclude unused CSS.

# Thanks
Thanks to the awesome develepors that build all the tools that make this all possible.
