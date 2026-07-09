# Asset Templates Extension Specification

- **Title:** Asset Templates
- **Identifier:** <https://stac-extensions.github.io/asset-templates/v0.1.0/schema.json>
- **Field Name Prefix:** -
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/extensions/README.md#extension-maturity):** Proposal
- **Owner**: @m-mohr

This document explains the Asset Templates Extension to the
[SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.

This extension offers a way to properly expose templated URLs for Assets in STAC, which are similar to the `assets` object in STAC.
The difference is that the URI can contain variables, so can't be resolved without replacing the variables with specific values.

Potential usecases are exposing assets to ZARR chunks or partitioned Parquet files.

All specifications are inspired by the`linkTemplates` in the
[STAC Link Templates extension](https://github.com/stac-extensions/link-templates), which itself is
[defined in OGC API - Records](https://docs.ogc.org/DRAFTS/20-004r1.html#sc_templated_links_with_variables).

- Examples:
  - [STAC Collection](examples/collection.json)
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Fields

The fields in the table below can be used in these parts of STAC documents:

- [ ] Catalogs
- [x] Collections
- [x] Item
- [ ] Item Properties (incl. Summaries in Collections)
- [ ] Assets (for both Collections and Items, incl. Item Asset Definitions in Collections)
- [ ] Links

| Field Name    | Type | Description |
| ------------- | ---- | ----------- |
| assetTemplates | Map\<string, [Asset Templates Object](#asset-templates-object)> | **REQUIRED**. An object of asset names mapping to templated assets. |

### Asset Templates Object

The Asset Templates Object follows the definition of the STAC
[Asset Object](https://github.com/radiantearth/stac-spec/blob/master/commons/assets.md#asset-object),
except that it has no `href`.

Instead of the `href`, the Asset Template Object adds the following fields to the Asset Object:

| Field Name  | Type             | Description |
| ----------- | ---------------- | ----------- |
| uriTemplate | string           | **REQUIRED**. Supplies a resolvable URI to a remote resource (or resource fragment). |
| varBase     | string           | The base URI to which the variable name can be appended to retrieve the definition of the variable as a JSON Schema fragment. |
| variables   | Map\<string, \*> | This object contains one key per substitution variable in the templated URL.  Each key defines the schema of one substitution variable using a JSON Schema fragment and can thus include things like the data type of the variable, enumerations, minimum values, maximum values, etc. |

Additionaly, all fields that can be used as defined STAC in Asset Objects, for examnple:

- `roles`
- `type`
- `title`

## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PR's are part of the repository and can be run locally to verify that changes are valid.
To run tests locally, you'll need `npm`, which is a standard part of any [node.js installation](https://nodejs.org/en/download/).

First you'll need to install everything with npm once. Just navigate to the root of this repository and on
your command line run:

```bash
npm install
```

Then to check markdown formatting and test the examples against the JSON schema, you can run:

```bash
npm test
```

This will spit out the same texts that you see online, and you can then go and fix your markdown or examples.

If the tests reveal formatting problems with the examples, you can fix them with:

```bash
npm run format-examples
```
