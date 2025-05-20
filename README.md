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

### Базовый код

1. Класс ```Product```
   
Представляет товар в ларьке

Конструктор принимает такие аргументы:
- ```id: number``` - уникальный индификатор
- ```name: string``` - название
- ```description: string``` - описание
- ```price: number``` - цена
- ```image: string``` - изображение
- ```tags: string[]``` - теги

2. Класс ```EventEmitter```

Обеспечивает модель событий в приложении

Класс имеет такие методы:
- ```on(event: string, handler: Function)``` - подписка на событие
- ```off(event: string, handler: Function)``` - отписка от события
- ```emit(event: string, payload<T>: T)``` - уведомление о наступлении события
- ```offAll()``` - сброс всех подписчиков
- ```onAll()``` - подписка на все события
- ```trigger()``` - генерирует заданное событие с заданными аргументами

3. Класс ```CustomerData```

Описывает данные покупателя

Конструктор принимает такие аргументы:
- ```phone: string``` - номер телефона
- ```address: string``` - адрес
- ```email: string``` - почта
- ```paymentMethod: string``` - метод доставки

### Компоненты модели данных (бизнес-логика)

1. Класс ```Cart```
   
Отвечает за управление корзиной пользователя

Конструктор принимает такие аргументы:
- ```items: Map<string, CartItem>``` - словарь товаров по id

Класс имеет такие методы:
- ```addProduct(product:Product, count: number = 1)``` - добавить товар
- ```removeProduct(productId: number)``` - убрать продукт
- ```pay()``` - оформление
- ```getTotal: number``` - общая цена

### Компоненты представления

1. Класс ```ProductCard```
   
Отображает карточку товара ```Product``` с возможностью покупки ```onAddtoCart: (product:Product) => void```. То есть показывает данные товара и кнопку "Купить".

2. Класс ```Catalog```

Отображает список товаров ```products: Product[]```, используя ```ProductCard```.

3. Класс ```CartPanel```

Интерфейс корзины покупателя. Отображает список товаров в корзине ```cart: Cart``` и кнопку "Оформить" ```onCheckOut: () => void```.

4. Класс ```CheckoutForm```

Форма подтверждения заказа. Пользователь вводит ```address```, ```phone```, ```email``` и подтверждает заказ ```onSubmit```.

### Ключевые типы данных

```
type ProductID = number;

interface Product {
  id: ProductID; // id продукта
  name: string; // название 
  description: string; // описание
  price: number; // цена
  imageUrl: string; // изображение
  tags: string[]; // тег
}

interface CustomerData {
  phone: string; // номер пользователя
  email: string; // почта пользователя
  address: string; // адрес пользователя
  paymentMethod: string; // метод доставки
}

interface ProductCardProps {
  product: Product; // показывает данные продукта
  onAddToCart: (product: Product) => void; // добавление в корзину
}

interface CatalogProps {
  products: Product[]; // список продуктов
  onAddToCart: (product: Product) => void; // Передаёт Product в ProductCard
}

interface CartPanelProps {
  cart: Cart; // cписок товаров 
  onCheckout: () => void; // кнопка «Оформить заказ»
}

interface OrderPayload {
  items: CartItem[];  // список покупок
  total: number; // количество
  customer: { // данные покупателя
    phone: string;
    email: string;
    address: string;
    paymentMethod: string;
  };
}

interface CheckoutFormProps {
  cart: Cart;
  onSubmit: (order: OrderPayload) => void;
}

interface CartItem {
  product: Product;
  count: number;
}

interface AddToCartEvent {
  product: Product;
}

// События
enum Events {
  ADD_TO_CART = 'ui:add-to-cart', // добавить товар
  CHECKOUT = 'ui:checkout', // добавить в корзину
  CART_UPDATED = 'cart:updated', // корзина 
  SUBMIT_ORDER = 'ui:submit-order', // оформление заказа
  ORDER_COMPLETE = 'order:complete' // заказ оформлен
}
```


