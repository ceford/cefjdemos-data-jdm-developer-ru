<!-- Filename: J4.x:Http_Header_Management / Display title: Заголовки HTTP -->

## Введение

Joomla 4 представила систему HTTP-заголовков, предназначенную для помощи владельцам сайтов в настройке HTTP-заголовков безопасности. Она реализована с помощью плагина *System - HTTP Headers*. На эту тему существует подробное [руководство для пользователей](jdocmanual?article=user/security/http-headers). Это руководство короче и охватывает некоторые моменты, важные для разработчиков.

## Система - Плагин HTTP Headers

### Вкладка плагина

Перейдите в **System → Plugins → System - HTTP Headers**, чтобы получить доступ к форме настройки плагина.

![System http headers plugin form](../../../en/images/security/security-http-headers-plugin.png)

- **X-Frame Options** Включено по умолчанию, но [документация](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/X-Frame-Options) утверждает, что этот заголовок устарел и вместо него следует использовать политику *frame-ancestors*.
- **Referrer-Policy** Значение по умолчанию - *strict-origin-when-cross-origin*.
- **Cross-Origin-Opener-Policy** Значение по умолчанию в Joomla - `same-origin`.
- **Force HTTP Headers** По умолчанию никакие заголовки не устанавливаются принудительно. Здесь можно указать альтернативу *X-Frame Options*. Значение `Content-Security-Policy` может быть одним из:
    - `'none'`
    - `'self' https://www.example.org;`
    - `'self' https://example.org https://example.com https://store.example.com;`

Используя подформу **Force HTTP Headers**, вы также можете принудительно установить следующие заголовки:

