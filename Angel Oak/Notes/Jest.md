### What is Jest

Jest is a JavaScript testing framework developed by Meta. It is commonly used for unit testing, integration testing, and mocking in Node.js and frontend applications. Jest works well with TypeScript, Babel, and React out of the box.

---

### Installation

For TypeScript projects:

```shell 
npm install --save-dev jest ts-jest @types/jest
```

---

### Configuration

Add a test script to `package.json`:
```json 
"scripts": {

  "test": "jest"

}
```
Optional: Create a `jest.config.js` or `jest.config.ts`:
```json 
module.exports = {

  preset: 'ts-jest',

  testEnvironment: 'node',

  verbose: true,

  collectCoverage: true,

  coverageDirectory: 'coverage',

};
```

---

### Writing Tests

Basic structure:
```ts
describe('FunctionName', () => {

  it('should return expected value', () => {

    const result = yourFunction();

    expect(result).toBe(expectedValue);

  });

});
```

Common matchers:

- `toBe()` – strict equality
- `toEqual()` – deep equality
- `toContain()` – substring or array item
- `toThrow()` – error throwing
- `toBeTruthy()` / `toBeFalsy()` – boolean checks

--- 
### Mocking

Manual module mocking:
```ts 
jest.mock('./moduleName');
```
Mocking functions:
```ts
const mockFn = jest.fn();

mockFn.mockReturnValue('value');
```
Resetting mocks:
```ts
jest.resetAllMocks();
```

---
### Coverage

Run tests with coverage:
```shell 
npm test -- --coverage
```

Generates a `coverage/` folder with HTML reports.

---

### File Naming Conventions

Jest automatically detects:

- Files ending in `.test.js`, `.test.ts`, `.spec.js`, `.spec.ts`
- Files inside `__tests__` directories

---

### Troubleshooting

- If tests aren't running, check file naming and configuration
- For TypeScript errors, ensure `ts-jest` is installed and configured
- Use `async/await` properly for asynchronous tests
- Reset mocks between tests to avoid state leakage

---

### Best Practices

- Keep tests isolated and deterministic
- Use descriptive test names
- Mock external dependencies to avoid side effects
- Test edge cases and error handling
- Use coverage reports to identify gaps
- Integrate tests into CI/CD pipelines
