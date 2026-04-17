# Pre-Buy Checklist · Discovery 4 SDV6

App web estilo checklist aeronáutico para inspección de compra de vehículo.
Persistencia local en el navegador (localStorage). Mobile-first.

## Características

- Checklist con 9 secciones · 50+ puntos de inspección
- Badges de severidad: **crít** / **imp** / **nota**
- Marcar cada ítem como OK o FALLO
- Notas por cada ítem
- Calculador automático de **descuento recomendado** y **precio objetivo**
- Veredicto final: GO / REVISAR / NO-GO
- Export a texto plano + compartir por WhatsApp
- Funciona offline una vez cargado

## Deployment en Portainer

### Opción A · Stack desde ficheros (más simple)

1. Subir los 3 ficheros al servidor vía SFTP/SCP a un path tipo
   `/opt/stacks/disco-checklist/`:
   - `index.html`
   - `nginx.conf`
   - `docker-compose.yml`

2. En Portainer → **Stacks** → **Add stack**
3. Nombre: `disco-checklist`
4. Build method: **Web editor**
5. Pegar el contenido de `docker-compose.yml`
6. **Deploy the stack**

Accesible en `http://IP_SERVIDOR:8090`

### Opción B · Imagen pre-built (más portable)

Construir localmente y subir al registro:

```bash
cd discovery-checklist
docker build -t bestfixing/disco-checklist:1.0 .
docker tag bestfixing/disco-checklist:1.0 registry.bestfixinglab.cloud/disco-checklist:1.0
docker push registry.bestfixinglab.cloud/disco-checklist:1.0
```

Y en Portainer usar esta imagen directamente sin volumes.

### Opción C · Exponer en bestfixinglab.cloud vía Cloudflare Tunnel

Añadir entrada al Tunnel existente:

```yaml
- hostname: disco.bestfixinglab.cloud
  service: http://disco-checklist:80
```

Y actualizar la ruta en `docker-compose.yml` para que use la network
compartida con el túnel (`bestfixing` o la que tengas).

## Actualización del checklist

Los datos están en el array `CHECKLIST` al inicio del `<script>` en
`index.html`. Formato:

```javascript
{
  t: "Texto del ítem",
  h: "Hint/explicación (acepta <code>)",
  sev: "crit" | "warn" | "info",
  cost: 1500  // coste si falla, se suma al descuento
}
```

Modificar, guardar, reiniciar el contenedor (`docker restart disco-checklist`).

## Limpiar datos guardados

En el navegador, botón **Reset** al final del checklist.
O manualmente: DevTools → Application → Local Storage → Clear.

## Uso en el concesionario

1. Abrir la web en el móvil antes de llegar
2. Rellenar matrícula / kms / precio pedido
3. Ir marcando conforme inspeccionas
4. Usar el campo notas de cada ítem para fotos/observaciones
5. Al terminar → botón **Exportar informe** para guardar
6. Botón **WhatsApp** para compartir veredicto con asesor/socio

---

BESTFIXING LAB · v1.0 · 2026
