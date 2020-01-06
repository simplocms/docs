---
id: helpers
title: Helpers
---

## Introduction

Laravel offers a lot of helper functions, which you can use everywhere you want. These functions can help with specific
problems and can solve some requirements.

> If you want to know more about Helpers and get all available Laravel's Helpers, go to official [Laravel Documentation](https://laravel.com/docs/5.8/helpers)

## Is In SIMPLO CMS More Helpers?

Of course! SIMPLO CMS increases count of helper available functions and provides for developers including Laravel's helpers also
custom helper functions. About them, you can find out more below.

### Obfuscate Email

You know certainly situation, when you need to show contact email address for all visitors of your website, but you are afraid
because of security issue. Robots can scan your website, get and save this email address and then send spam posts to this email.
If you want to avoid these potential problems, it's necessary to show email address with safety way and don't show email address
in raw format.

For these purpose, SIMPLO CMS offers `App\Helpers\EmailObfuscator` helper class with a few helper methods for different display
format of email addresses.

**`EmailObfuscator::makeLink(string $email[, array $attributes = [], string $print = self::PRINT_METHOD_JS]): string`**

The `makeLink` method returns safety email address link. The `$email` parameter is mandatory and accepts your raw email address.
Then you can specify another two optional parameters: 
- `$attributes` - it's array parameter and accepts all HTML attributes, which will be joined to the returned link
- `$print` - it's format for printing, you can choose from following: `EmailObfuscator::PRINT_METHOD_JS`, `EmailObfuscator::PRINT_METHOD_CSS` 
or `EmailObfuscator::PRINT_METHOD_ENCODE`

Return example: 
```php
use App\Helpers\EmailObfuscator;

// <a href="mailto:" onclick="event.preventDefault();window.location.href='mailto:' + 'te'+'st'+'y@'+'em'+'ai'+'l.'+'cz';"><script>document.write('te'+'st'+'y@'+'em'+'ai'+'l.'+'cz')</script></a>
echo EmailObfuscator::makeLink('testy@email.cz');
```

**`EmailObfuscator::safeJsPrint(string $email[, int $chunkSize = 2]): string`**

The `safeJsPrint` method returns safety email address in Javascript source code. The `$email` parameter is mandatory and accepts your raw email address.
Then you can specify another one optional parameter: 
- `$chunkSize` - it's integer parameter, which defines, how many letters will be in one chunk

Return example: 
```php
use App\Helpers\EmailObfuscator;

// <script>document.write('te'+'st'+'y@'+'em'+'ai'+'l.'+'cz')</script>
echo EmailObfuscator::safeJsPrint('testy@email.cz');
```

**`EmailObfuscator::safeCssPrint(string $email[, string $tag = 'span']): string`**

The `safeCssPrint` method returns safety email address in reserved text with CSS rules. The `$email` parameter is mandatory and accepts your raw email address.
Then you can specify another one optional parameter: 
- `$tag` - it's string parameter, which defines returned HTML element

Return example: 
```php
use App\Helpers\EmailObfuscator;

// <span style='unicode-bidi:bidi-override;direction:rtl;'>zc.liame@ytset</span>
echo EmailObfuscator::safeCssPrint('testy@email.cz');
```

**`EmailObfuscator::encode(string $email): string`**

The `encode` method returns safety email address in encoded output by randomly HTML tags. The `$email` parameter is mandatory and accepts your raw email address.

Return example: 
```php
use App\Helpers\EmailObfuscator;

// &#116;es&#116;y@&#101;&#109;&#97;&#105;&#108;&#46;c&#122;
echo EmailObfuscator::encode('testy@email.cz');
```

**`EmailObfuscator::safeComboPrint(string $email[, string $tag = 'span', int $chunkSize = 2]): string`**

The `safeComboPrint` method returns safety email address using combines JSS and CSS reserved text printing. The `$email` parameter is mandatory and accepts your raw email address.
Then you can specify another two optional parameters: 
- `$tag` - it's string parameter, which defines returned HTML elements
- `$chunkSize` - it's integer parameter, which defines, how many letters will be in one chunk

Return example: 
```php
use App\Helpers\EmailObfuscator;

// <span style='unicode-bidi:bidi-override;direction:rtl;'><script>document.write('zc'+'.l'+'ia'+'me'+'@y'+'ts'+'et')</script></span>
echo EmailObfuscator::safeComboPrint('testy@email.cz');
```

