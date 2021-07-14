# ¿Qué es Angular?

Este tema puede ayudarlo a comprender Angular: qué es Angular, qué ventajas ofrece y qué puede esperar al comenzar a construir sus aplicaciones.

Angular es una plataforma de desarrollo, construida sobre [TypeScript](https://www.typescriptlang.org/). Como plataforma Angular incluye:

- Un framework basado en componentes para crear aplicaciones web escalables
- Una colección de bibliotecas bien integradas que cubren una amplia variedad de características, que incluyen enrutamiento, administración de formularios, comunicación cliente-servidor y más.
- Un conjunto de herramientas para desarrolladores que lo ayudarán a desarrollar, compilar, probar y actualizar su código

Con Angular, está aprovechando una plataforma que puede escalar desde proyectos de un solo desarrollador hasta aplicaciones de nivel empresarial. Angular está diseñado para que la actualización sea lo más fácil posible, para que pueda aprovechar los últimos desarrollos con un mínimo de esfuerzo. Lo mejor de todo es que el ecosistema Angular consta de un grupo diverso de más de 1,7 millones de desarrolladores, autores de bibliotecas y creadores de contenido.

> Ver el [ejemplo en vivo](https://angular.io/generated/live-examples/what-is-angular/stackblitz.html)/[ejemplo de descarga](https://angular.io/generated/zips/what-is-angular/what-is-angular.zip) para ver un ejemplo práctico que contiene los fragmentos de código de esta guía.

---

## Aplicaciones Angular - lo esencial

Esta sección explica las ideas centrales detrás de Angular. Comprender estas ideas puede ayudarlo a diseñar y construir sus aplicaciones de manera más efectiva.

### Componentes

Los componentes son los bloques de construcción que componen una aplicación. Un componente incluye una clase de TypeScript con un decorador `@Component()`, una plantilla HTML y estilos. El decorador  `@Component()` especifica la siguiente información específica de Angular:

- Un selector de CSS que define cómo se usa el componente en una plantilla. Los elementos HTML de su plantilla que coinciden con este selector se convierten en instancias del componente.
- Una plantilla HTML que le indica a Angular cómo renderizar el componente.
- Un conjunto opcional de estilos CSS que definen la apariencia de los elementos HTML de la plantilla.

El siguiente es un componente angular mínimo.

```typescript
import { Component } from "@angular/core";

@Component({
  selector: "hello-world",
  template: `
    <h2>Hello World</h2>
    <p>This is my first component!</p>
  `,
})
export class HelloWorldComponent {
  // The code in this class drives the component's behavior.
}
```

Para utilizar este componente, escriba lo siguiente en una plantilla:

```typescript
<hello-world></hello-world>
```

Cuando Angular renderiza este componente, el DOM resultante se ve así:

```html
<hello-world>
  <h2>Hello World</h2>
  <p>This is my first component!</p>
</hello-world>
```

El modelo de componentes de Angular ofrece una encapsulación sólida y una estructura de aplicación intuitiva. Los componentes también facilitan la prueba unitaria de su aplicación y pueden mejorar la legibilidad general de su código.

Para obtener más información sobre lo que puede hacer con los componentes, consulte la sección [Componentes]().

### Plantillas

Cada componente tiene una plantilla HTML que declara cómo se representa ese componente. Defina esta plantilla en línea o por ruta de archivo.

Angular extiende HTML con sintaxis adicional que le permite insertar valores dinámicos desde su componente. Angular actualiza automáticamente el DOM renderizado cuando cambia el estado de su componente. Una aplicación de esta función es la inserción de texto dinámico, como se muestra en el siguiente ejemplo.

```typescript
<p>{{ message }}</p>
```

El valor del mensaje proviene de la clase de componente:

```typescript
import { Component } from "@angular/core";

@Component({
  selector: "hello-world-interpolation",
  templateUrl: "./hello-world-interpolation.component.html",
})
export class HelloWorldInterpolationComponent {
  message = "Hello, World!";
}
```

Cuando la aplicación carga el componente y su plantilla, el usuario ve lo siguiente:

```html
<p>Hello, World!</p>
```

Observe el uso de llaves dobles: le indican a Angular que interpole el contenido dentro de ellas.

Angular también admite enlaces de propiedad, para ayudarlo a establecer valores para propiedades y atributos de elementos HTML y pasar valores a la lógica de presentación de su aplicación.

```typescript
<p [id]="sayHelloId" [style.color]="fontColor">You can set my color in the component!</p>
```

Observe el uso de corchetes: esa sintaxis indica que está vinculando la propiedad o atributo a un valor en la clase del componente.

También puede declarar a los detectores de eventos para que escuchen y respondan a las acciones del usuario, como las pulsaciones de teclas, los movimientos del mouse, los clics y los toques. Declara un detector de eventos especificando el nombre del evento entre paréntesis:

```typescript
<button (click)="sayMessage()" [disabled]="canClick">Trigger alert message</button>
```

El ejemplo anterior llama a un método, que se define en la clase de componente:

```typescript
sayMessage() {
    alert(this.message);
}
```

El siguiente es un ejemplo de interpolación y enlaces dentro de una plantilla angular:

```typescript
// hello-world-bindings.component.ts
import { Component } from "@angular/core";

@Component({
  selector: "hello-world-bindings",
  templateUrl: "./hello-world-bindings.component.html",
})
export class HelloWorldBindingsComponent {
  fontColor = "blue";
  sayHelloId = 1;
  canClick = false;
  message = "Hello, World";
  sayMessage() {
    alert(this.message);
  }
}
```

```html
<!-- hello-world-bindings.component.html -->
<button (click)="sayMessage()" [disabled]="canClick">
  Trigger alert message
</button>
<p [id]="sayHelloId" [style.color]="fontColor">
  You can set my color in the component!
</p>
```

Puede agregar funcionalidad adicional a sus plantillas mediante el uso de [directivas](https://angular.io/guide/built-in-directives). Las directivas más populares en Angular son [`*ngIf`](https://angular.io/api/common/NgIf) y [`*ngFor`](https://angular.io/api/common/NgForOf). Puede utilizar directivas para realizar una variedad de tareas, como modificar dinámicamente la estructura DOM. Y también puede crear sus propias directivas personalizadas para crear excelentes experiencias de usuario.

El siguiente código es un ejemplo de la directiva [`*ngIf`](https://angular.io/api/common/NgIf).

```typescript
// hello-world-ngif.component.ts
import { Component } from "@angular/core";

@Component({
  selector: "hello-world-ngif",
  templateUrl: "./hello-world-ngif.component.html",
})
export class HelloWorldNgIfComponent {
  message = "I'm read only!";
  canEdit = false;

  onEditClick() {
    this.canEdit = !this.canEdit;
    if (this.canEdit) {
      this.message = "You can edit me!";
    } else {
      this.message = "I'm read only!";
    }
  }
}
```

```typescript
// hello-world-ngif.component.html
<h2>Hello World: ngIf!</h2>
<button (click)="onEditClick()">Make text editable!</button>
<div *ngIf="canEdit; else noEdit">
    <p>You can edit the following paragraph.</p>
</div>
<ng-template #noEdit>
    <p>The following paragraph is read only. Try clicking the button!</p>
</ng-template>
<p [contentEditable]="canEdit">{{ message }}</p>
```

Las plantillas declarativas de Angular le permiten separar limpiamente la lógica de su aplicación de su presentación. Las plantillas se basan en HTML estándar, por lo que son fáciles de crear, mantener y actualizar.

Para obtener más información sobre lo que puede hacer con las plantillas, consulte la sección [Plantillas](https://angular.io/guide/template-syntax).

### Inyección de dependencia

La inyección de dependencias le permite declarar las dependencias de sus clases de TypeScript sin ocuparse de su instanciación. En cambio, Angular maneja la instanciación por usted. Este patrón de diseño le permite escribir código más comprobable y flexible. Aunque comprender la inyección de dependencia no es fundamental para comenzar a usar Angular, lo recomendamos encarecidamente como una mejor práctica y muchos aspectos de Angular lo aprovechan hasta cierto punto.

Para ilustrar cómo funciona la inyección de dependencia, considere el siguiente ejemplo. El primer archivo `logger.service.ts`, define una clase `Logger`. Esta clase contiene una función `writeCount` que registra un número en la consola.

```typescript
import { Injectable } from "@angular/core";

@Injectable({ providedIn: "root" })
export class Logger {
  writeCount(count: number) {
    console.warn(count);
  }
}
```

A continuación, el archivo `hello-world-di.component.ts` define un componente angular. Este componente contiene un botón que usa la función `writeCount` de la clase Logger. Para acceder a esa función, el servicio `Logger` se inyecta en la clase `HelloWorldDI` agregando `private logger: Logger` al constructor.

```typescript
import { Component } from "@angular/core";
import { Logger } from "../logger.service";

@Component({
  selector: "hello-world-di",
  templateUrl: "./hello-world-di.component.html",
})
export class HelloWorldDependencyInjectionComponent {
  count = 0;

  constructor(private logger: Logger) {}

  onLogMe() {
    this.logger.writeCount(this.count);
    this.count++;
  }
}
```

Para obtener más información sobre la inyección de dependencia y Angular, consulte la sección [Inyección de dependencia en Angular](https://angular.io/guide/dependency-injection).

---

## CLI angular

Angular CLI es la forma más rápida, fácil y recomendada de desarrollar aplicaciones Angular. Angular CLI facilita una serie de tareas. Aquí hay unos ejemplos:

|                                                |                                                                                       |
| ---------------------------------------------- | ------------------------------------------------------------------------------------- |
| [ng build](https://angular.io/cli/build)       | Compila una aplicación Angular en un directorio de salida.                            |
| [ng serve](https://angular.io/cli/serve)       | Crea y sirve su aplicación, reconstruyéndola según los cambios de archivo.            |
| [ng generate](https://angular.io/cli/generate) | Genera o modifica archivos basándose en un esquema.                                   |
| [ng test](https://angular.io/cli/test)         | Ejecuta pruebas unitarias en un proyecto determinado.                                 |
| [ng e2e](https://angular.io/cli/e2e)           | Construye y sirve una aplicación Angular, luego ejecuta pruebas de un extremo a otro. |

Encontrará que Angular CLI es una herramienta valiosa para crear sus aplicaciones.

Para obtener más información sobre la CLI de Angular, consulte la sección [Referencia de la CLI](https://angular.io/cli).

---

## Bibliotecas propias

La sección, [Aplicaciones Angular - lo esencial](#aplicaciones-angular---lo-esencial), proporciona una breve descripción general de un par de elementos arquitectónicos clave que utilizará al crear aplicaciones Angular. Pero los muchos beneficios de Angular realmente se hacen evidentes cuando su aplicación crece y desea agregar funciones adicionales como la navegación del sitio o la entrada del usuario. Ahí es cuando puede aprovechar la plataforma Angular para incorporar una de las muchas bibliotecas de origen que proporciona Angular.

Algunas de las bibliotecas disponibles para usted incluyen:

|                                                              |                                                                                                                                                                               |
| ------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Angular Router](https://angular.io/guide/router)            | Navegación y enrutamiento avanzados del lado del cliente basados en componentes Angular. Admite carga diferida, rutas anidadas, coincidencia de rutas personalizadas y más. |
| [Angular Forms](https://angular.io/guide/forms-overview)     | Sistema uniforme de participación y validación de formularios.                                                                                                                |
| [Angular HttpClient](https://angular.io/guide/http)          | Cliente HTTP robusto que puede impulsar una comunicación cliente-servidor más avanzada.                                                                                       |
| [Angular Animations](https://angular.io/guide/animations)    | Sistema rico para conducir animaciones según el estado de la aplicación.                                                                                                      |
| [Angular PWA](https://angular.io/guide/service-worker-intro) | Herramientas para crear aplicaciones web progresivas (PWA), incluido un service worker y un manifiesto de aplicación web.                                                     |
| [Angular Schematics](https://angular.io/guide/schematics)    | Herramientas automatizadas de andamiaje, refactorización y actualización que simplifican el desarrollo a gran escala.                                                         |

Estas bibliotecas amplían la funcionalidad de su aplicación al mismo tiempo que le permiten concentrarse más en las características que hacen que su aplicación sea única. Y puede agregar estas bibliotecas sabiendo que están diseñadas para integrarse sin problemas y actualizarse simultáneamente con el framework Angular.

Estas bibliotecas solo son necesarias si y cuando pueden ayudarlo a agregar funcionalidad a sus aplicaciones o resolver un problema en particular.

---

## Próximos pasos

Este tema está destinado a brindarle una breve descripción general de lo que es Angular, las ventajas que brinda y lo que puede esperar al comenzar a construir sus aplicaciones.

Para ver Angular en acción, vea nuestro tutorial [Empezar](./Pruebalo/Empezar). Este tutorial utiliza
[stackblitz.com](https://stackblitz.com/), para que pueda explorar un ejemplo funcional de Angular sin ningún requisito de instalación.

Para explorar más las capacidades de Angular, recomendamos leer las secciones Comprensión de Angular y Guías para desarrolladores.
