# Localization

> Important: this is a typical Evolution CMS CE pattern.
> Before applying it, verify the real implementation on the live project.

## Goal

- Explain how multilingual projects may combine `bLang`, suffixed fields, controller helpers, and manager-side config.

## Verified Two-Language Pattern

- A real project uses two languages with `bLang`.
- The current language suffix is stored in config, for example `evo()->getConfig('_suffix')`.
- Controllers also keep the current language in session, for example `$_SESSION['_lang']`.
- Resource lists and menus are localized through `DocLister` and `DLMenu` with `blang => 1`.
- Text fields in `ClientSettings`, TVs, and `MultiTV` often use explicit suffixed keys such as `title_en`, `description_en`, `address_en`.

## Registry Support

- `evocms-template-registry` now supports `bLang`.
- Registry can expose multilingual context such as languages, suffixes, `bLang` settings, field catalog, template links, and resource-level localized field context.
- If registry is available, use its `blang` data before inventing translated fields or suffix conventions.
- `bLang` should be verified through its own metadata tables, especially `blang_tmplvars`, not only through `site_tmplvars` names.
- A manually created TV pair such as `missionTitle` plus `missionTitle_en` is not automatically a valid `bLang` pair until it is registered in `bLang` metadata.
- If registry exposes API support for adding TVs or dictionary entries, use that workflow when available so metadata and real entities stay in sync.

## Registry API Workflow For bLang

When `evocms-template-registry` is installed and write API is enabled, prefer this order:

1. Read current `bLang` state:
   - `GET /api/template-registry/blang`
   - `GET /api/template-registry/blang/health`
2. Read target resource/template context:
   - `GET /api/template-registry/resource-context?resource_id=...`
3. Manage dictionary strings through:
   - `GET/POST/PATCH/DELETE /api/template-registry/blang/lexicon`
4. Seed default `bLang` params if the project needs the standard localized core fields:
   - `POST /api/template-registry/blang/default-params`
5. Create or update real `bLang` fields through metadata, not by manual TV pairs:
   - `POST/PATCH/DELETE /api/template-registry/blang/fields`
6. Update manager side language model only through `bLang` settings API:
   - `PATCH /api/template-registry/blang/settings`
   - `DELETE /api/template-registry/blang/languages/{language}`
7. Update actual localized resource values through:
   - `PATCH /api/template-registry/resources/{resourceId}/blang-fields`
8. If the project shows drift between `bLang` links and template TV links, repair it through:
   - `POST /api/template-registry/blang/fix-template-links`

## What Each bLang Endpoint Is For

- `GET /api/template-registry/blang`
  - current languages, suffixes, settings, field catalog, template links
- `GET /api/template-registry/blang/health`
  - drift diagnostics between `blang_tmplvar_templates` and real MODX template TV bindings
- `GET/POST/PATCH/DELETE /api/template-registry/blang/lexicon`
  - UI/dictionary strings stored in the `blang` table
- `POST /api/template-registry/blang/default-params`
  - manager button analogue that seeds default `blang_tmplvars` and synchronizes generated localized TVs
- `POST/PATCH/DELETE /api/template-registry/blang/fields`
  - source of truth for custom localized fields and their template assignments
- `PATCH /api/template-registry/blang/settings`
  - languages, suffixes, default language, runtime `bLang` settings
- `DELETE /api/template-registry/blang/languages/{language}`
  - explicit language removal from active `bLang` model; does not drop old DB columns automatically
- `PATCH /api/template-registry/resources/{resourceId}/blang-fields`
  - actual localized values for one resource, both resource columns and localized TVs when applicable
- `POST /api/template-registry/blang/fix-template-links`
  - repair missing `bLang` template links based on current localized TV assignments

## Critical bLang Rules

- Do not create `_en` style TVs manually and assume `bLang` will adopt them.
- Treat `blang_tmplvars` plus `blang_tmplvar_templates` as the authoritative field model.
- Use `PATCH /api/template-registry/resources/{resourceId}/blang-fields` for content updates instead of writing random suffixed TVs directly.
- Before updating localized values, verify the field is present in `resource-context -> blang -> template_fields` for that exact resource template.
- When changing languages or suffixes, do it through `PATCH /api/template-registry/blang/settings`, not by editing tables ad hoc.
- If language removal is required, use the explicit delete-language endpoint and pick a replacement default when needed.
- If `blang/health` reports drift, fix metadata before adding more localized content.

## Repeated Building Blocks

