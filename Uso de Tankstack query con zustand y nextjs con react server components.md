```markdown
# Integración Avanzada de React Server Components con TanStack Query y Zustand en Next.js 14+

## Estructura del Proyecto
```bash
src/
├── app/
│   ├── dashboard/
│   │   ├── page.tsx          # Server Component (RSC)
│   │   └── DashboardContent.tsx # Client Component
│   ├── stores/
│   │   └── themeStore.ts    # Zustand Store
│   └── providers.tsx        # Context Providers
```

---

## Código Completo (TypeScript)

### Providers (Client Boundary)
```tsx
// app/providers.tsx
'use client';

import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
import { ReactNode, useState } from 'react';

export default function Providers({ children }: { children: ReactNode }) {
  const [queryClient] = useState(() => new QueryClient());

  return (
    <QueryClientProvider client={queryClient}>
      {children}
    </QueryClientProvider>
  );
}
```

### Zustand Store (Client-side)
```tsx
// app/stores/themeStore.ts
'use client';

import { create } from 'zustand';

type ThemeState = {
  darkMode: boolean;
  toggle: () => void;
};

export const useThemeStore = create<ThemeState>((set) => ({
  darkMode: false,
  toggle: () => set((state) => ({ darkMode: !state.darkMode })),
}));
```

### Server Component (RSC) - Dashboard Page
```tsx
// app/dashboard/page.tsx
import { QueryClient, dehydrate } from '@tanstack/react-query';
import { getProducts } from '@/api/products';
import DashboardContent from './DashboardContent';

export default async function DashboardPage() {
  const queryClient = new QueryClient();

  // Prefetch en el servidor
  await queryClient.prefetchQuery({
    queryKey: ['products'],
    queryFn: getProducts,
  });

  return (
    <div className="container">
      <h1>Dashboard Comercial</h1>
      {/* Pasar estado deshidratado al cliente */}
      <DashboardContent dehydratedState={dehydrate(queryClient)} />
    </div>
  );
}
```

### Client Component (Hydratación y Lógica)
```tsx
// app/dashboard/DashboardContent.tsx
'use client';

import { HydrationBoundary, useQuery } from '@tanstack/react-query';
import { useThemeStore } from '@/stores/themeStore';
import { getProducts } from '@/api/products';
import dynamic from 'next/dynamic';

// Carga dinámica para evitar SSR del toggle (opcional)
const ThemeToggle = dynamic(() => import('@/components/ThemeToggle'), {
  ssr: false
});

export default function DashboardContent({ dehydratedState }: { dehydratedState: any }) {
  // Usar datos hidratados del servidor
  const { data: products } = useQuery({
    queryKey: ['products'],
    queryFn: getProducts,
  });

  // Zustand: Estado del tema
  const darkMode = useThemeStore((state) => state.darkMode);

  return (
    <HydrationBoundary state={dehydratedState}>
      <div className={darkMode ? 'dark' : 'light'}>
        <ThemeToggle />

        {/* Lista de productos */}
        <div className="grid">
          {products?.map((product) => (
            <div key={product.id} className="card">
              <h3>{product.name}</h3>
              <p>${product.price}</p>
            </div>
          ))}
        </div>
      </div>
    </HydrationBoundary>
  );
}
```

---

## Ventajas Clave

| Tecnología            | Beneficios                                                                 |
|-----------------------|----------------------------------------------------------------------------|
| **React Server Components** | - Carga inicial más rápida (HTML desde servidor)                         |
| **TanStack Query**    | - Caching inteligente<br>- Revalidación automática<br>- Estado compartido entre client/server |
| **Zustand**           | - Estado global ligero<br>- Actualizaciones precisas con selectores       |

---

## Desventajas de No Usarlos

| Escenario             | Problemas                                                                  |
|-----------------------|----------------------------------------------------------------------------|
| **Sin RSC**           | - Tiempo de carga más lento<br>- Mayor JavaScript en cliente              |
| **Sin TanStack Query**| - Duplicación de peticiones<br>- Manejo manual de caché<br>- Estado inconsistente |
| **Sin Zustand**       | - Prop drilling<br>- Complejidad con Context API<br>- Re-renders innecesarios |

---

## Flujo de Datos Avanzado

1. **Server Component (RSC)**:
   - Prefetch de datos en el servidor
   - Genera HTML estático con datos iniciales
   - Envía estado deshidratado al cliente

2. **Client Component**:
   - Hidrata el estado de TanStack Query
   - Usa Zustand para temas/preferencias
   - Actualizaciones en tiempo real via WebSocket (ejemplo avanzado)

```tsx
// Ejemplo: Actualizaciones en tiempo real
const { data: inventory } = useQuery({
  queryKey: ['inventory'],
  queryFn: getLiveInventory,
  refetchInterval: 5000, // Revalida cada 5 segundos
});
```

---

## Consideraciones de Rendimiento

**Optimización con Selectores**:
```tsx
// Zustand: Evita re-renders innecesarios
const toggleTheme = useThemeStore((state) => state.toggle);
```

**Soporte para SSR/SSG**:
```tsx
// TanStack Query + Next.js
export async function generateStaticParams() {
  const products = await getProducts();
  return products.map((product) => ({ id: product.id }));
}
```

---

## Errores Comunes

**Problema**: Hidratación inconsistente entre server/client
**Solución**:
```tsx
// Usar skeletons o loading states
const { data, isPending } = useQuery({ queryKey: ['data'], queryFn: fetchData });

if (isPending) return <Skeleton />;
```

**Problema**: Zustand en Server Components
**Solución**:
```tsx
// Usar dynamic imports con SSR deshabilitado
const ClientSideComponent = dynamic(() => import('./ClientComponent'), {
  ssr: false,
});
```

---

## Conclusión Técnica

**Patrón Recomendado**:
```tsx
// Server Component (RSC)
async function Page() {
  // 1. Fetch de datos críticos en servidor
  // 2. Pasar a cliente via dehydrate
}

// Client Component
'use client';
function InteractivePart() {
  // 1. Hidratar datos de TanStack
  // 2. Zustand para estado UI
  // 3. Actualizaciones dinámicas
}
```

**Escalabilidad**: Soporta ~50k peticiones/día con optimizaciones.
**Seguridad**: Usar `server-only` para lógica sensible:
```bash
npm install server-only
```
```tsx
import 'server-only';
export async function getSensitiveData() {
  // Lógica solo para servidor
}
```
