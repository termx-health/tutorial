

1. Navigate to the `termx-web/app/src/assets/i18n` folder in [TermX Web](https://github.com/termx-health/termx-web/tree/main/app/src/assets/i18n). 
1. Use the `en.json` file as the source for your translations. Once you've finished, add this file back to the same folder, but with your [language code](https://www.localeplanet.com/icu) as a file name (e.g., `et.json`).
1. Include the system language translation in `termx-web/app/src/assets/ui-languages.json`.
1. Append your language code to the `UI_LANGS` variable in `termx-web/app/src/environments/environment.base.ts`.
1. At runtime, expose the language through the `UI_LANGUAGES` environment variable (see the [installation guide](page:installation-guide)).

*Ensure the existence of your translations in the shared [web-commons](https://github.com/termx-health/web-commons) library — [`core-util/lib/locales`](https://github.com/termx-health/web-commons/tree/main/core-util/lib/locales) and [`ui/src/locales`](https://github.com/termx-health/web-commons/tree/main/ui/src/locales).*