- `Helper::DocLister()` switches controller to `lang_content` when `blang => 1` is passed.
- `Helper::DLMenu()` switches controller to `lang_menu` when `blang => 1` is passed.
- Cache keys are often suffixed by the current language to avoid mixing localized results.
- Views often read text values through `$_suffix`, for example `title . $_suffix`.
- `ClientSettings` values may be split into `client_address` and `client_address_en`, then selected at runtime with `client_address . $_suffix`.
- `MultiTV` rows may contain parallel keys such as `title` and `title_en`.
- Helper methods may also read translation strings from the `blang` table directly for UI text or custom labels.

## Controller Side Pattern

Typical controller/bootstrap logic:

```php
public function setCommonData()
{
    $_SESSION['_lang'] = evo()->getConfig('_lang');

    $this->data['_suffix'] = evo()->getConfig('_suffix');
    $this->data['langs'] = $this->getLangs();
    $this->data['mainMenu'] = $this->getMainMenu();
}
```

Translation helper pattern from a real project:

```php
public static function translate($key)
{
    $translations = self::getTranslations();

    return $translations[$key] ?? $key;
}

public static function getTranslations($lang = false)
{
    $lang = $lang ?: ($_SESSION['_lang'] ?? 'ru');

    return Cache::rememberForever('Translations_' . $lang, function () use ($lang) {
        return DB::table('blang')->get()->pluck($lang, 'name')->toArray();
    });
}
```

Typical localized document query:

```php
$params = [
    'parents' => 14,
    'tvList' => 'image',
    'blang' => 1,
    'langFields' => 'pagetitle,menutitle,introtext,longtitle,description,content',
];

$rows = Helper::DocLister('ProductReviews_' . $this->docid, $params);
```

Typical localized menu query:

```php
$params = [
    'parents' => 0,
    'maxDepth' => 2,
    'blang' => 1,
];

$menu = Helper::DLMenu('MainMenu', $params);
```

## View Side Pattern

Typical view logic uses `$_suffix` to switch between base and `_en` keys:

```blade
<span>{!! evo()->getConfig('client_address' . $_suffix) !!}</span>

@if(!empty($item['title' . $_suffix]))
    <h6>{{ $item['title' . $_suffix] }}</h6>
    <p>{{ $item['text' . $_suffix] }}</p>
@endif
```

Language switcher may be rendered through `bLang`:

```php
$params = [
    'type' => 'list',
    'outerTpl' => '@CODE:[+list+]',
    'activeTpl' => '@CODE:<a href="[+url+]" class="active">[+title+]</a>',
    'listRow' => '@CODE:<a href="[+url+]" class="[+classes+]">[+title+]</a>',
    'listTpl' => '@CODE:[+wrapper+]',
];

$langs = evo()->runSnippet('bLang', $params);
```

## ClientSettings Pattern

- Global values may be duplicated by language using suffixed keys like `address_en`, `work_schedule_en`, `bottom_form_title_en`.
- Runtime access still uses the normal `client_` prefix rule.
- Example:
  - manager key `address` -> runtime `client_address`
  - manager key `address_en` -> runtime `client_address_en`
- Real project pattern: one tab may store only translated values such as `slogan_en`, `work_schedule_en`, `address_en`, `bottom_form_title_en`, `callback_form_text_en`.

## MultiTV Pattern

- `MultiTV` rows may duplicate only the text fields that need translation.
- Example keys: `title`, `description`, `title_en`, `description_en`.
- Views then read them using `$_suffix` instead of branching on every language manually.

## templatesedit Pattern

- Manager layout may explicitly group translated fields and remove language-specific duplicates from certain tabs.
- Example: projects may keep `managersTitle` and `managersTitle_en`, then adjust `templatesedit` rules so language tabs do not show the wrong duplicate fields.

Example:

```php
unset($rules['en']['fields']['managersTitle_en']);
unset($rules['en']['fields']['departmentsTitle_en']);
unset($rules['en']['fields']['content_en']);
unset($rules['en']['fields']['introtext_en']);
```

## Working Rule

- Always verify whether the project uses `bLang` or another localization layer before adding suffixed fields.
- Always verify the real `_suffix` values on the live project.
- Always verify whether localized values live in resource fields, TVs, `MultiTV`, `ClientSettings`, or all of them together.
- Do not invent `_en` fields in code until registry or live project config confirms they exist.
- Do not treat raw `_en` TV naming as sufficient `bLang` evidence without `blang_tmplvars` registration.
- For package-driven multilingual work, prefer `evocms-template-registry` write API over direct ad hoc DB edits so `bLang` metadata and generated TVs stay synchronized.

## Verify On Live Project

- whether `bLang` is installed and active
- what values `_lang` and `_suffix` actually take
- whether `DocLister` and `DLMenu` use `lang_content` and `lang_menu`
- which document fields are localized through `langFields`
- which TVs, `MultiTV` fields, and `ClientSettings` keys have suffixed variants
- whether `templatesedit` changes manager layout for translated fields
