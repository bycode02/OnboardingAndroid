# Flujo de Trabajo Git

Este documento describe el flujo de trabajo Git, las convenciones y las políticas que seguimos en el proyecto IPChile Multiplatform.

## Estructura de Ramas

Utilizamos un modelo de ramificación donde `main` es la rama principal del desarrollo. Las ramas feature se crean a partir de `main` y se fusionan de vuelta a `main` cuando están listas.

### Tipos de Ramas

| Tipo | Prefijo | Descripción | Ejemplo |
|------|---------|-------------|---------|
| Feature | `feat/` | Nuevas características o funcionalidades | `feat/notifications` |
| Fix | `fix/` | Corrección de errores | `fix/login-crash` |
| Documentation | `docs/` | Cambios en la documentación | `docs/api-reference` |
| Style | `style/` | Cambios que no afectan el significado del código (formato, espacios, etc.) | `style/lint-fixes` |
| Refactor | `refactor/` | Mejoras en el código sin cambios funcionales | `refactor/authentication-flow` |
| Test | `test/` | Añadiendo o corrigiendo pruebas | `test/login-integration` |
| Chore | `chore/` | Cambios en el proceso de build o herramientas | `chore/gradle-update` |
| Hotfix | `hotfix/` | Correcciones urgentes para producción | `hotfix/security-vulnerability` |
| Release | `release/` | Preparación para una nueva versión | `release/1.0.0` |

### Flujo de Trabajo

1. **Desarrollo de Nuevas Características**
   - Crear rama desde `main`: `git checkout -b feat/nombre-caracteristica main`
   - Desarrollar la característica
   - Enviar a remoto: `git push -u origin feat/nombre-caracteristica`
   - Crear Pull Request hacia `main`
   - Después de revisión y aprobación, fusionar a `main`

2. **Preparación para Release**
   - Crear rama `release/version` desde `main`
   - Realizar pruebas y ajustes finales
   - Después de certificación por QA, fusionar a `main` y etiquetar

3. **Correcciones Urgentes**
   - Crear rama `hotfix/descripcion` desde `main`
   - Implementar corrección
   - Fusionar a `main` y a la rama de release actual si existe

## Convenciones de Mensajes de Commit - ESTANDARIZADAS

