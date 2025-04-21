# Fase 3: Automatización (CI/CD - El Corazón de DevOps)

## 3.1. Conceptos Clave

### Integración Continua (CI - Continuous Integration)

* **Definición:** Práctica de desarrollo donde los desarrolladores integran (merge) cambios de código en un repositorio central *frecuentemente* (idealmente, varias veces al día).
* **Proceso Automatizado:** Cada integración dispara automáticamente un *build* y la ejecución de *tests automatizados* (unitarios, integración básica).
* **Objetivo Principal:** Detectar errores de integración, bugs y conflictos de código lo *antes posible* en el ciclo de desarrollo.
* **Beneficios:**
    * Feedback rápido a los desarrolladores.
    * Mejora la calidad del código.
    * Reduce el "infierno de la integración" (integration hell).
    * Acelera la colaboración.
* **Salida Típica:** Un artefacto construible y probado (ej. un JAR, un binario, una imagen Docker).

* **[Mis Notas sobre CI:]**
    * _Ejemplo práctico:_ Al hacer `git push` a una rama de feature, se dispara un pipeline que compila el código y corre tests unitarios. Si falla, el desarrollador recibe una notificación inmediata.
    * _Consideraciones:_ Necesita una buena suite de tests automatizados y un repositorio centralizado (Git).

### Entrega Continua (CDelivery - Continuous Delivery)

* **Definición:** Extensión de la Integración Continua. Asegura que *cada cambio* que pasa todas las fases automatizadas (build, tests adicionales como integración, UI tests) produce un artefacto que está *listo para ser desplegado* en producción.
* **Proceso Automatizado:** Incluye CI, y además automatiza la preparación del *release* (empaquetado, configuración para diferentes entornos).
* **Despliegue a Producción:** La decisión final de desplegar a producción suele requerir una *aprobación manual* (ej. clic de un botón por parte del Product Owner o equipo de QA).
* **Objetivo Principal:** Hacer que los despliegues sean eventos *rutinarios y de bajo riesgo*, que pueden ocurrir en cualquier momento bajo demanda.
* **Beneficios:**
    * Reduce el riesgo de los releases.
    * Permite entregar valor al usuario más rápidamente (aunque no necesariamente en cada commit).
    * Feedback más rápido del entorno de staging/UAT.

* **[Mis Notas sobre CDelivery:]**
    * _Ejemplo práctico:_ Un cambio pasa CI, luego se despliega automáticamente a un entorno de Staging. Tras pruebas manuales o automáticas en Staging, un responsable aprueba el despliegue a Producción, que se realiza con un script automatizado.
    * _Diferencia clave con CDeployment:_ El paso final a producción es manual/bajo demanda.

### Despliegue Continuo (CDeployment - Continuous Deployment)

* **Definición:** Extensión de la Entrega Continua. Cada cambio que pasa *todas* las etapas del pipeline (CI, tests automatizados, etc.) se despliega *automáticamente* a producción *sin intervención humana*.
* **Proceso Automatizado:** Todo el camino desde el commit hasta producción está automatizado.
* **Requiere Confianza Alta:** Necesita una cobertura de tests muy robusta y mecanismos de monitoreo y rollback eficientes.
* **Objetivo Principal:** Minimizar el tiempo desde que se escribe el código hasta que está en producción aportando valor al usuario. Maximizar el ciclo de feedback.
* **Beneficios:**
    * Entrega de valor extremadamente rápida.
    * Ciclos de feedback muy cortos con usuarios reales.
    * Reduce la presión sobre el equipo para hacer grandes releases.

* **[Mis Notas sobre CDeployment:]**
    * _Ejemplo práctico:_ Un cambio pasa CI, pasa tests de integración, performance y seguridad en Staging, y si todo está OK, se despliega directamente a producción sin que nadie intervenga. Estrategias como Canary Releases o Blue/Green deployments son comunes aquí.
    * _Consideraciones:_ No es adecuado para todos los contextos (ej. sistemas críticos que requieren validaciones regulatorias manuales). Requiere una cultura de calidad muy alta.

---

## 3.2. Herramientas CI/CD Populares (2025)

### GitHub Actions

