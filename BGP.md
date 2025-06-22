It's most commonly used between different Autonomous Systems (AS).

Es importante que usemos filtros para manejar las rutas.

## Path Vector

Es la categoría a la cual pertenece BGP.
BGP no anuncia sus rutas continuamente a menos que hayan cambios en ella.
- Su métrica está basada en una lista de "attributes":
	1. **Weight**
		1. En un valor local al *router*. El valor por defecto es 0 para todas las rutas no originadas del *router* local
	2. **Local Preference**
		1. Se usa dentro de un AS y es intercambiado entre *routers* iBGP
		2. Se usa el camino on la **preferencia más alta**
	3. **Originate**
		1. Se prefiere la ruta que fue creada por el *router* local
	4. **AS Path Length**
		1. Prefiere el camino con el **AS Path Length** más  corto
	5. Y más

## Tipos de BGP

- eBGP
	- Los vecinos se encuentran en la misma AS
- eBGP
	- Los vecinos se encuentran en distinta AS

## Configuración

En BGP tenemos que ingresar la red y netmask exacta, y la ruta que ingresemos debe encontrarse en la tabla de *routeo*.
Si no existe la ruta especifica en nuetra tabla de *routeo*, debemos crearla. Creamos la ruta como una "discard route"