Utilizamos [Conventional Commits](https://www.conventionalcommits.org/) adaptado para el proyecto IPChile Multiplatform con **formato estandarizado**.

### **🎯 FORMATO OBLIGATORIO**

```
<tipo>(<scope>): <descripción breve en minúsculas>

<cuerpo detallado con bullets opcionales>

[referencia PR]
```

### **📋 REGLAS ESTRICTAS**

1. **Tipo**: Siempre en **minúsculas**
2. **Scope**: Para historias técnicas usar formato **`ht-XX`** (minúsculas + guión)
3. **Descripción**: Primera palabra en **minúsculas**, máximo 50 caracteres
4. **Cuerpo**: Bullets con detalles técnicos (opcional pero recomendado)
5. **PR**: Siempre incluir `(#número)` al final del título o cuerpo

### **🏗 SCOPES PARA HISTORIAS TÉCNICAS**

```bash
# Historias de Notificaciones
feat(ht-01): implement notification payload specification
feat(ht-09): developer panel with FCM topics
feat(ht-17): koin dependency injection implementation
feat(ht-18): user topic subscription storage

# Features generales
feat(auth): smart login implementation  
feat(fcm): firebase cloud messaging setup
feat(ui): topic subscription screen

# Correcciones
fix(ht-08): token renewal verification logic
fix(auth): resolve permission denied in firestore

# Documentación  
docs(ht-09): testing strategy for background reception
docs(setup): installation and configuration guide
```

### **✅ EJEMPLOS CORRECTOS**

```bash
# Historia Técnica con detalles
feat(ht-09): developer panel with FCM topics for background reception testing

- DeveloperPanelScreen centralizado con 9 herramientas categorizadas
- TopicSubscriptionScreen para gestión de topics FCM con 9 topics predefinidos  
- Sistema de navegación URI-based: ipchile://developer-panel
- Testing optimizado: 60% más rápido (2 min vs 5+ min)
- Material Design 3 con indicadores visuales de prioridad

(#13)

# Feature simple
feat(auth): implement firebase anonymous authentication

- AnonymousAuthService interface para gestión de autenticación anónima
- Resolución de PERMISSION_DENIED en Firestore mediante request.auth válido
- ensureAnonymousAuth() para crear sesiones anónimas automáticamente

(#9)

# Fix con scope específico
fix(ht-08): resolve token renewal verification on startup

- SecureTokenManagementUseCase simplificado para verificación pragmática
- Verificación automática al startup: comparación token actual vs almacenado
- Implementación basada en investigación: tokens FCM extremadamente estables

(#11)

# Documentación
docs(testing): fcm reception testing strategy with topics

- Estrategia optimizada usando Topics FCM vs tokens individuales
- Configuraciones recomendadas por topic para testing
- Escenarios de testing específicos para HT-09

# Refactor
refactor(di): migrate to koin dependency injection

- Arquitectura modular de DI: CoreModule, NetworkModule, BusinessModule
- Estrategia de Flow sharing: Repositories (single), Use Cases (factory)
- Integración exitosa con MainApplication.kt usando startKoin
```

### **❌ EJEMPLOS INCORRECTOS (NO USAR)**

```bash
# ❌ Capitalización incorrecta
Feat/ht 08 token renewal
Feature/ht 04 smart login implementation  
Feat, ht-00, Notif basic infrastructure

# ❌ Scope mal formateado
feat(HT-17-NOTIF): implementar Koin DI 4.0.4
feat: Implement Firebase Anonymous Authentication  # Sin scope

# ❌ Descripción mal formateada
feat(ht-09): Developer Panel With FCM Topics  # Capitalizada
feat(ht-08): TOKEN RENEWAL VERIFICATION LOGIC  # Todo mayúsculas

# ❌ Sin PR reference
feat(auth): implement smart login
```

### **🎯 SCOPE ESPECÍFICOS POR ÁREA**

| Área | Scope | Ejemplo |
|------|-------|---------|
| **Historias Técnicas** | `ht-XX` | `feat(ht-18): user topic subscription storage` |
| **Autenticación** | `auth` | `feat(auth): firebase authentication setup` |
| **Notificaciones** | `notif` | `feat(notif): fcm background reception` |
| **Navegación** | `nav` | `feat(nav): uri-based navigation system` |
| **UI/Pantallas** | `ui` | `feat(ui): developer panel screen` |
| **Configuración** | `config` | `feat(config): firebase kmp sdk integration` |
| **Testing** | `test` | `test(ht-09): topic subscription scenarios` |
| **Documentación** | `docs` | `docs(api): rest api documentation` |
| **Build/Deps** | `build` | `chore(build): update gradle dependencies` |
| **CI/CD** | `ci` | `feat(ci): automated testing pipeline` |

### **📱 TEMPLATE PARA COMMITS DE HT**

```bash
# Para historias técnicas nuevas:
feat(ht-XX): <descripción funcionalidad principal>

- <Componente/Screen principal implementado>
- <Integración con sistema existente>  
- <Funcionalidad clave 1>
- <Funcionalidad clave 2>
- <Testing/Validación realizada>

(#PR)

# Para correcciones de HT:
fix(ht-XX): <descripción del problema resuelto>

- <Causa raíz identificada>
- <Solución implementada>
- <Validación de la corrección>

(#PR)

# Para documentación de HT:
docs(ht-XX): <tipo de documentación>

- <Sección/tema principal documentado>
- <Ejemplos/casos de uso incluidos>
- <Diagramas/referencias técnicas>

(#PR)
```

### **🔧 CONFIGURACIÓN DE COMMIT TEMPLATE**

Para facilitar el formato correcto, puedes configurar un template:

```bash
# Crear template de commit
git config commit.template .gitmessage

# Contenido de .gitmessage:
# <tipo>(<scope>): <descripción breve>
# 
# - <detalle 1>
# - <detalle 2>
# - <detalle 3>
# 
# (#PR)
```

## Pull Requests

### Proceso

1. **Creación**: Crear PR desde la rama de característica hacia `main`
2. **Descripción**: Utilizar la plantilla proporcionada y completar toda la información
3. **Revisión**: Asignar al menos 1 revisor
4. **CI/CD**: Asegurarse de que todas las pruebas automatizadas pasen
5. **Aprobación**: Se requiere al menos 1 aprobación para fusionar
6. **Fusión**: Utilizar "Squash and merge" para mantener el historial limpio

### Plantilla de Pull Request

```markdown
## Descripción
[Descripción del cambio implementado]

## Problema Resuelto
[Referencia al issue o descripción del problema]

## Tipo de Cambio
- [ ] feat: Nueva característica
- [ ] fix: Corrección de error
- [ ] docs: Cambio en la documentación
- [ ] style: Cambios de formato
- [ ] refactor: Refactorización de código
- [ ] test: Añadir o actualizar pruebas
- [ ] chore: Cambios en build o herramientas
- [ ] other: [Especificar]

## Pruebas Realizadas
[Descripción de las pruebas realizadas]

## Capturas de Pantalla (si aplica)
[Capturas de pantalla]

## Lista de Verificación
- [ ] Mi código sigue las guías de estilo del proyecto
- [ ] He realizado una auto-revisión de mi código
- [ ] He comentado mi código, especialmente en áreas difíciles de entender
- [ ] He realizado los cambios correspondientes a la documentación
- [ ] Mis cambios no generan nuevas advertencias
- [ ] He añadido pruebas que demuestran que mi corrección es efectiva o que mi característica funciona
- [ ] Las pruebas unitarias nuevas y existentes pasan localmente con mis cambios
```

## Resolución de Conflictos

1. **Prevención**: Actualizar frecuentemente la rama con los cambios de `main` usando rebase
   ```
   git checkout feat/branch
   git fetch origin
   git rebase origin/main
   ```

2. **Resolución**:
   - Si ocurren conflictos durante el rebase, resolverlos en cada commit
   - Usar herramientas como `git mergetool` o el editor integrado
   - Después de resolver, continuar con `git rebase --continue`
   - En caso de problemas complejos, considerar `git merge` en lugar de `rebase`

## Etiquetas y Versiones

Utilizamos [Semantic Versioning](https://semver.org/) (SemVer) para el versionado. Para más detalles sobre nuestro enfoque de versionado, incluyendo versiones alpha, beta y release candidate, consultar el documento [Estrategia de Versionado](versioning.md).

### Formato Básico
`MAJOR.MINOR.PATCH`

- **MAJOR**: Cambios incompatibles con versiones anteriores
- **MINOR**: Funcionalidad nueva compatible con versiones anteriores
- **PATCH**: Correcciones de errores compatibles con versiones anteriores

### Proceso de Etiquetado
```
git tag -a v1.0.0 -m "Versión 1.0.0"
git push origin v1.0.0
```

## Consideraciones Adicionales

- **Commits Atómicos**: Cada commit debe representar un cambio lógico y coherente
- **Rebase Interactivo**: Utilizar `git rebase -i` para limpiar el historial antes de enviar un PR
- **No Commits Directos**: Evitar commits directos a `main`
- **Protección de Ramas**: La rama `main` está protegida y requiere PR y aprobaciones
- **Hooks de Git**: Utilizar pre-commit y pre-push hooks para verificar calidad del código 