* **Descripción:** Plataforma de CI/CD integrada directamente en GitHub. Usa archivos YAML para definir workflows.
* **Características Clave (2025):**
    * Gran integración con el ecosistema GitHub (repos, issues, pull requests, registry, security scanning - GHAS).
    * Amplio Marketplace de acciones reutilizables (oficiales y de la comunidad).
    * Runners hospedados por GitHub (Linux, Windows, macOS) o auto-hospedados (self-hosted runners).
    * Modelo de precios basado en minutos de ejecución (con un generoso nivel gratuito para repos públicos y privados).
    * Soporte para matrix builds, variables de entorno, secretos encriptados.
    * Mejoras continuas en seguridad (ej. OIDC para autenticación sin secretos).
* **Pros:** Excelente integración si ya usas GitHub, fácil de empezar, comunidad activa.
* **Contras:** Fuertemente ligado a GitHub, puede ser costoso a gran escala si se usan muchos minutos de runners hospedados.

* **[Mis Notas sobre GitHub Actions:]**
    * _Ideal para:_ Proyectos hospedados en GitHub, equipos que buscan simplicidad y una solución integrada.
    * _Investigar:_ Reusable workflows, actions de seguridad (Dependabot, CodeQL), OIDC.

### GitLab CI/CD

* **Descripción:** Solución de CI/CD integrada en la plataforma GitLab. También usa archivos YAML (`.gitlab-ci.yml`).
* **Características Clave (2025):**
    * Parte de la plataforma "todo en uno" de GitLab (repo, registry, security, monitoring...).
    * Potente sistema de runners (compartidos o auto-hospedados), con buen soporte para Docker y Kubernetes.
    * Auto DevOps: Pipelines preconfigurados que detectan el lenguaje y automatizan build, test, deploy.
    * Integración nativa con Kubernetes.
    * Funcionalidades de seguridad integradas (SAST, DAST, Dependency Scanning, etc.) en niveles de pago.
    * Visualización de pipelines, gestión de entornos.
* **Pros:** Solución muy completa e integrada, potente, buena para auto-hospedar toda la plataforma.
* **Contras:** La interfaz puede ser compleja, las mejores características de seguridad están en tiers de pago.

* **[Mis Notas sobre GitLab CI/CD:]**
    * _Ideal para:_ Equipos que usan GitLab como plataforma central, organizaciones que buscan una solución integral (potencialmente auto-hospedada).
    * _Investigar:_ Auto DevOps, integración con Kubernetes (GitLab Agent), Parent-child pipelines.

### CircleCI

* **Descripción:** Plataforma CI/CD principalmente basada en la nube, conocida por su velocidad, flexibilidad y enfoque en la experiencia del desarrollador. Utiliza archivos YAML (`.circleci/config.yml`) para la configuración.
* **Características Clave (2025):**
    * **Rendimiento:** A menudo destacada por su rapidez en la ejecución de builds y tests, con optimizaciones como paralelismo, división de tests y caché avanzada.
    * **Orbs:** Paquetes reutilizables de configuración CircleCI que simplifican tareas complejas e integraciones (similar a las Actions de GitHub). Existe un registro público de Orbs.
    * **Amplio Soporte de Plataformas:** Ejecución de jobs en Docker, Linux (varias imágenes), macOS, Windows y arquitecturas ARM (runners en la nube o auto-hospedados).
    * **Workflows:** Permiten orquestar pipelines complejos con dependencias, ejecuciones secuenciales o paralelas.
    * **Debugging:** Facilita la depuración de jobs fallidos permitiendo el acceso vía SSH directamente al entorno de ejecución.
    * **Insights:** Panel de control para analizar el rendimiento del pipeline, identificar cuellos de botella y optimizar tiempos.
    * **Runners Auto-hospedados:** Opción para ejecutar jobs en tu propia infraestructura.
    * **Seguridad:** Gestión de secretos, soporte para OIDC, integraciones para escaneo de vulnerabilidades (a menudo a través de Orbs).
* **Pros:** Rendimiento y velocidad, configuración potente y reutilizable (Orbs), buenas capacidades de debugging (SSH), soporte multi-plataforma y multi-arquitectura, buena documentación.
* **Contras:** Puede volverse costoso a escala (precios basados en uso, concurrencia y clases de recursos), la configuración YAML puede ser compleja para pipelines muy elaborados, menos integrado con la gestión de código fuente comparado con GitHub Actions o GitLab CI si no usas esas plataformas.
* **[Mis Notas sobre CircleCI:]**
    * _Ideal para:_ Equipos que priorizan la velocidad de build/test, necesitan flexibilidad en sistemas operativos/arquitecturas, valoran las opciones de debugging y buscan optimizar el rendimiento del pipeline. Popular en startups y empresas tecnológicas.
    * _Investigar:_ CircleCI Orbs (cómo usarlos y crearlos), estrategias avanzadas de caché (`save_cache`, `restore_cache`), configuración de paralelismo, el dashboard de Insights, configuración de runners auto-hospedados (self-hosted runners).

