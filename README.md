# Good Meals (NeoForge 26.2)

Sistema de comida estilo Valheim / Enshrouded. Sustituye a Hearty Meals.

## Que hace

- **Vida base de 8 corazones** (16 de vida), configurable.
- **3 huecos de comida** en vertical a la izquierda de la barra de objetos. Al comer una comida definida se guarda en un hueco durante su tiempo y da sus efectos y su **vida maxima** (se suma a la base mientras esta activa).
- Cada comida **cura** al comerla la cantidad definida. Si es negativa (carne cruda) quita vida, pero nunca mata.
- **Categorias**: no puedes comer una comida si ya tienes activa otra de su misma categoria, ni ninguna si los 3 huecos estan ocupados.
- **Sin hambre**: se oculta la barra de hambre. La regeneracion natural solo funciona con los 3 huecos ocupados.
- Los items que no estan definidos se pueden comer siempre: curan su nutricion x `unlistedFoodHealMultiplier`, pero no usan huecos ni categorias.
- Morir hace perder las comidas activas.

## Descripcion de las comidas

1. Clase de comida (Carne, Pescado, Frutas, Verduras, Grano, Lacteos, Bebida...)
2. Duracion
3. Vida maxima (rojo)
4. Vida curada (verde)
5. Bufos: icono del efecto de Minecraft, nombre y nivel en romano

## Compilar

Hace falta JDK 25 y conexion a internet.

    ./gradlew build

El mod queda en `build/libs/goodmeals-1.3.0.jar`. Hay que ponerlo en el servidor **y en todos los clientes**.

## Comidas: JSON en `config/goodmeals/foods/`

La primera vez que arranca el servidor se crea la carpeta con dos archivos:

- `vanilla.json`: las comidas de Minecraft.
- `hbm.json`: comidas y bebidas de NTM Next (si el mod no esta instalado, simplemente no se usa).

**Se cargan todos los `.json` de la carpeta** (orden alfabetico; si una comida aparece en dos archivos gana el ultimo). Para otro mod (Farmer's Delight, AgriCraft, Cooking for Blockheads...) crea un archivo nuevo, por ejemplo `farmersdelight.json`. Los cambios se aplican con `/reload` y se envian solos a los jugadores conectados.

Formato:

    {
      "foods": {
        "minecraft:golden_carrot": {
          "category": "vegetable",
          "seconds": 900,
          "heal": 6,
          "max_health": 4,
          "effects": [
            { "effect": "minecraft:strength", "level": 2 },
            { "effect": "minecraft:speed", "level": 2 }
          ]
        }
      }
    }

- `category`: cualquier palabra. Con traduccion: `meat`, `fish`, `fruit`, `vegetable`, `grain`, `dairy`, `drink`. Cualquier otra se muestra tal cual.
- `seconds`: cuanto dura la comida activa (efectos y vida maxima).
- `heal`: puntos de vida que cura al comerla (2 = 1 corazon). Negativo = quita vida. Opcional.
- `max_health`: puntos de vida maxima que suma (o resta) mientras esta activa. Opcional.
- `effects`: lista de efectos con su nivel del juego (1 = I, 2 = II). Opcional. Valen efectos de otros mods con su id completo.

Los archivos incluyen un campo `_readme` que se ignora. Las lineas con errores se ignoran y se avisa en el log.

## Ajustes generales

`<mundo>/serverconfig/goodmeals-server.toml`: `baseMaxHealth`, `disableHunger`, `regenOnlyWithFullSlots`, `unlistedFoodHealMultiplier`, `showEffectIcons`. Se recarga sola al guardar.
