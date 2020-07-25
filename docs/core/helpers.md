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
because of a security issue. Robots can scan your website, get and save this email address and then send spam posts to this email.
If you want to avoid these potential security policies, it's necessary to show email address with safety way and don't show email address
in a raw format.

For these purpose, SIMPLO CMS offers `App\Helpers\EmailObfuscator` helper class with a few helper methods for different display
format of email addresses.

**`EmailObfuscator::makeLink(string $email[, array $attributes = [], string $print = self::PRINT_METHOD_JS]): string`**

The `makeLink` method returns safety email address link. The `$email` parameter is mandatory and accepts your raw email address.
Then you can specify another two optional parameters: 
- `$attributes` - it's an array parameter and accepts all HTML attributes, which will be joined to the returned link
- `$print` - it's a format for printing, you can choose from following: `EmailObfuscator::PRINT_METHOD_JS`, `EmailObfuscator::PRINT_METHOD_CSS` 
or `EmailObfuscator::PRINT_METHOD_ENCODE`

Return example: 
```php
use App\Helpers\EmailObfuscator;

// <a href="mailto:" onclick="event.preventDefault();window.location.href='mailto:' + 'em'+'ai'+'l@'+'si'+'mp'+'lo'+'.c'+'z';">
echo EmailObfuscator::makeLink('email@simplo.cz');
```

**`EmailObfuscator::safeJsPrint(string $email[, int $chunkSize = 2]): string`**

The `safeJsPrint` method returns safety email address in Javascript source code. The `$email` parameter is mandatory and accepts your raw email address.
Then you can specify another one optional parameter: 
- `$chunkSize` - it's an integer parameter, which defines, how many letters will be in one chunk

Return example: 
```php
use App\Helpers\EmailObfuscator;

// <script>document.write('em'+'ai'+'l@'+'si'+'mp'+'lo'+'.c'+'z')</script>
echo EmailObfuscator::safeJsPrint('email@simplo.cz');
```

**`EmailObfuscator::safeCssPrint(string $email[, string $tag = 'span']): string`**

The `safeCssPrint` method returns safety email address in reserved text with CSS rules. The `$email` parameter is mandatory and accepts your raw email address.
Then you can specify another one optional parameter: 
- `$tag` - it's a string parameter, which defines returned HTML element

Return example: 
```php
use App\Helpers\EmailObfuscator;

// <span style='unicode-bidi:bidi-override;direction:rtl;'>zc.olpmis@liame</span>
echo EmailObfuscator::safeCssPrint('email@simplo.cz');
```

**`EmailObfuscator::encode(string $email): string`**

The `encode` method returns safety email address in encoded output by randomly HTML tags. The `$email` parameter is mandatory and accepts your raw email address.

Return example: 
```php
use App\Helpers\EmailObfuscator;

// e&#109;a&#105;&#108;@&#115;&#105;m&#112;l&#111;&#46;cz
echo EmailObfuscator::encode('email@simplo.cz');
```

**`EmailObfuscator::safeComboPrint(string $email[, string $tag = 'span', int $chunkSize = 2]): string`**

The `safeComboPrint` method returns safety email address using combines JS and CSS reserved text printing. The `$email` parameter is mandatory and accepts your raw email address.
Then you can specify another two optional parameters: 
- `$tag` - it's a string parameter, which defines returned HTML elements
- `$chunkSize` - it's an integer parameter, which defines, how many letters will be in one chunk

Return example: 
```php
use App\Helpers\EmailObfuscator;

// <span style='unicode-bidi:bidi-override;direction:rtl;'><script>document.write('zc'+'.o'+'lp'+'mi'+'s@'+'li'+'am'+'e')</script></span>
echo EmailObfuscator::safeComboPrint('email@simplo.cz');
```

**`EmailObfuscator::replace(string $email[, $at = '&#064;', $dot = '&#046;']): string`**

