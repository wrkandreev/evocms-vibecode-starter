# Example bLang Localization Pattern

> Important: this is a reusable example pattern, not a drop in truth for every project.
> Before applying it, verify the real implementation on the live project.

## Why Keep This Example

- A real two-language project shows a stable multilingual pattern built around `bLang`, `$_suffix`, and suffixed field names.
- This gives the repository a practical baseline for older and current multilingual Evo projects.
- `evocms-template-registry` can now expose `bLang` metadata, so this pattern can be verified through registry instead of guessed from files alone.

## Registry-First Rule

- If registry is installed, first inspect its `blang` object and related resource context.
- Use registry data to confirm active languages, suffixes, translated field catalog, and template links before adding `_en` style fields.

## Helper Pattern

```php
public static function DocLister($key, $params, $lifetime = false)
{
    $params['api'] = 1;

    if (!empty($params['blang'])) {
        $params['controller'] = 'lang_content';

        if (!empty($key)) {
            $key .= '_' . evo()->getConfig('_suffix');
        }
    }

    return self::runDocListerWithCache($key, $params, $lifetime);
}

public static function DLMenu($key, $params)
{
    $params['api'] = 1;

    if (!empty($params['blang'])) {
        $params['controller'] = 'lang_menu';

        if (!empty($key)) {
            $key .= '_' . evo()->getConfig('_suffix');
        }
    }

    return self::runDlMenuWithCache($key, $params);
}
```

Real projects may also read translation strings directly:

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

## Base Controller Pattern

```php
public function setCommonData()
{
    $_SESSION['_lang'] = evo()->getConfig('_lang');

    $this->data['_suffix'] = evo()->getConfig('_suffix');
    $this->data['langs'] = $this->getLangs();
    $this->data['mainMenu'] = $this->getMainMenu();
}

protected function getLangs()
{
    return evo()->runSnippet('bLang', [
        'type' => 'list',
        'outerTpl' => '@CODE:[+list+]',
        'activeTpl' => '@CODE:<a href="[+url+]" class="active">[+title+]</a>',
        'listRow' => '@CODE:<a href="[+url+]" class="[+classes+]">[+title+]</a>',
        'listTpl' => '@CODE:[+wrapper+]',
    ]);
}
```

## Localized Query Pattern

```php
$params = [
    'parents' => 14,
    'tvList' => 'image',
    'blang' => 1,
    'langFields' => 'pagetitle,menutitle,introtext,longtitle,description,content',
];

$rows = Helper::DocLister('ProductReviews_' . $this->docid, $params);
```

## View Pattern

```blade
<a href="{{ MODX_SITE_URL . ($lang['_root'] ?? '') }}">
    <img src="{{ evo()->getConfig('client_logo') }}" alt="Logo">
</a>

<span>{!! evo()->getConfig('client_address' . $_suffix) !!}</span>

@if(!empty($item['title' . $_suffix]))
    <h6>{{ $item['title' . $_suffix] }}</h6>
    <p>{{ $item['text' . $_suffix] }}</p>
@endif

<div class="lang">
    {!! $langs !!}
</div>
```

## Manager-Side Pattern

```php
return [
    'caption' => 'Common (en)',
    'settings' => [
        'address_en' => [
            'caption' => 'Address',
            'type' => 'text',
        ],
        'bottom_form_title_en' => [
            'caption' => 'Bottom form title',
            'type' => 'text',
        ],
    ],
];
```

Real project example of an English-only tab:

```php
return [
    'caption' => 'Common (en)',
    'settings' => [
        'slogan_en' => ['caption' => 'Logo text', 'type' => 'text'],
        'work_schedule_en' => ['caption' => 'Work schedule', 'type' => 'text'],
        'address_en' => ['caption' => 'Address', 'type' => 'text'],
        'bottom_form_title_en' => ['caption' => 'Bottom form title', 'type' => 'text'],
        'callback_form_text_en' => ['caption' => 'Callback form text', 'type' => 'text'],
    ],
];
```

`MultiTV` may follow the same rule:

```php
$settings['fields'] = [
    'title' => [
        'caption' => 'Title',
        'type' => 'text',
    ],
    'title_en' => [
        'caption' => 'Title (en)',
        'type' => 'text',
    ],
    'description' => [
        'caption' => 'Description',
        'type' => 'textareamini',
    ],
    'description_en' => [
        'caption' => 'Description (en)',
        'type' => 'textareamini',
    ],
];
```

`templatesedit` may also adjust translated manager fields:

```php
unset($rules['en']['fields']['managersTitle_en']);
unset($rules['en']['fields']['departmentsTitle_en']);
unset($rules['en']['fields']['content_en']);
unset($rules['en']['fields']['introtext_en']);
```

## Working Rule

- Prefer one consistent suffix strategy such as `_en` across resource fields, `ClientSettings`, and `MultiTV`.
- Keep language-aware cache keys separated by suffix.
- Use `blang => 1` for document and menu queries when the project is built around `bLang`.

## Verify On Live Project

- actual suffix values like `''` and `_en`
- exact `bLang` snippet behavior
- exact translated field list passed in `langFields`
- whether translated manager fields already exist in `ClientSettings`, TVs, and `MultiTV`
