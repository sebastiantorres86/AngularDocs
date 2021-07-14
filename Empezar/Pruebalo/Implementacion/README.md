# Implementar una aplicación

La implementación de su aplicación es el proceso de compilar o construir su código y alojar JavaScript, CSS y HTML en un servidor web.

Esta sección se basa en los pasos anteriores del tutorial de [introducción](..\Empezar\README#empezando-con-angular) y le muestra cómo implementar su aplicación.

---

## Prerrequisitos

Una práctica recomendada es ejecutar su proyecto localmente antes de implementarlo. Para ejecutar su proyecto localmente, necesita lo siguiente instalado en su computadora:

- [Node.js](https://nodejs.org/en/).

- La [CLI de Angular](https://cli.angular.io/). Desde la terminal, instale Angular CLI globalmente con:

  ```bash
  npm install -g @angular/cli
  ```

  Con la CLI de Angular, puede usar el comando ngpara crear nuevos espacios de trabajo, nuevos proyectos, servir su aplicación durante el desarrollo o producir compilaciones para compartir o distribuir.

---

## Ejecutando su aplicación localmente

1. Descargue el código fuente de su proyecto StackBlitz haciendo clic en el ícono `Download Project` en el menú de la izquierda, frente a `Project`, para descargar sus archivos.

2. Cree un nuevo espacio de trabajo de Angular CLI usando el comando `ng new`, donde `my-project-name` es lo que le gustaría llamar a su proyecto:

   ```bash
   ng new my-project-name
   ```

   Este comando muestra una serie de mensajes de configuración. Para este tutorial, acepte la configuración predeterminada para cada solicitud.

3. En su nueva aplicación generada por CLI, reemplace la carpeta `/src` con la carpeta `/src` de su descarga de `StackBlitz`.

4. Utilice el siguiente comando de la CLI para ejecutar su aplicación localmente:

   ```bash
   ng serve
   ```

5. Para ver su aplicación en el navegador, vaya a http://localhost:4200/. Si el puerto predeterminado 4200 no está disponible, puede especificar otro puerto con el indicador de puerto como en el siguiente ejemplo:

   ```bash
   ng serve --port 4201

   ```

  Mientras sirve su aplicación, puede editar su código y ver que los cambios se actualizan automáticamente en el navegador. Para detener el ecomando `ng serv`, presione `Ctrl` + `c`.

---

## Construyendo y hospedando su aplicación

1. Para construir su aplicación para producción, use el comando `build`. De forma predeterminada, este comando usa la configuración de compilación `production`.

   ```bash
   ng build
   ```

   Este comando crea una carpeta `dist` en el directorio raíz de la aplicación con todos los archivos que un servicio de alojamiento necesita para atender su aplicación.

   > Si el comando `ng build` anterior arroja un error sobre paquetes faltantes, agregue las dependencias faltantes en el archivo `package.json` de su proyecto local para que coincida con la del proyecto StackBlitz descargado.

2. Copie el contenido de la carpeta `dist/my-project-name` a su servidor web. Debido a que estos archivos son estáticos, puede alojarlos en cualquier servidor web capaz de servir archivos: como `Node.js`, Java, .NET o cualquier backend como [Firebase](https://firebase.google.com/docs/hosting), [Google Cloud](https://cloud.google.com/solutions/web-hosting), o [App Engine](https://cloud.google.com/appengine/docs/standard/python/getting-started/hosting-a-static-website). Para obtener más información, consulte [Creación & servicio](https://angular.io/guide/build) e [implementación](https://angular.io/guide/deployment).

---

## Que sigue

En este tutorial, ha sentado las bases para explorar el mundo Angular en áreas como el desarrollo móvil, el desarrollo de UX/UI y la representación del lado del servidor. Puede profundizar más al estudiar más características de Angular, interactuar con la comunidad vibrante y explorar el ecosistema robusto.

### Aprendiendo más Angular

Para obtener un tutorial más detallado que lo guíe a través de la creación de una aplicación local y la exploración de muchas de las funciones más populares de Angular, consulte [Tour of Heroes]().

Para explorar los conceptos fundamentales de Angular, consulte las guías en la sección Comprensión de Angular, como [Descripción general de componentes de Angular](https://angular.io/guide/component-overview) o [Sintaxis de plantilla](https://angular.io/guide/template-syntax).

### Uniéndose a la comunidad

[Tuitea que has completado este tutorial](https://twitter.com/intent/tweet?url=https://angular.io/start&text=I%20just%20finished%20the%20Angular%20Getting%20Started%20Tutorial), dinos lo que piensas o envíanos [sugerencias para futuras ediciones](https://github.com/angular/angular/issues/new/choose).

Manténgase actualizado siguiendo el [Blog de Angular](https://blog.angular.io/).

### Explorando el ecosistema Angular

Para respaldar su desarrollo de UX/UI, consulte [Material angular](https://material.angular.io/).

La comunidad Angular también tiene una extensa [red de herramientas y bibliotecas de terceros](https://angular.io/resources).