The `replace` method returns safety email address using replacing characters `@` and `.`. The `$email` parameter is mandatory and accepts your raw email address.
Then you can specify another two optional parameters: 
- `$at` - it's a string parameter, which replaces `@`
- `$dot` - it's a string parameter, which replaces `.`

Return example: 
```php
use App\Helpers\EmailObfuscator;

// email&#064;simplo&#046;cz
echo EmailObfuscator::replace('email@simplo.cz');
```

**`EmailObfuscator::toJsExpression(string $email[, string $quote = "'", int $chunkSize = 2]): string`**

The `toJsExpression` method returns safety email address using splitting letters into chunks. The `$email` parameter is mandatory and accepts your raw email address.
Then you can specify another two optional parameters: 
- `$quote` - it's a string parameter, which defines quotes around one chunk (important for string data type)
- `$chunkSize` - it's an integer parameter, which defines, how many letters will be in one chunk

Return example: 
```php
use App\Helpers\EmailObfuscator;

// 'em'+'ai'+'l@'+'si'+'mp'+'lo'+'.c'+'z'
echo EmailObfuscator::toJsExpression('email@simplo.cz');
```

### Global Functions

In some cases, you can need to do something, what PHP functions cannot do. For these purposes, SIMPLO CMS contains a few static methods
for global issues.

**`Functions::createDateFromFormat($format[, $date = null])`**

The `createDateFromFormat` method returns a new `DateTime` object or null, when datetime cannot be created. The `$format` parameter is mandatory
and specifies, from which date format will be created new `DateTime` object. Then you can specify another one optional parameter: 
- `$date` - it's a parameter, which can be either datetime object (`DateTime` or `Carbon`) or string datetime

Return example: 
```php
use App\Helpers\Functions;

print_r(Functions::createDateFromFormat('Y-m-d H:i:s', '2019-12-31 23:59:59'));
```
```text
DateTime Object
(
    [date] => 2019-12-31 23:59:59.000000
    [timezone_type] => 3
    [timezone] => Europe/Prague
) 
```

**`Functions::combineTrans(array $keys): array`**

The `combineTrans` method returns array of keys and translation strings. The `$keys` parameter is mandatory and specifies translation keys, from which
you want to get final translation strings.

Return example: 
```php
use App\Helpers\Functions;

print_r(Functions::combineTrans(['general.language_name', 'auth.failed', 'passwords.reset']));
```
```text
Array
(
    [general.language_name] => English
    [auth.failed] => These credentials do not match our records.
    [passwords.reset] => Your password has been reset!
) 
```

**`Functions::combineTransToJson(array $keys): array`**

The `combineTransToJson` method returns array of keys and translation strings in JSON format. This format is useful for Javascript libraries, ex. in Vue.js. 
The `$keys` parameter is mandatory and specifies translation keys, from which you want to get final translation strings.

Return example: 
```php
use App\Helpers\Functions;

// {"general.language_name":"English","auth.failed":"These credentials do not match our records.","passwords.reset":"Your password has been reset!"}
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

The `normalizeSearchText` method returns text for a searching comparison. The input search text is trimmed, stripped about tags,
removed diacritics and getting lowercase.

Return example: 
```php
use App\Helpers\Functions;

// we need to search something
echo Functions::normalizeSearchText('We need to search <strong>something</strong>  ');
```

**`Functions::removeDiacritics(string $text): string`**

The `removeDiacritics` method returns text without diacritics. The `$text` parameter is mandatory and accepts text for 
removing diacritics.

Return example: 
```php
use App\Helpers\Functions;

