
Translating Text
==============================================================================

## Translate

```hbs
<h2 id="title">{{t 'hello.world'}}</h2>
```

### HTML Element Attributes

```hbs
<input type='email' value='Some value' placeholder={{t 'hello.world'}}>
```

### Helper/Component Attributes

```hbs
{{input value=email placeholder=(t 'hello.world')}}
```

### Fallback Translations

The `t` helper, as of 3.0.0, supports a fallback lookup if the primary translation key is missing.  In the below example, the translation key `"actual_key"` would be used in place of the primary key, `"missing_key"`, if translation missing for key.

```hbs
{{t 'missing_key' default='actual_key'}}
```

```js
this.intl.t('missing_key', {
  default: ['does_not_exist', 'does_exist'] /* default can also be an Array */
});
```

## Computed Property Macro

_This feature is based on ember-i18n's `translationMacro`._

`translationMacro` defines a computed property macro that makes it easy to
define translated computed properties. For example,

```js
import Component from '@ember/component':
import { inject as service } from '@ember/service';
import { translationMacro as t } from 'ember-intl';

export default Component.extend({
  // Injecting the service is optional
  intl: service(),

  // A simple translation.
  title: t('user.edit.title'),

  followersCount: 1,

  // A translation with interpolations. This computed property
  // depends on `count` and will send `{ count: this.followersCount }`
  // in to the translation.
  followersTitle: t('user.followers.title', { count: 'followersCount' })
});
```

The first argument is the translation key. The second is a hash where the keys
are interpolations in the translation and the values are paths to the values
relative to this.

If `intl` is available on the host object (the component in this case), the
macro expects it to be the `intl` service. If there is no such service
injection, the macro will lookup the `intl` service via the owner of the host
object.

If you want to pass static values instead of paths to dynamic value, you can use
the [`raw` function](https://github.com/kellyselden/ember-macro-helpers#raw)
like in [`ember-awesome-macros`](https://github.com/kellyselden/ember-awesome-macros)
/ [`ember-macro-helpers`](https://github.com/kellyselden/ember-macro-helpers).

```js
import Component from '@ember/component':
import { inject as service } from '@ember/service';
import { translationMacro as t, raw } from 'ember-intl';

export default Component.extend({
  // Injecting the service is optional
  intl: service(),

  userName: 'Tom',

  // A translation with dynamic and static values
  title: t('user.edit.title', { name: 'userName', brand: raw('Ember') })
});
```

Note: Even though `raw` is _named_ the same as in `ember-awesome-macros` /
`ember-macro-helpers`, they _are not_ the same. Be sure to always use the
correct `raw` with `ember-intl` and `ember-awesome-macros` / `ember-macro-helpers`.