- [Политика безопасности контента](https://scotthelme.co.uk/content-security-policy-an-introduction/)
- [Отчет только о политике безопасности контента](https://scotthelme.co.uk/content-security-policy-an-introduction/#testingapolicy)
- [Политика открытых соединений между источниками](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Cross-Origin-Opener-Policy)
- [Expect-CT](https://scotthelme.co.uk/a-new-security-header-expect-ct/)
- [Политика функций и политика разрешений](https://scotthelme.co.uk/a-new-security-header-feature-policy/)
- [NEL (Журналирование сетевых ответов)](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/NEL)
- [Политика рефереров](https://scotthelme.co.uk/a-new-security-header-referrer-policy/)
- [Report-to](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
- [Строгая политика транспортной безопасности](https://scotthelme.co.uk/hsts-the-missing-link-in-tls/)
- [Параметры X-Frame](https://scotthelme.co.uk/hardening-your-http-response-headers/#x-frame-options)

### Вкладка Strict-Transport-Security (HSTS)

![strict transport security settings](../../../en/images/security/security-http-headers-hsts.png)

Используйте кнопку *Переключить встроенную помощь* для получения информации о каждом параметре. Иллюстрированная справка:

[Форма для проверки или установки статуса HSTS preload и соответствия требованиям](https://hstspreload.org/)

### Вкладка Content-Security-Policy (CSP)

![Content security policy options](../../../en/images/security/security-http-headers-csp.png)

Как только включено, вы можете установить клиента, где вы хотите применить настроенный CSP, что позволяет вам установить `site`, `administrator` или `both`. CSP должен применяться как к фронтенду, так и к бэкенду. Иллюстрированные ссылки:

- [Политика безопасности содержимого](https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP)
- [Nonce](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/nonce)
- [Хэши скриптов и стилей](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/script-src)
- [Описание директив политики](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Content-Security-Policy/child-src) src доступно в меню этой страницы.

Последний параметр, называемый *Add Directive*, позволяет настроить список разрешенных источников для каждой директивы в соответствии с вашими потребностями. Например, для директивы *script-src* поле *Value* должно содержать источники, с которых вы хотите разрешить загрузку скриптов.

## Заметки

Когда вы настроили некоторые HTTP-заголовки безопасности непосредственно на сервере, могут возникнуть дублирующиеся записи.

Проверьте вывод заголовков HTTP в консоли браузера. В Google Chrome или Firefox: Инструменты разработчика → Сеть → вывод под заголовками. Затем вы можете отключить заголовки, которые вызывают дублирующиеся записи. Также проверьте консоль вашего браузера на возможные ошибки.

## Разработчики расширений

Большое преимущество Политики безопасности содержимого заключается в том, что заголовок блокирует весь встроенный JavaScript и встроенный CSS, влияющие на обработчики событий JavaScript через атрибуты HTML. При включенной защите браузера встроенный JavaScript и встроенный CSS также блокируются для расширений. Эта защита не включена по умолчанию, но может быть включена пользователями.

Где требуется встроенный JavaScript и CSS, поддержка одноразового значения (nonce) и хешей включена в API документа. При использовании ядро удостоверится, что они внесены в белый список, но при этом будет блокировать вредоносный код.

### Важные примечания для разработчиков расширений

Начиная с Joomla 4.0, Политика Безопасности Содержимого:

- поставляется с ядром.
- отключено по умолчанию.
- может быть включено администраторами сайта.
- настоятельно рекомендуется, чтобы ваш фронтенд и бэкенд расширения работали с включенной политикой безопасности контента.

При строгой политике безопасности контента будут заблокированы следующие функции:

- Выполнение JavaScript через обработчики событий HTML (обработчики onXXX, такие как onClick и подобные).
- Выполнение JavaScript-кода на странице, не переданного на страницу через API документа.
- Выполнение JavaScript-кода, внедренного в API DOM, такие как eval().
- Использование встроенного CSS на странице, не переданного на страницу через API документа.
- Использование встроенного CSS с использованием атрибута style в HTML.

Чтобы ваши расширения работали даже с включенной строгой политикой безопасности контента, самым простым способом является использование API документа для применения встроенного JavaScript и CSS. См. примеры ниже.

### Добавление JavaScript с использованием API Joomla

```php
    use Joomla\CMS\Factory;

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Add JavaScript from URL
    $wa->registerAndUseScript('com_example.sample', 'https://example.org/sample.js', [], ['defer' => true]);

    // Add inline JavaScript
    $wa->addInlineScript('
        document.addEventListener("DOMContentLoaded", function(event) {
            alert("An inline JavaScript Declaration");
        });
    ');
```

### Добавление CSS с использованием API Joomla

```php
    use Joomla\CMS\Factory;

    /** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
    $wa = Factory::getApplication()->getDocument()->getWebAssetManager();

    // Add Style from URL
    $wa->registerAndUseStyle('com_example.sample', 'https://example.org/sample.css');

    // Add inline Style
    $wa->addInlineStyle('
        body {
            background: #00ff00;
            color: rgb(0,0,255);
        }
    ');
```

## Дополнительные ресурсы

- [CSP Cheat Sheet](https://scotthelme.co.uk/csp-cheat-sheet/)
- [Content Security Policy - An Introduction](https://scotthelme.co.uk/content-security-policy-an-introduction/)
- [Security Headers](https://securityheaders.com/) является частью Probely и был изначально создан Скоттом Хелмом! Это бесплатный и простой в использовании инструмент, который поможет вам лучше внедрять и понимать современные функции безопасности, доступные для вашего веб-сайта.
- [CSP Evaluator](https://csp-evaluator.withgoogle.com/)
- [Web Fundamentals Content Security Policy](https://developers.google.com/web/fundamentals/security/csp)
- [Google's CSP Documentation](https://csp.withgoogle.com/docs/index.html)
- [CSP Is Dead, Long Live CSP!](https://research.google/pubs/pub45542/) О небезопасности белых списков и будущем политики безопасности содержимого
- [web.dev поиск Security](https://web.dev/s/results?q=security#gsc.tab=0&gsc.q=security&gsc.sort=)

*Переведено openai.com*

