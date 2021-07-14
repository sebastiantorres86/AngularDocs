# Agregar navegacion

Esta guía se basa en el primer paso del tutorial de introducción, [Introducción a una aplicación básica de Angular](..\Empezar\README#empezando-con-angular).

En esta etapa de desarrollo, la aplicación de la tienda en línea tiene un catálogo básico de productos.

En las siguientes secciones, agregará las siguientes funciones a la aplicación:

- Escriba una URL en la barra de direcciones para navegar a la página del producto correspondiente.
- Haga clic en los enlaces de la página para navegar dentro de su aplicación de una sola página.
- Haga clic en los botones de avance y retroceso del navegador para navegar por el historial del navegador de forma intuitiva.

---

## Asociar una ruta de URL con un componente

La aplicación ya usa Angular [`Router`](https://angular.io/api/router/Router) para navegar al `ProductListComponent`. Esta sección le muestra cómo definir una ruta para mostrar detalles de productos individuales.

1. Genere un nuevo componente para los detalles del producto. En la lista de archivos, haga clic con el botón derecho en la carpeta `app`, elija `Angular Generator` y [`Component`](https://angular.io/api/core/Component). Nombra al componente `product-details`.

2. En `app.module.ts`, agregue una ruta para los detalles del producto, con un `path` de `products/:productId` y `ProductDetailsComponent` para el `component`.

   ```typescript
   // src/app/app.module.ts
   @NgModule({
   imports: [
       BrowserModule,
       ReactiveFormsModule,
       RouterModule.forRoot([
       { path: '', component: ProductListComponent },
       { path: 'products/:productId', component: ProductDetailsComponent },
       ])
   ],
   ```

3. Abrir `product-list.component.html`.

4. Modifique el ancla del nombre del producto para incluir un [`routerLink`](https://angular.io/api/router/RouterLink) con `product.id` como parámetro.

   ```html
   <!-- src/app/product-list/product-list.component.html -->
   <div *ngFor="let product of products">
     <h3>
       <a
         [title]="product.name + ' details'"
         [routerLink]="['/products', product.id]"
       >
         {{ product.name }}
       </a>
     </h3>

     <!-- . . . -->
   </div>
   ```

   La directiva [`RouterLink`](https://angular.io/api/router/RouterLink) le ayuda a personalizar el elemento de anclaje. En este caso, la ruta, o URL, contiene un segmento fijo, `/products`. El segmento final es variable, insertando la propiedad `id` del producto actual. Por ejemplo, la URL de un producto con i`d` 1 sería similar a h`ttps://getting-started-myfork.stackblitz.io/products/1`.

5. Verifique que el enrutador funcione según lo previsto haciendo clic en el nombre del producto. La aplicación debería mostrar el `ProductDetailsComponent`, que actualmente dice "product-details works!"

   Observe que cambia la URL en la ventana de vista previa. El segmento final es `products/#` donde `#` es el número de la ruta en la que hizo clic.

   ![](images\product-details-works.png)

---

## Ver detalles del producto

`ProductDetailsComponent` maneja la exhibición de cada producto. El Angular Router muestra componentes basados ​​en la URL del navegador y [sus rutas definidas](https://angular.io/start/start-routing#define-routes).

En esta sección, usará el Angular Router para combinar los datos de `products` y la información de ruta para mostrar los detalles específicos de cada producto.

1. En `product-details.component.ts`, importar [`ActivatedRoute`](https://angular.io/api/router/ActivatedRoute) desde `@angular/router` y la matriz de `products` desde `../products`.

   ```typescript
   // src/app/product-details/product-details.component.ts
   import { Component, OnInit } from "@angular/core";
   import { ActivatedRoute } from "@angular/router";

   import { Product, products } from "../products";
   ```

2. Defina la propiedad `product`.

   ```typescript
   // src/app/product-details/product-details.component.ts
   export class ProductDetailsComponent implements OnInit {
     product: Product | undefined;
     /* ... */
   }
   ```

3. Inyecte [`ActivatedRoute`](https://angular.io/api/router/ActivatedRoute) en el `constructor()` agregando `private route: ActivatedRoute` como argumento dentro del paréntesis del constructor.

   ```typescript
   // src/app/product-details/product-details.component.ts
   export class ProductDetailsComponent implements OnInit {
     product: Product | undefined;

     constructor(private route: ActivatedRoute) {}
   }
   ```

   [`ActivatedRoute`](https://angular.io/api/router/ActivatedRoute) es específico para cada componente que carga el Angular Router. [`ActivatedRoute`](https://angular.io/api/router/ActivatedRoute) contiene información sobre la ruta y los parámetros de la ruta.

   Al inyectar [`ActivatedRoute`](https://angular.io/api/router/ActivatedRoute), está configurando el componente para utilizar un servicio. El paso [Gestión de datos]() cubre los servicios con más detalle.

4. En el método `ngOnInit()`, extraiga `productId` de los parámetros de ruta y busque el producto correspondiente en la matriz `products`.

   ```typescript
   // src / app / product-details / product-details.component.ts
   ngOnInit() {
   // First get the product id from the current route.
    const routeParams = this.route.snapshot.paramMap;
   const productIdFromRoute = Number(routeParams.get('productId'));

   // Find the product that correspond with the id provided in route.
   this.product = products.find(product => product.id === productIdFromRoute);
   }
   ```

   Los parámetros de la ruta corresponden a las variables de ruta que define en la ruta. Para acceder a los parámetros de la ruta utilizamos `route.snapshot`, que es el [`ActivatedRouteSnapshot`](https://angular.io/api/router/ActivatedRouteSnapshot) que contiene información sobre la ruta activa en ese momento concreto. La URL que coincide con la ruta proporciona el `productId`. Angular usa `productId` para mostrar los detalles de cada producto único.

5. Actualice la plantilla `ProductDetailsComponent` para mostrar los detalles del producto con un [`*ngIf`](https://angular.io/api/common/NgIf). Si existe un producto, `<div>` se muestra con un nombre, precio y descripción.

   ```html
   <!-- src/app/product-details/product-details.component.html -->
   <h2>Product Details</h2>

   <div *ngIf="product">
     <h3>{{ product.name }}</h3>
     <h4>{{ product.price | currency }}</h4>
     <p>{{ product.description }}</p>
   </div>
   ```

   La línea `<h4>{{ product.price | currency }}</h4>`, usa la pipe [`currency`](https://angular.io/api/common/CurrencyPipe) para transformar `product.price` de un número a una cadena de moneda. Una pipe es una forma en que puede transformar datos en su plantilla HTML. Para obtener más información sobre las tuberías angulares, consulte [Pipes](https://angular.io/guide/pipes).

Cuando los usuarios hacen clic en un nombre en la lista de productos, el enrutador los dirige a la URL distintiva del producto, muestra `ProductDetailsComponent` y muestra los detalles del producto.

![](images\product-details-works.png)

Para obtener más información sobre el enrutador angular, consulte [Enrutamiento y navegación](https://angular.io/guide/router).

---

## Que sigue

Ha configurado su aplicación para que pueda ver los detalles del producto, cada uno con una URL distinta.

Para continuar explorando Angular:

- Continúe con [Manejo de datos](../Manejo-de-datos) para agregar una función de carrito de compras, administrar los datos del carrito y recuperar datos externos para los precios de envío.
- Vaya a [Implementación]() para implementar su aplicación en Firebase o pasar al desarrollo local.
