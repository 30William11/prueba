---
title: "🧾 Implementación de Previsualización y Exportación PDF"
created: 2025-07-02
tags: [datatable, pdf, bootstrap, iframe, modal]
---

# 🧾 Implementación de Vista Previa y Exportación PDF

## 📌 Objetivo
Permitir al usuario visualizar una tabla en un **modal tipo vista previa PDF**, y generar un **PDF personalizado con encabezado, pie de página y estilo**.

---
![alt text](w1.jpeg)
## 🧱 Estructura Implementada

### 🔹 1. Botón para abrir vista previa

```html
<button class="btn btn-outline-secondary btn-sm" data-bs-toggle="modal" data-bs-target="#previewModal">
  <i class="fas fa-eye"></i> Previsualizar
</button>
```

---

### 🔹 2. Modal Bootstrap con `iframe` (archivo: `preview_modal.php`)

```html
<div class="modal fade" id="previewModal" tabindex="-1">
  <div class="modal-dialog modal-xl">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title">Vista Previa</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal"></button>
      </div>
      <div class="modal-body" style="height: 80vh;">
        <iframe src="/exportar/vista_previa_print.php" width="100%" height="100%" frameborder="0"></iframe>
      </div>
    </div>
  </div>
</div>
```

---

### 🔹 3. Script para mostrar modal (añadir al final del HTML donde se usa el botón)

```html
<script>
function mostrarVistaPrevia() {
  const modal = new bootstrap.Modal(document.getElementById('previewModal'));
  modal.show();
}
</script>
```

---

### 🔹 4. Estilos PDF con DataTables + Bootstrap 4

```js
$('#tablaPrincipal').DataTable({
  dom: 'Bfrtip',
  buttons: [
    {
      extend: 'pdfHtml5',
      text: '<i class="fas fa-file-pdf"></i> Exportar PDF',
      className: 'btn btn-danger btn-sm',
      title: 'Mi Título Personalizado',
      filename: 'reporte_personalizado',
      orientation: 'portrait',
      pageSize: 'A4',
      customize: function (doc) {
        doc.content.splice(0, 0, {
          text: 'UNIVERSIDAD CATÓLICA SEDES SAPIENTIAE\nSISTEMA DE HORARIOS',
          fontSize: 14,
          alignment: 'center',
          margin: [0, 0, 0, 20]
        });
        doc.footer = function (page, pages) {
          return {
            columns: [
              { text: 'Reporte generado automáticamente', alignment: 'left', margin: [40, 0] },
              { text: 'Página ' + page.toString() + ' de ' + pages.toString(), alignment: 'right', margin: [0, 0, 40] }
            ],
            fontSize: 9
          };
        };
        doc.defaultStyle.fontSize = 10;
        doc.styles.tableHeader.fillColor = '#0d6efd';
        doc.styles.tableHeader.color = 'white';
      }
    }
  ],
  language: {
    url: '//cdn.datatables.net/plug-ins/1.13.4/i18n/es-ES.json'
  }
});
```

---

## 🐛 Problemas detectados y corregidos

| Problema | Solución |
|---------|----------|
| ❌ Error 404: vista_previa_print.php no encontrado | ✅ Se cambió ruta de `/exporter/` a `/exportar/` |
| ❌ Modal no se abría | ✅ Se corrigió el ID y uso del `iframe` |
| ❌ Estilos no coherentes con los botones | ✅ Se reusaron los estilos de Bootstrap y botones originales |
| ❌ Exportación sin encabezado ni pie de página | ✅ Se agregó encabezado con nombre de universidad y pie con numeración |

---

## ✅ Resultado Esperado

### 🎯 Vista previa PDF

- Modal de tamaño completo (`modal-xl`)
- Contenido en `iframe` cargando `vista_previa_print.php`
- PDF con encabezado, pie de página, y tabla estilizada

---

## 🧩 Pendiente opcional

- [ ] Insertar **logo** institucional como imagen en PDF.
- [ ] Agregar **fecha y nombre de usuario** al pie del PDF.

---
