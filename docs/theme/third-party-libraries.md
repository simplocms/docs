---
id: theme-third-party-libraries
title: Theme third party libraries
---

# Theme: third party libraries

In `ThemeServiceProvider` register:

```php
public function register(): void
{
   parent::register();
   $this->app->registerDeferredProvider(\Anhskohbo\NoCaptcha\NoCaptchaServiceProvider::class);
   $this->registerFacade('NoCaptcha', \Anhskohbo\NoCaptcha\Facades\NoCaptcha::class);
}
```

Publish config

`php artisan theme:publish --provider="Anhskohbo\NoCaptcha\NoCaptchaServiceProvider"`

Register config in boot

`$this->getThemeService()->registerSystemConfigFrom('captcha');`
