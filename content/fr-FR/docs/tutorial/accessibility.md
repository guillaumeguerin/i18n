# Accessibilité

Parce que développer des applications de manière accessible est importante, nous sommes heureux de vous présenter [Devtron](https://electronjs.org/devtron) et [Spectron](https://electronjs.org/spectron) qui donnent aux développeurs l'opportunité de faire de meilleures applications pour tous le monde.

* * *

Développer une application accessible avec Electron est similaire à la création d'un site web, car les deux fonctionnent avec de L'HTML . Avec une application Electron, cependant, vous ne pouvez pas utiliser de ressources en ligne pour vos audits sur l'accessibilité car votre application n'a pas d'URL sur laquelle un outil peut pointer.

De nouvelles fonctionalités apportent des outils d'audits pour votre application Electron. Vous pouvez choisir d'ajouter des audits à vos tests avec Spectron, ou bien de les utiliser à l'intérieur de DevTools avec Devtron. Continuez la lecture pour une brève introduction de ces outils.

## Spectron

Dans le framework de tests Spectron, vous pouvez maintenant auditer chaque fenêtre et `<webview>` tag dans votre application. Par exemple:

```javascript
app.client.auditAccessibility().then(function (audit) {
  if (audit.failed) {
    console.error(audit.message)
  }
})
```

Vous pouvez en savoir plus sur cette fonctionnalité dans la [documentation de Spectron](https://github.com/electron/spectron#accessibility-testing).

## Devtron

Dans Devtron, il y a un nouvel onglet d'accessibilité qui vous permettra d'auditer une page de votre application, et trier et filtrer les résultats.

![capture d’écran devtron](https://cloud.githubusercontent.com/assets/1305617/17156618/9f9bcd72-533f-11e6-880d-389115f40a2a.png)

Ces deux outils utilisent la librairie [Accessibility Developer Tools](https://github.com/GoogleChrome/accessibility-developer-tools) construite par Google pour Chrome. Vous pouvez en savoir plus sur les règles de l'acessibilité qui sont utilisées par cette librairie sur le [wiki du projet](https://github.com/GoogleChrome/accessibility-developer-tools/wiki/Audit-Rules).

Si vous connaissez d'autres outils mesurant l'accessibilité pour Electron, ajoutez les à la documentation avec une pull request.

## Activer l'accessibilité

Les applications Electron gardent l'accessibilité désactivée par défault pour des raisons de performances, mais il y a plusieurs façons de la réactiver.

### Dans l'application

En utilisant [`app.setAccessibilitySupportEnabled(enabled)`](../api/app.md#appsetaccessibilitysupportenabledenabled-macos-windows), vous pouvez exposer un bouton pour activer l'acessibilité à vos utilisateurs dans les options de votre application. Les outils d'assistance du système de l'utilisateur auront la priorité et s'occuperont de votre application.

### Technologies d’assistance

Les applications Electron application activeront l'accessibilité de manière automatique quand ils détectent une technologie d'assistance (Windows) ou VoiceOver (macOS). Voir la documentation de Chrome sur [l'accessibilité](https://www.chromium.org/developers/design-documents/accessibility#TOC-How-Chrome-detects-the-presence-of-Assistive-Technology) pour plus de détails.

Sous macOS, des outils externes peuvent activer l'accessibilité à l'intérieur des applications Electron modifiant le paramètre `AXManualAccessibility` :

```objc
CFStringRef kAXManualAccessibility = CFSTR("AXManualAccessibility");

+ (void)enableAccessibility:(BOOL)enable inElectronApplication:(NSRunningApplication *)app
{
    AXUIElementRef appRef = AXUIElementCreateApplication(app.processIdentifier);
    if (appRef == nil)
        return;

    CFBooleanRef value = enable ? kCFBooleanTrue : kCFBooleanFalse;
    AXUIElementSetAttributeValue(appRef, kAXManualAccessibility, value);
    CFRelease(appRef);
}
```
