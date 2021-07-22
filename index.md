
# Documentación 

## Instalación del paquete typedoc

Este paquete se instala ejecutando ```typescript npm i -D typedoc``` porque lo queremos como dependencia de desarrollo, de ahí la '-D', que podría sustituirse por '--save-dev'.

Una vez instalado nos creamos un archivo llamado typedoc.json y dentro del mismo incluimos lo siguiente: 

```typescript
{
  "entryPoints": ["./src/ruta_a_archivo_documentado.ts"],
  "out": "./docs"
}
```

Ahora solo necesitamos saber cómo se debe comentar un código, para ello tendremos que leer la [documentación](https://typedoc.org/) del paquete sobre su uso adecuado. Además si estamos usando eslint, necesitaremos habilitar algunas reglas para que el editor no nos pida comentarios de jsdoc, ya que usaremos los de typedoc.

El eslintrc.json deberia tener una forma similar a esta en el apartado de reglas: 
```
    "rules": {
        "quotes": "off",
        "require-jsdoc": "off",
        "valid-jsdoc": "off"
    }
```

Una vez tengamos todo esto, necesitaremos establecer un script en nuestro package.json para generar la documentación: 

En este caso incluyo también el script correspondiente a los tests, que es la parte que veremos a continuación.
```
  "scripts": {
    "test": "mocha",
    "start": "tsc-watch --onSuccess \"node dist/basicFunctions.js\"",
    "doc": "typedoc; touch ./docs/.nojekyll"
  },
```

Básicamente para generar la documentación necesitamos instalar el paquete de typedoc, poner un script para que genere la documentacion según los entryPoints que le indicamos, y para ello obviamente deberemos haber comentado correctamente los ficheros deseados.

# Testing 

## Instalación de los paquetes mocha y chai

```
npm i -D mocha chai @types/mocha @types/chai
```

Como vemos, también instalamos paquetes que nos ayudan a integrar tanto mocha como chai a typescript, es decir, esos paquetes incluyen la adaptación al tipado que ofrece el lenguaje que estamos utilizando.

Luego de instalar ambos paquetes nos creamos un fichero .mocharc.json y ponemos lo siguiente en él: 

```
{
  "extension": ["ts"],
  "spec": "tests/**/*.spec.ts",
  "require": "ts-node/register"
}
```

Con esto indicamos que los tests estarán incluidos en ficheros con extension ts, y estarán situados en cualquier directorio dentro de la carpeta tests, donde además de ts tienen en su prefijo completo el .spec.ts. 

Con todo esto, más el script para lanzar los tests, ya solo nos queda ver como realmente se harían tests en una aplicación.



# Ejemplos

## Ejemplo de documentación


En ./src/ejercicio-01.spec.ts
```
/**
 * @description Function that calculates if a year is a lap year or not
 * @param year Consists on the year passed as parameter
 * @return Returns true or false deppending on the year
 * (true if lap year , false if not)
 *
 * Usage:
 * ```typescript
 * isLapYear(2000)
 * ```
 */
export function isLapYear(year: number): boolean {
  let leap: boolean = false;
  if (year % 4 == 0) {
    if (year % 100 == 0) {
      (year % 400 == 0) ? leap = true : leap = false;
    } else leap = true;
  } else leap = false;
  return leap;
}
```
## Ejemplo de tests


En ./tests/ejercicio-01.spec.ts
```
import 'mocha';
import {expect} from 'chai';
import {isLapYear} from '../src/ejercicio-01';
describe('Exercise-01: isLapYear function tests', () => {
  it('isLapYear(1900)', () => {
    expect(isLapYear(1900)).to.equal(false);
  });
  it('isLapYear(1996)', () => {
    expect(isLapYear(1996)).to.equal(true);
  });
  it('isLapYear(1997)', () => {
    expect(isLapYear(1997)).to.equal(false);
  });
  it('isLapYear(2000)', () => {
    expect(isLapYear(2000)).to.equal(true);
  });
});
```