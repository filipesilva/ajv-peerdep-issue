# ajv-peerdep-issue

This repo shows a problem with how `npm@5.6.0` hoists dependencies that have peer dependencies.

Running `npm install` will show the following warning:
```
npm WARN ajv-keywords@3.1.0 requires a peer of ajv@^6.0.0 but none is installed. You must install peer dependencies yourself.
```

The following dependencies are relevant:
- this repro itself depends on two packages: `ajv@5.5.2` and `webpack@3.11.0`.
- `webpack@3.11.0` depends on `ajv@^6.1.0` and `ajv-keywords@^3.1.0`.
- `ajv-keywords@3.1.0` has a peer dependency on `ajv@^6.0.0`


When `npm` resolves these dependencies they end up looking like this:
```
ajv-peerdep-issue@1.0.0 D:\sandbox\ajv-peerdep-issue
`-- ajv@5.5.2
`-- ajv-keywords@3.1.0
`-- webpack@3.11.0
  `-- ajv@6.1.0
```

This is a problem because the hoisted `ajv-keywords@3.1.0` will not have its peer dependency on `ajv@^6.0.0` met.
