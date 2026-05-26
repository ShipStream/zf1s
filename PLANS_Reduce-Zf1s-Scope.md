# Reduce zf1s Scope

## Goal

Replace the monolithic `zf1s/zf1` dependency with only the Zend Framework 1 packages actually used by ShipStream.

## Direct Packages Needed

- `zf1s/zend-barcode`
- `zf1s/zend-cache`
- `zf1s/zend-captcha`
- `zf1s/zend-config`
- `zf1s/zend-console-getopt`
- `zf1s/zend-controller`
- `zf1s/zend-date`
- `zf1s/zend-db`
- `zf1s/zend-exception`
- `zf1s/zend-filter`
- `zf1s/zend-http`
- `zf1s/zend-json`
- `zf1s/zend-locale`
- `zf1s/zend-log`
- `zf1s/zend-mail`
- `zf1s/zend-measure`
- `zf1s/zend-mime`
- `zf1s/zend-oauth`
- `zf1s/zend-pdf`
- `zf1s/zend-translate`
- `zf1s/zend-uri`
- `zf1s/zend-validate`
- `zf1s/zend-view`
- `zf1s/zend-xml`

## Expected Transitive Packages

Composer should also install these through package requirements:

- `zf1s/zend-crypt`
- `zf1s/zend-loader`
- `zf1s/zend-memory`
- `zf1s/zend-registry`
- `zf1s/zend-server`
- `zf1s/zend-service`
- `zf1s/zend-service-recaptcha`
- `zf1s/zend-text`

## Implementation Steps

1. Remove `zf1s/zf1` from `composer.json`.
2. Add the direct packages listed above.
3. Move existing patches from `zf1s/zf1` to their component packages:
   - `shell/patches/zf1s-zend-db-execute-last-query.diff` -> `zf1s/zend-db`
   - `shell/patches/zf1s-zend-pdf-truetype-encode-cp1252.diff` -> `zf1s/zend-pdf`
4. Run `composer update zf1s/* --with-dependencies`.
5. Verify `vendor/composer/autoload_namespaces.php` contains all required `Zend_*` namespaces.
6. Run a smoke test or relevant PHPUnit subset.

## Validation Notes

Zend usage was identified from first-party code under `app`, `lib`, `shell`, `packages`, and tests, excluding the installed `vendor/zf1s` package itself.

The codebase also includes local overrides under `app/code/core/Zend`, so those should remain untouched.
