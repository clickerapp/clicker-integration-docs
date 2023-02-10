---
sidebar_position: 2
---

# Возможности интеграции

Clicker - это сервис установки и сборки товаров для ритейлеров и брендов. Этот документ описывает технические аспекты интеграции с интернет-магазином.

При заключении договора мы предоставим вам **shopId** — идентификатор вашего магазина в системе Clicker

Интеграция позволяет:

<details> 
  <summary>
      <strong>Отображать виджет на страницах "карточка товара" и "корзина", и автоматически передавать заказы на установку в систему Clicker</strong>
  </summary>

Эта функция позволяет вам отображать галочку "Профессиональная установка" рядом с товаром на страницах "Карточка товара" или "Корзина". Также будет отображаться псевдоссылка, при нажатии на которую откроется pop-up окно с ценой и условиями оказания услуги. При переходе на страницу "Спасибо", заказ будет передан в систему Clicker.

![Пример элемента Clicker на странице "Карточка товара".](/img/widgets/1.___ver2_22_06_2020.png)

_Пример элемента Clicker на странице "Карточка товара"._
<br/>

![Пример элемента Clicker на странице "Корзина". Краткий вариант стоимости.](/img/widgets/3_____ver2_22_06_2020.png)

_Пример элемента Clicker на странице "Корзина". Краткий вариант стоимости._
<br/>

![Пример элемента Clicker на странице "Корзина". Подробный вариант стоимости.](/img/widgets/4.______ver2_22_06_2020.png)

_Пример элемента Clicker на странице "Корзина". Подробный вариант стоимости._
<br/>

![Пример модального pop-up окна Clicker на странице "Корзина"](/img/widgets/Untitled.png)

_Пример модального pop-up окна Clicker на странице "Корзина"_
<br/>

Интеграция происходит посредством подключения js кода Clicker на страницы "Карточка товара", "Страница категории", "Корзина" и "Спасибо".

