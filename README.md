# Features

A machine-readable list of what an API can actually do — the consumer-facing capability
list that sits between the marketing page and the reference documentation.

This is the schema behind the [Features property](https://apicommons.org/common/features/)
in [API Commons](https://apicommons.org).

A features list is normally a marketing artifact, which is exactly why it is worth making
machine-readable. It is the only place a provider states its capabilities in the
consumer's language rather than the implementation's, and the distance between what it
claims and what the OpenAPI actually exposes is a measurable thing rather than an
argument.

## Artifacts

- **[features-json-schema.yml](features-json-schema.yml)** — the JSON Schema (2020-12).
- **[features-example-1.yml](features-example-1.yml)** — a standalone list for a made-up
  messaging API, including the two cases that matter: a feature with no API behind it,
  and a feature that has been announced but not shipped.
- **[features-example-2.yml](features-example-2.yml)** — the `Features` property envelope,
  splitting platform features from console features.
- **[validate.py](validate.py)** — validates any document against the schema.

## Using it

```yaml
- name: Bulk send
  description: Send the same message to up to 10,000 recipients in one request.
  url: https://example.com/features/bulk
  category: Messaging
  status: ga          # announced | preview | beta | ga | deprecated | retired
  tiers:
    - Pro
    - Enterprise
  operations:
    - createBulkSend
    - getBulkSend
```

Only `name` is required. Three fields do the real work:

- **`operations`** — the operationIds implementing the feature. This is what earns the
  schema its keep. With it, "advertised but not in the API" becomes a query you can run
  against the OpenAPI. A feature with no `operations` is not a failure to record — it is
  usually a console or platform feature, and saying so beats leaving a reader to assume
  an endpoint exists.
- **`status`** — `announced` is deliberately separate from `preview`. Things get put on
  a features page before they ship, and a consumer will ask about them either way.
  Recording it honestly beats omitting it.
- **`tiers`** — which plans include the feature. Use the same names as the
  [Plans](https://github.com/api-commons/plans) property.

A features page is also an accidental scope-discovery surface: it routinely enumerates
admin actions, exports, and write operations that the reference gates behind elevated
scopes. Keep the list aligned with what is actually authorized at the default tier.

## Validating

```
pip install jsonschema pyyaml
python3 validate.py features-example-1.yml
```

## Support

Questions, corrections, and requests go in
[the issues](https://github.com/api-commons/features/issues).

## License

Two licenses, by kind of thing:

- **Artifacts** — the schemas, rulesets, fixtures, examples and API descriptions — are
  **[CC BY-NC-SA 4.0](LICENSE)** (Attribution–NonCommercial–ShareAlike).
- **Code** — the validator, test harness and packaging — is **[Apache-2.0](LICENSE-CODE)**.

API Commons licenses **artifacts** under CC BY-NC-SA 4.0 and **code** under Apache-2.0.
