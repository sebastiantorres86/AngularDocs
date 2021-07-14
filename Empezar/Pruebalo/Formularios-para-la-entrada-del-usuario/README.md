# Usar formularios para la entrada del usuario

Esta guía se basa en el paso [Manejo de datos](..\manejo-de-datos\README) del tutorial Introducción, [Introducción a una aplicación básica de Angular](..\Empezar\README).

Esta sección lo guía a través de la adición de una función de pago basada en formularios para recopilar información del usuario como parte del pago.

---

## Definir el modelo de formulario de pago

Este paso le muestra cómo configurar el modelo de formulario de pago en la clase de componente. El modelo de formulario determina el estado del formulario.

1. Abrir `cart.component.ts`.

2. Importe el servicio [`FormBuilder`](https://angular.io/api/forms/FormBuilder) del paquete `@angular/forms`. Este servicio proporciona métodos convenientes para generar controles.

   ```ts
   // src/app/cart/cart.component.ts
   import { Component } from "@angular/core";
   import { FormBuilder } from "@angular/forms";

   import { CartService } from "../cart.service";
   ```

3. Inyecte el servicio [`FormBuilder`](https://angular.io/api/forms/FormBuilder) en el `constructor()` `CartComponent`. Este servicio es parte del módulo `ReactiveFormsModule`, que ya ha importado.

   ```typescript
   // src/app/cart/cart.component.ts
   export class CartComponent {
     constructor(
       private cartService: CartService,
       private formBuilder: FormBuilder
     ) {}
   }
   ```

4. Para recopilar el nombre y la dirección del usuario, utilice el método `group()` de [`FormBuilder`](https://angular.io/api/forms/FormBuilder) para establecer la propiedad `checkoutForm` en un modelo de formulario que contenga los campos `name` y `address`.

   ```ts
   // src/app/cart/cart.component.ts
   export class CartComponent {
     items = this.cartService.getItems();
     checkoutForm = this.formBuilder.group({
       name: "",
       address: "",
     });
     constructor(
       private cartService: CartService,
       private formBuilder: FormBuilder
     ) {}
   }
   ```

5. Defina un método `onSubmit()` para procesar el formulario. Este método permite a los usuarios enviar su nombre y dirección. Además, este método utiliza el método `clearCart()` de `CartService` para restablecer el formulario y borrar el carrito.

   Toda la clase de componentes del carrito es la siguiente:

   ```ts
   // src/app/cart/cart.component.ts
   import { Component } from "@angular/core";
   import { FormBuilder } from "@angular/forms";

   import { CartService } from "../cart.service";

   @Component({
     selector: "app-cart",
     templateUrl: "./cart.component.html",
     styleUrls: ["./cart.component.css"],
   })
   export class CartComponent {
     items = this.cartService.getItems();
     checkoutForm = this.formBuilder.group({
       name: "",
       address: "",
     });
     constructor(
       private cartService: CartService,
       private formBuilder: FormBuilder
     ) {}

     onSubmit(): void {
       // Process checkout data here
       this.items = this.cartService.clearCart();
       console.warn("Your order has been submitted", this.checkoutForm.value);
       this.checkoutForm.reset();
     }
   }
   ```

---

## Crea el formulario de pago

Utilice los siguientes pasos para agregar un formulario de pago en la parte inferior de la vista **Cart**.

1. En la parte inferior de `cart.component.html`, agregue un elemento HTML `<form>` y un botón **Purchase**.

2. Utilice un enlace de propiedad `formGroup` para enlazar `checkoutForm` al `<form>` HTML.

   ```html
   <!-- src/app/cart/cart.component.html -->
   <form [formGroup]="checkoutForm">
     <button class="button" type="submit">Purchase</button>
   </form>
   ```

3. En la etiqueta `form`, use un enlace de eventos `ngSubmit` para escuchar el envío del formulario y llame al método `onSubmit()` con el valor `checkoutForm`.

   ```html
   <!-- src/app/cart/cart.component.html (cart component template detail) -->
   <form [formGroup]="checkoutForm" (ngSubmit)="onSubmit()"></form>
   ```

4. Agregue campos `<input>` para `name` y `address`, cada uno con un atributo [`formControlName`](https://angular.io/api/forms/FormControlName) que se vincule a los controles de formulario `checkoutForm` para `name` y `address` a sus campos `<input>`. El componente completo es el siguiente:

   ```html
   <!-- src/app/cart/cart.component.html -->
   <h3>Cart</h3>

   <p>
     <a routerLink="/shipping">Shipping Prices</a>
   </p>

   <div class="cart-item" *ngFor="let item of items">
     <span>{{ item.name }} </span>
     <span>{{ item.price | currency }}</span>
   </div>

   <form [formGroup]="checkoutForm" (ngSubmit)="onSubmit()">
     <div>
       <label for="name"> Name </label>
       <input id="name" type="text" formControlName="name" />
     </div>

     <div>
       <label for="address"> Address </label>
       <input id="address" type="text" formControlName="address" />
     </div>

     <button class="button" type="submit">Purchase</button>
   </form>
   ```

Después de poner algunos artículos en el carrito, los usuarios pueden revisar sus artículos, ingresar su nombre y dirección y enviar su compra.

![](images\cart-with-items-and-form.png)

Para confirmar el envío, abra la consola para ver un objeto que contiene el nombre y la dirección que envió.

---

## Que sigue

Tiene una aplicación de tienda en línea completa con un catálogo de productos, un carrito de compras y una función de pago.

[Continúe con la sección "Implementación"](../Implementacion) para pasar al desarrollo local o implemente su aplicación en Firebase o en su propio servidor.
