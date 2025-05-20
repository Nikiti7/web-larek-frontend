# Проектная работа "Веб-ларек"

Стек: HTML, SCSS, TS, Webpack

Структура проекта:
- src/ — исходные файлы проекта
- src/components/ — папка с JS компонентами
- src/components/base/ — папка с базовым кодом

Важные файлы:
- src/pages/index.html — HTML-файл главной страницы
- src/types/index.ts — файл с типами
- src/index.ts — точка входа приложения
- src/scss/styles.scss — корневой файл стилей
- src/utils/constants.ts — файл с константами
- src/utils/utils.ts — файл с утилитами

## Установка и запуск
Для установки и запуска проекта необходимо выполнить команды

```
npm install
npm run start
```

или

```
yarn
yarn start
```
## Сборка

```
npm run build
```

или

```
yarn build
```

## Архитектура

### Компоненты модели данных (бизнес-логика)

1. Класс ```Product```
   
Представляет товар в ларьке

Конструктор принимает такие аргументы:
- ```id: number``` - уникальный индификатор
- ```name: string``` - название
- ```description: string``` - описание
- ```price: number``` - цена
- ```image: string``` - изображение
- ```tags: string[]``` - теги

2. Класс ```Cart```
   
Отвечает за управление корзиной пользователя

Конструктор принимает такие аргументы:
- ```items: Map<string, CartItem>``` - словарь товаров по id

Класс имеет такие методы:
- ```addProduct(product:Product, count: number = 1)``` - добавить товар
- ```removeProduct(productId: number)``` - убрать продукт
- ```pay()``` - оформление
- ```getTotal: number``` - общая цена

###  Компоненты представления

1. Класс ```ProductCard```
   
Отображает карточку товара ```Product``` с возможностью покупки ```onAddtoCart: (product:Product) => void```. То есть показывает данные товара и кнопку "Купить".

2. Класс ```Catalog```

Отображает список товаров ```products: Product[]```, используя ```ProductCard```.

3. Класс ```CartPanel```

Интерфейс корзины покупателя. Отображает список товаров в корзине ```cart: Cart``` и кнопку "Оформить" ```onCheckOut: () => void```.

### Ключевые типы данных

```
type ProductID = number;

interface Product {
  id: ProductID;
  name: string;
  description: string;
  price: number;
  imageUrl: string;
  tags: string[];
}

interface CartItem {
  product: Product;
  count: number;
}

interface SearchEvent {
  query: string;
}

interface FilterTagEvent {
  tag: string;
}

interface AddToCartEvent {
  product: Product;
}

// События
enum Events {
  SEARCH = 'ui:search',
  FILTER_TAG = 'ui:filter-tag',
  ADD_TO_CART = 'ui:add-to-cart',
  CHECKOUT = 'ui:checkout',
  CART_UPDATED = 'cart:updated',
}
```

