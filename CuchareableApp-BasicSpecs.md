## **Especificaciones Iniciales CuchareableApp:**

### **1. Contexto**
- App para encontrar mejores puntos de comida en Nuevo Chimbote
- Usuarios: toda persona que busca lugares para comer
- Problema: dificultad para descubrir buenos lugares cercanos
- Similar: Versión inicial de Zomato pero para Nuevo Chimbote.

### **2. Requisitos**
**Funcionales:**
- Ver mapa con puntos de comida cercanos
- Filtrar por tipo de comida, precio, rating
- Ver detalles del restaurante (fotos, horarios, menú, rating)
- Navegación al lugar seleccionado

**No funcionales:**
- Funcionar sin internet (datos cached: mapa previamente cargado y restaurantes favoritos guardados)
- Cargar mapa en <3 segundos
- Soportar Android 5+

### **3. Restricciones**
- Presupuesto: APIs gratuitas (Google Maps, Firebase)
- Tiempo: 1 día (ajustable dependiendo de la complejidad)
- Equipo: 1 desarrollador
- Datos: Cloud Firestore (BD remoto), Room (Cache local)
- Metodología: Desarrollo asistido con IA agéntica.
- Arquitectura: MVVM (uso de LiveData)
- Seguridad: HTTPS APIs, no datos sensibles
- Escalabilidad: Paginación, lazy loading
- Documentación: README (presentación general del proyecto), código bien comentado, directorio docs/ para documentación detallada
- Modelo de ingresos: Ninguno. 100% open-source.

## **Reglas de Negocio - CuchareableApp**

### **1. Gestión de Usuarios**
- Un usuario puede tener máximo 100 restaurantes en favoritos
- Usuarios nuevos (< 24 horas) tienen sus reviews en moderación manual
- Máximo 3 reviews por usuario por día
- Para dejar review, el usuario debe haber estado a menos de 500m del restaurante

### **2. Reviews y Ratings**
- Rating obligatorio de 1-5 estrellas
- Comentario opcional (mínimo 10 caracteres, máximo 500)
- No se puede editar review después de 24 horas
- No se puede hacer review del mismo restaurante más de una vez por mes
- Reviews van a moderación automática si contienen palabras prohibidas

### **3. Restaurantes**
- Solo se muestran restaurantes dentro de 10km del usuario
- Restaurante debe tener mínimo 3 reviews para mostrar rating promedio
- Restaurantes sin actividad por 6 meses se marcan como "Info desactualizada"
- Máximo 50 fotos por restaurante

### **4. Búsquedas y Filtros**
- Búsquedas se limitan a un radio de 20km
- Máximo 50 resultados por búsqueda
- Historial de búsquedas se guarda por 30 días
- Filtros: categoría, precio ($-$$$), rating (mín 3.0), distancia

### **5. Funcionalidad Offline**
- Cache automático de restaurantes visitados por 7 días
- Favoritos se mantienen en cache permanentemente
- Máximo 200 restaurantes en cache local
- Fotos se cachean solo para favoritos (máximo 3 por restaurante)

### **6. Límites del Sistema**
- Máximo 1000 requests por usuario por hora (anti-spam)
- Sesión expira después de 30 días sin actividad
- Cache de mapas se limita a 50MB por dispositivo
- Sync automático cada vez que hay conexión a internet

### **7. Moderación de Contenido**
- Reviews con toxicidad >80% van a moderación manual
- 5 reportes = review oculta automáticamente
- Usuarios con 3 reviews rechazadas = suspensión temporal (7 días)
- Spam detectado = bloqueo inmediato de 24 horas

### **8. Datos y Privacidad**
- Ubicación se guarda por máximo 24 horas
- Reviews son públicas y no se pueden borrar (solo ocultar)
- Data personal se elimina después de 2 años sin actividad
- Usuario puede exportar sus datos en cualquier momento

### **9. Validaciones Básicas**
- Nombre de restaurante: 3-50 caracteres
- Horarios deben estar en formato HH:MM
- Teléfono debe tener formato peruano válido
- Fotos máximo 5MB cada una
- Categorías limitadas a lista predefinida (peruana, italiana, china, etc.)

### **10. Reglas de Negocio Especiales**
- Si restaurante cierra permanentemente → se marca como cerrado y no visible, no se elimina de la bd
- Restaurantes populares (>100 reviews) tienen prioridad en búsquedas
- Distancia se calcula en línea recta, no por ruta de manejo
