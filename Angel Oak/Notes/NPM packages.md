### Package Preparation

- Ensure your project has a valid `package.json` file with the following fields:
    
    - `name`: Unique package name (consider using a scoped name like `@your-org/package-name`)
    - `version`: Follow semver (e.g., `1.0.0`)
    - `main`: Entry point (e.g., `dist/index.js`)
    - `types`: Type definitions file (if using TypeScript)
    - `scripts`: Include build and test scripts (`npm run build`, `npm test`)
    - `repository`, `author`, `license`: Metadata for discoverability
- Run `npm run build` to compile your code (especially for TypeScript projects)
    
- Clean up unnecessary files using `.npmignore` or the `files` field in `package.json` to control what gets published

---

### Publishing 
```shell 
npm login 
npm publish 
```

#### Publishing to an Existing npm Package
### 1. **Update the Version**

npm requires each published version to be unique. You must update the `version` field in your `package.json` before publishing.

### 2. **Build Your Package**

If you're using TypeScript or bundling tools, run your build script:

```shell 
npm run build
```

### 3. **Login to npm **

```shell 
npm login 
```

### 4. **Publish the New Version**
```shell 
npm publish
```


## CI/CD Integration

To automate publishing on version changes:

- Use `npm version` in your pipeline
- Authenticate with npm using a token
- Run `npm publish` as part of your release job