// SIMPLO CMS je skvely system!
echo Functions::removeDiacritics('SIMPLO CMS je skvělý systém!');
```

**`Functions::compareDates(?Carbon $first, ?Carbon $second[, bool $nullIsInf = false]): int`**

The `compareDates` method compares two Carbon datetime objects and returns integer value according to the result of comparison. The return
possible values are following:
- `1` - the first value is greater than the second
- `0` - the both of them are equal
- `-1` - the second value is greater than the first

The `$first` and `$second` are mandatory parameters and accept `Carbon` datetime objects. Then, here is also one optional parameter:
- `$nullIsInf` - if the value is `false` and `$first` is null, then the method returns `-1`, otherwise if `$second` is null, then the method
returns `1`

Return example: 
```php
use App\Helpers\Functions;
use Carbon\Carbon;

$first = new Carbon('2019-12-31 10:00:00');
$second = new Carbon('2019-12-31 11:00:00');

// returns -1, because the $second is greater
echo Functions::compareDates($first, $second);
```

**`Functions::sanitizeHtmlClass(string $class[, string $fallback = '']): string`**

The `sanitizeHtmlClass` method removes special characters from HTML class name and returns just sanitize HTML class name. 
The `$class` is a mandatory parameter and accepts a name of HTML class, which will be sanitized. Then, you can use one optional parameter:
- `$fallback` - it's a fallback HTLM class name, which it will be used, when a sanitized HTML class name will be empty

Return example: 
```php
use App\Helpers\Functions;

// HTMLClassName
echo Functions::sanitizeHtmlClass('HTML Class Name');
```

**`Functions::associativeArrayToSequentialArray(array $input[, string $keyName = 'id', string $labelName = 'label', string $childrenName = null]): array`**

The `associativeArrayToSequentialArray` method transforms an associative array to sequential array. The `$input` parameter is mandatory and accepts input array for transformation. 
Then, the method accepts another three optional parameters:
- `$keyName` - it's a name of the key for each item
- `$labelName` - it's a name of the label for each item
- `$childrenName` - when an item inside the associative array will be array, then for named these items will be used this optional parameter

Return example: 
```php
use App\Helpers\Functions;

$input = [1 => 'Lukas', 2 => 'Peter', 3 => 'Ruslana', 4 => 'George', 5 => [23 => 'John', 26 => 'Patricia', 38 => 'Patrick']];

print_r(Functions::associativeArrayToSequentialArray($input, 'id', 'name', 'names'));
```
```text
Array
(
    [0] => Array
        (
            [id] => 1
            [name] => Lukas
        )

    [1] => Array
        (
            [id] => 2
            [name] => Peter
        )

    [2] => Array
        (
            [id] => 3
            [name] => Ruslana
        )

    [3] => Array
        (
            [id] => 4
            [name] => George
        )

    [4] => Array
        (
            [name] => 5
            [names] => Array
                (
                    [0] => Array
                        (
                            [id] => 23
                            [name] => John
                        )

                    [1] => Array
                        (
                            [id] => 26
                            [name] => Patricia
                        )

                    [2] => Array
                        (
                            [id] => 38
                            [name] => Patrick
                        )

                )

        )

)
```

**`Functions::joinPaths(string ...$paths): string`**

The `joinPaths` method returns joined given paths with a system separator. The `$paths` parameter is mandatory and accepts the unlimited paths for joining.

Return example: 
```php
use App\Helpers\Functions;

// public/upload/media/articles
echo Functions::joinPaths('public/upload', 'media/articles');
```

**`Functions::getClassName($classOrInstance): string`**

The `getClassName` method returns the short name of the given class or the given object. The `$classOrInstance` is a mandatory parameter and accepts either a class name or
an object.

Return example: 
```php
use App\Helpers\Functions;

// Carbon
echo Functions::getClassName('Carbon\Carbon');
```

**`Functions::mergeValidationRules(array ...$args): array`**

The `mergeValidationRules` method merges given validation rules. The `$args` parameter is mandatory and accepts validation rule arrays.

Return example: 
```php
use App\Helpers\Functions;

$validationRules1 = ['name' => ['required', 'max:128'], 'email' => ['required', 'email']];
$validationRules2 = ['surname' => ['max:128']];

