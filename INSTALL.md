# Installation

This guide covers a local development setup. The current code is not ready for
multi-tenant or production financial-data use; review the project status in
[README.md](README.md) before deployment.

## Requirements

- PHP compatible with the Composer constraint `^7.3|^8.0`, with the extensions
  required by Laravel and the locked dependencies (including a PDO driver for
  your database).
- Composer.
- MySQL or MariaDB.
- Node.js and npm to build the Laravel Mix assets.

## 1. Clone and install dependencies

```bash
git clone https://github.com/ScintillaDark/orgexpenses.git
cd orgexpenses
composer install
npm ci
```

## 2. Configure the environment

```bash
cp .env.example .env
```

Edit `.env` and set `APP_NAME`, `APP_URL`, and the database connection values.
Create the database named by `DB_DATABASE` before continuing.

For local development, set `MAIL_MAILER=log`. OTP messages will be written to
the application log instead of being sent through an SMTP server. For real
email delivery, configure the mail settings and sender address in `.env`.

Generate the Laravel application key:

```bash
php artisan key:generate
```

Never commit `.env` or paste credentials into issues, pull requests, or source
files.

## 3. Create the schema and public storage link

```bash
php artisan migrate
php artisan storage:link
```

The repository does not seed an initial user, role, permission, or currency
catalog. Create the first user and administrative role before signing in:

```bash
php artisan tinker
```

Run the following in Tinker:

```php
$user = \App\Models\User::create([
    'name' => 'Administrator',
    'email' => 'admin@example.test',
    'password' => \Illuminate\Support\Facades\Hash::make(
        \Illuminate\Support\Str::random(40)
    ),
    'is_active' => true,
]);

$role = \App\Models\Role::create(['name' => 'Administrator']);

foreach (['manage-users', 'manage-roles', 'manage-permissions', 'manage-accounts'] as $name) {
    $permission = \App\Models\Permission::create(['name' => $name]);
    $role->permissions()->attach($permission->id);
}

$user->roles()->attach($role);
```

Exit Tinker with `exit`. Sign in with the user's email; with `MAIL_MAILER=log`,
find the OTP in `storage/logs/laravel.log`.

## 4. Build assets and start the application

In development:

```bash
npm run dev
php artisan serve --host=127.0.0.1 --port=8000
```

Open <http://127.0.0.1:8000>. For a production asset build, use:

```bash
npm run prod
```

Configure a production web server, HTTPS, persistent storage, mail delivery,
and secure environment variables separately before deployment.

## Currency conversion

INR expenses use a rate of `1`. Other currencies require a matching record in
the `currencies` table. Set `EXCHANGE_RATE_API_KEY` in `.env` to allow the
application to refresh a configured rate. Add the currency catalog through a
trusted administrative process; it is not seeded by this repository.

## Tests

```bash
php artisan test
```

The checked-in tests are minimal scaffolding and do not yet exercise the
business workflows. Treat a passing test run as a basic smoke check, not as
evidence of production readiness.

## Troubleshooting

| Symptom | Check |
| --- | --- |
| Database connection error | Confirm the database exists and the `DB_*` values and PDO driver match. |
| Missing application key | Run `php artisan key:generate`. |
| OTP email does not arrive locally | Use `MAIL_MAILER=log` and inspect `storage/logs/laravel.log`. |
| Missing compiled assets | Run `npm run dev` or `npm run prod`. |
| Uploaded files are not accessible | Run `php artisan storage:link` and check storage permissions. |
