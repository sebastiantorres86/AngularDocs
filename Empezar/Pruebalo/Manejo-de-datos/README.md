# Manejo de datos

Esta guía se basa en el segundo paso del tutorial [Introducción a una aplicación básica de Angular](..\Empezar\README#empezando-con-angular), [Agregar navegación](..\Agregar-Navegacion\README#agregar-navegacion). En esta etapa de desarrollo, la aplicación de la tienda tiene un catálogo de productos con dos vistas: una lista de productos y detalles del producto. Los usuarios pueden hacer clic en el nombre de un producto de la lista para ver los detalles en una nueva vista, con una URL o ruta distinta.

Este paso del tutorial lo guía a través de la creación de un carrito de compras en las siguientes fases:

- Actualice la vista de detalles del producto para incluir un botón **Buy**, que agrega el producto actual a una lista de productos que administra un servicio de carrito.
- Agregue un componente de carrito, que muestra los artículos en el carrito.
- Agregue un componente de envío, que recupera los precios de envío de los artículos en el carrito utilizando Angular [`HttpClient`](https://angular.io/api/common/http/HttpClient) para recuperar los datos de envío de un archivo `.json`.

---

## Crear el servicio de carrito de compras

En Angular, un servicio es una instancia de una clase que puede poner a disposición de cualquier parte de su aplicación utilizando el [sistema de inyección de dependencias](https://angular.io/guide/glossary#dependency-injection-di) de Angular .

Actualmente, los usuarios pueden ver la información del producto y la aplicación puede simular el intercambio y las notificaciones sobre los cambios del producto.

El siguiente paso es crear una forma para que los usuarios agreguen productos a un carrito. Esta sección lo guía para agregar un botón **Buy** y configurar un servicio de carrito para almacenar información sobre los productos en el carrito.

### Definir un servicio de carrito

1. Para generar un servicio de carrito, haga clic con el botón derecho en la carpeta `app`, elija **Angular Generator** y elija **Service**. Nombra al nuevo servicio `cart`.

   ```typescript
   // src / app / cart.service.ts
   import { Injectable } from "@angular/core";

   @Injectable({
     providedIn: "root",
   })
   export class CartService {
     constructor() {}
   }
   ```

2. Importe la interfaz `Product` de `./products.js`.

3. En la clase `CartService`, defina una propiedad `items` para almacenar la matriz de los productos actuales en el carrito.

   ```typescript
   // src/app/cart.service.ts
   import { Product } from "./products";
   /* . . . */
   export class CartService {
     items: Product[] = [];
     /* . . . */
   }
   ```

4. Defina métodos para agregar artículos al carrito, devolver artículos del carrito y borrar los artículos del carrito.

   ```typescript
   // src/app/cart.service.ts
   export class CartService {
     items: Product[] = [];
     /* . . . */

     addToCart(product: Product) {
       this.items.push(product);
     }

     getItems() {
       return this.items;
     }

     clearCart() {
       this.items = [];
       return this.items;
     }
     /* . . . */
   }
   ```

   - El método `addToCart()` agrega un producto a una matriz de items.

   - El método `getItems()` recopila los artículos que los usuarios agregan al carrito y devuelve cada artículo con su cantidad asociada.

   - El método `clearCart()` devuelve una matriz vacía de artículos, que vacía el carrito.

### Utilice el servicio de carrito

Esta sección lo guía a través del uso del `CartService` para agregar un producto al carrito.

1. En `product-details.component.ts`, importe el servicio de carrito.

   ```typescript
   // src/app/product-details/product-details.component.ts
   import { Component, OnInit } from "@angular/core";
   import { ActivatedRoute } from "@angular/router";

   import { Product, products } from "../products";
   import { CartService } from "../cart.service";
   ```

2. Inyecte el servicio de carrito agregándolo al archivo `constructor()`.

   ```typescript
   // src/app/product-details/product-details.component.ts
   export class ProductDetailsComponent implements OnInit {
     constructor(
       private route: ActivatedRoute,
       private cartService: CartService
     ) {}
   }
   ```

3. Defina el método `addToCart()` que agrega el producto actual al carrito.

   ```typescript
   // src/app/product-details/product-details.component.ts
   export class ProductDetailsComponent implements OnInit {
     addToCart(product: Product) {
       this.cartService.addToCart(product);
       window.alert("Your product has been added to the cart!");
     }
   }
   ```

   El método `addToCart()` hace lo siguiente:

   - Toma el `product` actual como argumento.
   - Utiliza el método `addToCart()` de `CartService` para agregar el producto al carrito.
   - Muestra un mensaje de que ha agregado un producto al carrito.

4. En `product-details.component.html`, agregue un botón con la etiqueta **Buy** y vincule el evento `click()` al método `addToCart()`. Este código actualiza la plantilla de detalles del producto con un botón **Buy** que agrega el producto actual al carrito.

   ```html
   // src/app/product-details/product-details.component.html
   <h2>Product Details</h2>

   <div *ngIf="product">
     <h3>{{ product.name }}</h3>
     <h4>{{ product.price | currency }}</h4>
     <p>{{ product.description }}</p>

     <button (click)="addToCart(product)">Buy</button>
   </div>
   ```

5. Verifique que el nuevo botón **Buy** aparece como se esperaba actualizando la aplicación y haciendo clic en el nombre de un producto para mostrar sus detalles.

   ![](images\product-details-buy.png)

6. Haga clic en el botón **Buy** para agregar el producto a la lista almacenada de artículos en el carrito y mostrar un mensaje de confirmación.

   ![](images\buy-alert.png)

---

## Crear la vista del carrito

Para que los clientes vean su carrito, puede crear la vista del carrito en dos pasos:

1. Cree un componente de carro y configure el enrutamiento al nuevo componente.
2. Muestre los artículos del carrito.

### Configurar el componente del carrito

Para crear la vista de carro, siga los mismos pasos que hizo para crear `ProductDetailsComponent` y configurar el enrutamiento para el nuevo componente.

1. Genere un componente de carrito nombrado `cart` haciendo clic con el botón derecho en la carpeta `app`, eligiendo **Angular Generator** y **Component**.

   ```typescript
   // src/app/cart/cart.component.ts
   import { Component } from "@angular/core";

   @Component({
     selector: "app-cart",
     templateUrl: "./cart.component.html",
     styleUrls: ["./cart.component.css"],
   })
   export class CartComponent {
     constructor() {}
   }
   ```

   StackBlitz también genera un `ngOnInit()` de manera predeterminado en componentes. Puede ignorar el `ngOnInit()` en `CartComponent` para este tutorial.

2. Abra `app.module.ts` y agregue una ruta para el componente `CartComponent`, con una `path` de `cart`.

   ```typescript
   // src/app/app.module.ts
   @NgModule({
   imports: [
   BrowserModule,
   ReactiveFormsModule,
   RouterModule.forRoot([
     { path: '', component: ProductListComponent },
     { path: 'products/:productId', component: ProductDetailsComponent },
     { path: 'cart', component: CartComponent },
   ])
   ],
   ```

3. Actualice el botón **Checkout** para que se dirija a la URL `/cart`. En `top-bar.component.html`, agregue una directiva [`routerLink`](https://angular.io/api/router/RouterLink) que apunte a `/cart`.

   ```html
   <!-- src/app/top-bar/top-bar.component.html -->
   <a routerLink="/cart" class="button fancy-button">
     <i class="material-icons">shopping_cart</i>Checkout
   </a>
   ```

4. Verifique que el nuevo `CartComponent` trabaje como se esperaba haciendo clic en el botón **Checkout**. ¡Puedes ver el texto predeterminado "cart works!", y la URL tiene el patrón `https://getting-started.stackblitz.io/cart`, donde `getting-started.stackblitz.io` puede ser diferente para su proyecto StackBlitz.

   ![](images\cart-works.png)

### Mostrar los artículos del carrito

Esta sección le muestra cómo utilizar el servicio de carrito para mostrar los productos en el carrito.

1. En `cart.component.ts`, importe el `CartService` del archivo `cart.service.ts`.

   ```typescript
   // src/app/cart/cart.component.ts
   import { Component } from "@angular/core";
   import { CartService } from "../cart.service";
   ```

2. Inyecte el `CartService` para que `CartComponent` pueda usarlo agregándolo al archivo `constructor()`.

   ```typescript
   // src/app/cart/cart.component.ts
   export class CartComponent {
     constructor(private cartService: CartService) {}
   }
   ```

3. Defina la propiedad `items` para almacenar los productos en el carrito.

   ```typescript
   // src/app/cart/cart.component.ts
   export class CartComponent {
     items = this.cartService.getItems();

     constructor(private cartService: CartService) {}
   }
   ```

   Este código establece los elementos mediante el método `getItems()` de `CartService`. Definiste este método [cuando creaste](#definir-un-servicio-de-carrito) `cart.service.ts`.

4. Actualice la plantilla del carrito con un encabezado y use un `<div>` con un [`*ngFor`](https://angular.io/api/common/NgForOf) para mostrar cada uno de los elementos del carrito con su nombre y precio. La plantilla `CartComponent` resultante es la siguiente.

   ```html
   <!-- src/app/cart/cart.component.html -->
   <h3>Cart</h3>

   <div class="cart-item" *ngFor="let item of items">
     <span>{{ item.name }}</span>
     <span>{{ item.price | currency }}</span>
   </div>
   ```

5. Verifique que su carrito funcione como se esperaba:

   - Haga clic en **My Store**
   - Haga clic en el nombre de un producto para mostrar sus detalles.
   - Haga clic en **Buy** para agregar el producto al carrito.
   - Haga clic en **Checkout** para ver el carrito.

   ![](images\cart-page-full.png)

Para obtener más información sobre los servicios, consulte [Introducción a los servicios y la inyección de dependencia](https://angular.io/guide/architecture-services).

---

## Recuperar precios de envío

Los servidores a menudo devuelven datos en forma de flujo. Las transmisiones son útiles porque facilitan la transformación de los datos devueltos y la modificación de la forma en que solicita esos datos. Angular [`HttpClient`](https://angular.io/api/common/http/HttpClient) es una forma integrada de obtener datos de API externas y proporcionarlos a su aplicación como un flujo.

Esta sección le muestra cómo utilizar [`HttpClient`](https://angular.io/api/common/http/HttpClient) para recuperar los precios de envío de un archivo externo.

La aplicación que StackBlitz genera para esta guía viene con datos de envío predefinidos en formato `assets/shipping.json`. Utilice estos datos para agregar los precios de envío de los artículos en el carrito.

```json
[
  {
    "type": "Overnight",
    "price": 25.99
  },
  {
    "type": "2-Day",
    "price": 9.99
  },
  {
    "type": "Postal",
    "price": 2.99
  }
]
```

### Configurar AppModule para usar [`HttpClient`](https://angular.io/api/common/http/HttpClient)

Para usar Angular [`HttpClient`](https://angular.io/api/common/http/HttpClient), debe configurar su aplicación para usar [`HttpClientModule`](https://angular.io/api/common/http/HttpClientModule).

Angular [`HttpClientModule`](https://angular.io/api/common/http/HttpClientModule) registra los proveedores que su aplicación necesita para usar el servicio [`HttpClient`](https://angular.io/api/common/http/HttpClient) en toda su aplicación.

1. En `app.module.ts`, importe [`HttpClientModule`](https://angular.io/api/common/http/HttpClientModule) desde el paquete [`@angular/common/http`](https://angular.io/api/common/http) en la parte superior del archivo con las otras importaciones. Como hay varias otras importaciones, este fragmento de código se omiten por brevedad. Asegúrese de dejar las importaciones existentes en su lugar.

   ```typescript
   // src/app/app.module.ts
   import { HttpClientModule } from "@angular/common/http";
   ```

2. Para registrar los proveedores [`HttpClient`](https://angular.io/api/common/http/HttpClient) de Angular globalmente, agregue `HttpClientModule`](https://angular.io/api/common/http/HttpClientModule) al `AppModule` [`@NgModule()`](https://angular.io/api/core/NgModule) a la matriz `imports`.

   ```typescript
   // src/app/app.module.ts
   @NgModule({
     imports: [
       BrowserModule,
       HttpClientModule,
       ReactiveFormsModule,
       RouterModule.forRoot([
         { path: "", component: ProductListComponent },
         { path: "products/:productId", component: ProductDetailsComponent },
         { path: "cart", component: CartComponent },
       ]),
     ],
     declarations: [
       AppComponent,
       TopBarComponent,
       ProductListComponent,
       ProductAlertsComponent,
       ProductDetailsComponent,
       CartComponent,
     ],
     bootstrap: [AppComponent],
   })
   export class AppModule {}
   ```

### Configurar `CartService` para usar [`HttpClient`](https://angular.io/api/common/http/HttpClient)

El siguiente paso es inyectar el servicio [`HttpClient`](https://angular.io/api/common/http/HttpClient) en su servicio para que su aplicación pueda obtener datos e interactuar con APIs y recursos externos.

1. En `cart.service.ts`, importar [`HttpClient`](https://angular.io/api/common/http/HttpClient) desde el paquete [`@angular/common/http`](https://angular.io/api/common/http).

   ```typescript
   // src/app/cart.service.ts
   import { Injectable } from "@angular/core";
   import { HttpClient } from "@angular/common/http";
   import { Product } from "./products";
   ```

2. Inyectar [`HttpClient`](https://angular.io/api/common/http/HttpClient) en el `constructor()` de `CartService`.

   ```ts
   // export class CartService {
   items: Product[] = [];

   constructor(
       private http: HttpClient
   ) {}
   /* . . . */
   }
   ```

### Configurar `CartService` para obtener precios de envío

Para obtener datos de envío, `shipping.json` puede utilizar el método `get()` de [`HttpClient`](https://angular.io/api/common/http/HttpClient).

1. En `cart.service.ts`, debajo del método `clearCart()`, defina un nuevo método `getShippingPrices()` que utilice el método `get()` de [`HttpClient`](https://angular.io/api/common/http/HttpClient).

   ```ts
   // src/app/cart.service.ts
   export class CartService {
     /* . . . */
     getShippingPrices() {
       return this.http.get<{ type: string; price: number }[]>(
         "/assets/shipping.json"
       );
     }
   }
   ```

Para obtener más información sobre el [`HttpClient`](https://angular.io/api/common/http/HttpClient) de Angular, consulte la guía de [Interacción cliente-servidor](https://angular.io/guide/http).

---

## Crea un componente de envío

Ahora que ha configurado su aplicación para recuperar datos de envío, puede crear un lugar para representar esos datos.

1. Genere un nuevo componente nombrado `shipping` haciendo clic con el botón derecho en la carpeta `app`, eligiendo **Angular Generator** y seleccionando **Component**.

   ```ts
   // src/app/shipping/shipping.component.ts
   import { Component } from "@angular/core";

   @Component({
     selector: "app-shipping",
     templateUrl: "./shipping.component.html",
     styleUrls: ["./shipping.component.css"],
   })
   export class ShippingComponent {
     constructor() {}
   }
   ```

2. En `app.module.ts`, agregue una ruta para el envío. Especifique un `path` de `shipping` y un componente de `ShippingComponent`.

   ```ts
   // src/app/app.module.ts
   @NgModule({
     imports: [
       BrowserModule,
       HttpClientModule,
       ReactiveFormsModule,
       RouterModule.forRoot([
         { path: "", component: ProductListComponent },
         { path: "products/:productId", component: ProductDetailsComponent },
         { path: "cart", component: CartComponent },
         { path: "shipping", component: ShippingComponent },
       ]),
     ],
     declarations: [
       AppComponent,
       TopBarComponent,
       ProductListComponent,
       ProductAlertsComponent,
       ProductDetailsComponent,
       CartComponent,
       ShippingComponent,
     ],
     bootstrap: [AppComponent],
   })
   export class AppModule {}
   ```

   Todavía no hay un enlace al nuevo componente de envío, pero puede ver su plantilla en el panel de vista previa ingresando la URL que especifica su ruta. La URL tiene el patrón: `https://getting-started.stackblitz.io/shipping` donde la parte `getting-started.stackblitz.io` puede ser diferente para su proyecto StackBlitz.

### Configurar el `ShippingComponent` para usar `CartService`

Esta sección lo guía a través de la modificación de `ShippingComponent` para recuperar datos de envío a través de HTTP del archivo `shipping.json`.

1. En `shipping.component.ts`, importar `CartService`.

   ```ts
   // src/app/shipping/shipping.component.ts
   import { Component } from "@angular/core";

   import { CartService } from "../cart.service";
   ```

2. Inyecte el servicio de carrito en el `constructor()` de `ShippingComponent`.

   ```ts
   // src/app/shipping/shipping.component.ts
   constructor(private cartService: CartService) {
    }
   ```

3. Defina una propiedad `shippingCosts` que establezca la propiedad `shippingCosts` mediante el método `getShippingPrices()` de `CartService`.

   ```ts
   // src/app/shipping/shipping.component.ts
   export class ShippingComponent {
     shippingCosts = this.cartService.getShippingPrices();
   }
   ```

4. Actualice la plantilla `ShippingComponent` para mostrar los tipos de envío y los precios mediante la pipe [`async`](https://angular.io/api/common/AsyncPipe).

   ```html
   <!-- src/app/shipping/shipping.component.html -->
   <h3>Shipping Prices</h3>

   <div class="shipping-item" *ngFor="let shipping of shippingCosts | async">
     <span>{{ shipping.type }}</span>
     <span>{{ shipping.price | currency }}</span>
   </div>
   ```

   La pipe [`async`](https://angular.io/api/common/AsyncPipe) devuelve el último valor de un flujo de datos y continúa haciéndolo durante la vida útil de un componente determinado. Cuando Angular destruye ese componente, la pipe [`async`](https://angular.io/api/common/AsyncPipe) se detiene automáticamente. Para obtener información detallada sobre la pipe [`async`](https://angular.io/api/common/AsyncPipe), consulte la [documentación de la API de AsyncPipe](https://angular.io/api/common/AsyncPipe).

5. Agregue un enlace de la vista `CartComponent` a la vista `ShippingComponent`.

   ```html
   <!-- src/app/cart/cart.component.html -->
   <h3>Cart</h3>

   <p>
     <a routerLink="/shipping">Shipping Prices</a>
   </p>

   <div class="cart-item" *ngFor="let item of items">
     <span>{{ item.name }}</span>
     <span>{{ item.price | currency }}</span>
   </div>
   ```

6. Haga clic en el botón **Checkout** para ver el carrito actualizado. Recuerde que cambiar la aplicación hace que se actualice la vista previa, lo que vacía el carrito.

   ![](images\cart-empty-with-shipping-prices.png)

   Haga clic en el enlace para navegar a los precios de envío.

   ![](images\shipping-prices.png)

---

## Que sigue

Ahora tiene una aplicación de tienda con un catálogo de productos, un carrito de compras y puede buscar precios de envío.

Para continuar explorando Angular:

- Continúe con [Formularios de entrada del usuario](..\Formularios-para-la-entrada-del-usuario\README#Usar-formularios-para-la-entrada-del-usuario) para finalizar la aplicación agregando la vista del carrito de compras y un formulario de pago.
- Vaya a [Implementación]() para pasar al desarrollo local o implemente su aplicación en Firebase o en su propio servidor.