print_r(Functions::mergeValidationRules($validationRules1, $validationRules2));
```
```text
Array
(
    [name] => Array
        (
            [0] => required
            [1] => max:128
        )

    [email] => Array
        (
            [0] => required
            [1] => email
        )

    [surname] => Array
        (
            [0] => max:128
        )

)
```

**`Functions::mergeValidationMessages(...$args): array`**

The `mergeValidationMessages` method merges given validation messages. The `$args` parameter is mandatory and accepts either validation message arrays or
the key of translated validation messages.

Return example: 
```php
use App\Helpers\Functions;

$validationMessages1 = ['name.required' => 'Your name is required.'];
$validationMessages2 = ['surname.max' => 'Your surname may not be greater than :max characters.'];

print_r(Functions::mergeValidationMessages($validationMessages1, $validationMessages2, 'validation.seo'));
```
```text
Array
(
    [name.required] => Your name is required.
    [surname.max] => Your surname may not be greater than :max characters.
    [seo_title.max] => Maximal length of the SEO title should be :max characters.
    [seo_description.max] => Maximal length of the SEO description should be :max characters.
)
```

**`Functions::valuesToAssocArray(...$values): array`**

The `valuesToAssocArray` method encapsulated values into an associative array. The `$values` parameter is mandatory and accepts either array or scalar values.

Return example: 
```php
use App\Helpers\Functions;

print_r(Functions::valuesToAssocArray('green', 'blue', 'brown'));
```
```text
Array
(
    [green] => green
    [blue] => blue
    [brown] => brown
)
```

**`Functions::valuesToCollection(...$values): Collection`**

The `valuesToCollection` method encapsulated values into collection. The `$values` parameter is mandatory and accepts either array or scalar values.

Return example: 
```php
use App\Helpers\Functions;

print_r(Functions::valuesToCollection('green', 'blue', 'brown'));
```
```text
Illuminate\Support\Collection Object
(
    [items:protected] => Array
        (
            [green] => green
            [blue] => blue
            [brown] => brown
        )

)
```

### URL

SIMPO CMS also offers for developers helper in working with url addresses - `UrlHelper` class.

**`UrlHelper::normalizeUri(string $uri): string`**

The `normalizeUri` method returns only an url address without query string or ending `/` character. The `$uri` parameter is mandatory and
represents a given url address for a normalizing process. A normalized url address is good for matching with another normalized url addresses.

Return example: 
```php
use App\Helpers\UrlHelper;

// https://www.simplocms.com
echo UrlHelper::normalizeUri('https://www.simplocms.com?id=5');
```

### Google Analytics

Google Analytics is one of the most favourite and used global analytic tools for websites. SIMPLO CMS offers for developers a `GAHelper`
class with helpful methods in working with Google Analytics data. The `GAHelper` class has only methods and every object of this class represents
one connection to the account of Google Analytics using access token.

**`GAHelper::__construct(array $token, $profileId)`**

The `GAHelper` constructor accepts two mandatory parameters:
- `$token` - an array value with an access token to the Google Analytics account
- `$profileId` - a value with a profile id of the Google Analytics account

**`GAHelper::getData($from, $to, $metrics[, array $options = []])`**

The `getData` method returns `Google_Service_Analytics_GaData` object with fetch data or null, when during fetching data process will throw
exception. There are three mandatory parameters:
- `$from` - from which a datetime you want to fetch Google Analytics data
- `$to` - to which a datetime you want to fetch Google Analytics data
- `$metrics` - a comma-separated list of Google Analytics metrics, ex: 'ga:sessions,ga:pageviews'

You can also use optional `$options` parameter for setting another Google Analytics possible options, ex: 'ga:browser,ga:city'.

**`GAHelper::hasErrors()`**

The `hasErrors` method returns `true` value, when during fetching process it happened some problem.

**`GAHelper::getErrors()`**

The `getErrors` method returns `Collection` with errors, what happened during fetching process. If the `Collection` is empty, then
everything was successful.