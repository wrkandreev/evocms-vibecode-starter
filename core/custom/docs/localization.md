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

## Repeated Building Blocks

- `Helper::DocLister()` switches controller to `lang_content` when `blang => 1` is passed.
- `Helper::DLMenu()` switches controller to `lang_menu` when `blang => 1` is passed.
- Cache keys are often suffixed by the current language to avoid mixing localized results.
- Views often read text values through `$_suffix`, for example `title . $_suffix`.
- `ClientSettings` values may be split into `client_address` and `client_address_en`, then selected at runtime with `client_address . $_suffix`.
- `MultiTV` rows may contain parallel keys such as `title` and `title_en`.

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

## MultiTV Pattern

- `MultiTV` rows may duplicate only the text fields that need translation.
- Example keys: `title`, `description`, `title_en`, `description_en`.
- Views then read them using `$_suffix` instead of branching on every language manually.

## templatesedit Pattern

- Manager layout may explicitly group translated fields and remove language-specific duplicates from certain tabs.
- Example: projects may keep `managersTitle` and `managersTitle_en`, then adjust `templatesedit` rules so language tabs do not show the wrong duplicate fields.

## Working Rule

- Always verify whether the project uses `bLang` or another localization layer before adding suffixed fields.
- Always verify the real `_suffix` values on the live project.
- Always verify whether localized values live in resource fields, TVs, `MultiTV`, `ClientSettings`, or all of them together.
- Do not invent `_en` fields in code until registry or live project config confirms they exist.

## Verify On Live Project

- whether `bLang` is installed and active
- what values `_lang` and `_suffix` actually take
- whether `DocLister` and `DLMenu` use `lang_content` and `lang_menu`
- which document fields are localized through `langFields`
- which TVs, `MultiTV` fields, and `ClientSettings` keys have suffixed variants
- whether `templatesedit` changes manager layout for translated fields
