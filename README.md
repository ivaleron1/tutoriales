# Creación de Chat Bot 🤖

Este proyecto es un ejemplo de cómo crear un chatbot que pueda integrarse fácilmente con diferentes plataformas de mensajería, como aplicaciones móviles, sitios web, etc. Para lograr esto, se ha adoptado un enfoque agnóstico en cuanto a la plataforma, lo que significa que la lógica del chatbot está separada de la implementación específica de la interfaz de usuario.

## Descripción

El objetivo de este proyecto es crear un chatbot que pueda integrarse fácilmente con diferentes plataformas de mensajería, como aplicaciones móviles, sitios web, etc. Para lograr esto, se ha adoptado un enfoque agnóstico en cuanto a la plataforma, lo que significa que la lógica del chatbot está separada de la implementación específica de la interfaz de usuario.
Si quieres saber cómo crear un chat bot desde 0 visita nuestra guía de [Cómo crear un chat bot en 4 pasos](como-crear-chatbot.md) 

## Estructura del Proyecto 🏗️

La estructura del proyecto se basa en las siguientes partes:

- **Plataforma de procesamiento del lenguaje natural (NLP)**
- **Módulo de procesamiento de datos**
- **Entrada y salida de datos**
- **Integración de aplicaciones**

Sustituyendo las partes por las que vamos a usar en nuestro caso tenemos las siguientes tecnologías:

- **Dialogflow:** Plataforma que procesará los datos de entrada y salida, de la cual obtendremos parámetros para procesar.
- **Node.js (opcional):** Servidor intermedio, es decir, el entorno de ejecución que contendrá la lógica de procesamiento de los datos antes de enviarlos al cliente.
- **React Native:** Librería que nos ayudará a crear la interfaz de entrada y salida de datos.
- **Firebase:** Si se pretenden almacenar los datos, sería buena idea emplear esta herramienta.

## Configuración ⚙️

Se deben configurar los marcos de desarrollo y herramientas que vamos a emplear para ello.

## Preparación del motor de lenguaje 🗣️

Se deben definir los siguientes campos:

- **Agente:** Antes de cualquier cosa se debe crear el agente que será equivalente al bot que vamos a consultar.
- **Intenciones:** Son el conjunto de pares "pregunta-respuesta" que se esperan del motor y que le serán de utilidad para el entrenamiento.
- **Entidades:** Para que el motor entienda y distinga a qué hace referencia cada palabra, podemos definir entidades y entrenar el modelo en función de ellas.
- **Parámetros:** Los parámetros son las entidades que han sido encontradas en las strings de entrada.
- **Contexto:** Para que la inteligencia artificial no se confunda y mantenga el hilo de la conversación, se emplean los contextos, que pueden producirse como output de un intento y servir de input a otro.

## Crear la interfaz de entrada y salida de datos 💻

En el caso de React Native, mediante la creación de la interfaz de un chat y de la lógica que controla la dinámica visual de las burbujas de mensajes que van generándose, el botón de envío y demás, ya estaría preparado el sistema de recogida y muestra de datos.

## Servidor de procesamiento 🧠

Esta lógica de procesamiento puede encontrarse localmente en funciones o archivos concretos. Por un lado, debemos almacenar los datos en un array (si usamos la librería react-native-gifted-chat), ya que el componente que importamos nos provee de una constante denominada "messages" que hará uso del hook "useState" para actualizar su estado y reflejarlo visualmente.

## Integración de aplicaciones 🔌

Para comunicar los distintos módulos o partes de la aplicación, es posible realizar llamadas a la API de Dialogflow con el estándar REST.
