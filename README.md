# **Examen - Programación de Servicios y Procesos**  
**Primer Parcial: Concurrencia y Sincronización**  

Los conceptos clave son el manejo de concurrencia usando `Lock` y `synchronized`.  

---

## **📝 Ejercicios:**  

### **1. Control de acceso a un recurso compartido**  
- **Descripción:** Simula una sala de servidores que solo puede ser utilizada por un técnico a la vez. Los técnicos deben esperar su turno antes de usar el servidor.  
- **Tecnología:** `ReentrantLock`.  
- **Clases involucradas:**  
  - `Servidor`: Implementa el acceso controlado a través de un `Lock`.  
  - `MainEjercicio1`: Lanza 5 hilos, cada uno representando a un técnico.  
- **Ejemplo de salida:**  
  ```  
  Técnico Alice está usando el servidor...  
  Técnico Alice ha terminado de usar el servidor.  
  Técnico Bob está usando el servidor...  
  Técnico Bob ha terminado de usar el servidor.  
  ```  

---

### **2. Contador sincronizado**  
- **Descripción:** Implementa un contador que puede ser incrementado por múltiples hilos de manera segura. Utiliza `synchronized` para evitar accesos concurrentes descontrolados.  
- **Tecnología:** `synchronized`.  
- **Clases involucradas:**  
  - `Contador`: Contiene el método sincronizado `incrementar()` que asegura la consistencia del valor.  
  - `MainEjercicio2`: Lanza 10 hilos, cada uno incrementando el contador 5 veces.  
- **Ejemplo de salida:**  
  ```  
  Valor del contador: 1  
  Valor del contador: 2  
  Valor del contador: 3  
  ```  

---

### **3. Problema del productor-consumidor**  
- **Descripción:** Simula un sistema donde productores generan datos y consumidores los procesan, trabajando sobre una cola limitada. El acceso al buffer está protegido para evitar problemas de concurrencia.  
- **Tecnología:** `ReentrantLock`, `Condition`.  
- **Clases involucradas:**  
  - `Buffer`: Administra una cola limitada con bloqueo para manejar la concurrencia.  
  - `Productor`: Genera números aleatorios y los añade al buffer.  
  - `Consumidor`: Consume datos del buffer y los procesa.  
  - `MainEjercicio3`: Inicia 2 productores y 3 consumidores simultáneamente.  
- **Ejemplo de salida:**  
  ```  
  Productor produjo: 12  
  Consumidor consumió: 12  
  Productor produjo: 7  
  Consumidor consumió: 7  
  ```  

---

### **4. Control de lectores y escritores**  
- **Descripción:** Simula un sistema de base de datos con múltiples lectores y escritores. Los lectores pueden acceder simultáneamente, pero los escritores requieren acceso exclusivo.  
- **Tecnología:** `synchronized`.  
- **Clases involucradas:**  
  - `BaseDeDatos`: Contiene los métodos sincronizados `leer()` y `escribir()` para manejar el acceso concurrente.  
  - `MainEjercicio4`: Lanza 5 hilos lectores y 2 hilos escritores.  
- **Ejemplo de salida:**  
  ```  
  Lector A está leyendo...  
  Lector B está leyendo...  
  Lector A ha terminado de leer.  
  Escritor X está escribiendo...  
  Escritor X ha terminado de escribir.  
  ```  

---

## **📂 Estructura del proyecto**  

```plaintext
src/
├── ejercicio1/
│   ├── Servidor.java
│   ├── MainEjercicio1.java
├── ejercicio2/
│   ├── Contador.java
│   ├── MainEjercicio2.java
├── ejercicio3/
│   ├── Buffer.java
│   ├── Productor.java
│   ├── Consumidor.java
│   ├── MainEjercicio3.java
├── ejercicio4/
│   ├── BaseDeDatos.java
│   ├── MainEjercicio4.java
```

---