**`EmailObfuscator::replace(string $email[, $at = '&#064;', $dot = '&#046;']): string`**

The `replace` method returns safety email address using replacing characters `@` and `.`. The `$email` parameter is mandatory and accepts your raw email address.
Then you can specify another two optional parameters: 
- `$at` - it's string parameter, which replaces `@`
- `$dot` - it's string parameter, which replaces `.`

Return example: 
```php
use App\Helpers\EmailObfuscator;

// testy&#064;email&#046;cz
echo EmailObfuscator::replace('testy@email.cz');
```

**`EmailObfuscator::toJsExpression(string $email[, string $quote = "'", int $chunkSize = 2]): string`**

The `toJsExpression` method returns safety email address using splitting letters into chunks. The `$email` parameter is mandatory and accepts your raw email address.
Then you can specify another two optional parameters: 
- `$quote` - it's string parameter, which defines quotes around one chunk (important for string data type)
- `$chunkSize` - it's integer parameter, which defines, how many letters will be in one chunk

Return example: 
```php
use App\Helpers\EmailObfuscator;

// 'te'+'st'+'y@'+'em'+'ai'+'l.'+'cz'
echo EmailObfuscator::toJsExpression('testy@email.cz');
```

### Global Functions

**`Functions::createDateFromFormat($format, $date = null)`**

The `createDateFromFormat` method returns new `DateTime` object or null, when datetime cannot be created. The `$format` parameter is mandatory
and specifies, from which date format will be created new `DateTime` object. Then you can specify another one optional parameter: 
- `$date` - it's parameter, which can be either datetime object (`DateTime` or `Carbon`) or string datetime

Return example: 
```php
use App\Helpers\Functions;

// O:8:"DateTime":3:{s:4:"date";s:26:"2019-12-31 23:59:59.000000";s:13:"timezone_type";i:3;s:8:"timezone";s:13:"Europe/Prague";}
echo serialize(Functions::createDateFromFormat('Y-m-d H:i:s', '2019-12-31 23:59:59'));
```

**`Functions::combineTrans(array $keys): array`**

The `combineTrans` method returns array of keys and translation strings. The `$keys` parameter is mandatory and specifies translation keys, from which
you want to get final translation strings.

Return example: 
```php
use App\Helpers\Functions;

// {"general.language_name":"\u010ce\u0161tina","auth.failed":"P\u0159ihla\u0161ovac\u00ed \u00fadaje neodpov\u00eddaj\u00ed.","passwords.reset":"Va\u0161e heslo bylo resetov\u00e1no!"}
echo json_encode(Functions::combineTrans(['general.language_name', 'auth.failed', 'passwords.reset']));
```

**`Functions::combineTransToJson(array $keys): array`**

The `combineTransToJson` method returns array of keys and translation strings in JSON format. The `$keys` parameter is mandatory and specifies translation keys, from which
you want to get final translation strings.

Return example: 
```php
use App\Helpers\Functions;

// {"general.language_name":"\u010ce\u0161tina","auth.failed":"P\u0159ihla\u0161ovac\u00ed \u00fadaje neodpov\u00eddaj\u00ed.","passwords.reset":"Va\u0161e heslo bylo resetov\u00e1no!"}
echo Functions::combineTransToJson(['general.language_name', 'auth.failed', 'passwords.reset']);
```

**`Functions::arrayDistinctMerge(array &$array1, array &$array2): array`**

The `arrayDistinctMerge` method returns merge array of two input arrays. When here will be duplicated between these arrays about
key, a value in first array is replaced with a value from second array. The `$array1` and `$array2` parameters are mandatory and both of them accepts array value. 

Return example: 
```php
use App\Helpers\Functions;

$array1 = ["animal" => ["dog" => "Akita Inu"], 3];
$array2 = [6, "animal" => ["dog" => "Greyhound", "snake" => "Cobra"]];

print_r(Functions::arrayDistinctMerge($array1, $array2));
```
```text
Array
(
    [animal] => Array
        (
            [dog] => Greyhound
            [snake] => Cobra
        )

    [0] => 6
)
```

**`Functions::normalizeSearchText(string $text): string`**

The `normalizeSearchText` method returns text for searching comparison. The input search text is trimmed, stripped about tags,
removed diacritics and getting lowercase.

Return example: 
```php
use App\Helpers\Functions;

// we need to search something
echo Functions::normalizeSearchText('We need to search <strong>something</strong>  ');
```