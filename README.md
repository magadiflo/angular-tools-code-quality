# [Curso Angular 17 para el trabajo: Prettier + ESlint + Husky + Lintstaged](https://www.youtube.com/watch?v=gRMuLqOYIB0&t=16s)

- Curso tomado del canal de Youtube de LogiDev.
- This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 17.3.5.

---

## [Prettier](https://prettier.io/)

Es una herramienta de formateo de código. Su objetivo principal es asegurarse de que tu código se vea consistente y ordenado, sin importar quién lo haya escrito.

`Prettier` se encarga de ajustar el formato del código (por ejemplo, el espaciado, el uso de comillas, la longitud de las líneas, etc.) según reglas predefinidas. Esto hace que sea más fácil de leer y mantener el código, ya que todos los desarrolladores en un proyecto siguen el mismo estilo.

### Cómo generar configuraciones de prettier

A modo introductorio, vamos a ver cómo podemos generar configuraciones de prettier para usarlo en nuestros proyectos. El primer paso es ir a la siguiente dirección https://prettier.io/playground/ donde observaremos una interfaz dividido en 3 secciones:

1. Configuraciones
2. Código a la que se aplicarán las configuraciones
3. Vista previa del código con configuraciones aplicadas

Las configuraciones que realicemos debe ser de acuerdo a nuestras necesidad, de acuerdo al proyecto en el que vamos a trabajar, en ese sentido, todo el equipo debe estar de acuerdo con estas configuraciones, dado que todos vamos a trabajar con lo mismo.

Suponiendo que ya hemos jugado con las configuraciones y tenemos el que necesitamos para nuestro proyecto. El siguiente paso sería generar el `json` de estas configuraciones, para eso, damos clic en el botón `Copy config JSON`, tal como se ve a continuación:

![copy config json](./src/assets/02.copy-config-json.png)

Como resultado, tendremos copiado un json con las configuraciones realizadas que podemos usarlo más adelante en nuestro editor de código.

```json
{
  "arrowParens": "always",
  "bracketSameLine": false,
  "bracketSpacing": true,
  "semi": true,
  "experimentalTernaries": false,
  "singleQuote": false,
  "jsxSingleQuote": false,
  "quoteProps": "as-needed",
  "trailingComma": "all",
  "singleAttributePerLine": false,
  "htmlWhitespaceSensitivity": "css",
  "vueIndentScriptAndStyle": false,
  "proseWrap": "preserve",
  "insertPragma": false,
  "printWidth": 120,
  "requirePragma": false,
  "tabWidth": 2,
  "useTabs": false,
  "embeddedLanguageFormatting": "auto"
}
```

## [ESLint](https://eslint.org/)

Es una herramienta para identificar y corregir problemas en tu código Javascript o Typescript. A diferencia de Prettier, que se enfoca en el formato del código, `ESLint` se centra en encontrar errores y problemas de calidad del código, como variables no utilizadas, errores de sintaxis, problemas de rendimiento y buenas prácticas de programación. `ESLint` es altamente configurable, lo que significa que puedes definir tus propias reglas o usar reglas predefinidas según los estándares de la comunidad.

