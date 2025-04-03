# **Documentación del Debugging - Prueba Técnica**  

## **Bug 1: Problema de referencia en contexto**  

### **Descripción del problema**  
El sistema de autenticación permite iniciar sesión correctamente, pero al recargar la página, la sesión no se mantiene, a pesar de utilizar `localStorage`.  

### **Investigación y causa del problema**  
Después de revisar el código de `AuthContext`, encontre que en el `useEffect` encargado de obtener el usuario desde `localStorage`, el usuario sí se establecía correctamente, **pero la variable `isAuthenticated` se asignaba como `false`**, lo que impedía que el sistema reconociera al usuario como autenticado.  

### **Solución aplicada**  
corregi el valor de `isAuthenticated`, asegurando que si un usuario está presente en `localStorage`, este se establezca como `true`.  
Con este cambio, al recargar la página, el sistema reconoce correctamente la sesión y redirige a `/tasks`.  

---

## **Bug 2: Formulario de Login incompleto**  

### **Descripción del problema**  
El formulario de inicio de sesión permite enviar datos sin validación, lo que provoca que el usuario pueda intentar autenticarse con campos vacíos.  

### **Investigación y causa del problema**  
Al revisar el código, confirme que no existía ninguna validación en el formulario. Esto permitía el envío de datos incompletos.  

### **Solución aplicada**  
Se pueden implementar dos mecanismos de validación:  

1. **Validación en `handleSubmit`**  
   - Agregando una verificación para que, si algún campo está vacío (o solo contiene espacios en blanco, gracias a `.trim()`), se muestre un mensaje de error indicando que los campos son obligatorios.  `la que use`

2. **Deshabilitación del botón de envío**  
   - Se puede agregar la condición `disabled={!username.trim() || !password.trim()}` en la línea 84, impidiendo que el botón pueda ser presionado si los campos están vacíos o solo contienen espacios en blanco.  

---

## **Bug 3: Problema con el marcado de tareas**  

### **Descripción del problema**  
Al intentar marcar una tarea como completada, el cambio no se reflejaba correctamente en la interfaz.  

### **Investigación y causa del problema**  
Al revisar los componentes relacionados con las tareas (`TaskItem.tsx`), identifique dos problemas principales:  

1. **`onToggleCompleted` no se estaba destructurando en las `props` del componente `TaskItem`**, por lo que no existía en su contexto y generaba un error al intentar llamarlo.  
2. **La función `handleToggle` estaba vacía**, lo que impedía que el estado de la tarea cambiara al marcarla como completada.  

### **Solución aplicada**  
- Asegurando que `onToggleCompleted` se destructurara correctamente en `TaskItem`.  
- Implementado en `handleToggle`, pasándole el `id` de la tarea que se quiere marcar como completa a `onToggleCompleted`.  
- Se envolvió la función en un `try/catch` para manejar posibles errores.  
- Se corrigió el evento `onChange` del `input checkbox`, asegurando que ahora llame a `handleToggle` correctamente.  
