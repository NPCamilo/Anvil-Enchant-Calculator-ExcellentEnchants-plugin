# ⚒ Minecraft Anvil Enchantment Calculator

Calcula el orden óptimo de encantamientos para minimizar el costo de XP en el yunque de Minecraft.

## Características

- **Algoritmo de fuerza bruta** que prueba todas las combinaciones posibles de merge para encontrar el camino más barato (inspirado en [iamcal.github.io/enchant-order](https://iamcal.github.io/enchant-order/))
- **Vanilla + ExcellentEnchants**: soporta todos los encantamientos vanilla (17 tipos de objeto) y 80+ encantamientos del plugin ExcellentEnchants
- **Detección de conflictos** en tiempo real (ej: Sharpness ≠ Smite)
- **Presets rápidos**: Max Sword, Max Pickaxe, Fortune, Silk Touch, etc.
- **Penalización de prior work** calculada correctamente con la fórmula `2^usos - 1`
- **Umbral "Too Expensive!"** a 39 niveles por operación de yunque
- **UX completa**: búsqueda, selector de niveles con +/−, resumen de selección, vista previa del objeto final
- **100% client-side**: un solo archivo HTML, sin dependencias externas ni backend

## Encantamientos soportados

### Vanilla (17 tipos de objeto)
Espada, Hacha, Pico, Pala, Azada, Casco, Pechera, Grebas, Botas, Arco, Ballesta, Tridente, Maza, Caña de pescar, Escudo, Tijeras, Elytra

### ExcellentEnchants (80+ encantamientos)
Bane of Netherspawn, Ice Aspect, Telekinesis, Blast Mining, Veinminer, Night Vision, Regrowth, Frost Walker, Soulbound, y muchos más agrupados por categoría de objeto.

## Fórmulas del juego

| Concepto | Fórmula |
|----------|---------|
| Costo de encantamiento (libro) | `peso × nivel` |
| Costo de encantamiento (objeto) | `multiplicador_objeto × nivel` |
| Penalización previa | `2^usos_izq - 1 + 2^usos_der - 1` |
| Costo total de merge | `penalización + costo_encantamiento` |
| XP necesaria (nv ≤ 16) | `nivel² + 6×nivel` |
| XP necesaria (nv ≤ 31) | `2.5×nivel² - 40.5×nivel + 360` |
| XP necesaria (nv > 31) | `4.5×nivel² - 162.5×nivel + 2220` |

## Cómo funciona el algoritmo

1. Cada encantamiento seleccionado se crea como un libro individual (useCount = 0)
2. El algoritmo prueba **todas las combinaciones posibles** de merges:
   - Fusionar un libro con el objeto (usa multiplicador de objeto)
   - Fusionar dos libros entre sí (usa multiplicador de libro)
3. Usa **memoización** por firma de encantamientos para evitar cálculos repetidos
4. Para conjuntos > 6 encantamientos, usa un algoritmo greedy como fallback por rendimiento
5. Selecciona la secuencia con el **costo total más bajo**

## Uso local

Solo abre `index.html` en cualquier navegador moderno. No requiere servidor.

## Créditos

- Algoritmo basado en [iamcal.github.io/enchant-order](https://iamcal.github.io/enchant-order/)
- Datos ExcellentEnchants del plugin de Minecraft del mismo nombre
- Interfaz diseñada con estética forge/anvil

## Licencia

MIT
