# `<router-link>`

`<router-link>` — это компонент, используемый для создания ссылок в приложениях, использующих клиентский роутинг. Целевой путь указывается во входном параметре `to`. По умолчанию используется тег `<a>` и параметр `href`, но это поведение можно изменить указанием входного параметра `tag`. Кроме того, для ссылки автоматически устанавливается CSS-класс при активации соответствующего пути.

Использование `<router-link>` предпочтительнее, чем создание ссылок `<a href="...">` вручную, по следующим причинам:

- Единообразие вне зависимости от используемого режима навигации (HTML history или хеши), из-за чего не потребуется вносить изменения в код, если вы захотите сменить режим (или если приложение окажется открыто в IE9, не поддерживающем HTML history mode).

- В HTML5 history mode, `router-link` перехватывает клики, что предотвращает попытки браузера перезагрузить страницу.

- При использовании опции `base` в HTML5 history mode нет необходимости включать префиксы URL при указании входных параметров `to`.


### Входные параметры

- **to**

  - тип: `string | Location`

  - обязательный

  Указывает целевой путь ссылки. При клике, значение `to` будет передано в `router.push()` — поэтому им может быть как строка, так и описывающий путь объект.

  ``` html
  <!-- строка -->
  <router-link to="home">Home</router-link>
  <!-- результат рендеринга -->
  <a href="home">Home</a>

  <!-- javascript-выражение через `v-bind` -->
  <router-link v-bind:to="'home'">Home</router-link>

  <!-- Можно использовать сокращенную запись `v-bind` -->
  <router-link :to="'home'">Home</router-link>

  <!-- даст тот же результат -->
  <router-link :to="{ path: 'home' }">Home</router-link>

  <!-- с использованием именованного пути -->
  <router-link :to="{ name: 'user', params: { userId: 123 }}">User</router-link>

  <!-- с использованием строки запроса, получим `/register?plan=private` -->
  <router-link :to="{ path: 'register', query: { plan: 'private' }}">Register</router-link>
  ```

- **replace**

  - тип: `boolean`

  - значение по умолчанию: `false`

  При указании параметра `replace` вместо `router.push()` при клике будет вызываться `router.replace()`, и переход не оставит следов в браузерной истории.

  ``` html
  <router-link :to="{ path: '/abc'}" replace></router-link>
  ```

- **append**

  - тип: `boolean`

  - значение по умолчанию: `false`

  Указание параметра `append` заставляет считать пути по умолчанию относительными. Например, при переходе с `/a` по относительной ссылке `b`, без указания `append` мы получим `/b`, а с указанием этого параметра — `/a/b`.

  ``` html
  <router-link :to="{ path: 'relative/path'}" append></router-link>
  ```

- **tag**

  - тип: `string`

  - значение по умолчанию: `"a"`

  Время от времени хотелось бы, чтобы `<router-link>` использовал иной тег, например `<li>`. Мы можем использовать входной параметр `tag` для этих целей, и получившийся элемент всё так же будет реагировать на клики для навигации.

  ``` html
  <router-link to="/foo" tag="li">foo</router-link>
  <!-- отобразится как -->
  <li>foo</li>
  ```

- **active-class**

  - тип: `string`

  - значение по умолчанию: `"router-link-active"`

  В этом параметре можно изменить CSS-класс, применяемый к активным ссылкам. Обратите внимание, что значение по умолчанию тоже может быть изменено при помощи опции `linkActiveClass` конструктора роутера.

- **exact**

  - тип: `boolean`

  - значение по умолчанию: `false`

  По умолчанию активность ссылки устанавливается по стратегии **совпадения по включению**. Например, для `<router-link to="/a">` класс активности будет применён для всех ссылок, начинающихся с `/a/` или `/a`.

  Одним из следствий этого подхода является тот факт, что корневая ссылка `<router-link to="/">` будет считаться активной всегда. Чтобы заставить ссылку считаться активной только при полном совпадении, используйте входной параметр `exact`:

  ``` html
  <!-- эта ссылка будет активной только для корневого пути `/` -->
  <router-link to="/" exact>
  ```

  Больше примеров с подробными объяснениями использования класса активности можно найти [здесь](http://jsfiddle.net/fnlCtrl/dokbyypq/).

- **event**

  > 2.1.0+

  - тип: `string | Array<string>`

  - значение по умолчанию: `'click'`

  Указывает событие(я), способные вызвать переход по ссылке.

- **exact-active-class**

  > Добавлено в версии 2.5.0+

  - Тип: `string`

  - По умолчанию: `"router-link-exact-active"`

  Укажите активный CSS-класс, применяемый когда ссылка активна с точным соответствием маршрута. Обратите внимание, что значение по умолчанию также может быть настроено глобально с помощью опции `linkExactActiveClass` в конструкторе VueRouter.

### Применение класса активности ко внешнему элементу

Иногда хочется применить класс активности не к самому тегу `<a>`, а к другому элементу. Для этих целей можно использовать `<router-link>` для наружного элемента, а ссылку разместить внутри, вручную:

``` html
<router-link tag="li" to="/foo">
  <a>/foo</a>
</router-link>
```

Vue-router поймёт, что в качестве ссылки нужно использовать вложенный элемент (и укажет правильное значение `href` для неё), но вот класс активности будет применяться ко внешнему элементу `<li>`.