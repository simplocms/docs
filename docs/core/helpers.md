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

Return example: TODO

**`EmailObfuscator::safeJsPrint(string $email[, int $chunkSize = 2]): string`**

The `safeJsPrint` method returns safety email address in Javascript source code. The `$email` parameter is mandatory and accepts your raw email address.
Then you can specify another one optional parameter: 
- `$chunkSize` - it's integer parameter, which defines, how many letters will be in one chunk

Return example: TODO

**`EmailObfuscator::safeCssPrint(string $email[, string $tag = 'span']): string`**

The `safeCssPrint` method returns safety email address in reserved text with CSS rules. The `$email` parameter is mandatory and accepts your raw email address.
Then you can specify another one optional parameter: 
- `$tag` - it's string parameter, which defines returned HTML element

Return example: TODO

**`EmailObfuscator::encode(string $email): string`**

The `encode` method returns safety email address in encoded output by randomly HTML tags. The `$email` parameter is mandatory and accepts your raw email address.

Return example: TODO

**`EmailObfuscator::safeComboPrint(string $email[, string $tag = 'span', int $chunkSize = 2]): string`**

The `safeComboPrint` method returns safety email address using combines JSS and CSS reserved text printing. The `$email` parameter is mandatory and accepts your raw email address.
Then you can specify another two optional parameters: 
- `$tag` - it's string parameter, which defines returned HTML elements
- `$chunkSize` - it's integer parameter, which defines, how many letters will be in one chunk

Return example: TODO

**`EmailObfuscator::replace(string $email[, $at = '&#064;', $dot = '&#046;']): string`**

The `replace` method returns safety email address using replacing characters `@` and `.`. The `$email` parameter is mandatory and accepts your raw email address.
Then you can specify another two optional parameters: 
- `$at` - it's string parameter, which replaces `@`
- `$dot` - it's string parameter, which replaces `.`

Return example: TODO

**`EmailObfuscator::toJsExpression(string $email[, string $quote = "'", int $chunkSize = 2]): string`**

The `toJsExpression` method returns safety email address using splitting letters into chunks. The `$email` parameter is mandatory and accepts your raw email address.
Then you can specify another two optional parameters: 
- `$quote` - it's string parameter, which defines quotes around one chunk (important for string data type)
- `$chunkSize` - it's integer parameter, which defines, how many letters will be in one chunk

Return example: TODO