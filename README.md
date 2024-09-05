# Scheduled Milestones

A GitHub action that can auto-create weekly milestones.

This action is fork of GitHub Action [scheduled-milestones](https://github.com/marketplace/actions/scheduled-milestones) but has a wider range of options and up-to-date code.

## Options

This action supports the following options:

### title

A prefix for your scheduled milestone. All created milestones will look like `Up for Review (August 27)`, where "Up for Review" is your title.

* *Required*: `Yes`
* *Type*: `string`
* *Example*: `Up for Review`

### days

A comma-separated list of the days of the upcoming week that you want milestones for. For example if you want milestones for every Tuesday and Thursday of the week you would set this to `Tuesday,Thursday`. Just Tuesday? Set it to `Tuesday`. Fridays? I mean I guess if you're living on the edge, then `Friday`.

* *Required*: `Yes`
* *Type*: `string`

### date_options

This option allows you to control how the dates in your milestone titles should look. The options here should be a JSON-encoded string of the options that you would normally supply to `Date.toLocaleDateString()`. Check out the [docs on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleDateString) for `toLocaleDateString` for details.

Note: If you do not configure this, the default format for dates in your milestones will look like `M/D/YYYY`.

* *Required*: `Yes`
* *Type*: `string`

### count

Future week count.

Note: 4.

* *Required*: `No`
* *Type*: `number`

### format

Date format.

Note: `YYYY.MM.DD`.

* *Required*: `No`
* *Type*: `string`

### locale

This option allows you to control what locale will be set for the date in your milestone names. A string with a BCP 47 language tag. Corresponds to the locales parameter of the `Intl.DateTimeFormat()` constructor. Check out the [docs on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/toLocaleDateString) for `toLocaleDateString` for details.

* *Required*: `No`
* *Type*: `string`

## Output

### milestones

If any milestones were created, this will contain an array of their URLs.

## Example

```yaml
name: Weekly Milestones

on:
  schedule:
    - cron: 0 0 * * SUN # Run every Sunday at midnight

jobs:
  generate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Schedule Milestones
        uses: webmasterskaya/scheduled-weekly-milestones@v2
        id: scheduled
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          title: "Up for Review "
          days: Tuesday,Thursday
          date_options: {"month": "long", "day": "numeric"}
          locale: 'ru-RU'

      - name: Created Milestones
        run: echo ${{ steps.scheduled.outputs.milestones }}
```

```yaml
name: auto-create-milestone

on:
  schedule:
    - cron: 0 0 * * SAT

jobs:
  generate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: webmasterskaya/scheduled-weekly-milestones@v2
        id: run
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          title: d-
          days: Friday
          count: 4
          format: YYYY.MM.DD

      - name: result
        run: echo ${{ steps.run.outputs.milestones }}
```

For example, you can also look at the files [auto-create-milestone.yaml](.github/workflows/auto-create-milestone.yaml) and [scheduled-milestones.yml](.github/workflows/scheduled-milestones.yml)