Al igual que prettier, `ESLint` también tiene su [playground](https://eslint.org/play/) donde podemos jugar buscando las configuraciones que necesitemos, aunque como se mencionó en el párrafo anterior, `ESLint` viene con configuraciones predefinidas.

## Husky

Es una herramienta que permite ejecutar scripts de Git Hooks de forma sencilla. Los Git Hooks son scripts que Git ejecuta antes o después de ciertos eventos, como commits, push o merges.

- Es una herramienta que permite ejecutar scripts de `Git hooks`, como `pre-commit`, `pre-push`, `post-commit`, `post-push`, etc.
- Te permite automatizar tareas como la ejecución de linters, formateadores, pruebas, y otras verificaciones antes de realizar commits o push.
- Con Husky, puedes asegurarte de que tu código siempre pase ciertas verificaciones antes de ser enviado al repositorio.

Ten en cuenta que existen hooks del lado del cliente y servidor.

- `Hooks del lado del cliente`: quien va a ejecutar la acción del `commit` o `push` es el desarrollador, entonces los `hooks` que se ejecutarán del lado del cliente serán los `pre-commit`, `pre-push`, `post-commit`, `post-push`.

- `Hooks del lado del servidor`: suponiendo que el repositorio a donde estamos pusheando el código está en `GitHub`, entonces, en el servidor el repositorio detectará que se está haciendo un push, por lo que se se activará un hook del lado del servidor y realizará alguna acción como verificar o ejecutar alguna acción, etc.

**Nota**
- Nosotros estaremos aplicando los `Git Hooks` del lado del cliente, específicamente nos enfocaremos en el `pre-commit`.
- Utilizaremos `Husky` para poder ejecutar `Prettier` y `ESLint` antes de realizar algún `commit` o `push`.

### Funcionamiento de Husky

Como developer hago mi `git commit`, la herramienta `Husky` detectará qué `hook` estamos utilizando de git, en nuestro caso será `pre-commit`. En ese `pre-commit` nosotros colocaremos que se desencadene la ejecución de `Prettier` (formatee nuestro código) y `ESLint` (inspeccione nuestro código a ver si hay algún problema de codificación). Si todo está bien, entonces se procede a realizar el `commit`, caso contrario no se ejecutará el `commit`.

![flow husky](./src/assets/03.flow-husky.png)

## LintStaged

Es una librería que detecta qué archivos han sido modificados para que se apliquen únicamente a ellos las reglas que nosotros indiquemos: `format`, `linter`, etc. En otras palabras, si no usamos esta librería de `LintStaged` las reglas que hayamos definido `format`, `linter`, etc. se aplicarán a todo el proyecto.

---

# Configurando VS Code y Angular para usar herramientas de calidad de código

---

## Extensiones en VS Code

Para el tema de calidad del código necesitamos instalar las siguientes extensiones en nuestro VS Code:

- Prettier - Code formatter
- ESLint

## Estableciendo Prettier como formateador por defecto

Primero debemos instalar la dependencia de `Prettier` en nuestro VS Code. Luego debemos establecer Prettier como formateador por defecto de la siguiente manera.

`Primer paso`, en un archivo cualquiera damos clic secundario y seleccionamos `Format Document With...`.

![Step 1](./src/assets/prettier/01.set-default-prettier.png)

`Segundo paso`, configuramos el formateador por defecto.

![Step 2](./src/assets/prettier/02.select-default-prettier.png)

`Tercer paso`, seleccionamos a `Prettier` como nuestro formateador por defecto.

![Step 3](./src/assets/prettier/03.selected-default-formatter.png)

## Activar Configuración Requerida de Prettier

`Prettier: Require Config`, requiere un archivo de configuración `Prettier` para formatear. Esta configuración le dice a VS Code que formatee el código del proyecto en base a un archivo de configuración de `Prettier` llamada `.prettierrc` (si es que lo tiene) que estará ubicada en la raíz del proyecto. Este archivo de configuración `.prettierrc` tendrá el `.json` que copiamos de la página de playground de prettier. En otras palabras, si el archivo `.prettierrc` está presente en el proyecto, entonce que formatee el código en base a las reglas que tenga definida en su interior.

![Require Config](./src/assets/prettier/04.check-require-config.png)

**NOTA**

> En nuestro caso no hemos definido el archivo `.prettierrc` en el proyecto de Angular, por lo tanto, las reglas que aplicará `Prettier` para formatear el código será en base a las reglas que tenga definida la extensión de `Prettier` que instalamos en nuestro `VS Code`. Además, recordar que hemos sido explícitos al seleccionar a `Prettier` como formateador por defecto para el proyecto.


## [Instalando dependencia de ESLint en Angular](https://github.com/angular-eslint/angular-eslint)

En nuestro proyecto de Angular, instalaremos la dependencia de [angular-eslint](https://github.com/angular-eslint/angular-eslint) con el siguiente comando:

```bash
$ ng add @angular-eslint/schematics
```

Finalizada la instalación veremos que nos ha creado un archivo llamado `.eslintrc.json` en nuestra aplicación de `Angular 17`. Ahora, si estuvieramos usando la `versión 18 de Angular`, el archivo que crearía sería un `eslint.config.js`. Ambos representan lo mismo, solo que en una versión está en `json` y en la otra en un archivo de `JavaScript`.

![config ESLint](./src/assets/eslint/01.config.png)

## [Instalando dependencia de Prettier en Angular](https://prettier.io/docs/en/install)

En nuestro proyecto de Angular, instalaremos la dependencia de [prettier](https://prettier.io/docs/en/install) con el siguiente comando:

```bash
$ npm install --save-dev --save-exact prettier
```

En el archivo `package.json` agregaremos el script siguiente para ejecutar `prettier`:

```json
"scripts": {
  ...
  "format": "prettier --write \"./src/**/*.{ts,json,html}\""
}
```

## [Instalando dependencia de Husky en Angular](https://typicode.github.io/husky/get-started.htmly)

En nuestro proyecto de Angular, instalaremos la dependencia de [husky](https://typicode.github.io/husky/get-started.htmly) con el siguiente comando:

```bash
$ npm install --save-dev husky
```
Luego que hemos instalado la dependencia, ejecutamos el comando init de husky para poder hacer uso de los `Git Hooks`. El comando init simplifica la configuración de Husky en un proyecto. Crea un script de confirmación previa en `.husky/` y actualiza el script de preparación en `package.json`. Se pueden realizar modificaciones más adelante para adaptarlas a su flujo de trabajo.

```bash
$ npx husky init
```

Luego de haber ejecutado el comando init, veremos que se nos habrá creado el directorio `.husky` conteniendo un `Git Hook`, en este caso el `pre-commit`.

![git hook](./src/assets/husky/01.git-hooks.png)

## [Instalando dependencia de Lint-Staged en Angular](https://github.com/lint-staged/lint-staged)

Instalamos la dependencia de [lint-staged](https://github.com/lint-staged/lint-staged) a través del siguiente comando:

```bash
$ npm install --save-dev lint-staged
```

### [Configurando lint-staged](https://github.com/lint-staged/lint-staged?tab=readme-ov-file#configuration)

`Lint-staged` puede ser configurado de muchas formas. En nuestro caso lo haremos de la siguiente manera. 

- En la raíz del proyecto crearemos el archivo `.lintstagedrc.json`
- A continuación debemos agregar la siguiente configuración en dicho archivo.

  ```json
  {
    "*.ts": ["prettier --write", "eslint"],
    "*.html": ["eslint", "prettier --write"],
    "*.scss": "prettier --write"
  }
  ```

### Configurando Husky con lint-staged

A continuación agregamos el siguiente comando en el archivo `pre-commit` del `Git Hook`:

```
npx lint-staged
```

El comando anterior utilizará las configuraciones definidas en el archivo `.lintstagedrc.json`.

## Reiniciar Visual Studio Code para aplicar cambios

Es importante que luego de que instalemos y configuremos la aplicación con las distintas herramientas de `code quality` reiniciemos el editor de `VS Code` para que se apliquen los cambios.

## Probando ejecución de herramientas

Realicemos alguna modificación en nuestro componente `app.component`:

```html
<h1>{{ title }}</h1>
<img src="./assets/angular_icon_gradient.gif" alt="" class="img-main">
<router-outlet />
```

```typescript
@Component({
  selector: 'app-root',
  standalone: true,
  imports: [RouterOutlet],
  templateUrl: './app.component.html',
  styleUrl: './app.component.scss',
})
export class AppComponent {
  public title = 'Herramientas de Angular para Calidad de Código';
  public money: any = 50;
}
```

Suponiendo que ya hemos finalizado nuestras modificaciones, llega el momento de realizar un commit al repositorio, así que cuando realizamos dicho commit, nuestro `Git Hook` `pre-commit` se ejecutará, e internamente ejecutará las herramientas de `Prettier` y `ESLint` para formatear y verificar si el código está cumpliendo las reglas definidas.

Al realizar el commit de las modificaciones realizadas al componente, vemos el siguiente resultado:

![run-tools](./src/assets/husky/02.run-tools.png)

Como resultado `no se realizó el commit`, dado que `eslint` ha detectad fallas en el archivo `app.component.ts`, tal como se ve a continuación:

![eslint - error](./src/assets/eslint/02.es-lint-error.png)

Si damos click en `Quick Fix...` veremos una lista de posibles soluciones que se pueden aplicar de manera automática:

![quick fix](./src/assets/eslint/03.quick-fix.png)

Finalmente, vemos que en las nuevas versiones de Angular de Typescript se recomienta usar el `unknown` en reemplazo de `any`:

![quick fix](./src/assets/eslint/04.fixed.png)

Luego de haber solucionado todos los errores que se han detectado, volvemos a subir los cambios al repositorio, esta vez el `commit` pasará sin ningún problema.

![commit successfull](./src/assets/husky/03.commit-successfull.png)
