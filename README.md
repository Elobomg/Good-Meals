# Good Meals (NeoForge 26.2)

Sistema de comida estilo Valheim / Enshrouded. Sustituye a Hearty Meals.

## Que hace

- **Vida base de 8 corazones** (16 de vida), configurable.
- **3 huecos de comida** en vertical a la izquierda de la barra de objetos. Al comer una comida de la lista se guarda en un hueco durante el tiempo configurado y da sus efectos y su **vida maxima** (se suma a la base mientras la comida esta activa).
- Cada comida **cura** al comerla la cantidad configurada. Si es negativa (carne cruda) quita vida, pero nunca mata.
- **5 categorias**: Carne, Pescado, Frutas, Verduras y Grano. No puedes comer una comida si ya tienes activa otra de su misma categoria, ni ninguna de la lista si los 3 huecos estan ocupados.
- Los drops de mobs (ojo de arana, carne podrida) estan en Carne y **restan vida maxima** mientras estan activos.
- **Sin hambre**: se oculta la barra de hambre. La regeneracion natural (la misma que con la barra de hambre llena) solo funciona con los 3 huecos ocupados.
- Los items que no estan en la lista se pueden comer siempre: curan su nutricion x `unlistedFoodHealMultiplier`, pero no usan huecos ni categorias.
- Cada comida muestra en su descripcion la curacion, la vida maxima, la categoria y los bufos con el icono del propio efecto de Minecraft, el nivel en romano y la duracion.
- Morir hace perder las comidas activas.

## Compilar

Hace falta JDK 25 y conexion a internet.

    ./gradlew build

El mod queda en `build/libs/goodmeals-1.2.1.jar` (o la version que indique `gradle.properties`). Hay que ponerlo en el servidor **y en todos los clientes**.

## Configuracion (un solo archivo)

`<mundo>/serverconfig/goodmeals-server.toml` (se crea la primera vez que arranca el mundo). La lista de comidas esta en la clave `meals`: si vienes de una version anterior, que usaba la clave `foods` con otro formato, se genera sola con los valores nuevos y la lista vieja se ignora. Se recarga sola al guardar y se sincroniza a los clientes.

Ajustes generales: `baseMaxHealth`, `disableHunger`, `regenOnlyWithFullSlots`, `unlistedFoodHealMultiplier`, `showEffectIcons`.

Cada linea de `meals` define una comida entera:

    "item; categoria; vida; vida maxima; segundos; efecto:nivel, efecto:nivel"

Ejemplo: `"minecraft:golden_carrot; vegetable; 6; 4; 900; minecraft:strength:2, minecraft:speed:2"`

- `categoria`: cualquier palabra. Las de por defecto son `meat`, `fish`, `fruit`, `vegetable` y `grain`.
- `vida`: puntos de vida que cura al comerla (2 = 1 corazon). Negativo = quita vida.
- `vida maxima`: puntos de vida maxima que suma (o resta) mientras la comida esta activa.
- `segundos`: duracion de la comida activa.
- `efecto:nivel`: opcional. Nivel del juego (1 = I, 2 = II). Se admiten efectos de otros mods con su id completo.

Para quitar una comida del sistema, borra su linea. Para anadir otra, anade una linea nueva.