## **🎯 Objetivo del examen**  
- Practicar el uso de herramientas de sincronización (`Lock`, `synchronized`).  
- Identificar problemas comunes en programación concurrente y solucionarlos eficazmente.  
- Entender las diferencias prácticas entre las técnicas de sincronización.  


# **Apéndice Teórico: Concurrencia en Java**  

La concurrencia es un paradigma de programación que permite ejecutar múltiples tareas de manera simultánea, mejorando la eficiencia y el rendimiento de las aplicaciones. En Java, la concurrencia se implementa principalmente con **hilos (hebras)**, y para gestionar el acceso a los recursos compartidos, se utilizan mecanismos de sincronización.  

---

## **Conceptos clave**  

### **Hilos (Threads)**  
Un hilo es una unidad de ejecución dentro de un programa. Java proporciona la clase `Thread` y la interfaz `Runnable` para crear y ejecutar hilos.  
- **Ejemplo básico:**  
  ```java
  public class MiHilo extends Thread {
      @Override
      public void run() {
          System.out.println("Ejecutando un hilo");
      }
  }

  public class Main {
      public static void main(String[] args) {
          MiHilo hilo = new MiHilo();
          hilo.start(); // Inicia el hilo
      }
  }
  ```  

### **Clase monitora**  
En Java, cada objeto puede actuar como un **monitor**. Un monitor es una entidad que controla el acceso a secciones críticas del código, permitiendo que solo un hilo acceda a un recurso compartido a la vez.  

---

## **Sincronización en Java**  

La sincronización es esencial para evitar **condiciones de carrera** y garantizar que los recursos compartidos se utilicen de forma segura en entornos concurrentes. Java proporciona dos mecanismos principales para implementar sincronización:  

---

### **1. `synchronized`**  

#### **Cómo funciona:**  
El modificador `synchronized` permite que solo un hilo acceda a un método o bloque crítico a la vez.  

#### **Ejemplo básico:**  
```java
public class Contador {
    private int cuenta = 0;

    public synchronized void incrementar() {
        cuenta++;
    }

    public synchronized int getCuenta() {
        return cuenta;
    }
}
```

#### **Ventajas:**  
- Simple de usar.  
- Directamente asociado al objeto que actúa como monitor.  

#### **Desventajas:**  
- Menos flexible que otras herramientas, como `Lock`.  

---

### **2. `Lock` (de `java.util.concurrent.locks`)**  

#### **Cómo funciona:**  
Un `Lock` proporciona un control más explícito sobre el acceso a los recursos compartidos. Es más flexible que `synchronized` y permite condiciones personalizadas.  

#### **Ejemplo básico:**  
```java
import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class Contador {
    private int cuenta = 0;
    private final Lock lock = new ReentrantLock();

    public void incrementar() {
        lock.lock();
        try {
            cuenta++;
        } finally {
            lock.unlock();
        }
    }

    public int getCuenta() {
        lock.lock();
        try {
            return cuenta;
        } finally {
            lock.unlock();
        }
    }
}
```

#### **Ventajas:**  
- Soporta condiciones específicas con `Condition`.  
- Permite más control sobre los bloqueos y desbloqueos.  

#### **Desventajas:**  
- Requiere más código que `synchronized`.  
- Los errores en el uso de `lock()` y `unlock()` pueden causar problemas, como **deadlocks**.  

---

## **¿Cuándo usar cada uno?**  

- Usa **`synchronized`** para casos simples donde no necesitas configuraciones especiales.  
- Usa **`Lock`** cuando necesites más flexibilidad o funcionalidades avanzadas, como:  
  - Bloqueos no bloqueantes.  
  - Control de tiempo de espera.  
  - Condiciones personalizadas con `Condition`.  

---

**Nota:** Estos mecanismos garantizan la seguridad de los datos en contextos concurrentes, pero también pueden reducir el rendimiento si no se usan adecuadamente. Por ello, es importante planificar cuidadosamente qué se sincroniza y cómo.  