### Jenkins

* **Descripción:** Servidor de automatización open-source, el "clásico" de CI/CD. Muy maduro y extensible.
* **Características Clave (2025):**
    * Extremadamente flexible y personalizable a través de miles de plugins.
    * Soporte para `Pipelines as Code` usando `Jenkinsfile` (basado en Groovy).
    * Puede orquestar tareas complejas más allá de CI/CD.
    * Puede ejecutarse on-premise o en la nube.
    * Gran comunidad y base instalada.
* **Pros:** Muy potente y versátil, agnóstico a plataformas, gratuito y open source.
* **Contras:** Requiere más configuración y mantenimiento, la gestión de plugins puede ser compleja ("plugin hell"), la interfaz puede parecer anticuada (aunque Blue Ocean mejora esto), puede consumir bastantes recursos.

* **[Mis Notas sobre Jenkins:]**
    * _Ideal para:_ Organizaciones con necesidades complejas y diversas, entornos que requieren control total sobre la herramienta, equipos con experiencia en su gestión.
    * _Investigar:_ Jenkinsfile (declarative vs scripted), Blue Ocean UI, Configuration as Code (JCasC), plugins para Kubernetes.

### Argo CD (GitOps)

* **Descripción:** Herramienta declarativa de Entrega Continua basada en GitOps, específicamente para Kubernetes. Es un proyecto incubado por la CNCF.
* **Funcionamiento:** Monitorea un repositorio Git que contiene las definiciones deseadas del estado de las aplicaciones en K8s (manifests YAML, Helm charts, Kustomize). Compara este estado deseado con el estado real en el clúster y aplica los cambios necesarios para sincronizarlos.
* **Características Clave:**
    * Interfaz web para visualizar el estado de las aplicaciones y la sincronización.
    * Soporte multi-cluster y multi-tenant.
    * Detección de drift (diferencias entre Git y el clúster).
    * Políticas de sincronización (automática o manual), rollbacks.
    * Integración con SSO y RBAC.
* **Pros:** Implementa GitOps de forma robusta, mejora la auditoría y el control de versiones de los despliegues, buena UI.
* **Contras:** Enfocado exclusivamente en Kubernetes, curva de aprendizaje inicial para GitOps.

* **[Mis Notas sobre Argo CD:]**
    * _Ideal para:_ Equipos que usan Kubernetes extensivamente y quieren adoptar GitOps para gestionar despliegues.
    * _Investigar:_ Argo Rollouts (estrategias avanzadas de despliegue), ApplicationSets, integración con otras herramientas Argo (Workflows, Events).

### Flux (GitOps)

* **Descripción:** Herramienta declarativa de Entrega Continua basada en GitOps para Kubernetes (Proyecto Graduado de la CNCF). Es una alternativa popular a Argo CD.
* **Funcionamiento:** Utiliza un conjunto de controladores (operadores de Kubernetes) para monitorear fuentes (Git, Helm Repos, Buckets S3) y reconciliar el estado del clúster con las definiciones encontradas.
* **Características Clave:**
    * Enfoque modular basado en controladores (Source, Kustomize, Helm, Notification, Image Automation).
    * Se integra bien con herramientas nativas de K8s (Kustomize, Helm).
    * Extensible y sigue de cerca las APIs de Kubernetes.
    * Puede actualizar automáticamente imágenes de contenedores basado en políticas (Image Update Automation).
* **Pros:** Modular, extensible, alineado con las prácticas de Kubernetes, proyecto CNCF graduado.
* **Contras:** La interfaz gráfica no es su foco principal (aunque existen UIs de terceros), la configuración puede ser más compleja inicialmente debido a su modularidad.

* **[Mis Notas sobre Flux:]**
    * _Ideal para:_ Equipos que buscan una solución GitOps modular y extensible, muy integrada con el ecosistema Kubernetes/CNCF.
    * _Investigar:_ Kustomize Controller, Helm Controller, Image Reflection & Automation Controllers. Comparativa Flux vs Argo CD (ej. enfoque UI vs modularidad).

---

## 3.3. Prácticas Esenciales

### Pipelines as Code

