# Contributing

Contributions that improve correctness, documentation, accessibility, and
maintainability are welcome.

## Before opening a pull request

1. Search existing issues and pull requests; open an issue for larger changes.
2. Create a focused branch from `main`.
3. Keep changes scoped and add tests for changed behavior.
4. Run the relevant checks:

   ```bash
   composer validate
   php artisan test
   npm run prod
   ```

5. Update documentation when setup or behavior changes.
6. Open a pull request using the repository template and explain the motivation,
   implementation, and validation performed.

Do not include `.env` files, API keys, personal data, uploaded receipts, or
other credentials in commits or test fixtures. Report security issues through
the private process in [SECURITY.md](SECURITY.md).
