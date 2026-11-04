# CLAUDE.md

Starter Express REST API for users, backed by an in-memory store. It is the base project for the Claude Code course.

## Commands

```
npm run dev     # start on http://localhost:3000, restarting on file changes
npm test        # run all tests with the node:test runner
npm run lint    # ESLint; CI runs this and npm test on every push and PR

node --test tests/users.test.js                   # run one test file
node --test --test-name-pattern="GET /health"     # run tests whose name matches
```

## Conventions

- Use British Spelling
- Use CommonJS (`require` / `module.exports`), not ES modules. ESLint parses files with `sourceType: "script"`, so `import` fails lint.
- Access data only through functions in `db/store.js`, never from module-level state or arrays in a route file.
- Send errors as `{ "error": "<message>" }` with the matching status code. Validate request bodies in the route handler.
- Convert `:id` params with `Number()` before calling the store, because it compares ids with `===`.
- Write tests with `node:test`, `node:assert`, and `supertest`, passing the `app` exported from `server.js`. Do not start a real server in tests.
- In tests, do not assert exact user counts or rely on data created by another test. Store state is shared across a test file's tests and is never reset.

## Architecture

- `server.js` builds the Express app, mounts one router per resource, and exports `app`. It only calls `listen` when run directly, which is what lets tests import it without opening a port.
- `routes/` has one file per resource. To add a resource, create its router here and mount it in `server.js`.
- `db/store.js` holds the data in memory, standing in for a real database. Data resets on restart.