* **Definición:** Práctica de definir y gestionar la pipeline de CI/CD (stages, steps, environments, parameters) en un archivo de código (ej. YAML, Groovy) que se almacena en el control de versiones junto con el código de la aplicación.
* **Ejemplos:** `Jenkinsfile` (Jenkins), `.gitlab-ci.yml` (GitLab CI), workflow `.yml` (GitHub Actions).
* **Beneficios:**
    * **Versionado:** Los cambios en el pipeline se rastrean y pueden revertirse.
    * **Colaboración:** El pipeline es visible y modificable por el equipo (vía Pull Requests).
    * **Revisión de Código:** Se pueden revisar los cambios del pipeline.
    * **Reproducibilidad:** Fácil de replicar el pipeline en diferentes entornos o instancias.
    * **Auditoría:** Historial claro de quién cambió qué y cuándo en el pipeline.
    * **Disaster Recovery:** La definición del pipeline está respaldada en Git.

* **[Mis Notas sobre Pipelines as Code:]**
    * _Clave:_ Tratar la configuración de tu pipeline como tratas el código de tu aplicación.
    * _Consideraciones:_ Elegir la sintaxis adecuada (declarativa suele ser más simple, script permite más lógica compleja).

### Testing Automatizado en el Pipeline

* **Definición:** Integrar la ejecución de diferentes tipos de tests automatizados como etapas dentro del pipeline de CI/CD.
* **Tipos Comunes:**
    * **Unit Tests:** Prueban unidades aisladas de código (funciones, clases). Rápidos, deben ejecutarse en cada commit (CI).
    * **Integration Tests:** Prueban la interacción entre varios componentes o servicios. Más lentos, pueden ejecutarse después de CI o en entornos de staging.
    * **Smoke Tests:** Un subconjunto pequeño y rápido de tests end-to-end que verifican la funcionalidad *crítica* de la aplicación después de un despliegue para asegurar que no está "rota" fundamentalmente.
    * _(Otros importantes: End-to-End tests, Performance tests, Security tests - SAST/DAST/SCA)._
* **Objetivo:** Obtener feedback rápido sobre la calidad y funcionalidad en diferentes niveles a lo largo del pipeline. Asegurar que los cambios no introducen regresiones.
* **Beneficios:** Detección temprana de errores, mayor confianza en los releases, reducción de pruebas manuales repetitivas.

* **[Mis Notas sobre Testing Automatizado:]**
    * _Pirámide de Tests:_ Priorizar más tests unitarios (rápidos y baratos), menos tests de integración, y aún menos tests E2E (lentos y frágiles).
    * _Estrategia:_ Definir qué tests se ejecutan en qué etapa del pipeline (ej. unit en CI, integration/smoke en CD).

### Gestión de Artefactos (Artifact Management)

* **Definición:** Proceso y herramientas para almacenar, versionar, gestionar y distribuir los *artefactos* (outputs binarios) generados por el proceso de CI/CD.
* **Tipos de Artefactos:** Librerías (`.jar`, `.dll`), binarios ejecutables, paquetes (`npm`, `PyPI`, `RPM`), imágenes de contenedor (Docker/OCI), plantillas de infraestructura (Helm charts).
* **Repositorios de Artefactos:** Herramientas centralizadas que actúan como fuente única de verdad para los artefactos.
    * **Nexus Repository Manager (Sonatype):** Muy popular, soporta múltiples formatos, gestión de proxies, hosting, grupos. Open source (OSS) y versión Pro.
    * **JFrog Artifactory:** Otro líder del mercado, amplio soporte de formatos, alta disponibilidad, integración con seguridad (JFrog Xray). Versión gratuita y Enterprise.
    * **Registros de Contenedores:** Repositorios especializados para imágenes Docker/OCI.
        * `Docker Hub`: Registro público y privado por defecto.
        * `AWS ECR` (Elastic Container Registry)
        * `Google GCR` (Google Container Registry) / `Artifact Registry`
        * `Azure ACR` (Azure Container Registry)
        * `GitLab Container Registry`: Integrado en GitLab.
        * `Harbor`: Solución open source auto-hospedada.
* **Beneficios:** Builds fiables y reproducibles, gestión de dependencias, auditoría, seguridad (escaneo de artefactos), mejora del rendimiento (cacheo de artefactos).

* **[Mis Notas sobre Gestión de Artefactos:]**
    * _Importancia:_ Evita reconstruir dependencias constantemente y asegura que se usa la versión correcta del artefacto en cada despliegue.
    * _Práctica común:_ El pipeline de CI construye el artefacto y lo publica en el repositorio. El pipeline de CD lo descarga del repositorio para desplegarlo.
