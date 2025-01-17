<!-- Filename: J4.x:Web_Assets / Display title: Веб-активы -->

## О веб-ресурсах

Полное описание веб-активов можно найти в документации программистов Joomla!, воспроизведенной на [этом сайте](jdocmanual?article=docus/web-asset-manager/index) или доступной с [оригинального источника](https://manual.joomla.org/docs/general-concepts/web-asset-manager/).

## Дополнительные примеры

В Jdocmanual многие статьи содержат блоки кода, которые выигрывают от подсветки синтаксиса. Вот как это достигается:

```php
/** @var Joomla\CMS\WebAsset\WebAssetManager $wa */
$wa = $this->document->getWebAssetManager();

// Register and attach a custom item in one run
$wa->registerAndUseStyle('highlight', 'https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/styles/default.min.css', [], [], []);

// Register and attach a custom item in one run
$wa->registerAndUseScript('highlight','https://cdnjs.cloudflare.com/ajax/libs/highlight.js/11.9.0/highlight.min.js', [], [], ['core']);

// Add an inline content as usual, will be rendered in flow after all assets
$wa->addInlineScript('hljs.highlightAll();');
```

Этот код находится в файле `default.php`, который используется для отображения этой страницы! Его внедрение немного сложно, потому что вызов `hljs.highlightAll()` должен быть выполнен после загрузки файлов `css` и `js`, а также каждый раз при изменении содержимого страницы с помощью вызова JavaScript.

Другие подсветчики синтаксиса также доступны!

*Переведено openai.com*