Инструкции по интеграции: [Подключение виджета на страницу "Карточка товара" Корзина" и "Спасибо"](#1-Podklyuchenie-popup-vidzheta-na-stranitsy-Kartochka-tovara-Korzina-i-Spasibo)
<br/><br/>

  </details>

<a name="iframe-profits"></a>

<details> 
          <summary>
              <strong>Отображать форму заказа услуг на отдельной странице "Установка и сборка"</strong>
          </summary>

Используется для отдельной информационной страницы вашего магазина ("Установка и настройка", "Дополнительные услуги" и пр.). Нужно для того, чтобы дать возможность покупателям ознакомиться с ценами и условиями оказания услуг до покупки товара. Или оформить заказ на услуги после покупки товара (например, покупатель думал, что установит товар самостоятельно, товар уже доставили, и в этот момент покупатель решил заказать услуги установки)

![Пример страницы с iframe Clicker](/img/widgets/img-2021-10-28-10-48-51.png)

_Пример страницы с iframe Clicker_

Форма создания заказа подключается через iframe виджет Clicker. Цветовая схема формы кастомизируется под стиль вашего сайта.

Инструкции по интеграции: [Установка iframe формы на отдельной странице "Установка и настройка"](#2-Ustanovka-iframe-formy-na-otdelnoi-stranitse-Ustanovka-i-nastroika)

  </details>

<br/>

# 1. Подключение popup виджета на страницы "Карточка товара", "Корзина" и "Спасибо"

Для инициализации скрипта, необходимо подключить его на страницы:

- **Карточка товара** - для отображения опции по установке ("галочки") в карточке товара (это опционально, но желательно)
- **Корзина** - для отображения опции по установке ("галочки") на странице корзина. Также необходимо подключить скрипт на все страницы подтверждения заказа (между "Корзиной" и "Спасибо").
- **Спасибо** - для того, чтобы заказ на установку был передан в Clicker в автоматическом режиме

Скрипт сконструирован таким образом, чтобы не влиять на скорость загрузки вашего сайта, так как работает асинхронно. Условно, интеграцию можно разделить на две части: страницы с чекбоксами, где покупатель будет выбирать товар и услугу к нему; и страница после создания заказа магазина (обычно такая страница называется "спасибо"), где будет инициировано создание заказа услуг на установку.

## 1.1. Подключение на страницу "Карточка товара"

### Установка скрипта

В любом месте страницы добавьте код с параметрами виджета:

```html
<script data-shop="myShop" data-city="Ekaterinburg" data-page="product" defer src="https://app.clicker.one/ClickerServices.js"></script>
```

Обмен информацией со скриптом происходит через передачу ему data-атрибутов тега script. Также существуют другие способы, но они будут описаны в главе [расширенные возможности установки скрипта](#advanced-script-installation).

**data-shop** [обязательно] - идентификатор вашего магазина в системе Clicker

**data-city** [обязательно] - город отображения виджета; [список доступных городов](#51-Goroda) (с учетом регистра)

**data-page** <a name="data-page"></a>[обязательно] - принимает строку, определяющую на какой странице запускается скрипт. В данном случае ожидается значение _product_ (_product_ страница товара или списка товаров; _cart_: страница корзины магазина).

При текущих настройках скрипт автоматически запустится при загрузке страницы и проставит в нужных местах чекбоксы ("галочки") с ценой (описано ниже) для заказа услуг установки. Если для вас не подходит такой вариант, например получение города у вас асинхронное, то другие способы подключения и опциональные передаваемые параметры описаны ниже в блоке "расширенные возможности установки скрипта".

<a name="advanced-script-installation"></a>

<details> 
          <summary>
              <strong>Расширенные возможности установки скрипта</strong>
          </summary>

Скрипт Clicker выставляет наружу объект _window.installationServiceState_, который имеет следующие свойства:

```ts
{
	// Общие
	shop: string;
	city: string;
	page: 'product' | 'cart' | 'thanks';
	orderSource?: string;
	// Страница с чекбоксами, настройки распространяются на все блоки
	pageTheme?: string;
	serviceCart: { ... }[];
	checkedByDefault?: boolean;
	currencySymbol?: string;
	modalButtonText?: string;
	tipText?: string;
	zeroPriceText?: string;
	decimalSeparator?: string;
	thousandSeparator?: string;
	decimalPlaces?: number;
	discount?: number;
	discountText?: string;
	beforeInitialization?: () => any;
	afterInitialization?: () => any;
	onCartChange?: (cart: { ... }[]) => any;
	resetScript: (page?: 'product' | 'cart') => Promise<void>;
	// Системные
	preventAutoRun?: boolean;
	createOrder: (cart: { ... }[]) => Promise<void>;
}
```

Таким образом при инициализации страницы вы можете передать данные не через дата-атрибуты, а напрямую в стейт, если так удобнее. Пример:

```html
<script>
  window.installationServiceState = {
    city: "Moscow",
    shop: "myShop",
    page: "product",
  };
</script>
<script defer src="https://app.clicker.one/ClickerServices.js"></script>
```

**Описание параметров**

Все дополнительные параметры можно передавать как в стейт, так и через дата-атрибуты, стандартным способом. Названия параметров идентичны, за исключением написания: дата-атрибуты требуют приписки data- и вместо пробелов ставится знак минус (-), а при передаче в стейт приставка data- не требуется, а само название параметра пишется в camelCase стиле.

**1.** Общие (дополнительные)

**orderSource** - если вам нужно знать в заказе из какого именно раздела/страницы был запущен скрипт (сделан заказ), то это можно указать здесь. Эти данные отобразятся в ЛК Clicker.

**2.** Страница с чекбоксами

Параметры страницы с чекбоксами дублируют данные, которые можно передавать отдельным блокам с чекбоксами (будет описано ниже в [соответствующей главе интеграции](#Bloki-uslug)), но распространяются сразу на все блоки страницы.

_Параметры для чтения:_

**resetScript** - метод заново запускает скрипт, проходя повторно весь цикл инициализации (нужно, если какие-то данные на странице изменились).
**createOrder** - метод создания заказа для ручного вызова, принимает в себя корзину и создаёт заказ (будет описано в соответствующей главе интеграции).
**serviceCart** - массив услуг, чекбоксы которых отметил пользователь на текущей странице (актуально на странице с чекбоксами)

_Принимаемые параметры:_

**pageTheme** - название цветовой темы; если не передано, будет использована тема магазина по умолчанию. Тема состоит из двух частей - цвета активных элементов и цвет фона страницы. Обе части темы записываются подряд через знак точки. Пример: _greenBright.white_ [Список всех доступных тем](#52-Dostupnye-tsvetovye-temy-vidzhetov).
**beforeInitialization** - принимает любую вашу функцию и запускает её прямо перед инициализацией скрипта на странице с чекбоксами.
**afterInitialization** - принимает любую вашу функцию и запускает её сразу после инициализации скрипта на странице с чекбоксами.
**onCartChange** - принимает любую вашу функцию и запускает её при добавлении/удалении пользователем услуг на странице с чекбоксами.
**preventAutoRun** \*\*- скрипт не запускается автоматически. Нужно если какие-то данные, необходимые для инициализации скрипта, требуется получать асинхронно. Затем, когда вы получите недостающие данные, воспользуйтесь методом _window.installationServiceState.resetScript()_, пример:

```js
window.installationServiceState = { // Проставляем известные на текущий момент данные
  shop: "myShop",
	page: 'product',
};

...

getCity().then(cityName => {       // Делаем асинхронный запрос и ждём его завершения
	window.installationServiceState.city = cityName;    // Проставляем полученные данные
	window.installationServiceState.resetScript();      // Запускаем скрипт Clicker
})
```

**Работа с корзиной**

Если вам требуется работать с корзиной услуг, которые выбрал пользователь, доступны следующие способы:

1. _window.installationServiceState.serviceCart -_ непосредственно сама корзина с данными по услугам (описание ниже)
2. Вы можете передать свою функцию свойству _window.installationServiceState.onCartChange_, которая будет вызываться при каждом изменении корзины и получать на вход обновлённую корзину.

<a name="service-data"></a>Корзина представляет из себя массив следующих объектов:

```js
productId: string;
serviceId: string;
title: string;
description?: string;
includedWorks?: string;
notIncludedWorks?: string;
price: number;
quantity: number;
discount: number;
discountPrice: number;
hasDiscount: boolean;
isFree: boolean;
```

**productId** - идентификатор товара, к которому относится услуга
**serviceId** - идентификатор услуги
**title** - название услуги
**description** - описание услуги
**includedWorks** - что входит в услугу
**notIncludedWorks** - что не входит в услугу
**price** - цена одной услуги
**quantity** - количество данной услуги у текущего товара
**discount** - скидка на услугу
**discountPrice** - цена одной услуги со скидкой
**hasDiscount** - если ли скидка (true если discount \> 0)
**isFree** - бесплатная ли услуга (true если discount = 100)

</details>

### Блоки услуг

Рядом с товаром, в том месте, где вы хотите отобразить "галочку" с опцией установки, вставьте следующий код. "Галочка" будет отображаться только на тех товарах, для которых доступна услуга установки. Скрытием и отображением "галочки" управляет скрипт Clicker, руководствуясь [правилами сопоставления товар→услуга](#4-Pravila-sopostavleniya-tovar-usluga), поэтому вам не нужно ставить дополнительные условия в коде, вы можете добавить элемент на все товары.

```html
<template
  data-installation-service-block
  data-product-id="ID2343423"
  data-title="Газовая плита GEFEST Брест 1200 C7 K8"
  data-url="https://shop.com/item/334"
  data-price="11450"
  data-quantity="1"
  data-match-properties="%5B%7B%22key%22%3A%22title%22%2C%22value%22%3A%22%D0%A1%D1%..."
></template>
```

**data-installation-service-block** [обязательный] - атрибут, по которому скрипт Clicker найдет блок и подставит в него нужные значения.

**data-product-id** [обязательный] - уникальный (!) идентификатор товара магазина (артикул)

**data-title** [обязательный] - название товара.

**data-url** - url страницы товара

**data-price** - цена товара в рублях

**data-quantity** - количество товара в корзине покупателя (если параметр не задан, то подставляется значение 1); допустимые значения: любое положительное число

**data-match-properties** [обязательный] <a name="data-match-properties"></a>- JSON массив объектов, по которым будет проводиться сопоставление товаров и услуг. Каждый объект содержит два параметра, key и value, и описывает одно свойство товара. Свойства товара могут быть любыми, это настраивается индивидуально под каждый магазин (наиболее часто используемые - категория товаров), но бывают и различные особенности (например, размер телевизора, мощность кондиционера, наличие гидромассажа у ванны и пр.). Логика сопоставления описана в [Правилах сопоставления товар → услуга](#4-Pravila-sopostavleniya-tovar-usluga). Пример массива:

```json
[
  {
    "key": "category", // этот параметр будет влиять на выбор услуги
    "value": "Стиральные машины"
  },
  {
    "key": "Размер (см)", // этот параметр не будет влиять на выбор услуги, но вы можете его оставить
    "value": 60
  },
  {
    "key": "Тип", // этот параметр будет влиять на выбор услуги
    "value": "Встраиваемая"
  },
  {
    "key": "Цвет", // этот параметр не будет влиять на выбор услуги, но вы можете его оставить
    "value": "Белый"
  }
]
```

**Важно!** Из-за особенностей стандарта дата атрибутов если вы передадите туда JSON строку, то может возникнуть конфликт символов кавычек с разметкой html. Поэтому рекомендуется обрабатывать JSON строку c помощью метода encodeURIComponent и получившийся результат передавать скрипту.

В случае если для блока будет найдена подходящая услуга, он станет видимым, а внутрь будет добавлена следующая разметка:

![Пример элемента Clicker на странице "Корзина"](/img/widgets/img-2021-10-28-16-44-35.png)

_Пример элемента Clicker на странице "Корзина"_

Если какое-то из обязательных полей блока заполнено неверно или услуги не найдены, то блок не отображается. Все ошибки пишутся в читаемой форме в консоль.

Если вам не подходят дефолтные тексты, то они задаются через передачу параметров блоку. Все HTML элементы имеют уникальные классы и легко кастомизируются через css. HTML разметку тоже можно полностью переделать, поместив её внутрь блока. Обо всех этих возможностях подробнее описано в "расширенных возможностях интеграции блока".

<a name="checkbox-block-additional"></a>

<details> 
            <summary>
                <strong>Расширенные возможности интеграции блока добавления услуги</strong>
            </summary>
    Любой блок может принимать следующие дата атрибуты, помимо стандартных (в примере показаны значения по умолчанию):

```html
<template
  ...
  data-checked-by-default="false"
  data-currency-symbol=" р."
  data-modal-button-text="Профессиональная установка: "
  data-decimal-places="0"
  data-decimal-separator="."
  data-thousand-separator=""
  data-discount="0"
  data-discount-text="Акция!"
  data-zero-price-text="Бесплатно"
  ;
  data-tip-text="Добавлено. Оплата после оказания услуги."
></template>
```

Эти же атрибуты принимает в себя тег script при интеграции виджета (а также объект _window.installationServiceState_, если вы используете интеграцию через стейт). Если конкретному блоку передано конкретное значение, то будет использовано оно. Если нет, то будет использовано значение, переданное скрипту при интеграции (т.е. глобальное значение). Если и там ничего не передано, то будет использовано значение по умолчанию (оно было показано в примере выше).

**data-checked-by-default** - услуга будет добавлена к заказу по умолчанию; допустимые значения: true/false.

**data-currency-symbol** - знак валюты, который будет использоваться рядом с ценой.

**data-decimal-places** - количество знаков в десятичной части цены (пример со значением '2' ⇒ 1990.00); допустимые значения: любое число от нуля и выше.

**data-decimal-separator** - разделить десятичной части цены (пример со значением ',' ⇒ 1990,00).

**data-thousand-separator** - разделитель тысяч целой части цены (пример со значением '.' ⇒ 1.990,00). По умолчанию отсутствует

**data-discount** - размер скидки в процентах; допустимые значения: число от 0 до 100.

**data-discount-text** - текст, указывающий покупателю, что на товар действует скидка.

**data-zero-price-text** - специальный текст для случаев, когда скидка равна 100%.

**data-modal-button-text** - текст кнопки вызова pop-up окна.

**data-tip-text** - текст, который появится после добавления услуги в корзину.

При загрузке скрипта на место элемента template будет помещён блок выбора услуг со следующей разметкой по умолчанию:

```html
<installation-service-block class="installation-service-block" data-installation-service-block>
  <div class="installation-service-block__main-container">
    <label class="installation-service-block__checkbox-label"
      ><input type="checkbox" class="installation-service-block__checkbox-button" data-is-sb-toggle-service-handler
    /></label>
    <strong class="installation-service-block__modal-button" data-is-sb-modal-handler></strong>
    <span class="installation-service-block__price-container">
      <span class="installation-service-block__price">
        <span class="installation-service-block__price-value" data-is-sb-price-value></span><span class="installation-service-block__price-currency"></span
      ></span>
      <strong class="installation-service-block__price-discount">
        <strong class="installation-service-block__discount-title"></strong>
        <strong class="installation-service-block__discount-zero-price-text"></strong>
        <span class="installation-service-block__discount-price">
          <span class="installation-service-block__discount-price-value" data-is-sb-discount-price-value></span
          ><span class="installation-service-block__discount-price-currency"></span
        ></span>
      </strong>
      <span class="installation-service-block__quantity-container">
        <span class="installation-service-block__quantity-before">(</span><span class="installation-service-block__quantity-value" data-is-sb-quantity></span
        ><span class="installation-service-block__quantity-after"> шт.)</span></span
      >
    </span>
  </div>
  <i class="installation-service-block__tip"></i>
</installation-service-block>
```

<a name="data-attrs"></a>Все блоки, которые отрисовывают данные с сервера или имеют обработчики событий, помечены дополнительно data-атрибутом. В случае, если вы будете использовать свою разметку внутри блока (будет описано далее), не забудьте указать как минимум эти атрибуты, названия классов указывать не обязательно. Однако без использования наших классов скрипт хоть и отрисует блок, но лишь с голой информацией и без стилей, остальное ложится на вашу верстку, в том числе и свойство display динамических элементов.

Ниже приведены все динамические элементы блока, заголовок выполнен в формате css запроса:

**[data-is-sb-toggle-service-handler].installation-service-block_checkbox-button** - чекбокс или кнопка или любой другой удобный вам элемент, активирует добавление/удаление услуги из корзины клиента (_window.installationServiceState.cart_). После добавления весь элемент _installation-service-block_ получает дополнительный класс _installation-service-block_checked_.

**[data-is-sb-modal-handler].installation-service-block_modal-button** - псевдоссылка, при нажатии на которую, открывается модальное окно с описанием услуги

**[data-is-sb-price-value].installation-service-block_price-value** <a name="installation-service-block_price-value"></a> - элемент, в который проставляется значение цены

.**installation-service-block_price-currency** - элемент, в который проставляется знак валюты

.**installation-service-block_price** - используется, чтобы работать с ценой, когда к ней применяется скидка (при стандартных стилях старая цена зачеркивается)

.**installation-service-block_price-discount** <a name="installation-service-block_price-discount"></a> - контейнер скидки, который становится видимым если у блока присутствует параметр _discount._ Также в этом случае весь элемент _installation-service-block_ получает дополнительный класс _installation-service-block_with-discount_, а если скидка равна 100 то ещё _installation-service-block_free_

**[data-is-sb-discount-price-value].installation-service-block_discount-price-value** - элемент, в который проставляется значение цены со скидкой. Будет виден, только если у блока присутствует параметр _discount._

.**installation-service-block_discount-price-currency** - элемент, в который проставляется знак валюты для цены со скидкой. Будет виден, только если у блока присутствует параметр _discount._

.**installation-service-block_discount-title** - параметр _data-discount-text_, указывающий, что на товар действует скидка. Будет виден, только если у блока присутствует параметр _discount._

.**installation-service-block_discount-zero-price-text** - параметр _data-zero-price-text._ Когда скидка равна 100%, отобразится вместо цены со скидкой.

.**installation-service-block_quantity-container** - контейнер с числовым количеством услуг, отображается только если количество больше 1 (добавляется и удаляется класс _installation-service-block_quantity-container_hidden_)

.**installation-service-block_quantity-value** - элемент, в который проставляется количество услуг

.**installation-service-block_tip** - параметр _data-tip-text_, который отобразится при добавлении услуги в корзину (добавляется и удаляется класс _installation-service-block_tip_hidden_).

**'installation-service-block-' + encodeURIComponent(thisBlock.productId)** - этот класс получает каждый блок, для которого нашлась услуга. Для каждого блока класс уникальный т.к. генерируется на основе productId блока

Также вы можете полностью изменить содержимое блока, например поменять местами в нём элементы или добавить свои (например сделать вместо чекбокса кнопку):

```html
<template data-installation-service-block>
  <button data-is-sb-modal-handler>Нажмите, чтобы заказать услугу установки</button>
  <div data-is-sb-price-value></div>
</template>
```

В таком случае скрипт не будет вставлять внутрь блока свой шаблон (идёт проверка, что внутри блока _data-installation-service-block_ уже имеется содержимое), а сделает его видимым с текущим содержимым и добавит лишь значения полученных данных услуги.

  </details>

## 1.2. Подключение на страницу "Корзина"

### Блоки услуг

Подключение скрипта на страницу "Корзина" происходит аналогичным образом, с несколькими блоками [installation-service-block](#Bloki-uslug-1), для каждого товара в корзине. За исключением лишь того, что атрибут _[data-page](#data-page)_ должен быть равным _cart._

Также необходимо подключить скрипт на все страницы подтверждения заказа (между "Корзиной" и "Спасибо"), где выводится список товаров.

Если ваша "Корзина" может обновляться без перезагрузки страницы (например, при изменении количества товаров в корзине, при нажатии на кнопки + или - рядом с товаром), и вы хотите, чтобы количество заказанных услуг всегда соответствовало количеству заказанных товаров, то при обновлении данных рекомендуется вызывать скрипт Clicker повторно, воспользовавшись методом _window.installationServiceState.resetScript()_

### Вывод блока с общей стоимостью услуг

На странице "Корзина" вам, возможно, потребуется вывести элемент с общей стоимостью услуг. Этот блок опционален, его выводить необязательно.

#### Краткий вариант

Для вывода краткого варианта блока стоимости, добавьте на страницу следующий код:

```html
<template data-installation-total-block-mini></template>
```

После добавления хотя бы одной услуги в корзину, блок становится видимым и в нём отобразится общая стоимость услуг:

![Пример элемента Clicker на странице "Корзина". Краткий вариант стоимости.](/img/widgets/3_____ver2_22_06_2020.png)

_Пример элемента Clicker на странице "Корзина". Краткий вариант стоимости._

<details> 
    <summary><strong>Дополнительные параметры блока (сокращенный вариант) с общей стоимостью услуг</strong></summary> 
     Блок может принимать следующие дополнительные параметры (в примере показаны значения по умолчанию):

```html
<template
  data-installation-total-block-mini
  data-title="Услуги установки"
  data-tip-text="Подсказка"
  data-currency-symbol=" р."
  data-tip-text="оплачиваются отдельно после оказания услуги"
  data-price-total-text="Итого за установку:"
  data-zero-price-text="Бесплатно"
  data-decimal-places="0"
  data-decimal-separator="."
  data-thousand-separator=""
></template>
```

Блок total ведёт себя таким же образом, как и [блоки с услугами](#checkbox-block-additional): принимаемые параметры по смыслу и поведению практически идентичны (выше показаны все параметры со значениями по умолчанию), кастомизация выполняется тем же способом, и также если при инициализации внутрь передана html разметка, то будет использована она, если нет, то создастся шаблон по умолчанию:

```html
<installation-total-block-mini class="installation-total-block-mini" data-installation-total-block-mini>
  <div class="installation-total-block__title"></div>
  <strong class="installation-total-block__price-container">
    <span class="installation-total-block__price">
      <span class="installation-total-block__price-value" data-is-t-price-value></span><span class="installation-total-block__price-currency"></span
    ></span>
    <strong class="installation-total-block__price-discount">
      <strong class="installation-total-block__discount-title"></strong>
      <strong class="installation-total-block__discount-zero-price-text"></strong>
      <span class="installation-total-block__discount-price">
        <span class="installation-total-block__discount-price-value" data-is-t-discount-price-value></span
        ><span class="installation-total-block__discount-price-currency"></span
      ></span>
    </strong>
  </strong>
  <i class="installation-total-block__tip"></i>
</installation-total-block-mini>
```

Видимость всего блока изменяется добавлением/удалением класса _clicker-total-block_hidden,_ если пользователем выбрана хотя бы одна услуга. Также как и с блоками чекбоксов, все элементы, получающие данные с нашего сервера дополнительно помечены data- атрибутом. Перечень всех динамических элементов:

**.installation-total-block_title** - заголовок блока, сюда будет помещён _data-title._

**[data-is-t-price-value].installation-total-block_price-value** - элемент для значения общей цены всех услуг

**.installation-total-block_price-currency** - знак валюты _data-currency-symbol._

**.installation-total-block_tip-text** - завершающий текст _data-tip._

**.installation-total-block_price-discount** - блок становится видимым, если хотя бы у одной услуги в корзине пользователя есть скидка. Блок ведёт себя так же, как аналогичный [в блоке добавления услуги](#checkbox-block-additional)

  </details>

#### Подробный вариант

Подробный вариант отличается от сокращенного тем, что в дополнении к общей цене, выводит список всех выбранных покупателем услуг. Для вывода блока, добавьте на страницу следующий код:

```html
<template data-installation-total-block-full></template>
```

![Пример элемента Clicker на странице "Корзина". Подробный вариант стоимости.](/img/widgets/4.______ver2_22_06_2020.png)

Пример элемента Clicker на странице "Корзина". Подробный вариант стоимости.

<details> 
    <summary><strong>Дополнительные параметры блока (подробный вариант) с общей стоимостью услуг</strong></summary>

Поведение и принимаемые параметры идентичны [сокращенному варианту](#Kratkii-variant), только добавляется один новый: **data-price-total-text** - фраза "Итого за установку: " по умолчанию.

Содержимое шаблона по умолчанию:

```html
<installation-total-block-full class="installation-total-block-full" data-installation-total-block-full>
  <strong class="installation-total-block__title"></strong>
  <div class="installation-total-block__service-container" data-is-t-service-container></div>
  <span class="installation-total-block__total-container">
    <span class="installation-total-block__total-text"></span>
    <strong class="installation-total-block__price-container">
      <span class="installation-total-block__price">
        <span class="installation-total-block__price-value" data-is-t-price-value></span><span class="installation-total-block__price-currency"></span
      ></span>
      <strong class="installation-total-block__price-discount">
        <strong class="installation-total-block__discount-title"></strong>
        <strong class="installation-total-block__discount-zero-price-text"></strong>
        <span class="installation-total-block__discount-price">
          <span class="installation-total-block__discount-price-value" data-is-t-discount-price-value></span
          ><span class="installation-total-block__discount-price-currency"></span
        ></span>
      </strong>
    </strong>
  </span>
  <i class="installation-total-block__tip"></i>
</installation-total-block-full>
```

Отличается от сокращенной версией добавленным контейнером **[data-is-t-service-container].installation-total-block_service-container**, куда помещаются названия услуг из корзины с их ценой. Далее описан экземпляр записи такой услуги (изменить в нём разметку на данный момент самостоятельно нельзя, только применять свои стили):

```html
<div class="installation-total-block-service-item">
  <div class="installation-total-block-service-item__title"></div>
  <strong class="installation-total-block-service-item__price-container">
    <span class="installation-total-block-service-item__price">
      <span class="installation-total-block-service-item__price-value"></span><span class="installation-total-block-service-item__price-currency"></span
    ></span>
    <strong class="installation-total-block-service-item__price-discount">
      <strong class="installation-total-block-service-item__discount-title"></strong>
      <strong class="installation-total-block-service-item__discount-zero-price-text"></strong>
      <span class="installation-total-block-service-item__discount-price">
        <span class="installation-total-block-service-item__discount-price-value"></span
        ><span class="installation-total-block-service-item__discount-price-currency"></span
      ></span>
    </strong>
  </strong>
</div>
```

Отображение названия и цены со скидкой идентично отображению подобных данных во всех остальных блоках.

  </details>

#### Интерактивный вариант beta

Наиболее полный вариант блока с общей стоимостью, который помимо всех данных из подробного варианта включает в себя элементы управления корзиной услуг, такие как удаление и изменение количества.

```html
<template data-installation-cart-block></template>
```

После добавления хотя бы одной услуги в корзину, блок становится видимым, в нём отобразится общая информация об услугах и элементы управления.

<details> 
        <summary>
            <strong>Дополнительные параметры блока (интерактивный вариант) с общей стоимостью услуг</strong>
        </summary>

<a name="data-always-visible"></a>
Поведение и принимаемые параметры идентичны [полному варианту](#Podrobnyi-variant), только добавляются два новых:
**data-always-visible** - если требуется всегда отображать данный блок, даже если нет заказанных услуг; принимаемые значения: _true, false_; по умолчанию _false_
**data-delete-text** - текст кнопки удалить; "удалить" по умолчанию.

Разметка блока по умолчанию:

```html
<installation-cart-block class="installation-cart-block" data-installation-cart-block>
  <strong class="installation-cart-block__title"></strong>
  <div class="installation-cart-block__service-container" data-is-cb-service-container></div>
  <span class="installation-cart-block__total-container">
    <span class="installation-cart-block__total-text"></span>
    <strong class="installation-cart-block__price-container">
      <span class="installation-cart-block__price">
        <span class="installation-cart-block__price-value" data-is-cb-total-price-value></span><span class="installation-cart-block__price-currency"></span
      ></span>
      <strong class="installation-cart-block__price-discount">
        <strong class="installation-cart-block__discount-title"></strong>
        <strong class="installation-cart-block__discount-zero-price-text"></strong>
        <span class="installation-cart-block__discount-price">
          <span class="installation-cart-block__discount-price-value" data-is-cb-total-discount-price-value></span
          ><span class="installation-cart-block__discount-price-currency"></span
        ></span>
      </strong>
    </strong>
  </span>
  <i class="installation-cart-block__tip"></i>
</installation-cart-block>
```

Видимость всего блока изменяется добавлением/удалением класса _installation-cart-block_hidden,_ если пользователем выбрана хотя бы одна услуга (если не [alwaysVisible](#data-always-visible)). Также как и любой другой блок, этот может принимать в себя любую разметку. В этом случае требуется указать data-атрибуты у интерактивных элементов, подробнее про данный способ [здесь](#data-attrs). Ниже приведены динамические элементы блока, которые получают внешние данные, заголовок выполнен в формате css запроса:<br/><br/>

**[data-is-cb-total-price-value].installation-cart-block_price-value** - значение цены всех услуг  
**[data-is-cb-total-discount-price-value].installation-cart-block_discount-price-value** - значение цены со скидкой для всех услуг  
**[data-is-cb-service-container].installation-cart-block_service-container** - внутрь этого элемента будут помещены элементы услуг. Если не передать элемент _data-is-cb-service-container_, то контейнером будет назначен сам _installation-cart-block_. Далее представлена разметка одного элемента услуги:<br/><br/>

```html
<installation-cart-block-service-item class="installation-cart-block-service-item" data-is-cb-service-item>
  <span class="installation-cart-block-service-item__service-name" data-is-cb-service-name></span>
  <div class="installation-cart-block-service-item__inner-container">
    <span class="installation-cart-block-service-item__quantity-container">
      <b class="installation-cart-block-service-item__decrease-quantity-button" data-is-cb-decrease-quantity-listener>-</b>
      <input type="number" class="installation-cart-block-service-item__service-quantity" data-is-cb-quantity-value />
      <b class="installation-cart-block-service-item__increase-quantity-button" data-is-cb-increase-quantity-listener>+</b>
    </span>
    <strong class="installation-cart-block-service-item__price-container">
      <span class="installation-cart-block-service-item__price">
        <span class="installation-cart-block-service-item__price-value" data-is-cb-price-value></span
        ><span class="installation-cart-block-service-item__price-currency"></span
      ></span>
      <strong class="installation-cart-block-service-item__price-discount">
        <strong class="installation-cart-block-service-item__discount-title"></strong>
        <strong class="installation-cart-block-service-item__discount-zero-price-text"></strong>
        <span class="installation-cart-block-service-item__discount-price">
          <span class="installation-cart-block-service-item__discount-price-value" data-is-cb-discount-price-value></span
          ><span class="installation-cart-block-service-item__discount-price-currency"></span
        ></span>
      </strong>
    </strong>
    <i class="installation-cart-block-service-item__delete-button" data-is-cb-delete-listener></i>
  </div>
</installation-cart-block-service-item>
```

**[data-is-cb-service-item].installation-cart-block-service-item** - непосредственно сам элемент услуги. Если вы пишите свою разметку, не забудьте указать данный элемент в пределах _installation-cart-block_ (при инициализации образец будет удалён, а все реальные экземпляры услуг будет созданы на его основе и помещены в _data-is-cb-service-container_)  
**[data-is-cb-service-name].installation-cart-block-service-item_service-name** - название услуги  
**[data-is-cb-decrease-quantity-listener].installation-cart-block-service-item_decrease-quantity**-button - кнопка уменьшения на 1 количества услуги  
**[data-is-cb-increase-quantity-listener].installation-cart-block-service-increase-quantity-button** - кнопка увеличения на 1 количества услуги  
**[data-is-cb-quantity-value].installation-cart-block-service-item_service-quantity** - значение количества услуги. Если элемент input, то с его помощью также можно изменять количество  
**[data-is-cb-price-value].installation-cart-block-service-item_price-value** - значение цены услуги  
**[data-is-cb-discount-price-value].installation-cart-block-service-item_discount-price-value** - значение цены со скидкой  
**[data-is-cb-delete-listener].installation-cart-block-service-item_delete-button** - кнопка, удаляющая услугу из корзины пользователя<br/><br/>

Отображение названия и цены со скидкой идентично отображению подобных данных во всех остальных блоках (подробное описание смотрите в расширенной интеграции [блока](#installation-service-block_price-discount), [услуги](#installation-service-block_price-value)).

</details>

Также данный вариант блока предусматривает возможность встраивания в существующую корзину магазина.

<details> 
  <summary>
      <strong>Блок, как часть существующей корзины магазина</strong>
  </summary>

Если вы хотите, чтобы данные услуги и её элементы управления отображались в общей корзине магазина под каждым товаром, то интегрировать блок installation-cart-block следует для каждого товара в корзине, но с указанием productId:

```html
...
<template data-installation-cart-block data-product-id="N00123"></template>
...
<template data-installation-cart-block data-product-id="N00456"></template>
...
```

Один такой блок будет отвечать только за услуги, заказанные для конкретного товара.
На практике рациональнее всего использовать следующий подход. Вы помечаете элемент товара корзины магазина нашими данными (_data-installation-cart-block_, _data-product-id_, _data-always-visible_). В нём задаёте место (_data-is-cb-service-container_), где нужно будет вывести список услуг (если не задать, то услуги добавляются в конец _data-installation-cart-block_). И указываете образец (c html разметкой) одного элемента услуги, пометив его как _data-is-cb-service-item_. Сделать это нужно обязательно, т.к. к одному товару может быть привязано несколько услуг. Далее скрипт удалит исходный шаблон одной услуги и заполнит контейнер актуальными элементами с данными услуг.

  </details>

## 1.3. Подключение на страницу "Спасибо"

Подключить скрипт на страницу "Спасибо" необходимо для того, чтобы заказ на установку был передан в Clicker. Скрипт выполняется автоматически (по умолчанию), работает асинхронно и не влияет на загрузку страницы.

Для того, чтобы пользователь не закрыл страницу раньше времени, скрипт отображает прелоадер в нижней части экрана, пока данные отправляются на backend Clicker. Затем прелоадер пропадает.

![Прелоадер](/img/widgets/Untitled_1.png)

Прелоадер (выезжающая плашка в нижней части страницы)

**Внимание!** Если вы не уверены, что все скрипты на вашей странице загружаются асинхронно и не препятствуют загрузке страницы, то для более быстрой отрисовки прелоадера рекомендуется подключать скрипт в header. Возможно подключение в другом месте, но в таком случае может произойти ситуация, когда прелоадер отобразится с задержкой, пользователь этого не увидит, закроет страницу "Спасибо", заказ на установку не успеет попасть в Clicker и **будет потерян**.

Далее описана интеграция скрипта на страницу, где требуется создавать заказ на установку:

```html
<script
  data-city="Ekaterinburg"
  data-shop="myShop"
  data-page="thanks"
  data-clients-first-name="Иван"
  data-clients-middle-name="Петрович"
  data-clients-last-name="Иванов"
  data-clients-address="Пролетарская 20, г. Москва"
  data-clients-phone="89990001101"
  data-retailers-order-number="N00324"
  data-order-source="main_page"
  data-cart="%5B%7B%22productId%22%3A%20%22123%22%2C%20%22quantity%22%3A%201%7D%5D..."
  async
  src="https://app.clicker.one/ClickerServices.js"
></script>
```

**data-shop** [обязательно] - идентификатор вашего магазина в системе Clicker

**data-city** [обязательно]- город отображения виджета; [список доступных городов](#51-Goroda) (с учетом регистра)

**page** [обязательно] - принимает строку, определяющую на какой странице запускается скрипт. В данном случае ожидает только одно значение: _thanks_

**data-clients-first-name** - имя покупателя, опциональный параметр

**data-clients-middle-name** - отчество покупателя, опциональный параметр

**data-clients-last-name** - фамилия покупателя, опциональный параметр

**data-clients-phone** [обязательно] - телефон покупателя в произвольном формате

**data-clients-address** [обязательно] - адрес покупателя в произвольном формате

**data-retailers-order-number** - номер заказа в системе ритейлера. Опциональный параметр, но настоятельно рекомендуется его проставлять, чтобы ваши сотрудники могли ориентироваться в заказах по вашим номерам.

**data-order-source** - если вам нужно знать в заказе из какого именно раздела/страницы был запущен скрипт (сделан заказ), то это можно указать здесь. Эти данные отобразятся в ЛК Clicker.

**data-cart** [обязательно] - строка с JSON массивом товаров магазина(!) в корзине покупателя. Скрипт Clicker сам сохраняет в cookies состояния "галочек": на какие товары покупатель выбрал установку на предыдущих страницах (карточка товара и корзина). Заказ создается только если такие товары были. Если услуги не были выбраны, то скрипт отработает незаметно для пользователя, не создаст заказ и не отобразит прелоадер. Поэтому вы можете передавать в скрипт все товары из корзины покупателя (ничего страшного, если в cart окажутся товары, установка для которых не предусмотрена, или не была выбрана).

<a name="cart-data"></a>Содержимое одного объекта в массиве cart практически идентично тому, что вы использовали при встраивании блока услуги на страницу, а именно:

```ts
productId: string;
title: string;
quantity?: number;
url: string;
price: number;
discount?: number;
```

**productId** [обязательно] - уникальный(!) идентификатор товара, обязательный параметр  
**title** - название товара  
**url** - url страницы товара  
**price** - цена товара в рублях  
**quantity** - количество товара в корзине покупателя (если параметр не задан, то подставляется значение 1); допустимые значения: любое положительное число  
**discount** - если на оплату услуги требуется сделать скидку, указывается размер в процентах; допустимые значения: число от 0 до 100<br/><br/>

**Важно!** Из-за особенностей стандарта дата атрибутов если вы передадите туда JSON строку, то может возникнуть конфликт символов кавычек с разметкой html. Поэтому рекомендуется обрабатывать JSON строку c помощью метода encodeURIComponent и получившийся результат передавать скрипту. Если вам неудобен такой способ, ознакомьтесь с расширенными возможностями интеграции скрипта ниже.

<details> 
    <summary><strong>Расширенные возможности интеграции скрипта</strong></summary>

На странице создания заказа глобальный объект _window.installationServiceState_ может принимать следующие данные:

```ts
{
	// Общие
	shop: string;
	city: string;
	page: 'product' | 'cart' | 'thanks';
	// Страница создания заказа
	cart: { ... }[];
	retailersOrderNumber?: string;
	clientsFirstName?: string;
	clientsMiddleName?: string;
	clientsLastName?: string;
	clientsAddress?: string;
	clientsPhone?: string;
	returnOrderData?: boolean;
	onOrderCreate?: (response: { ... }) => any;
	// Системные
	preventAutoRun?: boolean;
	createOrder: (cart: { ... }[]) => Promise<void>;
}
```

**onOrderCreate** - способ получения информации о заказе, описание ниже в [соответствующей главе](#get-order-data).
**returnOrderData** - способ получения информации о заказе, описание ниже в [соответствующей главе](#get-order-data).
**preventAutoRun** - создание заказа не произойдёт автоматически (необходимо вручную использовать метод createOrder).

Для чтения доступен следующий метод:
**createOrder** - создаёт заказ; метод принимает в себя корзину товаров магазина, описанную ранее.

По аналогии с расширенной интеграцией скрипта на страницу товара, вы можете передавать данные напрямую, записывая их в объект _window.installationServiceState_. Для этого нужно передать вашу корзину свойству _cart_ объекта _installationServiceState._ Пример:

```html
<script>
  { ...

  const shopCart = getCart()        // Динамически получаете какие либо данные
  const serviceCart = [             // Создаёте корзину для передачи скрипту Clicker
    {
      productId: shopCart[0].productId,
      title: shopCart[0].title,
      categoryId: shopCart[0].categoryId,
      url: getUrl(),
      price: shopCart[0].price,
      quantity: shopCart[0].quantity,
    },
  ];

   window.installationServiceState = {        // Записываете данные напрямую в стейт
  	page: 'thanks'
  	cart: serviceCart,
    city: "Ekaterinburg",
    shop: "myShop",
    clientsFirstName: "...",
    clientsMiddleName: "...",
    clientsLastName: "...",
    clientsAddress: "...",
    clientsPhone: "...",
  };

  ... }
</script>
<script defer src="https://app.clicker.one/ClickerServices.js"></script>
```

Также, как и в прошлом примере, вы можете использовать параметр _preventAutoRun_, чтобы получить какие-то данные асинхронно, затем, собрав корзину, передать её методу _window.installationServiceState.createOrder(cart)_, таким образом инициировав создание заказа. Этот же способ подходит, если ваш магазин выполнен по принципу Single Page Application.

**Получение информации о заказе** <a name="get-order-data"></a>

Если вам требуется получать информацию о созданном заказе на установку, для этого есть два способа, оба требуют работать со стейтом виджета (справка по работе с ним есть [здесь](#advanced-script-installation)).

1. Задать метод стейта _onOrderCreate_, который выполнится сразу после окончания создания заказа, даже в случае неудачи. Не вызывается, когда передана пустая корзина или нет совпадений (в отличии от следующего способа). На вход функция получит объект с данными о заказе (описание ниже). Пример использования:

   ```js
   window.installationServiceState = {
   	onOrderCreate: (data) => {
   		if (data.status === 'created') {
   			// здесь делаем что-то с полученными данными
   		}
   	},
   	... // Любые другие данные, которые вы хотите передать в стейт
   }

   ```

2. Установить значение свойства _window.installationServiceState.returnOrderData_ как _true._ Метод _window.installationServiceState.createOrder_ теперь будет всегда возвращать промис с данными (описание ниже) при любом вызове (даже если передана пустая корзина). При переданном false (по умолчанию) ничего не возвращается.

Объект с информацией о заказе имеет следующую структуру:

```ts
status: 'created' | 'error' | 'empty';
response: {
  orderId: number;
  message?: string;
  services: { ... }[];
	products: { ... }[];
};
```

**status** - _created_ если заказ создан; _error_ если произошла ошибка; _empty_ если передана пустая корзина или не было соответствий (_onOrderCreate_ не получает такой ответ)  
**orderId** - номер заказа; если не создан, то 0  
**message** - сообщение в случае ошибки  
**services** - массив услуг, которые пользователь добавил в заказ, описание объекта услуги есть [здесь](#service-data).  
**products** - массив товаров магазина, для которых пользователь заказал услуги, описание идентично [корзине товаров при создании заказа](#cart-data).

  </details>

# 2. Установка iframe формы на отдельной странице "Установка и настройка"

[Что это дает?](#iframe-profits)

Добавьте следующий код в тело страницы, где вы хотите отобразить iframe:

```html
<script data-shop-id="myShopId" src="https://widget.clicker.one/loadWidget.js" async></script>
```

Обмен информацией с iframe происходит через передачу ему data-атрибутов тега script (как data-shop-id в примере выше).

### Обязательные атрибуты

**data-shop-id** - идентификатор вашего магазина в системе Clicker

### Дополнительные атрибуты

**data-metric-id** - id пользователя для метрики  
**data-city -** город отображения виджета, [список доступных городов](#51-Goroda); если не передан, будет использован город по умолчанию для магазина  
**data-theme** - название цветовой темы; если не передано, будет использована тема магазина по умолчанию. Тема состоит из двух частей - цвета активных элементов и цвет фона страницы. Обе части темы записываются подряд через знак точки. Пример: _greenBright.white_ [Список всех доступных тем](#52-Dostupnye-tsvetovye-temy-vidzhetov).  
**data-is-sales-rep-name-required** - Boolean, требуется ли сделать поле код менеджера обязательным для заполнения. По умолчанию false.  
**data-is-retailers-order-number-required** - Boolean, требуется ли сделать поле номер заказа в системе ритейлера обязательным для заполнения. По умолчанию false.  
**data-order-source** - Если виджет установлен на нескольких страницах, то здесь можно передавать название источника (страницы), отобразится в заказе.

### Предзаполненение формы

Чтобы направить пользователя по ссылке на уже предзаполненную форму виджета, вы можете передать дополнительные параметры, в зависимости от которых будут заранее выбраны услуги или заполнены данные о покупателе.

**Данные клиента**

**data-name** - имя клиента  
**data-phone** - телефон клиента (10 цифр)  
**data-address** - адрес клиента  
**data-comment** - комментарий к заказу  
**data-sales-rep-name** - поле код менеджера  
**data-retailers-order-number** - номер заказа в системе ритейлера

**Данные одной услуги**
Для одной услуги достаточно передать данные товара в отдельные поля ([правила сопоставления](#4-Pravila-sopostavleniya-tovar-usluga) описаны ниже)

**data-title** - название товара, для которого предназначена услуга  
**data-quantity** - количество найденной услуги  
**data-match-properties** - массив свойств товара, по которым будет проводиться сопоставление товар → услуга, точно такой же используется в [интеграции popup-виджета](#data-match-properties), там можно ознакомиться с описанием. После формирования данного массива к нему следует последовательно применить метод JSON.stringify и encodeURIComponent, затем получившуюся строку передать дата-атрибуту data-match-properties. Пример:

```js
encodeURIComponent(
  JSON.stringify([
    {
      "key": "size",
      "value": 20
    },
    {
      "key": "category",
      "value": "Холодильники"
    },
		{
			...
		}
  ])
);

// Результат: %5B%7B%22key%22%3A%22size%22%2C%22value%22%3A20%7D%2C%7B%22key%22%3A%22category%22%2C%22value%22%3A%22%D0%A5%D0%BE%D0%BB%D0%BE%D0%B4%D0%B8%D0%BB%D1%8C%D0%BD%D0%B8%D0%BA%D0%B8%22%7D%5D
```

**Данные нескольких услуг**

**data-cart** - массив, состоящий из объектов с описанными выше данными услуг, с той лишь разницей, что все дата атрибуты пишутся без _data-_ и в camelCase. После формирования данного массива к нему следует последовательно применить метод JSON.stringify и encodeURIComponent, затем получившуюся строку передать дата-атрибуту data-cart. Пример:

```js
encodeURIComponent(
  JSON.stringify([
    {
      title: "Холодильник Bosh",
      matchProperties: [
        {
          key: "size",
          value: 20,
        },
        {
          key: "category",
          value: "Холодильники",
        },
      ],
    },
    {
      title: "Сплит-система 7800 Вт",
      quantity: 2,
      matchProperties: [
        {
          key: "weight",
          value: 30,
        },
        {
          key: "category",
          value: "Сплит-системы",
        },
      ],
    },
  ])
);

// Результат: %5B%7B%22title%22%3A%22%D0%A5%D0%BE%D0%BB%D0%BE%D0%B4%D0%B8%D0%BB%D1%8C%D0%BD%D0%B8%D0%BA%20Bosh%22%2C%22matchProperties%22%3A%5B%7B%22key%22%3A%22size%22%2C%22value%22%3A20%7D%2C%7B%22key%22%3A%22category%22%2C%22value%22%3A%22%D0%A5%D0%BE%D0%BB%D0%BE%D0%B4%D0%B8%D0%BB%D1%8C%D0%BD%D0%B8%D0%BA%D0%B8%22%7D%5D%7D%2C%7B%22title%22%3A%22%D0%A1%D0%BF%D0%BB%D0%B8%D1%82-%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0%207800%20%D0%92%D1%82%22%2C%22quantity%22%3A2%2C%22matchProperties%22%3A%5B%7B%22key%22%3A%22weight%22%2C%22value%22%3A30%7D%2C%7B%22key%22%3A%22category%22%2C%22value%22%3A%22%D0%A1%D0%BF%D0%BB%D0%B8%D1%82-%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D1%8B%22%7D%5D%7D%5D
```

### Дополнительные возможности передачи данных

Наиболее удобный способ передачи данных виджету описан выше. Здесь описаны дополнительные возможности.

**Передача через GET-параметр**
Актуально для передачи данных на странице виджета (widget.clicker.one/order/[myShop]). Передаваемые данные идентичны стандартному способу, но название параметров будут отличаться - отсутствует префикс **data-** и сочетания слов написаны camelCase. Пример: было _data-additional-option,_ стало _additionalOption_.

**Передача через POST-запрос**
Актуально при передаче данных на страницу виджета (widget.clicker.one/order/[myShop]) с помощью формы. Передаваемые данные идентичны стандартному способу, но имена полей формы будут отличаться - отсутствует префикс **data-** и сочетания слов написаны camelCase. Пример: было _data-additional-option,_ стало _additionalOption_. Пример формы:

```html
<form method="post" action="https://widget.clicker.one/order/[myShop]" target="_blank">
  <input type="hidden" name="city" value="Moscow" />
  <input type="hidden" name="shopId" value="holodilnik" />
  <input
    type="hidden"
    name="cart"
    value="%5B%7B%22categoryId%22:%22%D0%92%D1%81%D1%82%D1%80%D0%B0%D0%B8%D0%B2%D0%B0%D0%B5%D0%BC%D1%8B%D0%B5%20%D0%B3%D0%B0%D0%B7%D0%BE%D0%B2%D1%8B%D0%B5%20%D0%B2%D0%B0%D1%80%D0%BE%D1%87%D0%BD%D1%8B%D0%B5%20%D0%BF%D0%B0%D0%BD%D0%B5%D0%BB%D0%B8%22%7D,%7B%22title%22:%22%D0%A1%D0%BF%D0%BB%D0%B8%D1%82-%D1%81%D0%B8%D1%81%D1%82%D0%B5%D0%BC%D0%B0%207800%20%D0%92%D1%82%22%7D%5D"
  />
  <input type="submit" value="Открыть в новом окне" />
</form>
```

# 3. Интеграция через API [в разработке]

# 4. Правила сопоставления товар услуга

Чтобы правильно отобразить услугу к товару, нужно сопоставить каждому товару на стороне интернет магазина, услугу из базы доступных услуг на стороне Clicker. Clicker хранит данные о доступных услугах, их свойствах и ценах на своей стороне. Сопоставление происходит на бэкенде Clicker, правила сопоставления хранятся там же. Правила сопоставления настраиваются индивидуально для каждого магазина.

Правила сопоставления используют параметры товара **data-title** и **data-match-properties**

В правилах сопоставления можно задать следующие операторы:

```
equal // свойство товара должно быть равно заданному параметру
not_equal // свойство товара должно быть НЕ равно заданному параметру
range {x..y} // числовое свойство товара находится заданном диапазоне
contains_in_list // свойство товара содержится в заданном массиве строк
regexp // свойство товара соответствует регулярному выражению
empty // значение отсутствует либо null
not_empty // любое не пустое значение
boolean // true или false
```

Также в правилах сопоставления работают логические операторы **OR,** **AND, NOT**

Вне зависимости от способа сопоставления Товар → Услуга, параметр **city** обязателен и влияет как на стоимость услуги, так и на наличие конкретной услуги в конкретном городе. Например, стоимость установки стиральной машины в г. Москва и г. Волгоград будет разной. Возможна ситуация, когда конкретная услуга будет доступна в одном городе, но недоступна в другом (например, установка газовых плит недоступна в некоторых региональных городах).

#### Пример 1: сопоставление по категории

Это самый простой вариант, когда товары на вашем сайте распределены по категориям таким же самым образом, как разбиты услуги в Clicker. Тогда достаточно одного параметра - названия категории (или id категории)

**Правила сопоставления для услуг (настраиваются в ЛК Clicker)**

| Услуга                                       | Параметры сопоставления                                                                               |
| -------------------------------------------- | ----------------------------------------------------------------------------------------------------- |
| Установка отдельно стоящей стиральной машины | category equal "Отдельностоящие стиральные машины"                                                    |
| Установка встраиваемой стиральной машины     | category contains_in_list "Встраиваемые стиральные машины", "Встраиваемые стиральные машины до 60 см" |

**Товар 1**

```js
data-title: "Стиральная машина Haier HW60-BP10929B"
data-match-properties:
[
	{
		"key": "category", // этот параметр будет влиять на выбор услуги
		"value": "Отдельностоящие стиральные машины"
	},
  {
		"key": "Размер (см)", // этот параметр не будет влиять на выбор услуги, но вы можете его оставить
		"value": 60
	},
  {
		"key": "Цвет", // этот параметр не будет влиять на выбор услуги, но вы можете его оставить
		"value": "Белый"
	}
]
```

На этот товар сопоставится услуга "Установка отдельно стоящей стиральной машины"

**Товар 2**

```js
data-title: "Встраиваемая стиральная машина Candy CF3284"
data-match-properties:
[
	{
		"key": "category", // этот параметр будет влиять на выбор услуги
		"value": "Встраиваемые стиральные машины"
	},
  {
		"key": "Размер (см)", // этот параметр не будет влиять на выбор услуги, но вы можете его оставить
		"value": 60
	},
  {
		"key": "Цвет", // этот параметр не будет влиять на выбор услуги, но вы можете его оставить
		"value": "Белый"
	}
]
```

На этот товар сопоставится услуга "Установка встраиваемой стиральной машины"

**Товар 3**

```js
data-title: "Сковородка Tefal XF234"
data-match-properties:
[
	{
		"key": "category", // этот параметр будет влиять на выбор услуги
		"value": "Сковородки"
	}
]
```

На этот товар услуга не сопоставится.

#### Пример 2: сопоставление по категории и дополнительному параметру

Это более сложный вариант, когда товары на вашем сайте распределены по категориям не так, как разбиты услуги в Clicker. Тогда кроме категории нужны другие параметры, по которым Clicker сможет понять какая услуга относится к этому товару

**Правила сопоставления для услуг (настраиваются в ЛК Clicker)**

| Услуга                                                                 | Параметры сопоставления                                                                                   |
| ---------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- |
| Установка проточного водонагревателя                                   | "category" equal "Водонагреватели" AND "Тип" = "Проточный"                                                |
| Установка электрического накопительного водонагревателя до 50л         | "category" equal "Водонагреватели" AND "Тип" equal "Накопительный" AND "Объем (в литрах)" range {0..50}   |
| Установка электрического накопительного водонагревателя от 51л до 80л  | "category" equal "Водонагреватели" AND "Тип" equal "Накопительный" AND "Объем (в литрах)" range {51..80}  |
| Установка электрического накопительного водонагревателя от 81л до 150л | "category" equal "Водонагреватели" AND "Тип" equal "Накопительный" AND "Объем (в литрах)" range {81..150} |

**Товар 1**

```js
data-title: "THERMEX VRTF 242"
data-match-properties:
[
	{
		"key": "category", // этот параметр будет влиять на выбор услуги
		"value": "Водонагреватели"
	},
  {
		"key": "Максса (кг)", // этот параметр не будет влиять на выбор услуги, но вы можете его оставить
		"value": 60
	},
  {
		"key": "Тип", // этот параметр будет влиять на выбор услуги
		"value": "Проточный"
	}
]
```

На этот товар сопоставится услуга "Установка проточного водонагревателя"

**Товар 2**

```js
data-title: "THERMEX Praktik 80 V"
data-match-properties:
[
	{
		"key": "category", // этот параметр будет влиять на выбор услуги
		"value": "Водонагреватели"
	},
  {
		"key": "Объем (в литрах)", // этот параметр будет влиять на выбор услуги
		"value": 80
	},
  {
		"key": "Тип", // этот параметр будет влиять на выбор услуги
		"value": "Накопительный"
	}
]
```

На этот товар сопоставится услуга "Установка электрического накопительного водонагревателя от 51л до 80л"

#### Пример 3: сопоставление по категории и нескольким дополнительном параметрам. Логические AND и OR на группу условий

Это самый сложный вариант, когда вам нужна тонкая настройка

**Правила сопоставления для услуг (настраиваются в ЛК Clicker)**

| Услуга                                                              | Параметры сопоставления                                                                                                                                                                  |
| ------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Установка прямоугольной акриловой ванны без гидромассажа            | "category" equal "Ванны акриловые" AND "Форма" equal "Прямоугольная" AND { "Гидромассаж" equal "Не предусмотрен" OR "Комплектация" equal "Без гидромассажа"}                             |
| Установка угловой или сложной формы акриловой ванны с гидромассажем | "category" equal "Ванны акриловые" AND "Форма" not_equal "Прямоугольная" AND { "Гидромассаж" equal "В комплекте" OR "Комплектация" contains_in_list "С гидромассажем", "С аквамассажем"} |

**Товар 1**

```json
data-title: "Акриловая ванна BelBagno BB67-1700"
data-match-properties:
[
	{
		"key": "category", // этот параметр будет влиять на выбор услуги
		"value": "Ванны акриловые"
	},
  {
		"key": "Максса (кг)", // этот параметр не будет влиять на выбор услуги, но вы можете его оставить
		"value": 60
	},
  {
		"key": "Форма", // этот параметр будет влиять на выбор услуги
		"value": "Прямоугольная"
	},
  {
		"key": "Гидромассаж", // этот параметр будет влиять на выбор услуги
		"value": "Не предусмотрен"
	}
]
```

На этот товар сопоставится услуга "Установка прямоугольной акриловой ванны без гидромассажа"

**Товар 2**

```json
data-title: "Акриловая ванна Maestro FGH17-310"
data-match-properties:
[
	{
		"key": "category", // этот параметр будет влиять на выбор услуги
		"value": "Ванны акриловые"
	},
  {
		"key": "Максса (кг)", // этот параметр не будет влиять на выбор услуги, но вы можете его оставить
		"value": 80
	},
  {
		"key": "Форма", // этот параметр будет влиять на выбор услуги
		"value": "Асимметричная угловая"
	},
  {
		"key": "Комплектация", // этот параметр будет влиять на выбор услуги
		"value": "С аквамассажем"
	}
]
```

На этот товар сопоставится услуга "Установка угловой или сложной формы акриловой ванны с гидромассажем"

#### Пример 4: сопоставление по названию товара по регулярному выражению

Если ваша структура данных такова, что затруднительно их поделить по категориям или дополнительным параметрам, можно сделать сопоставление по регулярному выражению по названию.

<br/>

> ☝ Мы не рекомендуем использовать этот способ, так как его сложно отлаживать, и есть риск, что при изменении названий товаров с вашей стороны интеграция может сломаться.

<br/>
<br/>

**Правила сопоставления для услуг (настраиваются в ЛК Clicker)**

| Услуга                                                         | Параметры сопоставления                                       |
| -------------------------------------------------------------- | ------------------------------------------------------------- |
| Установка проточного водонагревателя                           | title regexp "(?!._газовый._)(водонагреватель).\*(проточный)" |
| Установка электрического накопительного водонагревателя до 50л | title regexp "(?!._газовый._                                  |

**Товар 1**

```js
data-title: "Водонагреватель проточный Electrolux Smartfix 2.0 T"
```

На этот товар сопоставится услуга "Установка проточного водонагревателя"

**Товар 2**

```js
data-title: "Водонагреватель THERMEX ES 50 L"
```

На этот товар сопоставится услуга "Установка электрического накопительного водонагревателя до 50л"

# 5. Переменные

## 5.1 Города

| Параметр City | Город           |
| ------------- | --------------- |
| Belgorod      | Белгород        |
| Blagoveshensk | Благовещенск    |
| Bryansk       | Брянск          |
| Ekaterinburg  | Екатеринбург    |
| Ivanovo       | Иваново         |
| Kaluga        | Калуга          |
| Kazan         | Казань          |
| Krasnodar     | Краснодар       |
| Krasnokamsk   | Краснокамск     |
| Krasnoyarsk   | Красноярск      |
| Lipetsk       | Липецк          |
| Moscow        | Москва          |
| NNovgorod     | Нижний Новгород |
| Novosibirsk   | Новосибирск     |
| Perm          | Пермь           |
| Podolsk       | Подольск        |
| Pskov         | Псков           |
| Rostovnd      | Ростов-на-Дону  |
| Samara        | Самара          |
| Saratov       | Саратов         |
| Simferopol    | Симферополь     |
| Sochi         | Сочи            |
| StPetersburg  | Санкт-Петербург |
| Tolyatti      | Тольятти        |
| Tula          | Тула            |
| Tyumen        | Тюмень          |
| Ufa           | Уфа             |
| Volgograd     | Волгоград       |
| Voronezh      | Воронеж         |
| Khabarovsk    | Хабаровск       |

## 5.2 Доступные цветовые темы виджетов

| Цвета элементов | Цвет фона страницы |
| --------------- | ------------------ |
| redBright       | white              |
| redCalm         | gray               |
| blueBright      | dark               |
| blueCalm        |                    |
| lightBlueBright |                    |
| lightBlueCalm   |                    |
| roseBright      |                    |
| roseCalm        |                    |
| yellowBright    |                    |
| yellowCalm      |                    |
| greenBright     |                    |
| greenCalm       |                    |
| turquoiseBright |                    |
| turquoiseCalm   |                    |
| darkBlueBright  |                    |
| darkBlueCalm    |                    |
| orangeBright    |                    |
| orangeCalm      |                    |
| black           |                    |

# 🕙 История версий

<details> 
    <summary>2.07, 29.07.2022</summary> 
    - Новые match types у matchProperties
</details>
<details> 
    <summary>2.06, 30.06.2022</summary> 
    - В iframe виджет добавлена передача параметров data-is-sales-rep-name-required, data-is-retailers-order-number-required, data-order-source при запуске.
</details>
<details> 
    <summary>2.05, 20.05.2022</summary> 
    - Расширен список городов.
</details>
<details> 
    <summary>2.04, 13.04.2022</summary> 
    - В iframe виджет добавлена передача параметров data-sales-rep-name, data-retailers-order-number при запуске.
</details>
<details> 
    <summary>2.03, 21.01.2022</summary> 
    - Убрана необходимость передавать matchProperties для получения отчёта модального виджета<br/>
    - Отчёт модального виджета больше не выбрасывает ошибку, а отдаёт только status: error
</details>
<details> 
    <summary>2.02, 09.12.2021</summary> 
    - Новый блок cart у popup виджета<br/>
    - Новая система передачи данных о товаре для сопоставления с услугой (matchProperties)<br/>
    - Добавлен город: Тюмень
</details>
<details> 
    <summary>2.01, 09.11.2021</summary> 
    - Полный рефакторинг модального виджета, уход от сторонних библиотек в коде
</details>
<details> 
    <summary>1.31, 14.10.2021</summary> 
    - Убрано поле email пользователя, окончательно убран функционал догоняющих писем
</details>
<details> 
    <summary>1.30, 01.10.2021</summary> 
    - Добавлена новая система темизации iframe виджета
</details>
<details> 
    <summary>1.29, 17.08.2021</summary> 
    - В pop-up виджет добавлен необязательный параметр retailersOrderNumber
</details>
<details> 
    <summary>1.28, 11.08.2021</summary> 
    - Добавлена возможность передавать iframe виджету несколько услуг
</details>
<details> 
    <summary>1.27, 26.07.2021</summary> 
    - Изменён способ интеграции iframe виджета<br/>
    - Добавлено оглавление<br/>
    - Скрыт устаревший функционал (догоняющие письма, отдельная страница виджета)
</details>
<details> 
    <summary>1.26, 28.04.2021</summary> 
    - Добавлены новые города<br/>
    - При интеграции добавлен параметр additionalOption
    - При интеграции добавлен параметр 
</details>
