# Bun
## bun en windows instalando lynx

```bash
bun create rspeedy@latest
```
## bun en windows pc instalando lynx
520 packages installed [67.06s]
```bash
 bun install
```
## bun en macos m2 instalando lynx
518 packages installed [8.03s]
```bash
 bun install
```
## bun en wsl pc instalando lynx
521 packages installed [12.74s]
```bash
 bun install
```
## bun en Linux Arch instalando lynx
521 packages installed [15s]
```bash
 bun install
```



# Yarn
## yarn en windows instalando lynx

## Yarn en wsl pc instalando lynx
510 packages installed [12s]
```bash
 yarn install
```
## Yarn en powershell instalando lynx
510 packages installed [17s]
```bash
 yarn install
```
## Yarn en macos m2 instalando lynx
518 packages installed [8.03s]
```bash
 Yarn install
```
## Yarn en Linux Arch instalando lynx
518 packages installed [11s]
```bash
 Yarn install
```


# Pnpm
## Ejecutando en windows con Pnpm
520 packages installed [61s]
```bash
 Pnpm install
```
## pnpm en wsl pc instalando lynx
510 packages installed [15s]
```bash
 pnpm install
```
## pnpm en macos m2 instalando lynx
518 packages installed [12s]
```bash
 pnpm install
```
## pnpm en Linux Arch instalando lynx
518 packages installed [12s]
```bash
 pnpm install
```

| **Plataforma**   | **Bun**                | **Yarn**               | **Pnpm**               |
|-------------------|------------------------|------------------------|------------------------|
| **Windows**       | 520 paquetes / 67.06s  | 510 paquetes / 17s     | 520 paquetes / 61s     |
| **WSL (Windows)** | 521 paquetes / 12.74s  | 510 paquetes / 12s     | 510 paquetes / 15s     |
| **macOS M2**      | 518 paquetes / 8.03s   | 518 paquetes / 8.03s   | 518 paquetes / 12s     |
| **Linux Arch**    | 521 paquetes / 15s     | 518 paquetes / 11s     | 518 paquetes / 12s     |


## Análisis clave:
### Rendimiento por gestor :
- Bun destaca en macOS M2 (8.03s) y WSL (12.74s).
- Yarn muestra tiempos consistentes en macOS M2 (8.03s) y WSL (12s).
- Pnpm es notablemente más lento en Windows (61s) comparado con otras plataformas.
### Comparación entre plataformas :
- Windows nativo : Tiempos significativamente mayores (especialmente Bun y Pnpm).
- WSL : Mejora notable en Windows, con tiempos similares a Linux nativo.
- macOS M2/Linux Arch : Rendimiento óptimo en sistemas Unix-like.
### Variación de paquetes :
- Diferencias menores en el número de paquetes instalados (510-521), posiblemente por resolución de dependencias o caché.
### Observaciones:
- Bun muestra mayor velocidad en entornos Unix-like (macOS/Linux).
- Yarn y Pnpm tienen comportamientos similares en macOS y Linux, pero Yarn es más rápido en Windows (PowerShell/WSL).
- Windows nativo presenta overhead significativo para todos los gestores.
